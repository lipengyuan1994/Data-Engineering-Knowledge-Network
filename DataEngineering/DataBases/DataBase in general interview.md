
[SQL Interview Questions CHEAT SHEET (2022) - InterviewBit](https://www.interviewbit.com/sql-interview-questions/#sql)
[Top 50 Database Interview Questions and Answers (2022) (guru99.com)](https://www.guru99.com/database-interview-questions.html)



### What is Data Integrity?

Data Integrity is the assurance of accuracy and consistency of data over its entire life-cycle and is a critical aspect of the design, implementation, and usage of any system which stores, processes, or retrieves data. It also defines integrity constraints to enforce business rules on the data when it is entered into an application or a database.

---


### Enlist the various relationships of database.

The various relationships of database are:

-   **One-to-one:** Single table having drawn relationship with another table having similar kind of columns.
-   **One-to-many:** Two tables having primary and foreign key relation.
-   **Many-to-many:** Junction table having many tables related to many tables.
---

### Define Normalization.

Organized data void of inconsistent dependency and redundancy within a database is called [normalization](https://www.guru99.com/database-normalization.html).

---

### Define Denormalization.

Boosting up database performance, adding of redundant data which in turn helps rid of complex data is called denormalization.

---

### Enlist the advantages of normalizing database.

Advantages of normalizing database are:

-   No duplicate entries
-   Saves storage space
-   Boasts the query performances.
---

### Define DDL and DML.

Managing properties and attributes of database is called Data Definition Language(DDL).

Manipulating data in a database such as inserting, updating, deleting is defined as Data Manipulation Language. (DML)

---

### Enlist some commands of DDL.

They are:

**CREATE:**

Create is used in the CREATE TABLE statement. Syntax is:

`CREATE TABLE [column name] ( [column definitions] ) [ table parameters]`
`
**ALTER:**

It helps in modification of an existing object of database. Its syntax is:

`ALTER objecttype objectname parameters.`

**DROP:**

It destroys an existing database, index, table or view. Its syntax is:

`DROP objecttype objectname.`

---

### Define Join and enlist its types.

Joins help in explaining the relation between different tables. They also enable you to select data with relation to data in another table.

The various types are:

-   **INNER JOINs:** Blank rows are left in the middle while more than equal to two tables are joined.
-   **OUTER JOINs:** Divided into Left Outer Join and Right Outer Join. Blank rows are left at the specified side by joining tables in other side.
- left-anti, left-semi   WIKI 
---

### What is Executive Plan?

Executive plan can be defined as:

-   SQL Server caches collected procedure or the plan of query execution and used thereafter by subsequent calls.
-   An important feature in relation to performance enhancement.
-   Data execution plan can be viewed textually or graphically.

---

### What is Database partitioning?

Division of logical database into independent complete units for improving its management, availability and performance is called Database partitioning.

### Explain the importance of partitioning.

Splitting of one table which is large into smaller database entities logically is called database partitioning. Its benefits are:

-   To improve query performance in situations dramatically when mostly rows which are heavily accessed are in one partition.
-   Accessing large parts of a single partition
-   Slower and cheaper storage media can be used for data which is seldom used.

---

### What is a View?

A view in SQL is a virtual table based on the result-set of an SQL statement. A view contains rows and columns, just like a real table. The fields in a view are fields from one or more real tables in the database.

---


