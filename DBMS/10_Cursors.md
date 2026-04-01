# Cursors - Database Documentation

## Overview
A Cursor is a database object that allows row-by-row processing of a result set. While set-based operations are generally preferred in SQL Server for performance, cursors are useful when you need to perform different operations on each row or when set-based solutions are not feasible.

## Key Characteristics

### 1. **Row-by-Row Processing**
- Processes one row at a time
- Allows different logic per row
- Provides fine-grained control

### 2. **Performance Considerations**
- Generally slower than set-based operations
- Should be used only when necessary
- Can cause blocking and resource issues

### 3. **Use Cases**
- Complex row-by-row logic
- Operations requiring different actions per row
- Integration with external systems
- When set-based approach is not possible

## Types of Cursors

### 1. **Static Cursor**
- Snapshot of data at cursor creation
- Changes to underlying data not visible
- Uses tempdb for storage
- Most resource-intensive

### 2. **Dynamic Cursor**
- Reflects all changes in real-time
- Most flexible but slowest
- Highest resource usage
- Shows all inserts, updates, deletes

### 3. **Keyset-Driven Cursor**
- Membership fixed at cursor creation
- Values can change
- Deleted rows appear as missing
- Balanced performance

### 4. **Forward-Only Cursor**
- Can only move forward
- Fastest cursor type
- No scrolling backward
- Default for most operations

### 5. **Fast-Forward Cursor**
- Optimized forward-only cursor
- Read-only
- Best performance
- Default for SELECT statements

## Cursor Syntax

### Basic Cursor Structure
```sql
DECLARE cursor_name CURSOR
FOR
SELECT column1, column2, ...
FROM table_name
WHERE condition;

OPEN cursor_name;

FETCH NEXT FROM cursor_name INTO @variable1, @variable2, ...;

WHILE @@FETCH_STATUS = 0
BEGIN
    -- Process row
    -- Logic here
    
    FETCH NEXT FROM cursor_name INTO @variable1, @variable2, ...;
END

CLOSE cursor_name;
DEALLOCATE cursor_name;
```

### Simple Cursor Example
```sql
DECLARE @EmployeeID INT;
DECLARE @FirstName VARCHAR(50);
DECLARE @LastName VARCHAR(50);

DECLARE EmployeeCursor CURSOR FOR
SELECT EmployeeID, FirstName, LastName
FROM Employees
WHERE DepartmentID = 5;

OPEN EmployeeCursor;

FETCH NEXT FROM EmployeeCursor INTO @EmployeeID, @FirstName, @LastName;

WHILE @@FETCH_STATUS = 0
BEGIN
    PRINT 'Processing: ' + @FirstName + ' ' + @LastName;
    
    -- Process each employee
    UPDATE Employees
    SET LastProcessed = GETDATE()
    WHERE EmployeeID = @EmployeeID;
    
    FETCH NEXT FROM EmployeeCursor INTO @EmployeeID, @FirstName, @LastName;
END

CLOSE EmployeeCursor;
DEALLOCATE EmployeeCursor;
```

## Cursor Options

### Static Cursor
```sql
DECLARE EmployeeCursor CURSOR
STATIC
FOR
SELECT EmployeeID, FirstName, LastName
FROM Employees;
```

### Dynamic Cursor
```sql
DECLARE EmployeeCursor CURSOR
DYNAMIC
FOR
SELECT EmployeeID, FirstName, LastName
FROM Employees;
```

### Keyset-Driven Cursor
```sql
DECLARE EmployeeCursor CURSOR
KEYSET
FOR
SELECT EmployeeID, FirstName, LastName
FROM Employees;
```

### Forward-Only Cursor
```sql
DECLARE EmployeeCursor CURSOR
FORWARD_ONLY
FOR
SELECT EmployeeID, FirstName, LastName
FROM Employees;
```

### Fast-Forward Cursor
```sql
DECLARE EmployeeCursor CURSOR
FAST_FORWARD
FOR
SELECT EmployeeID, FirstName, LastName
FROM Employees;
```

## Scrollable Cursors

### Scroll Options
```sql
DECLARE EmployeeCursor CURSOR
SCROLL
FOR
SELECT EmployeeID, FirstName, LastName
FROM Employees;

OPEN EmployeeCursor;

-- Move to first row
FETCH FIRST FROM EmployeeCursor INTO @EmployeeID, @FirstName, @LastName;

-- Move to last row
FETCH LAST FROM EmployeeCursor INTO @EmployeeID, @FirstName, @LastName;

-- Move to next row
FETCH NEXT FROM EmployeeCursor INTO @EmployeeID, @FirstName, @LastName;

-- Move to previous row
FETCH PRIOR FROM EmployeeCursor INTO @EmployeeID, @FirstName, @LastName;

-- Move to absolute position
FETCH ABSOLUTE 5 FROM EmployeeCursor INTO @EmployeeID, @FirstName, @LastName;

-- Move relative to current position
FETCH RELATIVE 2 FROM EmployeeCursor INTO @EmployeeID, @FirstName, @LastName;

CLOSE EmployeeCursor;
DEALLOCATE EmployeeCursor;
```

