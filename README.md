# Library Management System (`LibraryDB`)

**Course:** Programming & Professional Foundations (PPF Week 9 – Mini Project)  
**Assessment Type:** Individual Mini Project (20 Marks)  
**Database Engine Compatibility:** MySQL 8.0+ | PostgreSQL | SQLite 3 | Online SQL Compilers (OneCompiler, Programiz, SQLiteOnline, DB-Fiddle)

---

## Project Overview

The **Library Management System (`LibraryDB`)** is a relational database mini-project designed to manage student records, book inventory, book lending transactions, return workflows, and overdue fine tracking. 

This repository demonstrates core Database Management System (DBMS) concepts required by the PPF Week 9 specification:
* **Relational Schema Design (3NF):** Elimination of data redundancy and partial/transitive dependencies across three normalized tables (`Students`, `Books`, and `Book_Issue`).
* **Referential & Domain Integrity:** Enforcement of `PRIMARY KEY`, `FOREIGN KEY` (`ON DELETE CASCADE`), `UNIQUE`, `NOT NULL`, `DEFAULT`, and `CHECK` constraints.
* **SQL Query Operations & Joins:** Single-table filtering, multi-table `INNER JOIN` and `LEFT JOIN` queries, `GROUP BY` departmental aggregations (`COUNT`), and `UPDATE` data manipulation statements.
* **Web Application Integration:** Interactive browser-based SQL workbench (`library_sql_playground.html`) and 3-tier client-server architecture explanation.
* **Distributed Database Architecture:** Multi-campus horizontal fragmentation, catalog replication, and Two-Phase Commit (2PC) synchronization design.

---

## Repository Structure

| File | Description |
| :--- | :--- |
| `library_management_system.sql` | Complete, universal SQL script containing table creation (DDL), sample data insertion (DML), and all required/bonus queries. |
| `library_sql_playground.html` | Self-contained interactive SQL workbench and library portal that runs `LibraryDB` live in the browser via WebAssembly (`sql.js`). |
| `Library_Management_System_Project_Report.docx` | Complete academic project report with ER diagrams, query outputs, web/distributed architecture explanations, and viva preparation. |
| `README.md` | Project documentation, schema summary, and execution instructions. |

---

## Database Schema

### 1. `Students` Table
Stores registered student patron details and academic department affiliations.

| Column | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `student_id` | `INT` | `PRIMARY KEY` | Unique institutional student ID |
| `first_name` | `VARCHAR(50)` | `NOT NULL` | Student's first name |
| `last_name` | `VARCHAR(50)` | `NOT NULL` | Student's last name |
| `email` | `VARCHAR(100)` | `UNIQUE, NOT NULL` | Official university email address |
| `phone` | `VARCHAR(15)` | `NULL` | Contact phone number |
| `department` | `VARCHAR(50)` | `NOT NULL` | Academic department |
| `enrollment_year` | `INT` | `NOT NULL, CHECK (>= 2020)` | Year of enrollment |

### 2. `Books` Table
Stores catalog metadata and real-time physical copy counts.

| Column | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `book_id` | `INT` | `PRIMARY KEY` | Unique catalog book ID |
| `title` | `VARCHAR(150)` | `NOT NULL` | Full book title |
| `author` | `VARCHAR(100)` | `NOT NULL` | Primary author name |
| `isbn` | `VARCHAR(20)` | `UNIQUE, NOT NULL` | International Standard Book Number |
| `category` | `VARCHAR(50)` | `NOT NULL` | Subject classification |
| `total_copies` | `INT` | `NOT NULL, CHECK (>= 0)` | Total copies owned by the library |
| `available_copies` | `INT` | `NOT NULL, CHECK (>= 0)` | Copies currently available on shelf |

### 3. `Book_Issue` Table
Associative entity resolving the Many-to-Many ($M:N$) relationship between `Students` and `Books`.

