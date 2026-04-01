# Functions - Database Documentation

## Overview
A Function in SQL Server is a database object that returns a single value or a table. Functions are similar to stored procedures but have key differences: they must return a value, cannot modify database state, and can be used in SQL statements.

## Types of Functions

### 1. **Scalar Functions**
Return a single value of a specific data type.

### 2. **Table-Valued Functions**
Return a table that can be used like a regular table in queries.

### 3. **System Functions**
Built-in functions provided by SQL Server.

## Key Characteristics

### Differences from Stored Procedures
- **Must Return a Value**: Functions always return a value
- **Cannot Modify Data**: Cannot use INSERT, UPDATE, DELETE (except in table variables)
- **Can be Used in SELECT**: Can be called in SELECT, WHERE, and other clauses
- **Deterministic**: Should produce same output for same input (when possible)
- **No Side Effects**: Should not modify database state

## Scalar Functions

### Syntax
```sql
CREATE FUNCTION [schema_name.]function_name
    (@parameter1 datatype [= default_value],
     @parameter2 datatype [= default_value],
     ...)
RETURNS return_datatype
[WITH {ENCRYPTION | SCHEMABINDING | RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT}]
AS
BEGIN
    DECLARE @return_value datatype;
    
    -- Logic to calculate return value
    SET @return_value = ...;
    
    RETURN @return_value;
END
```

### Example: Calculate Age
```sql
CREATE FUNCTION FN_CalculateAge
    (@BirthDate DATE)
RETURNS INT
AS
BEGIN
    DECLARE @Age INT;
    SET @Age = DATEDIFF(YEAR, @BirthDate, GETDATE());
    
    -- Adjust if birthday hasn't occurred this year
    IF DATEADD(YEAR, @Age, @BirthDate) > GETDATE()
        SET @Age = @Age - 1;
    
    RETURN @Age;
END
```

### Usage
```sql
SELECT 
    FirstName,
    LastName,
    BirthDate,
    dbo.FN_CalculateAge(BirthDate) AS Age
FROM Employees;
```

### Example: Format Currency
```sql
CREATE FUNCTION FN_FormatCurrency
    (@Amount DECIMAL(18,2),
     @CurrencyCode VARCHAR(3) = 'USD')
RETURNS VARCHAR(50)
AS
BEGIN
    DECLARE @Formatted VARCHAR(50);
    
    IF @CurrencyCode = 'USD'
        SET @Formatted = '$' + FORMAT(@Amount, 'N2');
    ELSE IF @CurrencyCode = 'EUR'
        SET @Formatted = '€' + FORMAT(@Amount, 'N2');
    ELSE
        SET @Formatted = @CurrencyCode + ' ' + FORMAT(@Amount, 'N2');
    
    RETURN @Formatted;
END
```

## Table-Valued Functions

### Inline Table-Valued Functions
Return a single SELECT statement result.

```sql
CREATE FUNCTION FN_GetEmployeesByDepartment
    (@DepartmentID INT)
RETURNS TABLE
AS
RETURN
(
    SELECT 
        EmployeeID,
        FirstName,
        LastName,
        Email,
        HireDate
    FROM Employees
    WHERE DepartmentID = @DepartmentID
);
```

### Usage
```sql
SELECT * FROM dbo.FN_GetEmployeesByDepartment(5);
```

### Multi-Statement Table-Valued Functions
Allow multiple statements and use table variables.

```sql
CREATE FUNCTION FN_GetEmployeeHierarchy
    (@ManagerID INT)
RETURNS @Result TABLE
(
    EmployeeID INT,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Level INT,
    ManagerID INT
)
AS
BEGIN
    DECLARE @Level INT = 0;
    
    -- Insert manager
    INSERT INTO @Result
    SELECT EmployeeID, FirstName, LastName, @Level, ManagerID
    FROM Employees
    WHERE EmployeeID = @ManagerID;
    
    -- Insert direct reports
    INSERT INTO @Result
    SELECT EmployeeID, FirstName, LastName, @Level + 1, ManagerID
    FROM Employees
    WHERE ManagerID = @ManagerID;
    
    RETURN;
END
```

## Function Options

### SCHEMABINDING
Prevents changes to underlying objects.

```sql
CREATE FUNCTION FN_GetProductPrice
    (@ProductID INT)
RETURNS DECIMAL(18,2)
WITH SCHEMABINDING
AS
BEGIN
    RETURN (
        SELECT Price
        FROM dbo.Products
        WHERE ProductID = @ProductID
    );
END
```

### ENCRYPTION
Encrypts the function definition.