## Cursor Variables

### Using Cursor Variables
```sql
DECLARE @EmployeeCursor CURSOR;

SET @EmployeeCursor = CURSOR FOR
SELECT EmployeeID, FirstName, LastName
FROM Employees;

OPEN @EmployeeCursor;

FETCH NEXT FROM @EmployeeCursor INTO @EmployeeID, @FirstName, @LastName;

WHILE @@FETCH_STATUS = 0
BEGIN
    -- Process row
    FETCH NEXT FROM @EmployeeCursor INTO @EmployeeID, @FirstName, @LastName;
END

CLOSE @EmployeeCursor;
DEALLOCATE @EmployeeCursor;
```

## Cursor with Parameters

### Parameterized Cursor
```sql
DECLARE @DepartmentID INT = 5;

DECLARE EmployeeCursor CURSOR
FOR
SELECT EmployeeID, FirstName, LastName
FROM Employees
WHERE DepartmentID = @DepartmentID;

OPEN EmployeeCursor;
-- Process cursor
CLOSE EmployeeCursor;
DEALLOCATE EmployeeCursor;
```

## @@FETCH_STATUS Values

### Status Codes
- **0** - Successfully fetched row
- **-1** - Fetch statement failed or row is beyond result set
- **-2** - Row fetched is missing (keyset cursor)

### Example Usage
```sql
DECLARE @EmployeeID INT;
DECLARE EmployeeCursor CURSOR FOR
SELECT EmployeeID FROM Employees;

OPEN EmployeeCursor;

FETCH NEXT FROM EmployeeCursor INTO @EmployeeID;

WHILE @@FETCH_STATUS = 0
BEGIN
    PRINT 'Employee ID: ' + CAST(@EmployeeID AS VARCHAR);
    FETCH NEXT FROM EmployeeCursor INTO @EmployeeID;
END

IF @@FETCH_STATUS = -1
    PRINT 'End of result set';
ELSE IF @@FETCH_STATUS = -2
    PRINT 'Row is missing';

CLOSE EmployeeCursor;
DEALLOCATE EmployeeCursor;
```

## Common Patterns

### Processing with Different Logic per Row
```sql
DECLARE @OrderID INT;
DECLARE @OrderStatus VARCHAR(20);
DECLARE @ProcessResult VARCHAR(100);

DECLARE OrderCursor CURSOR FOR
SELECT OrderID, Status
FROM Orders
WHERE Status IN ('Pending', 'Processing');

OPEN OrderCursor;

FETCH NEXT FROM OrderCursor INTO @OrderID, @OrderStatus;

WHILE @@FETCH_STATUS = 0
BEGIN
    IF @OrderStatus = 'Pending'
    BEGIN
        EXEC PROC_ProcessPendingOrder @OrderID, @ProcessResult OUTPUT;
    END
    ELSE IF @OrderStatus = 'Processing'
    BEGIN
        EXEC PROC_CompleteProcessingOrder @OrderID, @ProcessResult OUTPUT;
    END
    
    PRINT 'Order ' + CAST(@OrderID AS VARCHAR) + ': ' + @ProcessResult;
    
    FETCH NEXT FROM OrderCursor INTO @OrderID, @OrderStatus;
END

CLOSE OrderCursor;
DEALLOCATE OrderCursor;
```

### Cursor with Error Handling
```sql
DECLARE @EmployeeID INT;
DECLARE @ErrorCount INT = 0;

DECLARE EmployeeCursor CURSOR FOR
SELECT EmployeeID
FROM Employees
WHERE Status = 'Active';

OPEN EmployeeCursor;

FETCH NEXT FROM EmployeeCursor INTO @EmployeeID;

WHILE @@FETCH_STATUS = 0
BEGIN
    BEGIN TRY
        -- Process employee
        EXEC PROC_ProcessEmployee @EmployeeID;
    END TRY
    BEGIN CATCH
        SET @ErrorCount = @ErrorCount + 1;
        PRINT 'Error processing EmployeeID ' + CAST(@EmployeeID AS VARCHAR) + ': ' + ERROR_MESSAGE();
    END CATCH
    
    FETCH NEXT FROM EmployeeCursor INTO @EmployeeID;
END

CLOSE EmployeeCursor;
DEALLOCATE EmployeeCursor;

PRINT 'Total errors: ' + CAST(@ErrorCount AS VARCHAR);
```