| Column | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `issue_id` | `INT` | `PRIMARY KEY` | Unique circulation transaction ID |
| `student_id` | `INT` | `FOREIGN KEY -> Students(student_id)` | Borrowing student ID (`ON DELETE CASCADE`) |
| `book_id` | `INT` | `FOREIGN KEY -> Books(book_id)` | Issued book ID (`ON DELETE CASCADE`) |
| `issue_date` | `DATE` | `NOT NULL` | Date the book was issued |
| `due_date` | `DATE` | `NOT NULL` | Scheduled return deadline |
| `return_date` | `DATE` | `NULL` | Actual return date (`NULL` if still issued) |
| `status` | `VARCHAR(20)` | `CHECK IN ('Issued', 'Returned', 'Overdue')` | Current circulation state |
| `fine_amount` | `DECIMAL(6,2)` | `DEFAULT 0.00` | Overdue fine amount |

---

## How to Run the Project

### Option 1: Run Online (OneCompiler / Programiz / SQLiteOnline / DB-Fiddle)
1. Open `library_management_system.sql` and copy the entire script.
2. Paste it into any online SQL editor (e.g., OneCompiler MySQL/SQLite, Programiz SQL, or SQLiteOnline).
3. Click **Run** to create the tables, populate the 6 students, 10 books, and 7 issue records, and view all query outputs immediately.

### Option 2: Run Interactive Web Workbench (`library_sql_playground.html`)
1. Download and open `library_sql_playground.html` in any web browser (Chrome, Edge, Firefox, Safari).
2. Use the **Live SQL Workbench** tab to execute preset queries or custom SQL statements, or use the **Interactive Library Portal** tab to issue and return books in real time.

### Option 3: Run Locally on MySQL / MariaDB CLI
1. Uncomment `CREATE DATABASE IF NOT EXISTS LibraryDB;` and `USE LibraryDB;` at the top of `library_management_system.sql`.
2. Execute the script via terminal:
   ```bash
   mysql -u root -p < library_management_system.sql
   ```

---

## Summary of Tasks Implemented

1. **Task 1 – Database Creation:** Initializes `LibraryDB`.
2. **Task 2 – Table Creation:** Creates `Students`, `Books`, and `Book_Issue` with primary keys, foreign keys, and domain constraints.
3. **Task 3 – Sample Data Population:** Inserts **6 student records** (exceeding the minimum 5 requirement), **10 book records**, and **7 circulation records** covering `Issued`, `Returned`, and `Overdue` states.
4. **Task 4 – Available Books & Student Queries:**
   * Displays available books where `available_copies > 0`.
   * Displays complete student profile records.
5. **Task 5 – Books Issued to a Specific Student:** Uses a 3-table `INNER JOIN` (`Book_Issue`, `Students`, `Books`) to display all books issued to Student `101` (`Aarav Sharma`).
6. **Task 6 – Website Connectivity Architecture:** Documented in the report and demonstrated live via `library_sql_playground.html` using a 3-tier architecture (Frontend UI $\rightarrow$ REST API with Connection Pool & Prepared Statements $\rightarrow$ `LibraryDB`).
7. **Task 7 – Distributed Multi-Campus Database:** Explains horizontal fragmentation by campus node, catalog replication, and Two-Phase Commit (2PC) for cross-campus lending consistency.

---

## Assessment Rubric Checklist (20 Marks)

| S.No. | Evaluation Criteria | Marks | Status |
| :---: | :--- | :---: | :--- |
| 1 | Database Design and Table Creation | 4 | Completed |
| 2 | SQL Queries and Data Manipulation | 4 | Completed |
| 3 | Implementation and Working Output | 4 | Completed |
| 4 | Project Report and Screenshots | 2 | Completed |
| 5 | GitHub Repository and Code Submission | 1 | Completed |
| 6 | Viva / Project Presentation | 5 | Completed |
| **Total** | | **20** | **Complete** |
