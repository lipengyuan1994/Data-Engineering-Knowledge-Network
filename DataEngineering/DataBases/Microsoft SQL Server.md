
[Oracle vs SQL Server – Difference Between Them (guru99.com)](https://www.guru99.com/oracle-vs-sql-server.html)
[Top 50 SQL Server Interview Questions and Answers (2022) (guru99.com)](https://www.guru99.com/sql-server-questions.html)

### What is recursive stored procedure?

SQL Server supports recursive [[stored procedure]] which calls by itself. The recursive stored procedure can be defined as a method of problem-solving wherein the solution arrives repetitively. It can nest up to 32 levels.
```SQL
CREATE PROCEDURE [dbo].[Fact]
(
@Number Integer,
@RetVal Integer OUTPUT
)
AS
DECLARE @In Integer
DECLARE @Out Integer
IF @Number != 1
BEGIN
SELECT @In = @Number – 1
EXEC Fact @In, @Out OUTPUT - Same stored procedure has been called again(Recursively)
SELECT @RetVal = @Number * @Out
END
ELSE
BEGIN
SELECT @RetVal = 1
END
RETURN
GO
```

---

### What are the differences between local and global temporary tables?

-   Local temporary tables are visible when there is a connection, and are deleted when the connection is closed.

`CREATE TABLE #<tablename>`

-   Global temporary tables are visible to all users, and are deleted when the connection that created it is closed.

`CREATE TABLE ##<tablename>`

---
###  What is CHECK constraint?

A CHECK constraint can be applied to a column in a table to limit the values that can be placed in a column. Check constraint is to enforce integrity.

---
### What is sub query and its properties?

A sub-query is a query which can be nested inside a main query like Select, Update, Insert or Delete statements. This can be used when expression is allowed. Properties of sub query can be defined as

-   A sub query should not have order by clause
-   A sub query should be placed in the right hand side of the comparison operator of the main query
-   A sub query should be enclosed in parenthesis because it needs to be executed first before the main query
-   More than one sub query can be included
---
### What are the types of sub query?

There are three types of sub query –

-   Single row sub query which returns only one row
-   Multiple row sub query which returns multiple rows
-   Multiple column sub query which returns multiple columns to the main query. With that sub query result, Main query will be executed.
---
### What is COALESCE in SQL Server?

COALESCE is used to return first non-null expression within the arguments. This function is used to return a non-null from more than one column in the arguments.

Example –

`Select COALESCE(empno, empname, salary) from employee;`

---
### How exceptions can be handled in SQL Server Programming?

Exceptions are handled using TRY—-CATCH constructs and it is handles by writing scripts inside the TRY block and error handling in the CATCH block.

---
### What is a Trigger?

Triggers are used to execute a batch of SQL code when insert or update or delete commands are executed against a table. Triggers are automatically triggered or executed when the data is modified. It can be executed automatically on insert, delete and update operations.

There are four types of triggers and they are:

-   Insert
-   Delete
-   Update
-   Instead of
---

### What is Bulkcopy in SQL?

Bulkcopy is a tool used to copy large amount of data from Tables. This tool is used to load large amount of data in SQL Server.

---

### What is the difference between UNION and UNION ALL?

-   UNION: To select related information from two tables UNION command is used. It is similar to JOIN command.
-   UNION All: The UNION ALL command is equal to the UNION command, except that UNION ALL selects all values. It will not remove duplicate rows, instead it will retrieve all rows from all tables.
---

### When do you, use join, when do you use sub query?


---

### What are the differences between Stored Procedure and the dynamic SQL?

Stored Procedure is a set of statements which is stored in a compiled form. Dynamic SQL is a set of statements that dynamically constructed at runtime and it will not be stored in a Database and it simply execute during run time.

---

### Which SQL server table is used to hold the stored procedure scripts?

Sys.SQL_Modules is a SQL Server table used to store the script of stored procedure. Name of the stored procedure is saved in the table called Sys.Procedures.

---

### What is the difference between GETDATE and SYSDATETIME?

Both are same but GETDATE can give time till milliseconds and SYSDATETIME can give precision till nanoseconds. SYSDATE TIME is more accurate than GETDATE.

---

