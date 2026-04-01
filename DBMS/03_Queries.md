# SQL Queries - Database Documentation

## Overview
SQL Queries are statements used to retrieve, manipulate, and manage data in a relational database. Queries form the foundation of database interaction and can range from simple SELECT statements to complex operations involving multiple tables, aggregations, and subqueries.

## Types of SQL Statements

### 1. **Data Query Language (DQL)**
- SELECT statements for retrieving data

### 2. **Data Manipulation Language (DML)**
- INSERT, UPDATE, DELETE statements

### 3. **Data Definition Language (DDL)**
- CREATE, ALTER, DROP statements

### 4. **Data Control Language (DCL)**
- GRANT, REVOKE statements

### 5. **Transaction Control Language (TCL)**
- COMMIT, ROLLBACK, SAVEPOINT statements

## SELECT Statement

### Basic Syntax
```sql
SELECT [DISTINCT] column1, column2, ...
FROM table_name
[WHERE condition]
[GROUP BY column1, column2, ...]
[HAVING condition]
[ORDER BY column1 [ASC|DESC], ...]
[OFFSET n ROWS]
[FETCH NEXT m ROWS ONLY]
```

### Simple SELECT
```sql
SELECT * FROM Employees;
```

### Selecting Specific Columns
```sql
SELECT 
    EmployeeID,
    FirstName,
    LastName,
    Email
FROM Employees;
```

### Using Aliases
```sql
SELECT 
    EmployeeID AS ID,
    FirstName AS 'First Name',
    LastName AS 'Last Name',
    Salary * 12 AS AnnualSalary
FROM Employees;
```

### DISTINCT
```sql
SELECT DISTINCT DepartmentID
FROM Employees;
```

## WHERE Clause

### Comparison Operators
```sql
SELECT * FROM Employees
WHERE Salary > 50000;

SELECT * FROM Employees
WHERE HireDate >= '2020-01-01';

SELECT * FROM Employees
WHERE DepartmentID = 5;
```

### Logical Operators
```sql
-- AND
SELECT * FROM Employees
WHERE Salary > 50000 AND DepartmentID = 5;

-- OR
SELECT * FROM Employees
WHERE DepartmentID = 5 OR DepartmentID = 10;

-- NOT
SELECT * FROM Employees
WHERE NOT DepartmentID = 5;
```

### IN Operator
```sql
SELECT * FROM Employees
WHERE DepartmentID IN (5, 10, 15);
```

### LIKE Operator
```sql
-- Starts with
SELECT * FROM Employees
WHERE FirstName LIKE 'John%';

-- Contains
SELECT * FROM Employees
WHERE Email LIKE '%@company.com';

-- Single character wildcard
SELECT * FROM Employees
WHERE FirstName LIKE 'J_n';
```

### BETWEEN Operator
```sql
SELECT * FROM Employees
WHERE Salary BETWEEN 40000 AND 60000;

SELECT * FROM Employees
WHERE HireDate BETWEEN '2020-01-01' AND '2023-12-31';
```

### NULL Handling
```sql
-- IS NULL
SELECT * FROM Employees
WHERE ManagerID IS NULL;

-- IS NOT NULL
SELECT * FROM Employees
WHERE Email IS NOT NULL;

-- COALESCE
SELECT 
    FirstName,
    COALESCE(MiddleName, '') AS MiddleName,
    LastName
FROM Employees;
```

## Joins

### INNER JOIN
Returns matching records from both tables.

```sql
SELECT 
    e.EmployeeID,
    e.FirstName,
    e.LastName,
    d.DepartmentName
FROM Employees e
INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

### LEFT JOIN (LEFT OUTER JOIN)
Returns all records from left table and matching records from right table.

```sql
SELECT 
    e.EmployeeID,
    e.FirstName,
    d.DepartmentName
FROM Employees e
LEFT JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

### RIGHT JOIN (RIGHT OUTER JOIN)
Returns all records from right table and matching records from left table.

```sql
SELECT 
    e.EmployeeID,
    e.FirstName,
    d.DepartmentName
FROM Employees e
RIGHT JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

### FULL OUTER JOIN
Returns all records when there is a match in either table.

```sql
SELECT 
    e.EmployeeID,
    e.FirstName,
    d.DepartmentName
FROM Employees e
FULL OUTER JOIN Departments d ON e.DepartmentID = d.DepartmentID;
```

### CROSS JOIN
Returns Cartesian product of both tables.

```sql
SELECT 
    e.FirstName,
    p.ProjectName
