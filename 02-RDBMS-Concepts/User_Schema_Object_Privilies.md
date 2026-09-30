# Topic 3: Schema, User, Objects & Privileges

## 1. User

A **User** is a database account used to identify and authenticate a person or application in Oracle Database.

A user can log in to the database and can receive permissions to access database resources.

### Example

```sql
CREATE USER MAIDHA IDENTIFIED BY password;
```

Here, `MAIDHA` is the database user.

### Simple Definition

> **User = Database account / login identity**

---

## 2. Schema

A **Schema** is a logical collection of database objects owned by a user.

In Oracle, a user and its schema are closely connected. A user's schema normally has the same name as the user.

### Example

```text
MAIDHA User
     ↓
MAIDHA Schema
     ↓
STUDENTS
COURSES
TEACHERS
```

### Simple Definition

> **Schema = Logical container/namespace for a user's database objects**

---

## 3. User vs Schema

User and Schema are related, but they are **not exactly the same thing**.

| User                              | Schema                             |
| --------------------------------- | ---------------------------------- |
| Database account                  | Collection/namespace of objects    |
| Used for login and authentication | Used for organizing/owning objects |
| Related to security               | Related to database objects        |
| Can have privileges               | Contains objects owned by the user |

### Easy Memory Trick

> **User = Who are you?**
> **Schema = What objects do you own?**

---

# 4. Database Objects

A **database object** is a structure created and stored in an Oracle database.

Common database objects include:

* Table
* View
* Index
* Sequence
* Synonym
* Procedure
* Function
* Package
* Trigger

---

## 5. Table

A **Table** is used to store data in rows and columns.

Example:

```sql
CREATE TABLE students (
    student_id NUMBER,
    name VARCHAR2(50),
    department VARCHAR2(50)
);
```

Here, `STUDENTS` is a table object.

### Simple Definition

> **Table = Stores data in rows and columns**

### Common Table Types

* Heap-organized table — normal/default table
* Temporary table — used for temporary data
* External table — accesses data stored outside the Oracle database
* Index-organized table (IOT) — data is organized according to a primary key index
* Partitioned table — large table divided into partitions

### Primary Key

A Primary Key is **not limited to IOT**.

Normal, temporary, and partitioned tables can also have a Primary Key.

The important difference is:

> **IOT requires a Primary Key, while other common table types do not necessarily require one.**

---

# 6. View

A **View** is a virtual representation of data based on one or more tables or other views.

Example:

```sql
CREATE VIEW cs_students AS
SELECT name, department
FROM students
WHERE department = 'Computer Science';
```

The view can then be queried:

```sql
SELECT * FROM cs_students;
```

### Simple Definition

> **View = Virtual/customized view of data**

---

# 7. Index

An **Index** is a database object that can improve the efficiency of finding rows in a table.

Example:

```sql
CREATE INDEX idx_student_name
ON students(name);
```

### Simple Definition

> **Index = Helps Oracle find data efficiently**

Indexes also require storage and maintenance, so they should be designed appropriately.

---

# 8. Sequence

A **Sequence** generates numeric values, often used for identifiers.

Example:

```sql
CREATE SEQUENCE student_seq
START WITH 1
INCREMENT BY 1;
```

Get the next value:

```sql
SELECT student_seq.NEXTVAL FROM dual;
```

### Simple Definition

> **Sequence = Generates a sequence of numbers**

---

# 9. Synonym

A **Synonym** is an alternative name for another database object.

Example:

```sql
CREATE SYNONYM students
FOR maidha.students;
```

Instead of:

```sql
SELECT * FROM maidha.students;
```

the synonym can allow:

```sql
SELECT * FROM students;
```

assuming the required privileges exist.

### Simple Definition

> **Synonym = Alternative name for a database object**

---

# 10. Procedure

A **Procedure** is a stored PL/SQL program that performs a specific task.

Example:

```sql
CREATE OR REPLACE PROCEDURE show_message
AS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello Oracle');
END;
/
```

### Simple Definition

> **Procedure = Stored program used to perform a task**

---

# 11. Function

A **Function** is a stored PL/SQL program that returns a value.

Example:

```sql
CREATE OR REPLACE FUNCTION add_numbers(
    a NUMBER,
    b NUMBER
)
RETURN NUMBER
AS
BEGIN
    RETURN a + b;
END;
/
```

