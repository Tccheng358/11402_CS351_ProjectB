# 02 Software Requirements Specification (SRS)

## 1. Introduction
### 1.1 Purpose
This document specifies the software requirements for **Project B: CSV Mini Database & Query Engine** (Jira ID: DF11402-7). It outlines the functional and non-functional requirements to build an educational C++ database that parses CSV files, builds indices, and processes queries.

### 1.2 Scope
The system will act as an in-memory database that loads raw CSV text, converts it to structured formats, and allows users to query it using a simple custom grammar. The primary focus is on educational concepts: parsing, indexing, querying, and performance trade-offs.

## 2. Overall Description
The software is a command-line or programmatic library application written in C++ that interacts with standard `.csv` files. It will evaluate different parsing strategies (custom vs. third-party) and analyze the performance impact of indexing and caching.

## 3. Functional Requirements

### 3.1 CSV Data Parsing
- **FR1.1 Custom Parser:** The system MUST include a robust custom parser built from scratch.
- **FR1.2 Quoted Fields:** The parser MUST correctly handle CSV quoted fields containing delimiters (e.g., `,` or newlines).
- **FR1.3 Third-Party Integration (Optional):** The system MAY support a lightweight C++ parsing library integration via vcpkg or Conan.
- **FR1.4 Data Structuring:** The system MUST convert parsed text data into accessible in-memory data structures (rows and columns).

### 3.2 Indexing System
- **FR2.1 Index Creation:** The system MUST allow the creation of index structures on specific columns to speed up data retrieval.
- **FR2.2 Retrieval Engine:** The system MUST retrieve structured data efficiently using the created indices.

### 3.3 Query Grammar and Engine
- **FR3.1 Simple Grammar:** The system MUST define and implement a simple textual querying language to interact with the data (e.g., a simplified SQL-like syntax).
- **FR3.2 Query Execution:** The engine MUST correctly parse incoming textual queries, evaluate the grammar, and return the matched records.

### 3.4 Caching Mechanism
- **FR4.1 Query Caching:** The system SHOULD implement a caching mechanism so that repeated queries perform significantly faster.

## 4. Non-Functional Requirements

### 4.1 Performance & Trade-offs
- **NFR1.1 Profiling Capability:** The system MUST provide a means to calculate and report spatial (memory usage) and temporal (execution time) costs.
- **NFR1.2 Evaluation:** The system MUST allow comparison between different data structures, search algorithms (e.g., linear vs. indexed), and caching strategies.

### 4.2 Language & Technology Stack
- **NFR2.1 Language:** The core engine MUST be written in C++.
- **NFR2.2 Educational Focus:** The code architecture MUST prioritize clear readability and demonstration of foundational database concepts over enterprise-level fault tolerance.