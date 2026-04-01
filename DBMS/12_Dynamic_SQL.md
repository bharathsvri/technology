# Dynamic SQL - Database Documentation

## Overview
Dynamic SQL allows you to construct and execute SQL statements dynamically at runtime. This provides flexibility when table names, column names, or query conditions are not known until execution time. However, dynamic SQL must be used carefully due to security and performance considerations.

## Key Characteristics

### 1. **Runtime Construction**
- SQL statements built as strings
- Executed at runtime
- Provides flexibility

### 2. **Security Concerns**
- Vulnerable to SQL injection
- Must use parameterization
- Requires careful validation

### 3. **Performance**
- Cannot be pre-compiled
- Execution plans not cached effectively
- May have performance overhead

## Basic Dynamic SQL

### EXEC Statement
```sql
DECLARE @SQL NVARCHAR(MAX);
SET @SQL = N'SELECT * FROM Employees WHERE DepartmentID = 5';
EXEC (@SQL);
```

### EXEC with Variables
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @DepartmentID INT = 5;

SET @SQL = N'SELECT * FROM Employees WHERE DepartmentID = ' + CAST(@DepartmentID AS NVARCHAR);
EXEC (@SQL);
```

### sp_executesql (Recommended)
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @DepartmentID INT = 5;

SET @SQL = N'SELECT * FROM Employees WHERE DepartmentID = @DeptID';
EXEC sp_executesql @SQL, N'@DeptID INT', @DeptID = @DepartmentID;
```

## Parameterized Dynamic SQL

### Using sp_executesql with Parameters
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @DepartmentID INT = 5;
DECLARE @MinSalary DECIMAL(18,2) = 50000;

SET @SQL = N'
    SELECT EmployeeID, FirstName, LastName, Salary
    FROM Employees
    WHERE DepartmentID = @DeptID
        AND Salary >= @MinSal
    ORDER BY Salary DESC';

EXEC sp_executesql 
    @SQL,
    N'@DeptID INT, @MinSal DECIMAL(18,2)',
    @DeptID = @DepartmentID,
    @MinSal = @MinSalary;
```

### Multiple Parameters
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @TableName NVARCHAR(128) = 'Employees';
DECLARE @ColumnName NVARCHAR(128) = 'FirstName';
DECLARE @SearchValue NVARCHAR(100) = 'John';

SET @SQL = N'
    SELECT *
    FROM ' + QUOTENAME(@TableName) + N'
    WHERE ' + QUOTENAME(@ColumnName) + N' = @SearchVal';

EXEC sp_executesql 
    @SQL,
    N'@SearchVal NVARCHAR(100)',
    @SearchVal = @SearchValue;
```

## Dynamic Table and Column Names

### Dynamic Table Name
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @TableName NVARCHAR(128) = 'Employees';

SET @SQL = N'SELECT * FROM ' + QUOTENAME(@TableName);
EXEC sp_executesql @SQL;
```

### Dynamic Column Names
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @TableName NVARCHAR(128) = 'Employees';
DECLARE @ColumnName NVARCHAR(128) = 'FirstName';

SET @SQL = N'SELECT ' + QUOTENAME(@ColumnName) + N' FROM ' + QUOTENAME(@TableName);
EXEC sp_executesql @SQL;
```

### Multiple Dynamic Columns
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @Columns NVARCHAR(MAX) = 'EmployeeID, FirstName, LastName, Email';

SET @SQL = N'SELECT ' + @Columns + N' FROM Employees';
EXEC sp_executesql @SQL;
```

## Security: SQL Injection Prevention

### ❌ Vulnerable to SQL Injection
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @SearchValue NVARCHAR(100) = 'John''; DROP TABLE Employees; --';

SET @SQL = N'SELECT * FROM Employees WHERE FirstName = ''' + @SearchValue + '''';
EXEC (@SQL);  -- DANGEROUS!
```

### ✅ Safe with Parameterization
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @SearchValue NVARCHAR(100) = 'John''; DROP TABLE Employees; --';

SET @SQL = N'SELECT * FROM Employees WHERE FirstName = @SearchVal';
EXEC sp_executesql @SQL, N'@SearchVal NVARCHAR(100)', @SearchVal = @SearchValue;
-- Safe - parameter is treated as data, not SQL
```

### Using QUOTENAME for Identifiers
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @TableName NVARCHAR(128) = 'Employees';

-- Safe - QUOTENAME handles special characters
SET @SQL = N'SELECT * FROM ' + QUOTENAME(@TableName);
EXEC sp_executesql @SQL;
```

