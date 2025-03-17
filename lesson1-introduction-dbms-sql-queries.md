### Introduction to DBMS and SQL Keys

---

#### How do you store information frequently:
- Notes
- Workpad
- Excel Sheet
- Google Sheet

#### What is a Database?
- **Database**: A collection of related data. 
- **DBMS (Database Management System)**: A system used to manage a database efficiently.

#### Types of Database Systems:
1. **Relational Databases** (e.g., MySQL, Oracle, PostgreSQL)
   - Represent data in multiple related tables.
   - Strict schema enforcement.
   - Data is structured in rows and columns, with tables related using keys.

2. **Non-Relational Databases** (e.g., MongoDB, Redis, Cassandra)
   - Store data as documents, key-value pairs, or graphs.
   - Do not follow strict schema rules, allowing semi-structured or unstructured data.

#### Schema:
- **Schema**: Structure/Design of a database that defines the organization of data.

---

### Relational Database Properties:
1. **Tables**: Each table stores specific information (e.g., Student table stores student details).
2. **Unique Rows**: Every row in a table is unique.
3. **Same Data Type**: All values in a column share the same data type.
4. **Atomic Values**: Each cell contains a single, indivisible value.
5. **Column Sequence Irrelevant**: Column sequence does not affect the data; rely on column names.

---

### SQL Keys in Relational Databases:

#### Super Key:
- A combination of columns that can uniquely identify a row.
- Example: `Name + Phone`, `Name + Email`.
- Super keys may include additional attributes beyond what is necessary to uniquely identify rows.

**Example:**

| Name      | PSP | Batch | Phone Number | Email           |
|-----------|-----|-------|--------------|-----------------|
| Alice     | 75  | B1    | 1234567890   | alice@mail.com  |
| Bob       | 82  | B2    | 2345678901   | bob@mail.com    |
| Charlie   | 78  | B3    | 3456789012   | charlie@mail.com|

- Super keys:
  - `Name + Phone Number`
  - `Email + PSP`
  - `Phone Number + Email`

#### Candidate Key:
- A minimal set of columns used to uniquely identify a row.
- Characteristics:
  - No unnecessary attributes.
  - A table can have multiple candidate keys.
  - One of the candidate keys is selected as the primary key.

**Example:**

| StudentID | Name   | ClassID | Email           |
|-----------|--------|---------|-----------------|
| 101       | Alice  | C1      | alice@mail.com  |
| 102       | Bob    | C2      | bob@mail.com    |
| 103       | Charlie| C3      | charlie@mail.com|

- Candidate keys:
  - `StudentID`
  - `Email`

#### Primary Key:
- A candidate key selected to uniquely identify each row.
- Properties:
  - **Uniqueness**: Must be unique.
  - **Non-nullability**: Cannot be null.
  - **Immutability**: Should not change (to avoid referential integrity issues).
- Can be a **composite primary key** if multiple columns are needed to uniquely identify rows.

#### Foreign Key:
- A column in one table that references a primary key in another table, ensuring referential integrity.

**Example:**

| StudentID | Name   | BatchID |
|-----------|--------|---------|
| 101       | Alice  | B1      |
| 102       | Bob    | B2      |

| BatchID | Batch Name |
|---------|------------|
| B1      | Batch A    |
| B2      | Batch B    |

Here, `BatchID` in the **Students** table is a foreign key that references `BatchID` in the **Batch** table.

---

### Important Concepts:
1. **Composite Key**: A key made of multiple columns to uniquely identify a row.
2. **Referential Integrity**: Ensures relationships between tables remain consistent when data is updated or deleted.

---

### MySQL Installation and Setup:
- Install MySQL: `sudo apt install mysql-server`
- Start MySQL: `sudo systemctl start mysql`
- Check MySQL Status: `sudo systemctl status mysql`

### Basic SQL Commands:
- Create a database: `CREATE DATABASE Users`
- Use a database: `USE Users`
- Create a table: (example with `id` as a primary key)
- Describe table structure: `DESCRIBE Users`
- Select all data: `SELECT * FROM Users`
- Drop a table: `DROP TABLE IF EXISTS Students` (Caution when using `DROP` or `DELETE`)

---

### Constraints on Foreign Keys

Foreign keys ensure referential integrity by establishing a link between two tables, typically between the primary key of one table and a column in another table.

#### Constraints on Foreign Keys:
- **CASCADE**: If a referenced row (in the parent table) is deleted or updated, all rows in the child table that reference this foreign key are also deleted or updated.
- **SET NULL**: If the referenced row is deleted, all rows in the child table that reference this foreign key will have the foreign key column set to NULL.
- **NO ACTION**: The default setting. No action is taken when the referenced row is deleted or updated. However, if the referential integrity is violated, the operation will fail.
- **SET DEFAULT**: If the referenced row is deleted or updated, the foreign key column in the child table will be set to its default value.

