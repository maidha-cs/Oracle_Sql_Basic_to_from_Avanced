# Oracle Database — Partitioning

## 1. Introduction

**Partitioning** is a database technique used to divide a large table or index into smaller, manageable pieces called **partitions**.

The partitions belong to the same logical table, but Oracle can manage and access the data in smaller portions.

### Simple Example

Suppose a `SALES` table contains millions of rows.

Instead of keeping all data in one large partition:

```text
SALES
│
└── Millions of rows
```

We can divide it into partitions:

```text
SALES
│
├── 2024 Data
├── 2025 Data
└── 2026 Data
```

The table is still logically one `SALES` table.

---

# 2. Why is Partitioning Used?

Partitioning is useful when a table contains a **large amount of data**.

Main benefits include:

* Makes large tables easier to manage.
* Can improve query performance in suitable cases.
* Allows Oracle to work with only relevant partitions when possible.
* Makes maintenance operations easier.
* Helps organize data logically.
* Can make backup and data-management operations easier in large databases.

---

# 3. Partition

A **partition** is a smaller logical portion of a partitioned table or index.

Example:

```text
SALES
│
├── SALES_2024
├── SALES_2025
└── SALES_2026
```

Here:

* `SALES` = main table
* `SALES_2024` = partition
* `SALES_2025` = partition
* `SALES_2026` = partition

---

# 4. Partitioning Key

A **partitioning key** is the column or set of columns Oracle uses to decide which partition should contain a row.

For example, if sales data is divided according to date:

```text
sale_date
```

can be the partitioning key.

If student data is divided according to department:

```text
department
```

can be the partitioning key.

---

# 5. Types of Partitioning

Oracle provides different partitioning methods.

The four important types are:

1. Range Partitioning
2. List Partitioning
3. Hash Partitioning
4. Composite Partitioning

---

# 6. Range Partitioning

**Range Partitioning** divides data according to a **range of values**.

It is commonly used with:

* Dates
* Years
* Numbers
* Salaries
* Age ranges

### Example

Sales data can be divided by year:

```text
SALES
│
├── 2024
├── 2025
└── 2026
```

Here, the value of the date determines the partition.

### Simple Definition

> Range Partitioning divides data into partitions based on a range of values.

---

# 7. List Partitioning

**List Partitioning** divides data according to **specific values or categories**.

It is useful when data belongs to predefined groups.

### Example

A university has students from different departments:

```text
STUDENTS
│
├── CS
├── IT
├── BBA
└── SE
```

Here, `department` can be used to divide the data.

### Simple Definition

> List Partitioning divides data based on specific values or categories.

### Common Examples

* Department
* Country
* Region
* Product category
* Status

---

# 8. Hash Partitioning

**Hash Partitioning** distributes rows among partitions using a **hash algorithm**.

Unlike Range or List Partitioning, the user does not normally define specific value ranges or categories for each partition.

Oracle calculates the hash value and uses it to determine the partition.

### Example

```text
STUDENTS
│
├── Partition 1
├── Partition 2
├── Partition 3
└── Partition 4
```

Oracle distributes the rows among these partitions.

### Main Purpose

Hash Partitioning is useful for distributing data more evenly among partitions.

### Simple Definition

> Hash Partitioning distributes data among partitions using a hash algorithm.

---

# 9. Composite Partitioning

**Composite Partitioning** combines two partitioning methods.

It uses partitioning at more than one level.

### Example

First divide sales by year:

```text
SALES
│
├── 2025
│
└── 2026
```

Then divide each year using another method:

```text
2025
├── Partition 1
├── Partition 2
└── Partition 3

2026
├── Partition 1
├── Partition 2
└── Partition 3
```

So composite partitioning can combine methods such as:

* Range + Hash
* Range + List
* List + Hash

### Simple Definition

> Composite Partitioning combines two partitioning methods to divide data at multiple levels.

---

# 10. Range vs List vs Hash vs Composite

| Type      | Data is divided based on            |
| --------- | ----------------------------------- |
| Range     | Range of values                     |
| List      | Specific values/categories          |
| Hash      | Hash algorithm                      |
| Composite | Combination of partitioning methods |

### Easy Memory Trick

```text
Range     → Range
List      → List / Categories
Hash      → Hash Algorithm
Composite → Combination
```

---

# 11. Partitioning Does NOT Mean Separate Tables

This is an important point.

Suppose we have:

```text
SALES
│
├── 2024 Partition
├── 2025 Partition
└── 2026 Partition
```

These are **partitions of one table**, not three completely independent tables.

Logically:

```text
One Table
    ↓
Multiple Partitions
```

---

# 12. Partitioning and Large Data

Partitioning is especially useful for **large tables**.

For example:

```text
Customer Transactions
→ Millions of rows
→ Years of historical data
```

Instead of treating all data as one huge storage portion, Oracle can organize it into partitions.

This makes large datasets easier to manage.

---

# 13. Partition Pruning

One important performance concept related to partitioning is **Partition Pruning**.

Partition pruning means Oracle can identify the relevant partition or partitions for a query and avoid scanning unnecessary partitions.

### Example

Suppose sales are partitioned by year:

```text
SALES
│
├── 2024
├── 2025
└── 2026
```

If a query asks only for 2026 sales, Oracle may access only the relevant partition instead of scanning every partition.

```text
Query
  ↓
2026 Sales
  ↓
2026 Partition
```

This can reduce the amount of data Oracle needs to examine.

---

# 14. Important Terms

### Partition

A smaller logical portion of a partitioned table or index.

### Partitioning Key

The column or columns used to determine partition placement.

### Partitioning Method

The technique used to divide data, such as Range, List, Hash, or Composite.

### Partition Pruning

The process of eliminating unnecessary partitions from consideration during a query.

---

# 15. Quick Revision

```text
Partitioning
     ↓
Large Table
     ↓
Smaller Partitions
     ↓
Easier Management
     ↓
Potential Performance Benefits
```

### Four Main Types

```text
Range      → Range of values
List       → Specific categories
Hash       → Hash-based distribution
Composite  → Combination of methods
```

---

# 16. Exam Definitions

**Partitioning:**

> Partitioning is a database technique that divides a large table or index into smaller, manageable partitions.

**Range Partitioning:**

> Range Partitioning divides data based on a range of values.

**List Partitioning:**

> List Partitioning divides data based on specific values or categories.

**Hash Partitioning:**

> Hash Partitioning distributes data among partitions using a hash algorithm.

**Composite Partitioning:**

> Composite Partitioning combines two partitioning methods to divide data at multiple levels.

**Partition Pruning:**

> Partition pruning allows Oracle to avoid unnecessary partitions when processing a query.

---

# Final Memory Trick 🧠

> **Partitioning = Large Data → Smaller Logical Parts**

```text
Range      → Range
List       → Category
Hash       → Automatic Distribution
Composite  → Combination
```
