# 03 Software Design Specification (SDS)

## 1. Introduction
This document outlines the software design for **Project B: CSV Mini Database & Query Engine** (Jira ID: DF11402-7). It details the architectural components, class interfaces, and underlying data structures required to fulfill the project's educational objectives: CSV handling, indexing, querying, and performance profiling.

## 2. System Architecture Overview
The system will be composed of five core subsystems:
1.  **Data Storage & Representation:** In-memory representation of tabular CSV data (Rows and Columns).
2.  **Parser Engine:** Reads and ingests raw CSV files. Supports interchangeable parsing strategies.
3.  **Indexing Engine:** Creates auxiliary data structures to optimize search operations.
4.  **Query Engine:** Consists of a lexer, parser, and executor to interpret a custom querying grammar in natural text strings.
5.  **Performance Profiler & Cache Manager:** Measures search times, records memory usage, and employs caching mechanisms.

## 3. Core Component Design

### 3.1 Data Storage Module
- **`Column` / `Field`:** Represents individual data points. Data types will primarily be treated as strings, with optional casting features for numeric indices.
- **`Row`:** A single entry (record) representing one parsed line from the CSV.
- **`Table / Database`:** A collection of `Row`s. Holds metadata such as column headers for query mapping.

### 3.2 CSV Parser Engine
To support the project's requirements for parsing flexibility, a Strategy pattern will be used:
- **`ICsvParser` (Interface):** Defines the `parse(filepath)` method returning a `Table`.
- **`CustomCsvParser`:** A robust, from-scratch implementation explicitly managing edge cases, such as commas inside quotes and embedded newlines.
- **`ThirdPartyCsvParser` (Optional):** Wraps an integrated third-party library (via vcpkg or Conan) into the parser interface.

### 3.3 Indexing Engine
The engine requires multiple search paths to teach spatial and temporal trade-offs:
- **`IIndex` (Interface):** Defines standard operations like `build(columnData)` and `find(value)`.
- **`LinearSearchIndex` (Baseline):** Performs an explicit O(N) scan. Used for benchmark comparisons.
- **`HashIndex`:** Implemented using standard hash maps for O(1) exact-match retrieval.
- **`BstIndex` (Optional for Extensions):** Implemented using Binary Search Trees or B-trees for range query support.

### 3.4 Query Engine & Grammar
Designed to interpret simple query strings.
- **Grammar Structure:** E.g., `SELECT <col1, col2> FROM <table> WHERE <col> = <value>`.
- **`Lexer / Tokenizer`:** Breaks down the raw input string into manageable tokens (e.g., `KEYWORD_SELECT`, `IDENTIFIER`, `OPERATOR_EQ`, `VALUE`).
- **`QueryParser`:** Analyzes token streams to ensure grammatical correctness and builds a `Query` object (AST equivalent).
- **`ExecutionEngine`:** Takes a `Query` object, determines the optimal `IIndex` to use, executes the search, filters non-selected columns, and returns a formatted result set.

### 3.5 Performance Profiling & Caching
- **`QueryCache`:** An LRU (Least Recently Used) mapping of `<queryString, resultSet>`. Checked before query compilation/execution.
- **`Profiler`:** A utility singleton or wrapper utilizing C++ `<chrono>` to time query execution components (parsing, indexing, retrieval) and providing a memory footprint estimation of index structures compared to the raw data representation.

## 4. Integration & Data Flow
1. Upon system start-up, a user specifies a CSV and parsing strategy.
2. The `Parser Engine` reads the file and populates the `Data Storage Module`.
3. The user or system invokes the `Indexing Engine` to build specific indices on chosen columns.
4. A user inputs a textual query. The `Query Engine` checks the `QueryCache`.
5. If a cache miss occurs, the query is lexed, parsed, and executed. The `ExecutionEngine` routes the search to the appropriate `Index`, gathers the `Row`s, and caches the result.
6. The `Profiler` outputs the temporal cost metrics.