# Stored Procedures (SP) - Database Documentation

## Overview
A Stored Procedure (SP) is a precompiled collection of SQL statements and optional control-of-flow statements stored under a name and processed as a unit. Stored procedures are stored in the database and can be called by applications, triggers, or other stored procedures.

## Key Characteristics

### 1. **Precompiled Execution**
- Stored procedures are compiled once and stored in the database
- Execution plans are cached, resulting in better performance
- Reduces network traffic by executing multiple statements in a single call

### 2. **Parameters**
- **Input Parameters**: Pass values into the procedure
- **Output Parameters**: Return values from the procedure
- **Input/Output Parameters**: Can both receive and return values

### 3. **Benefits**
- **Performance**: Faster execution due to precompilation
- **Security**: Can control access to data through procedures
- **Maintainability**: Centralized business logic
- **Reusability**: Can be called from multiple applications
- **Reduced Network Traffic**: Execute multiple operations in one call

## Syntax Structure

### Basic Syntax
```sql
CREATE PROCEDURE [schema_name.]procedure_name
    [@parameter1 datatype [= default_value] [OUTPUT],
     @parameter2 datatype [= default_value] [OUTPUT],
     ...]
AS
BEGIN
    -- SQL statements
    -- Control-of-flow statements
    -- Error handling
END
```

### Example
```sql
CREATE PROCEDURE PROC_GetEmployeeDetails
    @EmployeeID INT,
    @DepartmentName VARCHAR(50) OUTPUT
AS
BEGIN
    SET NOCOUNT ON;
    
    SELECT 
        EmployeeID,
        FirstName,
        LastName,
        Email
    FROM Employees
    WHERE EmployeeID = @EmployeeID;
    
    SELECT @DepartmentName = DepartmentName
    FROM Departments d
    INNER JOIN Employees e ON d.DepartmentID = e.DepartmentID
    WHERE e.EmployeeID = @EmployeeID;
END
```

## Execution

### Calling a Stored Procedure
```sql
-- Basic execution
EXEC procedure_name @param1 = value1, @param2 = value2;

-- With output parameter
DECLARE @OutputValue VARCHAR(50);
EXEC PROC_GetEmployeeDetails 
    @EmployeeID = 123,
    @DepartmentName = @OutputValue OUTPUT;
SELECT @OutputValue;
```

## Control-of-Flow Statements

### IF...ELSE
```sql
IF @Condition = 1
BEGIN
    -- Statements
END
ELSE
BEGIN
    -- Statements
END
```

### WHILE Loop
```sql
WHILE @Counter < 10
BEGIN
    -- Statements
    SET @Counter = @Counter + 1;
END
```

### TRY...CATCH
```sql
BEGIN TRY
    -- Statements that might cause errors
END TRY
BEGIN CATCH
    -- Error handling
    SELECT ERROR_MESSAGE(), ERROR_NUMBER();
END CATCH
```

## Transaction Management

### Using Transactions
```sql
CREATE PROCEDURE PROC_TransferFunds
    @FromAccount INT,
    @ToAccount INT,
    @Amount DECIMAL(18,2)
AS
BEGIN
    BEGIN TRANSACTION;
    
    BEGIN TRY
        UPDATE Accounts SET Balance = Balance - @Amount
        WHERE AccountID = @FromAccount;
        
        UPDATE Accounts SET Balance = Balance + @Amount
        WHERE AccountID = @ToAccount;
        
        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION;
        THROW;
    END CATCH
END
```

## Best Practices

1. **Use SET NOCOUNT ON**: Reduces network traffic by not sending row count messages
2. **Error Handling**: Always implement proper error handling with TRY...CATCH
3. **Transaction Management**: Use transactions for operations that must be atomic
4. **Parameter Validation**: Validate input parameters before processing
5. **Naming Conventions**: Use consistent naming (e.g., PROC_ prefix)
6. **Documentation**: Include comments explaining the procedure's purpose
7. **Avoid SELECT ***: Specify column names explicitly
8. **Index Usage**: Design procedures to leverage existing indexes

## Modifying and Dropping

### Alter Procedure
```sql
ALTER PROCEDURE procedure_name
    @parameter datatype
AS
BEGIN
    -- Updated statements
END
```

### Drop Procedure
```sql
DROP PROCEDURE [IF EXISTS] procedure_name;
```

## System Stored Procedures

### View Procedure Definition
```sql
EXEC sp_helptext 'procedure_name';
```

### View Procedure Dependencies
```sql
EXEC sp_depends 'procedure_name';
```

### List All Procedures
```sql
SELECT 
    ROUTINE_SCHEMA,
    ROUTINE_NAME,
    CREATED,
    LAST_ALTERED
FROM INFORMATION_SCHEMA.ROUTINES
WHERE ROUTINE_TYPE = 'PROCEDURE';
```

## Performance Considerations

1. **Execution Plan Caching**: First execution compiles and caches the plan
2. **Parameter Sniffing**: SQL Server may optimize for first parameter values
3. **Recompilation**: Use WITH RECOMPILE if parameters vary significantly
4. **Statistics**: Keep statistics updated for optimal performance

## Security

### Grant Execute Permission
```sql
GRANT EXECUTE ON procedure_name TO username;
```

### Execute as Specific User
```sql
CREATE PROCEDURE procedure_name
WITH EXECUTE AS 'username'
AS
BEGIN
    -- Statements execute with specified user's permissions
END
```

## Common Patterns

### Pagination
```sql
CREATE PROCEDURE PROC_GetPagedResults
    @PageNumber INT = 1,
    @PageSize INT = 10
AS
BEGIN
    DECLARE @Offset INT = (@PageNumber - 1) * @PageSize;
    
    SELECT *
    FROM TableName
    ORDER BY ID
    OFFSET @Offset ROWS
    FETCH NEXT @PageSize ROWS ONLY;
END
```

### Dynamic SQL (Use with Caution)
```sql
CREATE PROCEDURE PROC_DynamicQuery
    @TableName NVARCHAR(128),
    @ColumnName NVARCHAR(128)
AS
BEGIN
    DECLARE @SQL NVARCHAR(MAX);
    SET @SQL = N'SELECT ' + QUOTENAME(@ColumnName) + 
               N' FROM ' + QUOTENAME(@TableName);
    EXEC sp_executesql @SQL;
END
```

## Related Concepts
- See `02_Functions.md` for User-Defined Functions
- See `03_Queries.md` for SQL Query concepts
- See `04_Recurring_JV_Procedures.md` for specific Recurring JV procedures

