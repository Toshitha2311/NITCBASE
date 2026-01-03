# NITCBase – Mini Database Management System

NITCBase is a **Mini Database Management System** developed as part of a university laboratory course. It is a lightweight DBMS designed for educational purposes, demonstrating core database concepts such as query processing, buffering, block access, and indexing using B+ Trees.

---

## Table of Contents

* [Prerequisites](#prerequisites)
* [Getting Started](#getting-started)
* [Installation](#installation)
* [Running NITCBase](#running-nitcbase)
* [Usage](#usage)
* [Documentation](#documentation)
* [Contributing](#contributing)
* [License](#license)

---

## Prerequisites

Before running **NITCBase**, ensure that you have the following installed:

* Linux-based operating system (tested on **Ubuntu**)
* C/C++ compiler (**gcc / g++**)
* **Git**

---

## Getting Started

Follow the steps below to set up and run NITCBase on your system.

### Installation

1. Clone the repository:

```bash
git clone https://github.com/Toshitha2311/NITCBASE
```

2. Navigate to the project directory:

```bash
cd nitcbase/mynitcbase
```

3. Build the project:

```bash
make
```

> If you face build issues, ensure all required packages and build tools are installed.

---

### Running NITCBase

After a successful build, run the executable:

```bash
./nitcbase
```

---

## Usage

NITCBase provides a **command-line interface** for executing SQL-like database operations such as:

* `INSERT`
* `SELECT`
* `PROJECT`
* `JOIN`

For detailed command syntax and examples, refer to the **XFS-Documentation** provided with the project.

---

## Documentation

For detailed information about:

* System architecture
* Design methodology
* Internal layers
* Development roadmap

please refer to the official **NITCBase Documentation**.

---

## Algebra Layer

The **Front End** parses SQL-like queries and converts them into a sequence of method calls to the **algebra layer** and **schema layer**.

The algebra layer handles:

* Insert operations
* Data retrieval
* Creation of target relations for query results

### Return Values

All algebra layer functions return one of the following status codes:

* `SUCCESS` – Operation completed successfully
* `E_RELNOTOPEN` – Relation is not open
* `E_NATTRMISMATCH` – Number of attributes mismatch
* `E_ATTRTYPEMISMATCH` – Attribute type mismatch
* `E_DISKFULL` – Insufficient disk space
* `E_NOTPERMITTED` – Operation not permitted on catalog relations (`RELATIONCAT`, `ATTRIBUTECAT`)

---

### Functions

#### Insert

```c
int insert(char relName[ATTR_SIZE], int numberOfAttributes, char record[][ATTR_SIZE]);
```

#### Project (Specified Attributes)

```c
int project(char srcRel[ATTR_SIZE], char targetRel[ATTR_SIZE], int tar_nAttrs, char tar_Attrs[][ATTR_SIZE]);
```

#### Project (All Attributes / Copy Relation)

```c
int project(char srcRel[ATTR_SIZE], char targetRel[ATTR_SIZE]);
```

#### Select

```c
int select(char srcRel[ATTR_SIZE], char targetRel[ATTR_SIZE], char attr[ATTR_SIZE], int op, char strVal[ATTR_SIZE]);
```

#### Join

```c
int join(char srcRelOne[ATTR_SIZE], char srcRelTwo[ATTR_SIZE], char targetRel[ATTR_SIZE], char attrOne[ATTR_SIZE], char attrTwo[ATTR_SIZE]);
```

---

## Block Access Layer

The **Block Access Layer** processes update and retrieval requests from the algebra and schema layers. It operates on disk blocks that are buffered by the **Buffer Layer**.

### Functions

#### Linear Search

```c
RecId linearSearch(int relId, char *attrName, Attribute attrVal, int op);
```

#### Search

```c
int search(int relId, Attribute *record, char *attrName, Attribute attrVal, int op);
```

#### Insert Record

```c
int insert(int relId, union Attribute *record);
```

#### Rename Relation

```c
int renameRelation(char *oldName, char *newName);
```

#### Rename Attribute

```c
int renameAttribute(char *relName, char *oldName, char *newName);
```

#### Delete Relation

```c
int deleteRelation(char *relName);
```

#### Project Record

```c
int project(int relId, Attribute *record);
```

---

## B+ Tree Layer

The **B+ Tree Layer** provides indexing support for efficient search and retrieval of records based on attribute values.

### Functions

#### Create Index

```c
int bPlusCreate(int relId, char attrName[ATTR_SIZE]);
```

#### Search Index

```c
RecId bPlusSearch(int relId, char attrName[ATTR_SIZE], union Attribute attrVal, int op);
```

#### Destroy Index

```c
int bPlusDestroy(int rootBlockNum);
```

#### Insert Entry

```c
int bPlusInsert(int relId, char attrName[ATTR_SIZE], union Attribute attrVal, RecId recordId);
```

#### Internal Operations

```c
int findLeafToInsert(int rootBlock, Attribute attrVal, int attrType);
int insertIntoLeaf(int relId, char attrName[ATTR_SIZE], int blockNum, Index entry);
int splitLeaf(int leafBlockNum, Index indices[]);
int insertIntoInternal(int relId, char attrName[ATTR_SIZE], int blockNum, Index entry);
int splitInternal(int intBlockNum, InternalEntry internalEntries[]);
int createNewRoot(int relId, char attrName[ATTR_SIZE], Attribute attrVal, int lChild, int rChild);
```

---

## Contributing

Contributions are welcome! Please create a pull request and follow proper naming conventions. After review, useful contributions will be merged.

---

## License

NITCBase is licensed under the **MIT License**.

---

Thank you for using **NITCBase**. If you encounter any issues or have questions, feel free to open an issue on the GitHub repository.
