# 01 Project Plan: CSV Mini Database & Query Engine

Based on the core learning objectives and overview, the development of this project will follow a phased approach.

## Phase 1: Foundation and Setup
- **Repository Initialization:** Set up standard C++ tooling, CMake, and a testing framework (e.g., GoogleTest).
- **Data Representation:** Define base C++ classes and structures to hold tabular data (rows and columns) in memory.

## Phase 2: CSV Parsing Implementation
- **Objective:** Handle raw text data from CSV files and convert it into structured formats.
- **Tasks:**
  - Implement a robust custom CSV parser capable of handling edge cases, specifically quoted fields containing delimiters.
  - *(Optional)* Evaluate and integrate a third-party C++ parsing library using a package manager (vcpkg/Conan) as an alternative backend.
  - Write unit tests validating both parsing approaches against malformed and standard CSV inputs.

## Phase 3: Indexing Engine
- **Objective:** Create and utilize index structures to significantly speed up data retrieval.
- **Tasks:**
  - Implement a baseline linear search for comparative baseline metrics.
  - Design and implement secondary indexing (e.g., Hash Maps for exact matches, B-Trees/BSTs for range queries).
  - Bind the indices to the parsed structured data.

## Phase 4: Query Engine & Grammar
- **Objective:** Design and implement a simple, intuitive querying language to interact with the database.
- **Tasks:**
  - Define the query grammar (e.g., simple `SELECT [cols] FROM [file] WHERE [condition]`).
  - Build a basic lexer and parser to tokenize and interpret user query strings.
  - Connect the parsed queries to the indexing engine to execute data retrieval correctly.

## Phase 5: Performance Profiling & Optimization
- **Objective:** Understand the spatial and temporal costs associated with the implemented data structures and algorithms.
- **Tasks:**
  - Profile the search algorithms to analyze temporal execution trade-offs.
  - Measure the memory overhead (spatial cost) of the custom index structures versus the raw data.
  - Implement and evaluate a caching mechanism (e.g., LRU cache for frequent queries) and document the performance gains.