FROM Employees e
CROSS JOIN Projects p;
```

### Self Join
Joining a table to itself.

```sql
SELECT 
    e1.EmployeeID,
    e1.FirstName AS Employee,
    e2.FirstName AS Manager
FROM Employees e1
LEFT JOIN Employees e2 ON e1.ManagerID = e2.EmployeeID;
```

## Aggregation Functions

### Common Aggregate Functions
```sql
SELECT 
    COUNT(*) AS TotalEmployees,
    COUNT(DISTINCT DepartmentID) AS UniqueDepartments,
    AVG(Salary) AS AverageSalary,
    MIN(Salary) AS MinSalary,
    MAX(Salary) AS MaxSalary,
    SUM(Salary) AS TotalSalary
FROM Employees;
```

### GROUP BY
```sql
SELECT 
    DepartmentID,
    COUNT(*) AS EmployeeCount,
    AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY DepartmentID;
```

### HAVING Clause
Filters groups after aggregation.

```sql
SELECT 
    DepartmentID,
    COUNT(*) AS EmployeeCount,
    AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY DepartmentID
HAVING COUNT(*) > 5;
```

## Subqueries

### Scalar Subquery
Returns a single value.

```sql
SELECT 
    EmployeeID,
    FirstName,
    Salary,
    (SELECT AVG(Salary) FROM Employees) AS AvgSalary
FROM Employees;
```

### Correlated Subquery
References outer query.

```sql
SELECT 
    e1.EmployeeID,
    e1.FirstName,
    e1.Salary
FROM Employees e1
WHERE e1.Salary > (
    SELECT AVG(e2.Salary)
    FROM Employees e2
    WHERE e2.DepartmentID = e1.DepartmentID
);
```

### EXISTS
Checks for existence of rows.

```sql
SELECT *
FROM Departments d
WHERE EXISTS (
    SELECT 1
    FROM Employees e
    WHERE e.DepartmentID = d.DepartmentID
);
```

### IN with Subquery
```sql
SELECT *
FROM Employees
WHERE DepartmentID IN (
    SELECT DepartmentID
    FROM Departments
    WHERE Location = 'New York'
);
```

## Common Table Expressions (CTEs)

### Simple CTE
```sql
WITH HighEarners AS (
    SELECT *
    FROM Employees
    WHERE Salary > 80000
)
SELECT * FROM HighEarners;
```

### Recursive CTE
```sql
WITH EmployeeHierarchy AS (
    -- Anchor member
    SELECT 
        EmployeeID,
        FirstName,
        ManagerID,
        0 AS Level
    FROM Employees
    WHERE ManagerID IS NULL
    
    UNION ALL
    
    -- Recursive member
    SELECT 
        e.EmployeeID,
        e.FirstName,
        e.ManagerID,
        eh.Level + 1
    FROM Employees e
    INNER JOIN EmployeeHierarchy eh ON e.ManagerID = eh.EmployeeID
)
SELECT * FROM EmployeeHierarchy;
```

## Window Functions

### ROW_NUMBER()
```sql
SELECT 
    EmployeeID,
    FirstName,
    Salary,
    ROW_NUMBER() OVER (ORDER BY Salary DESC) AS SalaryRank
FROM Employees;
```

### RANK() and DENSE_RANK()
```sql
SELECT 
    EmployeeID,
    FirstName,
    Salary,
    RANK() OVER (ORDER BY Salary DESC) AS Rank,
    DENSE_RANK() OVER (ORDER BY Salary DESC) AS DenseRank
FROM Employees;
```

### PARTITION BY
```sql
SELECT 
    EmployeeID,
    DepartmentID,
    Salary,
    AVG(Salary) OVER (PARTITION BY DepartmentID) AS DeptAvgSalary
FROM Employees;
```

### LAG() and LEAD()
```sql
SELECT 
    EmployeeID,
    Salary,
    LAG(Salary, 1) OVER (ORDER BY EmployeeID) AS PreviousSalary,
    LEAD(Salary, 1) OVER (ORDER BY EmployeeID) AS NextSalary
