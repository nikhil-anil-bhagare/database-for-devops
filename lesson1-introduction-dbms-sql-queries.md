**Introduction to DBMS and SQL Keys**

**1. Database Overview:**
- A database stores related data efficiently. A DBMS (Database Management System) is used to manage it.
- **Relational Database (RDBMS)**: Represents data in tables with strict schemas (e.g., MySQL, Oracle).
- **Non-Relational Database**: Stores data as documents, key-value pairs, etc., without strict schemas (e.g., MongoDB, Redis).

**2. Schema:**
- A schema defines the structure or design of a database.

**3. RDBMS Properties:**
- Tables store specific information.
- Rows are unique, columns have atomic values.
- No guaranteed column sequence, rely on column names.

**4. Keys in Relational Databases:**

- **Super Key**: A set of columns used to uniquely identify a row.
  - Example:

  | Name   | PSP  | Batch | Phone Number | Email          |
  |--------|------|-------|--------------|----------------|
  | Alice  | 100  | A1    | 9876543210   | alice@mail.com |
  | Bob    | 200  | A2    | 9876543222   | bob@mail.com   |

  - **Super Key Example**:  
    - `Name + Phone Number`
    - `Name + Email`
    - These combinations can uniquely identify a row.

- **Candidate Key**: A minimal set of columns that uniquely identifies a row without any extra attributes.
  - Example: 

  | StudentID | ClassID | Name   | Age |
  |-----------|---------|--------|-----|
  | 1         | 101     | Alice  | 20  |
  | 2         | 102     | Bob    | 22  |

  - **Candidate Key Example**:  
    - `StudentID + ClassID`
    - Both are minimal and can uniquely identify a row.

- **Primary Key**: A candidate key chosen to uniquely identify rows in a table.
  - Properties: Unique, non-null, and immutable.
  - Example:

  | EmployeeID | Name   | Email          |
  |------------|--------|----------------|
  | E001       | Alice  | alice@mail.com |
  | E002       | Bob    | bob@mail.com   |

  - **Primary Key**: `EmployeeID` is used to uniquely identify rows.

- **Composite Key**: When no single column can uniquely identify a row, a combination of two or more columns is used.
  - Example: 

  | OrderID | ProductID | Quantity |
  |---------|-----------|----------|
  | 101     | 501       | 2        |
  | 101     | 502       | 1        |

  - **Composite Primary Key**: `OrderID + ProductID` together form a unique identifier.

- **Foreign Key**: A column in one table that references a primary key in another table to maintain referential integrity.
  - Example:

  | BatchID | BatchName |
  |---------|-----------|
  | 1       | Batch A   |
  | 2       | Batch B   |

  | StudentID | Name   | BatchID |
  |-----------|--------|---------|
  | 1         | Alice  | 1       |
  | 2         | Bob    | 2       |

  - **Foreign Key Example**: `BatchID` in the **Student** table references `BatchID` in the **Batch** table.
    - If BatchID 1 is deleted from the **Batch** table, corresponding rows in **Student** table can be set to `NULL`, cascaded, or restricted based on foreign key constraints.

**5. SQL Commands with Examples:**

- **Create Database**: 
  - Command: `CREATE DATABASE Users;`
  - Example: Creates a database named **Users**.

- **Use Database**: 
  - Command: `USE Users;`
  - Example: Switches to the **Users** database for performing operations.

- **Create Table**:
  - Command: 
  ```sql
  CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name VARCHAR(50),
    Email VARCHAR(50) UNIQUE
  );
  ```
  - Example: Creates a table named **Employees** with columns for `EmployeeID` (primary key), `Name`, and `Email` (unique constraint).

- **Insert Data**:
  - Command:
  ```sql
  INSERT INTO Employees (EmployeeID, Name, Email)
  VALUES (1, 'Alice', 'alice@mail.com'), (2, 'Bob', 'bob@mail.com');
  ```
  - Example: Inserts two rows of data into the **Employees** table.

- **Select Data**:
  - Command: `SELECT * FROM Employees;`
  - Example: Retrieves all rows from the **Employees** table.

  | EmployeeID | Name   | Email          |
  |------------|--------|----------------|
  | 1          | Alice  | alice@mail.com |
  | 2          | Bob    | bob@mail.com   |

- **Describe Table**:
  - Command: `DESCRIBE Employees;`
  - Example: Displays the structure of the **Employees** table.

  | Field      | Type         | Null | Key  | Default | Extra |
  |------------|--------------|------|------|---------|-------|
  | EmployeeID | INT          | NO   | PRI  | NULL    |       |
  | Name       | VARCHAR(50)  | YES  |      | NULL    |       |
  | Email      | VARCHAR(50)  | YES  | UNI  | NULL    |       |

- **Drop Table**:
  - Command: `DROP TABLE IF EXISTS Employees;`
  - Example: Deletes the **Employees** table if it exists.

- **Composite Key**:
  - Command:
  ```sql
  CREATE TABLE Orders (
    OrderID INT,
    ProductID INT,
    Quantity INT,
    PRIMARY KEY (OrderID, ProductID)
  );
  ```
  - Example: Creates an **Orders** table where the combination of `OrderID` and `ProductID` forms the composite primary key.

- **Foreign Key**:
  - Command:
  ```sql
  CREATE TABLE Students (
    StudentID INT,
    Name VARCHAR(50),
    BatchID INT,
    FOREIGN KEY (BatchID) REFERENCES Batches(BatchID) ON DELETE CASCADE ON UPDATE CASCADE
  );
  ```
  - Example: Creates a **Students** table where `BatchID` is a foreign key referencing the **BatchID** in the **Batches** table, with cascading updates and deletions.

### Problem Statement to solve:
**Context**: You are working as a Database Engineer for a **library management system**. You need to design and implement a database to manage **Books**, **Members**, and their **Borrowing records**.
Design and implement the following database tables using **SQL** and apply the knowledge of **keys** (primary key, composite key, and foreign key).

### Requirements:

1. **Create a Database**:
   - Create a database called `LibraryDB`.

2. **Tables and Keys**:
   - **Books Table**:  
     Stores information about books in the library.
     - Columns: `BookID` (Primary Key), `Title`, `Author`, `ISBN` (unique).
     - Requirements: Each book has a unique `BookID` and `ISBN` (International Standard Book Number).

   - **Members Table**:  
     Stores details of library members.
     - Columns: `MemberID` (Primary Key), `FirstName`, `LastName`, `Email` (unique).
     - Requirements: Each member has a unique `MemberID` and `Email`.

   - **Borrowing Table**:  
     Tracks which members borrow which books.
     - Columns: `BorrowID` (Primary Key), `MemberID` (Foreign Key referencing Members), `BookID` (Foreign Key referencing Books), `BorrowDate`, `ReturnDate`.
     - Requirements: A member cannot borrow the same book multiple times without returning it first. Use a **Composite Key** (`MemberID`, `BookID`).

### SQL Queries:
- Add you Queries here