## Common Patterns

### Dynamic WHERE Clause
```sql
CREATE PROCEDURE PROC_SearchEmployees
    @FirstName NVARCHAR(50) = NULL,
    @LastName NVARCHAR(50) = NULL,
    @DepartmentID INT = NULL,
    @MinSalary DECIMAL(18,2) = NULL
AS
BEGIN
    DECLARE @SQL NVARCHAR(MAX);
    DECLARE @WhereClause NVARCHAR(MAX) = N'';
    
    SET @SQL = N'SELECT EmployeeID, FirstName, LastName, DepartmentID, Salary
                 FROM Employees
                 WHERE 1 = 1';
    
    IF @FirstName IS NOT NULL
        SET @WhereClause = @WhereClause + N' AND FirstName = @FirstName';
    
    IF @LastName IS NOT NULL
        SET @WhereClause = @WhereClause + N' AND LastName = @LastName';
    
    IF @DepartmentID IS NOT NULL
        SET @WhereClause = @WhereClause + N' AND DepartmentID = @DeptID';
    
    IF @MinSalary IS NOT NULL
        SET @WhereClause = @WhereClause + N' AND Salary >= @MinSal';
    
    SET @SQL = @SQL + @WhereClause;
    
    EXEC sp_executesql 
        @SQL,
        N'@FirstName NVARCHAR(50), @LastName NVARCHAR(50), @DeptID INT, @MinSal DECIMAL(18,2)',
        @FirstName = @FirstName,
        @LastName = @LastName,
        @DeptID = @DepartmentID,
        @MinSal = @MinSalary;
END
```

### Dynamic ORDER BY
```sql
CREATE PROCEDURE PROC_GetEmployeesSorted
    @SortColumn NVARCHAR(128) = 'EmployeeID',
    @SortDirection NVARCHAR(4) = 'ASC'
AS
BEGIN
    DECLARE @SQL NVARCHAR(MAX);
    
    -- Validate sort column to prevent injection
    IF @SortColumn NOT IN ('EmployeeID', 'FirstName', 'LastName', 'Salary', 'HireDate')
        SET @SortColumn = 'EmployeeID';
    
    IF @SortDirection NOT IN ('ASC', 'DESC')
        SET @SortDirection = 'ASC';
    
    SET @SQL = N'
        SELECT EmployeeID, FirstName, LastName, Salary, HireDate
        FROM Employees
        ORDER BY ' + QUOTENAME(@SortColumn) + N' ' + @SortDirection;
    
    EXEC sp_executesql @SQL;
END
```

### Dynamic PIVOT
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @Columns NVARCHAR(MAX);

-- Build column list dynamically
SELECT @Columns = STUFF((
    SELECT ', ' + QUOTENAME(DepartmentName)
    FROM Departments
    FOR XML PATH(''), TYPE
).value('.', 'NVARCHAR(MAX)'), 1, 2, '');

SET @SQL = N'
    SELECT *
    FROM (
        SELECT DepartmentName, EmployeeID, Salary
        FROM Employees e
        INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID
    ) AS SourceTable
    PIVOT (
        COUNT(EmployeeID)
        FOR DepartmentName IN (' + @Columns + N')
    ) AS PivotTable';

EXEC sp_executesql @SQL;
```

### Dynamic Table Creation
```sql
CREATE PROCEDURE PROC_CreateTempTable
    @TableName NVARCHAR(128),
    @ColumnDefinitions NVARCHAR(MAX)
AS
BEGIN
    DECLARE @SQL NVARCHAR(MAX);
    
    -- Validate table name
    IF @TableName NOT LIKE '[a-zA-Z][a-zA-Z0-9_]*'
    BEGIN
        RAISERROR('Invalid table name', 16, 1);
        RETURN;
    END
    
    SET @SQL = N'CREATE TABLE ' + QUOTENAME(@TableName) + N' (' + @ColumnDefinitions + N')';
    
    EXEC sp_executesql @SQL;
END
```

## Returning Values from Dynamic SQL

### Using OUTPUT Parameters
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @EmployeeCount INT;

SET @SQL = N'SELECT @Count = COUNT(*) FROM Employees WHERE DepartmentID = @DeptID';

EXEC sp_executesql 
    @SQL,
    N'@DeptID INT, @Count INT OUTPUT',
    @DeptID = 5,
    @Count = @EmployeeCount OUTPUT;

SELECT @EmployeeCount AS EmployeeCount;
```

### Using Result Sets
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @TableName NVARCHAR(128) = 'Employees';