FROM Employees;
```

## INSERT Statement

### Basic INSERT
```sql
INSERT INTO Employees (FirstName, LastName, Email, DepartmentID)
VALUES ('John', 'Doe', 'john.doe@company.com', 5);
```

### INSERT Multiple Rows
```sql
INSERT INTO Employees (FirstName, LastName, Email, DepartmentID)
VALUES 
    ('John', 'Doe', 'john.doe@company.com', 5),
    ('Jane', 'Smith', 'jane.smith@company.com', 5),
    ('Bob', 'Johnson', 'bob.johnson@company.com', 10);
```

### INSERT from SELECT
```sql
INSERT INTO EmployeesArchive (EmployeeID, FirstName, LastName)
SELECT EmployeeID, FirstName, LastName
FROM Employees
WHERE HireDate < '2020-01-01';
```

## UPDATE Statement

### Basic UPDATE
```sql
UPDATE Employees
SET Salary = 55000
WHERE EmployeeID = 123;
```

### UPDATE Multiple Columns
```sql
UPDATE Employees
SET 
    Salary = 55000,
    DepartmentID = 5,
    LastModified = GETDATE()
WHERE EmployeeID = 123;
```

### UPDATE with JOIN
```sql
UPDATE e
SET e.Salary = e.Salary * 1.1
FROM Employees e
INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID
WHERE d.DepartmentName = 'Sales';
```

## DELETE Statement

### Basic DELETE
```sql
DELETE FROM Employees
WHERE EmployeeID = 123;
```

### DELETE with JOIN
```sql
DELETE e
FROM Employees e
INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID
WHERE d.DepartmentName = 'Closed';
```

### TRUNCATE
```sql
TRUNCATE TABLE EmployeesArchive;
```

## UNION and UNION ALL

### UNION (Removes Duplicates)
```sql
SELECT FirstName FROM Employees
UNION
SELECT FirstName FROM Contractors;
```

### UNION ALL (Keeps Duplicates)
```sql
SELECT FirstName FROM Employees
UNION ALL
SELECT FirstName FROM Contractors;
```

## CASE Statement

### Simple CASE
```sql
SELECT 
    EmployeeID,
    FirstName,
    CASE DepartmentID
        WHEN 1 THEN 'IT'
        WHEN 2 THEN 'HR'
        WHEN 3 THEN 'Finance'
        ELSE 'Other'
    END AS DepartmentName
FROM Employees;
```

### Searched CASE
```sql
SELECT 
    EmployeeID,
    Salary,
    CASE
        WHEN Salary > 80000 THEN 'High'
        WHEN Salary > 50000 THEN 'Medium'
        ELSE 'Low'
    END AS SalaryCategory
FROM Employees;
```

## Pagination

### OFFSET and FETCH
```sql
SELECT *
FROM Employees
ORDER BY EmployeeID
OFFSET 10 ROWS
FETCH NEXT 10 ROWS ONLY;
```

## Best Practices

1. **Use Specific Columns**: Avoid SELECT * in production code
2. **Index Usage**: Design queries to leverage indexes
3. **Avoid Functions in WHERE**: Can prevent index usage
4. **Use Parameterized Queries**: Prevents SQL injection
5. **Limit Result Sets**: Use pagination for large datasets
6. **Optimize Joins**: Ensure proper join conditions and indexes
7. **Use EXISTS Instead of COUNT**: More efficient for existence checks
8. **Avoid Nested Subqueries**: Consider CTEs or joins when possible
9. **Use Appropriate Data Types**: Match column data types
10. **Test Performance**: Use execution plans to optimize queries

## Performance Optimization

### Execution Plan
```sql
SET STATISTICS IO ON;
SET STATISTICS TIME ON;

SELECT * FROM Employees WHERE DepartmentID = 5;
```

### Index Hints
```sql
SELECT * FROM Employees WITH (INDEX(IX_DepartmentID))
WHERE DepartmentID = 5;
```

### Query Hints
```sql
SELECT * FROM Employees
WHERE DepartmentID = 5
OPTION (MAXDOP 4);
```

## Error Handling

### TRY...CATCH
```sql
BEGIN TRY
    UPDATE Employees
    SET Salary = -1000
    WHERE EmployeeID = 123;
END TRY
BEGIN CATCH
    SELECT 
        ERROR_NUMBER() AS ErrorNumber,
        ERROR_MESSAGE() AS ErrorMessage,
        ERROR_SEVERITY() AS ErrorSeverity;
END CATCH
```

## Related Concepts
- See `01_Stored_Procedures.md` for Stored Procedures
- See `02_Functions.md` for User-Defined Functions
- See `04_Recurring_JV_Procedures.md` for specific Recurring JV procedures

