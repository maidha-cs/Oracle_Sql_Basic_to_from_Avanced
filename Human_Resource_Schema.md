# Oracle Human Resources (HR) Schema Overview
The **Oracle HR Schema** is a standard relational database benchmark provided by Oracle to practice SQL queries, database modeling, and schema relationships.
## 1. What is the HR Schema?
The HR Schema represents a real-world enterprise database structure for a global corporation. It stores organized information about **employees, their jobs, compensation, internal department hierarchies, and global office locations**.
## 2. Core Tables & Database Objects
The HR Schema consists of **7 primary interconnected tables**:
| Table Name | Description | Key Attributes |
| **`EMPLOYEES`** | Stores individual staff/worker details. | `employee_id` (PK), `salary`, `job_id` (FK), `department_id` (FK), `manager_id` (FK) |
| **`DEPARTMENTS`** | Defines company business units/sections. | `department_id` (PK), `department_name`, `manager_id` (FK), `location_id` (FK) |
| **`JOBS`** | Defines official job positions and pay scales. | `job_id` (PK), `job_title`, `min_salary`, `max_salary` |
| **`JOB_HISTORY`** | Tracks former roles held by employees. | `employee_id` (FK), `start_date`, `end_date`, `job_id` (FK), `department_id` (FK) |
| **`LOCATIONS`** | Physical addresses of company offices. | `location_id` (PK), `street_address`, `city`, `country_id` (FK) |
| **`COUNTRIES`** | Nations where facilities are based. | `country_id` (PK), `country_name`, `region_id` (FK) |
| **`REGIONS`** | High-level geographical continents/zones. | `region_id` (PK), `region_name` |
## 3. Foreign Key Relationships (Architectural Flow)
The tables are linked using primary-foreign key relationships:
1. **Employee Management:**
   * Each employee belongs to **one Department** (`EMPLOYEES.DEPARTMENT_ID` $\rightarrow$ `DEPARTMENTS.DEPARTMENT_ID`).
   * Each employee holds **one Job role** (`EMPLOYEES.JOB_ID` $\rightarrow$ `JOBS.JOB_ID`).
   * Employees report to a **Manager** who is also an employee (`EMPLOYEES.MANAGER_ID` $\rightarrow$ `EMPLOYEES.EMPLOYEE_ID`).
2. **Geographical Location Hierarchy:**
   * **`REGIONS`** $\rightarrow$ contains multiple **`COUNTRIES`** (`region_id`).
   * **`COUNTRIES`** $\rightarrow$ contains multiple **`LOCATIONS`** (`country_id`).
   * **`LOCATIONS`** $\rightarrow$ hosts multiple **`DEPARTMENTS`** (`location_id`).
3. **Career History:**
   * If an employee changes jobs or departments, a record is added to **`JOB_HISTORY`** with `start_date` and `end_date`
## 4. Visual Schema Diagram
    REGIONS ||--o{ COUNTRIES : "contains"
    COUNTRIES ||--o{ LOCATIONS : "located in"
    LOCATIONS ||--o{ DEPARTMENTS : "hosts"
    DEPARTMENTS ||--o{ EMPLOYEES : "employs"
    JOBS ||--o{ EMPLOYEES : "defines role"
    EMPLOYEES ||--o{ JOB_HISTORY : "tracks past roles"
    DEPARTMENTS ||--o{ JOB_HISTORY : "historical dept"
    JOBS ||--o{ JOB_HISTORY : "historical role"