### Simple Definition

> **Function = Stored program that returns a value**

### Procedure vs Function

| Procedure                       | Function                                        |
| ------------------------------- | ----------------------------------------------- |
| Performs a task                 | Returns a value                                 |
| Does not require a return value | Must have a return value                        |
| Commonly used for operations    | Commonly used for calculations/returning values |

### Memory Trick

> **Procedure → Perform**
> **Function → Return**

---

# 12. Trigger

A **Trigger** is a stored program that automatically executes when a specified database event occurs.

Common events include:

```text
INSERT
UPDATE
DELETE
```

Example use:

When an employee's salary is updated, a trigger can automatically record the old and new salary in an audit table.

### Simple Definition

> **Trigger = Automatically runs when a specified database event occurs**

---

# 13. Package

A **Package** groups related PL/SQL elements together, such as procedures, functions, variables, and other program elements.

Example:

```text
BANK_PACKAGE
│
├── add_customer()
├── update_customer()
├── delete_customer()
└── find_customer()
```

### Simple Definition

> **Package = A group of related PL/SQL program elements**

---

# 14. Privileges

A **Privilege** is permission to perform a particular action or access a database resource.

For example, a user may be allowed to:

* Read data
* Insert data
* Update data
* Delete data

---

## GRANT

`GRANT` is used to give privileges to a user.

Example:

```sql
GRANT SELECT ON students TO ali;
```

This gives `ALI` permission to read the `STUDENTS` table.

Multiple privileges can also be granted:

```sql
GRANT SELECT, INSERT ON students TO ali;
```

Now ALI can:

```text
SELECT  → Read data
INSERT  → Add data
```

but does not automatically have:

```text
UPDATE  → ❌
DELETE  → ❌
```

---

## REVOKE

`REVOKE` is used to remove a privilege.

Example:

```sql
REVOKE SELECT ON students FROM ali;
```

This removes ALI's `SELECT` privilege on the `STUDENTS` table.

---

# 15. Common Object Privileges

For a table, common privileges include:

```text
SELECT  → Read data
INSERT  → Add rows
UPDATE  → Modify data
DELETE  → Delete rows
```

Example:

```sql
GRANT SELECT, UPDATE
ON students
TO ali;
```

ALI can now read and update the table, assuming there are no other restrictions.

---

# 16. Complete Relationship

The relationship between all these concepts can be understood like this:

```text
                  ORACLE DATABASE
                         │
                    USER: MAIDHA
                         │
                      SCHEMA
                      MAIDHA
                         │
          ┌──────────────┼──────────────┐
          │              │              │
       STUDENTS       COURSES        TEACHERS
        TABLE           TABLE          TABLE
          │
          │
       GRANT SELECT
          │
          ↓
       USER: ALI
          │
          ↓
   Can read MAIDHA.STUDENTS
```

---

# 17. Easy Real-Life Example

Imagine a university.

### User

A student or employee has an account.

```text
User = Ali
```

### Schema

That user's database workspace/namespace.

```text
Ali Schema
```

### Objects

The workspace contains database objects:

```text
Students Table
Courses Table
Student View
Student Index
```

### Privileges

Privileges determine what another user is allowed to do.

```text
SELECT  → Read
INSERT  → Add
UPDATE  → Change
DELETE  → Delete
```

---

# 18. Important Exam Definitions

### User

> A user is a database account used to identify and authenticate a person or application.

### Schema

> A schema is a logical collection of database objects owned by a user.

### Database Object

> A database object is a structure created and stored in a database, such as a table, view, index, sequence, procedure, function, package, or trigger.

### Privilege

> A privilege is permission to perform a specific action or access a database resource.

### GRANT

> GRANT is used to give privileges to a user or other authorized database principal.

### REVOKE

> REVOKE is used to remove privileges.

---

# 19. Final Memory Trick

```text
USER
↓
Who are you?
(Database Account)

SCHEMA
↓
Whose objects?
(Logical collection/namespace)

OBJECT
↓
What is inside?
(Table, View, Index, etc.)

PRIVILEGE
↓
What can you do?
(SELECT, INSERT, UPDATE, DELETE)
```

## One-Line Summary

> **User identifies the database account, Schema organizes/owns its objects, Objects store or process database information, and Privileges control what users are allowed to do.**