```sql
CREATE FUNCTION FN_EncryptedFunction
    (@Input INT)
RETURNS INT
WITH ENCRYPTION
AS
BEGIN
    RETURN @Input * 2;
END
```

## Deterministic vs Non-Deterministic

### Deterministic Functions
Always return the same result for the same input.

```sql
CREATE FUNCTION FN_AddNumbers
    (@A INT, @B INT)
RETURNS INT
AS
BEGIN
    RETURN @A + @B;
END
```

### Non-Deterministic Functions
May return different results for the same input.

```sql
CREATE FUNCTION FN_GetCurrentTime
    ()
RETURNS DATETIME
AS
BEGIN
    RETURN GETDATE();
END
```

## Best Practices

1. **Use Appropriate Type**: Choose scalar vs table-valued based on needs
2. **Avoid Side Effects**: Functions should not modify database state
3. **Performance**: Table-valued functions can be inlined by optimizer
4. **Naming**: Use consistent naming (e.g., FN_ prefix for functions)
5. **Documentation**: Include comments explaining purpose and parameters
6. **Error Handling**: Handle NULL values appropriately
7. **Deterministic When Possible**: Helps with indexing and optimization

## Modifying and Dropping

### Alter Function
```sql
ALTER FUNCTION function_name
    (@parameter datatype)
RETURNS return_type
AS
BEGIN
    -- Updated logic
    RETURN value;
END
```

### Drop Function
```sql
DROP FUNCTION [IF EXISTS] function_name;
```

## System Functions

### String Functions
- `LEN()`, `SUBSTRING()`, `REPLACE()`, `UPPER()`, `LOWER()`, `TRIM()`

### Date Functions
- `GETDATE()`, `DATEADD()`, `DATEDIFF()`, `YEAR()`, `MONTH()`, `DAY()`

### Aggregate Functions
- `SUM()`, `AVG()`, `COUNT()`, `MIN()`, `MAX()`

### Mathematical Functions
- `ABS()`, `ROUND()`, `CEILING()`, `FLOOR()`, `POWER()`, `SQRT()`

### Conversion Functions
- `CAST()`, `CONVERT()`, `PARSE()`, `TRY_CAST()`, `TRY_CONVERT()`

## Common Patterns

### String Manipulation
```sql
CREATE FUNCTION FN_FormatFullName
    (@FirstName VARCHAR(50),
     @LastName VARCHAR(50))
RETURNS VARCHAR(101)
AS
BEGIN
    RETURN LTRIM(RTRIM(@FirstName)) + ' ' + LTRIM(RTRIM(@LastName));
END
```

### Date Calculations
```sql
CREATE FUNCTION FN_GetBusinessDays
    (@StartDate DATE,
     @EndDate DATE)
RETURNS INT
AS
BEGIN
    DECLARE @Days INT = 0;
    DECLARE @CurrentDate DATE = @StartDate;
    
    WHILE @CurrentDate <= @EndDate
    BEGIN
        IF DATEPART(WEEKDAY, @CurrentDate) NOT IN (1, 7) -- Not Saturday or Sunday
            SET @Days = @Days + 1;
        
        SET @CurrentDate = DATEADD(DAY, 1, @CurrentDate);
    END
    
    RETURN @Days;
END
```

### Validation Functions
```sql
CREATE FUNCTION FN_IsValidEmail
    (@Email VARCHAR(255))
RETURNS BIT
AS
BEGIN
    IF @Email LIKE '%_@_%._%'
        RETURN 1;
    RETURN 0;
END
```

## Performance Considerations

1. **Inline Table-Valued Functions**: Can be optimized by query optimizer
2. **Multi-Statement Table-Valued Functions**: May have performance overhead
3. **Scalar Functions in WHERE Clause**: Can prevent index usage
4. **Computed Columns**: Consider using computed columns instead of functions for frequently accessed calculations

## Security

### Grant Execute Permission
```sql
GRANT EXECUTE ON function_name TO username;
```

## Viewing Functions

### List All Functions
```sql
SELECT 
    ROUTINE_SCHEMA,
    ROUTINE_NAME,
    ROUTINE_TYPE,
    DATA_TYPE,
    CREATED,
    LAST_ALTERED
FROM INFORMATION_SCHEMA.ROUTINES
WHERE ROUTINE_TYPE = 'FUNCTION';
```

### View Function Definition
```sql
EXEC sp_helptext 'function_name';
```

### View Function Dependencies
```sql
EXEC sp_depends 'function_name';
```

## Related Concepts
- See `01_Stored_Procedures.md` for Stored Procedures
- See `03_Queries.md` for SQL Query concepts
- See `04_Recurring_JV_Procedures.md` for specific Recurring JV procedures