#### Example of Foreign Key with Constraints:

**Students Table:**

| StudentID | Name   | BatchID |
|-----------|--------|---------|
| 101       | Alice  | B1      |
| 102       | Bob    | B2      |

**Batch Table:**

| BatchID | Batch Name |
|---------|------------|
| B1      | Batch A    |
| B2      | Batch B    |

Here, `BatchID` in the **Students** table is a foreign key that references `BatchID` in the **Batch** table.

**SQL Example**:

```sql
CREATE TABLE Batch (
    BatchID INT PRIMARY KEY,
    BatchName VARCHAR(50)
);

CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50),
    BatchID INT,
    FOREIGN KEY (BatchID) REFERENCES Batch(BatchID) 
    ON DELETE CASCADE 
    ON UPDATE CASCADE
);
```

In this example:
- **ON DELETE CASCADE**: If a row in the **Batch** table is deleted, any corresponding rows in the **Students** table that reference the deleted `BatchID` will also be deleted.
- **ON UPDATE CASCADE**: If the `BatchID` in the **Batch** table is updated, all corresponding `BatchID` values in the **Students** table will be updated automatically. 

This ensures referential integrity is maintained when rows in the parent table (Batch) are deleted or updated.


### Problem statement to solve

You are tasked with designing a database for a **University Course Management System** that stores information about students, courses, and the batches they are assigned to. Your goal is to explore and implement different types of **SQL keys** (super keys, candidate keys, primary keys, composite keys, and foreign keys) and practice using **foreign key constraints**.

#### Requirements:

1. **Entities**:
   - **Students**:
     - Each student has a unique `StudentID`, `Name`, `Email`, and `PhoneNumber`.
     - Students are assigned to a batch and enroll in various courses.
   
   - **Batches**:
     - Each batch is identified by a unique `BatchID`.
     - Each batch has a `BatchName`.

   - **Courses**:
     - Each course has a unique `CourseID` and `CourseName`.
     - Each student can enroll in multiple courses, and each course can have multiple students (many-to-many relationship).
   
2. **Relationships**:
   - Each student is assigned to a batch (one-to-many relationship).
   - A student can enroll in multiple courses (many-to-many relationship), so you need to create a **junction table** called `Enrollments` to store the relationship between `Students` and `Courses`.

#### Tasks:
1. **Create Tables**:
   - Design tables for `Students`, `Batches`, `Courses`, and `Enrollments`.
   
2. **Explore Different Keys**:
   - Identify **super keys**, **candidate keys**, and **primary keys** in each table.
   - Identify a scenario where a **composite key** (a key made up of multiple columns) is required.
   - Example: A composite key in the `Enrollments` table might consist of `StudentID` and `CourseID` to uniquely identify each enrollment.

3. **Implement Foreign Key Constraints**:
   - Link the `BatchID` in the **Students** table to the `BatchID` in the **Batches** table using a **foreign key**.
   - Link the `StudentID` and `CourseID` in the **Enrollments** table to their corresponding primary keys in the **Students** and **Courses** tables, respectively.
   
4. **Explore Foreign Key Constraints**:
   - Implement and experiment with different **foreign key constraints** (`CASCADE`, `SET NULL`, `NO ACTION`, `SET DEFAULT`) in various scenarios.
   - Example:
     - **ON DELETE CASCADE**: If a student record is deleted, automatically delete all corresponding records in the `Enrollments` table.
     - **ON UPDATE CASCADE**: If the `BatchID` of a student is updated, the change should reflect across all related records.

#### Example Tables:

1. **Students** Table:

| StudentID | Name   | Email          | PhoneNumber  | BatchID |
|-----------|--------|----------------|--------------|---------|
| 101       | Alice  | alice@mail.com | 1234567890   | B1      |
| 102       | Bob    | bob@mail.com   | 2345678901   | B2      |

2. **Batches** Table:

| BatchID | BatchName |
|---------|-----------|
| B1      | Batch A   |
| B2      | Batch B   |

3. **Courses** Table:

| CourseID | CourseName |
|----------|------------|
| C101     | Math       |
| C102     | Physics    |

4. **Enrollments** Table:

| EnrollmentID | StudentID | CourseID |
|--------------|-----------|----------|
| 1            | 101       | C101     |
| 2            | 102       | C102     |

#### Questions to Answer:
1. What are the possible **super keys** and **candidate keys** for each table?
2. Can you create a **composite key** in the `Enrollments` table using `StudentID` and `CourseID`?
3. What happens when you delete a batch with **ON DELETE CASCADE** in the **Batches** table?
4. How does **ON UPDATE CASCADE** affect records when you update `BatchID` in the **Students** table?