SET @SQL = N'SELECT * FROM ' + QUOTENAME(@TableName);

-- Result set is returned to caller
EXEC sp_executesql @SQL;
```

### Inserting Results into Table
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @TableName NVARCHAR(128) = 'Employees';

CREATE TABLE #TempResults (
    EmployeeID INT,
    FirstName VARCHAR(50),
    LastName VARCHAR(50)
);

SET @SQL = N'INSERT INTO #TempResults SELECT EmployeeID, FirstName, LastName FROM ' + QUOTENAME(@TableName);
EXEC sp_executesql @SQL;

SELECT * FROM #TempResults;
DROP TABLE #TempResults;
```

## Error Handling in Dynamic SQL

### Error Handling Pattern
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @TableName NVARCHAR(128) = 'NonExistentTable';

BEGIN TRY
    SET @SQL = N'SELECT * FROM ' + QUOTENAME(@TableName);
    EXEC sp_executesql @SQL;
END TRY
BEGIN CATCH
    PRINT 'Error in dynamic SQL: ' + ERROR_MESSAGE();
    PRINT 'SQL Statement: ' + @SQL;
END CATCH
```

### Nested Error Handling
```sql
BEGIN TRY
    DECLARE @SQL NVARCHAR(MAX) = N'
        BEGIN TRY
            SELECT * FROM NonExistentTable;
        END TRY
        BEGIN CATCH
            SELECT ERROR_MESSAGE() AS ErrorMessage;
        END CATCH';
    
    EXEC sp_executesql @SQL;
END TRY
BEGIN CATCH
    PRINT 'Outer error: ' + ERROR_MESSAGE();
END CATCH
```

## Performance Considerations

### Execution Plan Caching
```sql
-- Dynamic SQL execution plans are cached per statement text
-- Parameterized queries share execution plans
DECLARE @SQL NVARCHAR(MAX) = N'SELECT * FROM Employees WHERE EmployeeID = @ID';

-- These share the same execution plan
EXEC sp_executesql @SQL, N'@ID INT', @ID = 1;
EXEC sp_executesql @SQL, N'@ID INT', @ID = 2;
EXEC sp_executesql @SQL, N'@ID INT', @ID = 3;
```

### OPTION RECOMPILE
```sql
DECLARE @SQL NVARCHAR(MAX) = N'
    SELECT * FROM Employees 
    WHERE DepartmentID = @DeptID
    OPTION (RECOMPILE)';

EXEC sp_executesql @SQL, N'@DeptID INT', @DeptID = 5;
```

## Best Practices

### 1. **Always Use Parameterization**
- Use sp_executesql with parameters
- Never concatenate user input directly
- Prevents SQL injection

### 2. **Validate Input**
- Validate table/column names
- Use whitelist approach when possible
- Check for valid identifiers

### 3. **Use QUOTENAME**
- For identifiers (table/column names)
- Handles special characters
- Prevents injection through identifiers

### 4. **Error Handling**
- Always use TRY...CATCH
- Log dynamic SQL statements
- Provide meaningful error messages

### 5. **Performance**
- Parameterize for plan reuse
- Consider execution plan caching
- Use OPTION RECOMPILE when needed

### 6. **Documentation**
- Document why dynamic SQL is needed
- Comment complex string building
- Explain security considerations

### 7. **Testing**
- Test with various inputs
- Test edge cases
- Test for SQL injection attempts

## Common Use Cases

### 1. **Flexible Search**
- Dynamic WHERE clauses based on user input
- Multiple optional filters
- Custom sorting

### 2. **Generic Procedures**
- Procedures that work with multiple tables
- Table name as parameter
- Reusable code

### 3. **Reporting**
- Dynamic column selection
- Dynamic grouping
- Custom report generation

### 4. **Administration**
- Schema management
- Dynamic table creation
- Maintenance scripts

## Security Checklist

- ✅ Use parameterized queries (sp_executesql)
- ✅ Validate all input
- ✅ Use QUOTENAME for identifiers
- ✅ Whitelist table/column names when possible
- ✅ Limit permissions (principle of least privilege)
- ✅ Log dynamic SQL execution
- ✅ Test for SQL injection vulnerabilities
- ❌ Never concatenate user input directly
- ❌ Never use EXEC with string concatenation
- ❌ Never trust user input

## Related Concepts
- See `01_Stored_Procedures.md` for dynamic SQL in procedures
- See `11_Error_Handling.md` for error handling
- See `03_Queries.md` for static SQL alternatives

