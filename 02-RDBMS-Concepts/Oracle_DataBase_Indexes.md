# Oracle Database — Indexes

## 1. What is an Index?

An **Index** is a database structure that helps Oracle find rows more efficiently.

It is similar to the index of a book:

* Book → contains the actual information
* Book index → helps you find the information quickly

Similarly:

* Table → contains the actual data
* Index → helps Oracle locate the required rows

Indexes can improve query performance, especially when working with large tables.

**Important:** Oracle's optimizer decides whether using an index is beneficial for a particular query.

---

## 2. Why are Indexes Used?

Indexes can help:

* Find rows more efficiently
* Improve performance of suitable queries
* Reduce the amount of data Oracle needs to examine
* Support searching and sorting in appropriate situations
* Improve access to large tables

However, indexes are not always beneficial.

Indexes also require:

* Storage space
* Maintenance when table data changes

---

# 3. B-Tree Index

**B-Tree** is a common general-purpose index structure in Oracle.

B-Tree means **Balanced Tree**.

Conceptually, it has levels such as:

**Root → Branch → Leaf → Row Location**

The leaf level contains information that helps Oracle locate the corresponding table rows.

### Suitable for:

* Many general-purpose queries
* High-cardinality columns
* OLTP-type workloads
* Equality and range searches

### Memory Trick:

**B-Tree = General-purpose Index**

---

# 4. Bitmap Index

A **Bitmap Index** represents values using bitmap information (conceptually 0s and 1s).

It is particularly useful for columns with **low cardinality**.

### Low Cardinality

Low cardinality means a column has a relatively small number of distinct values.

Examples:

* Gender → Male/Female
* Status → Active/Inactive
* Yes/No
* Small set of categories

### Suitable for:

* Data warehouse environments
* Analytical workloads
* Read-heavy systems
* Low-cardinality columns

### Important:

Bitmap indexes are not automatically better than B-Tree indexes.

High-concurrency OLTP workloads can have concurrency/locking considerations with bitmap indexes.

### Memory Trick:

**Bitmap = Low Cardinality + Analytics**

---

# 5. Unique Index

A **Unique Index** prevents duplicate indexed key values.

For example, if a column must contain unique student IDs, duplicate IDs should not be allowed.

### Important Difference

**Primary Key**

* A database constraint
* Enforces uniqueness
* Also does not allow NULL

**Unique Constraint**

* A database constraint
* Enforces uniqueness
* Oracle normally allows multiple NULL values in a unique constraint

**Unique Index**

* A physical database structure
* Ensures indexed key values are unique

Oracle may use/create a unique index to enforce a primary key or unique constraint.

### Memory Trick:

**Primary Key / Unique Constraint = Rule**

**Unique Index = Physical Index Structure**

---

# 6. Composite Index

A **Composite Index** is an index created on **two or more columns**.

Example concept:

**Department + City**

Instead of indexing only one column, multiple columns are included in one index.

### Why use it?

It can support queries that frequently use a combination of columns.

### Column Order is Important

Consider:

**Department + City**

and:

**City + Department**

These are different index definitions.

The first column is called the:

### Leading Column

Example:

**Department + City + Name**

* Department → Leading column
* City → Second column
* Name → Third column

The leading column is important when Oracle considers whether the composite index can efficiently support a query.

### Memory Trick:

**Composite = Combination of Multiple Columns**

---

# 7. Function-Based Index

A **Function-Based Index** is an index created on the result of a **function or expression** applied to one or more columns.

Normal index:

**Name**

Function-based index:

**LOWER(Name)**

Here Oracle can index the result produced by the function/expression.

### Examples of functions/expressions

* LOWER()
* UPPER()
* Mathematical expressions
* Date-related expressions
* Other expressions

### Why use it?

It can help queries that repeatedly search using the same function or expression.

### Memory Trick:

**Function-Based = Function/Expression Result + Index**

---

# 8. Index-Organized Table (IOT)

An **Index-Organized Table (IOT)** is a table in which the table data itself is stored in an **index-organized structure**.

This is different from a normal table.

### Normal Table

Conceptually:

**Table → Data**

**Index → Helps locate Data**

The table and index are separate structures.

### IOT

Conceptually:

**Table Data → Index-Organized Structure**

The table is organized according to its primary key, making primary-key-based access efficient for suitable workloads.

### Important:

IOT is not simply another normal index type.

It is a different way of organizing a table.

### Suitable for:

* Tables frequently accessed using primary key
* Workloads where index-organized storage is appropriate
* Specific storage and access patterns

### Memory Trick:

**Normal Table → Table + Separate Index**

**IOT → Table itself is Index-Organized**

---

# 9. Primary Key vs Index

These two concepts should not be confused.

### Primary Key

A **Primary Key** is a constraint that identifies rows uniquely.

It:

* Must be unique
* Does not allow NULL
* Defines a logical data integrity rule

### Index

An **Index** is a structure used to help Oracle access data efficiently.

Therefore:

**Primary Key ≠ Index**

However, Oracle commonly uses a unique index to enforce a primary key.

---

# 10. Index and Query Optimizer

Oracle has a component called the **Optimizer**.

The optimizer examines a SQL statement and chooses an execution plan.

It may decide:

* Use an index
* Scan the table
* Use another access method

Therefore:

> Having an index does NOT guarantee that Oracle will use it for every query.

The decision depends on factors such as:

* Query conditions
* Data distribution
* Number of rows
* Statistics
* Available indexes
* Estimated cost

---

# 11. Important Index Terms

### Cardinality

The number of distinct values in a column.

Example:

A column containing:

**Male, Female**

has low cardinality.

A column containing thousands of different IDs has high cardinality.

### Leading Column

The first column in a composite index.

### Index Key

The column value or expression used by an index to organize/search data.

### Index Structure

The internal structure used by Oracle to store index information and locate rows.

---

# 12. Main Types Covered

| Index / Concept | Main Idea                                  |
| --------------- | ------------------------------------------ |
| B-Tree          | General-purpose index                      |
| Bitmap          | Useful for low-cardinality data            |
| Unique          | Prevents duplicate indexed key values      |
| Composite       | Uses multiple columns                      |
| Function-Based  | Uses function/expression result            |
| IOT             | Table data organized in an index structure |

---

# 13. Quick Revision

### B-Tree

**General-purpose**

### Bitmap

**Low cardinality**

### Unique

**No duplicate indexed key values**

### Composite

**Multiple columns**

### Function-Based

**Function/expression result**

### IOT

**Table organized as an index structure**

---

# 14. Exam Definitions

### Index

> An index is a database structure that helps Oracle locate table rows efficiently.

### B-Tree Index

> A B-Tree index is a balanced tree-based index structure used for general-purpose data access.

### Bitmap Index

> A bitmap index uses bitmap information and is particularly useful for low-cardinality columns and analytical workloads.

### Unique Index

> A unique index ensures that duplicate indexed key values are not allowed.

### Composite Index

> A composite index is an index created on two or more columns.

### Function-Based Index

> A function-based index is created on the result of a function or expression applied to one or more columns.

### Index-Organized Table

> An Index-Organized Table is a table whose data is stored in an index-organized structure, typically ordered by the primary key.

---

# 15. Final Memory Map

**Oracle Indexes**

→ B-Tree
→ Bitmap
→ Unique
→ Composite
→ Function-Based
→ Index-Organized Table

Remember:

**B-Tree → General**

**Bitmap → Low Cardinality**

**Unique → No Duplicate**

**Composite → Multiple Columns**

**Function-Based → Function Result**

**IOT → Table as Index-Organized Structure**