### Cursor in Stored Procedure
```sql
CREATE PROCEDURE PROC_ProcessEmployeesByDepartment
    @DepartmentID INT
AS
BEGIN
    SET NOCOUNT ON;
    
    DECLARE @EmployeeID INT;
    DECLARE @FirstName VARCHAR(50);
    DECLARE @ProcessedCount INT = 0;
    
    DECLARE EmployeeCursor CURSOR LOCAL FAST_FORWARD
    FOR
    SELECT EmployeeID, FirstName
    FROM Employees
    WHERE DepartmentID = @DepartmentID
        AND Status = 'Active';
    
    OPEN EmployeeCursor;
    
    FETCH NEXT FROM EmployeeCursor INTO @EmployeeID, @FirstName;
    
    WHILE @@FETCH_STATUS = 0
    BEGIN
        -- Process employee
        EXEC PROC_UpdateEmployeeStatus @EmployeeID;
        SET @ProcessedCount = @ProcessedCount + 1;
        
        FETCH NEXT FROM EmployeeCursor INTO @EmployeeID, @FirstName;
    END
    
    CLOSE EmployeeCursor;
    DEALLOCATE EmployeeCursor;
    
    RETURN @ProcessedCount;
END
```

## Cursor Scope

### LOCAL vs GLOBAL
```sql
-- Local cursor (default in stored procedures)
DECLARE EmployeeCursor CURSOR LOCAL
FOR SELECT * FROM Employees;

-- Global cursor
DECLARE EmployeeCursor CURSOR GLOBAL
FOR SELECT * FROM Employees;
```

## Updatable Cursors

### FOR UPDATE Clause
```sql
DECLARE EmployeeCursor CURSOR
FOR
SELECT EmployeeID, Salary
FROM Employees
FOR UPDATE OF Salary;

OPEN EmployeeCursor;

FETCH NEXT FROM EmployeeCursor INTO @EmployeeID, @Salary;

WHILE @@FETCH_STATUS = 0
BEGIN
    -- Update current row
    UPDATE Employees
    SET Salary = Salary * 1.1
    WHERE CURRENT OF EmployeeCursor;
    
    FETCH NEXT FROM EmployeeCursor INTO @EmployeeID, @Salary;
END

CLOSE EmployeeCursor;
DEALLOCATE EmployeeCursor;
```

## Best Practices

### 1. **Avoid When Possible**
- Prefer set-based operations
- Use cursors only when necessary
- Consider alternatives (CTEs, window functions)

### 2. **Choose Right Type**
- Use FAST_FORWARD for read-only operations
- Use STATIC when snapshot is acceptable
- Avoid DYNAMIC unless necessary

### 3. **Keep Cursors Short**
- Process quickly
- Don't hold locks unnecessarily
- Close and deallocate promptly

### 4. **Error Handling**
- Always use TRY...CATCH
- Ensure cleanup in error scenarios
- Check @@FETCH_STATUS properly

### 5. **Performance**
- Use LOCAL scope when possible
- Choose appropriate cursor type
- Consider batch processing

### 6. **Cleanup**
- Always CLOSE cursor
- Always DEALLOCATE cursor
- Even in error handlers

## Alternatives to Cursors

### Set-Based Operations
```sql
-- Instead of cursor, use set-based update
UPDATE Employees
SET LastProcessed = GETDATE()
WHERE DepartmentID = 5;
```

### WHILE Loop with TOP
```sql
WHILE EXISTS (SELECT 1 FROM Employees WHERE Status = 'Pending')
BEGIN
    UPDATE TOP (100) Employees
    SET Status = 'Processing'
    WHERE Status = 'Pending';
    
    -- Process batch
    WAITFOR DELAY '00:00:01';
END
```

### Recursive CTE
```sql
WITH EmployeeHierarchy AS (
    -- Anchor
    SELECT EmployeeID, ManagerID, 0 AS Level
    FROM Employees
    WHERE ManagerID IS NULL
    
    UNION ALL
    
    -- Recursive
    SELECT e.EmployeeID, e.ManagerID, eh.Level + 1
    FROM Employees e
    INNER JOIN EmployeeHierarchy eh ON e.ManagerID = eh.EmployeeID
)
SELECT * FROM EmployeeHierarchy;
```

## Performance Considerations

### Cursor vs Set-Based
- **Cursors**: Row-by-row, slower, more resource-intensive
- **Set-Based**: Bulk operations, faster, more efficient

### When Cursors Are Acceptable
- Complex per-row logic that can't be set-based
- Integration with external systems requiring row-by-row processing
- Operations where set-based approach is not feasible
- Small result sets where performance impact is minimal

## Related Concepts
- See `01_Stored_Procedures.md` for cursor usage in procedures
- See `03_Queries.md` for set-based alternatives
- See `09_Transactions.md` for cursor transactions
- See `11_Error_Handling.md` for error handling with cursors

