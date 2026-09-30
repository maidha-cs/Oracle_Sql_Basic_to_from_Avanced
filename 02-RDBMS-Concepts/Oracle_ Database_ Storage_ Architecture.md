# Topic 5: Oracle Database Storage Architecture

## 1. Introduction

Oracle Database uses a storage architecture to organize and store data efficiently.

The main storage hierarchy is:

```text
Database
   ↓
Tablespace
   ↓
Segment
   ↓
Extent
   ↓
Data Block
   ↓
Rows
```

A simple way to understand it is:

> **Tablespace organizes storage, datafiles provide physical storage, segments store objects, extents contain groups of blocks, and blocks store data.**

---

# 2. Tablespace

A **Tablespace** is a logical storage container in an Oracle database.

It is used to logically organize the storage of database objects.

Example:

```text
Oracle Database
      ↓
USERS Tablespace
```

A tablespace can contain one or more datafiles.

### Simple Definition

> **Tablespace = Logical storage container**

---

# 3. Datafile

A **Datafile** is a physical file stored on disk that provides storage for a tablespace.

Example:

```text
USERS Tablespace
      │
      ├── users01.dbf
      └── users02.dbf
```

A tablespace can have multiple datafiles.

### Simple Definition

> **Datafile = Physical file used to store database data**

### Tablespace vs Datafile

| Tablespace                     | Datafile                       |
| ------------------------------ | ------------------------------ |
| Logical structure              | Physical file                  |
| Used to organize storage       | Provides physical disk storage |
| Can contain multiple datafiles | Belongs to a tablespace        |

### Memory Trick

> **Tablespace = Logical**
> **Datafile = Physical**

---

# 4. Segment

A **Segment** is a logical storage structure associated with a database object that requires storage.

For example, a table can have a table segment.

```text
STUDENTS Table
      ↓
STUDENTS Segment
```

Other objects such as indexes can also have segments when they require storage.

### Simple Definition

> **Segment = Storage allocated for a database object**

---

# 5. Extent

An **Extent** is a set of contiguous data blocks allocated for a segment.

A segment can have multiple extents.

Example:

```text
STUDENTS Segment
      │
      ├── Extent 1
      ├── Extent 2
      └── Extent 3
```

### Simple Definition

> **Extent = A group of data blocks allocated to a segment**

When a segment needs more space, Oracle can allocate additional extents.

---

# 6. Data Block

A **Data Block** is the smallest unit of data storage used by Oracle Database.

Actual table rows are stored in data blocks.

Example:

```text
Extent
   │
   ├── Block 1
   ├── Block 2
   ├── Block 3
   └── Block 4
```

### Simple Definition

> **Data Block = Basic unit of Oracle data storage**

---

# 7. Row

A **Row** represents a record in a table.

Example:

```text
STUDENTS

ID    NAME       DEPARTMENT
1     Ali        Computer Science
2     Sara       IT
```

Each line is a row/record.

Rows are stored inside data blocks.

```text
Data Block
    ↓
Rows
```

---

# 8. Complete Storage Hierarchy

The main concept of this topic can be represented as:

```text
DATABASE
   ↓
TABLESPACE
   ↓
DATAFILE
   ↓
SEGMENT
   ↓
EXTENT
   ↓
DATA BLOCK
   ↓
ROW
```

However, remember that **tablespace → datafile** describes the logical-to-physical storage relationship, while **segment → extent → block** describes how object storage is allocated.

---

# 9. Example

Suppose we create a table:

```sql
CREATE TABLE students (
    student_id NUMBER,
    name VARCHAR2(50),
    department VARCHAR2(50)
);
```

Conceptually, Oracle manages its storage like this:

```text
USERS Tablespace
       ↓
Datafile
       ↓
STUDENTS Segment
       ↓
Extents
       ↓
Data Blocks
       ↓
Student Rows
```

The actual physical details are managed by Oracle; users normally work with tables and other logical objects rather than manually managing individual blocks.

---

# 10. Tablespace and Object Storage

Database objects are stored using storage structures managed within tablespaces.

For example:

```text
USERS Tablespace
       │
       ├── STUDENTS table
       │       ↓
       │    Segment
       │
       ├── COURSES table
       │       ↓
       │    Segment
       │
       └── Student index
               ↓
             Segment
```

This shows why tablespaces are important for organizing database storage.

---

# 11. Important Differences

## Tablespace vs Segment

**Tablespace** is a logical storage container.

**Segment** is storage associated with a particular database object.

```text
Tablespace
   ↓
contains/organizes storage for objects

Segment
   ↓
storage for a particular object
```

---

## Segment vs Extent

**Segment** is the overall storage structure for an object.

**Extent** is a group of blocks allocated to that segment.

```text
Segment
   ↓
Extent 1
Extent 2
Extent 3
```

---

## Extent vs Data Block

**Extent** = group of data blocks.

**Data Block** = individual/basic storage unit.

```text
Extent
  ↓
Block
Block
Block
Block
```

---

# 12. Easy Real-Life Example

Imagine a large library:

```text
Library
   ↓
Section
   ↓
Shelf
   ↓
Group of books
   ↓
Individual book
```

Oracle can be understood similarly:

```text
Database
   ↓
Tablespace
   ↓
Segment
   ↓
Extent
   ↓
Data Block
```

This analogy is only for understanding the hierarchy; Oracle's actual storage architecture is more detailed.

---

# 13. Quick Revision

| Term           | Easy Meaning                  |
| -------------- | ----------------------------- |
| **Tablespace** | Logical storage container     |
| **Datafile**   | Physical file on disk         |
| **Segment**    | Storage for a database object |
| **Extent**     | Group of data blocks          |
| **Data Block** | Basic unit of Oracle storage  |
| **Row**        | Actual record in a table      |

---

# 14. Exam Definitions

### Tablespace

> A tablespace is a logical storage container used to organize database storage.

### Datafile

> A datafile is a physical operating-system file that provides storage for a tablespace.

### Segment

> A segment is a logical storage structure associated with a database object.

### Extent

> An extent is a set of data blocks allocated to a segment.

### Data Block

> A data block is the smallest unit of data storage used by Oracle Database.

---

# 15. Final Memory Trick

Remember these five words:

```text
TABLESPACE → Container
DATAFILE   → Physical file
SEGMENT    → Object storage
EXTENT     → Group of blocks
BLOCK      → Basic storage unit
```

### One-line summary

> **Oracle organizes storage using tablespaces and datafiles, while object storage is managed through segments, extents, and data blocks.**
