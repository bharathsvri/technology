# Temporary Tables and Table Variables - Database Documentation

## Overview
Temporary Tables and Table Variables are used to store temporary data during query execution, stored procedure execution, or batch operations. They provide a way to hold intermediate results, break complex operations into steps, and improve query performance in certain scenarios.

## Types of Temporary Storage

### 1. **Local Temporary Tables (#TableName)**
- Visible only to current session
- Automatically dropped when session ends
- Can be shared within same session

### 2. **Global Temporary Tables (##TableName)**
- Visible to all sessions
- Dropped when last session using it closes
- Shared across multiple connections

### 3. **Table Variables (@TableVariable)**
- Similar to temporary tables but in-memory
- Scoped to batch, procedure, or function
- Automatically cleaned up

### 4. **Common Table Expressions (CTEs)**
- Temporary result set for single statement
- Not stored physically
- See `03_Queries.md` for details

## Local Temporary Tables

### Creating Local Temporary Table
```sql
CREATE TABLE #TempEmployees (
    EmployeeID INT,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Salary DECIMAL(18,2)
);
```

### Inserting Data
```sql
INSERT INTO #TempEmployees (EmployeeID, FirstName, LastName, Salary)
SELECT EmployeeID, FirstName, LastName, Salary
FROM Employees
WHERE DepartmentID = 5;
```

### Using Temporary Table
```sql
SELECT * FROM #TempEmployees WHERE Salary > 50000;
```

### Dropping Temporary Table
```sql
DROP TABLE #TempEmployees;
-- Or automatically dropped when session ends
```

### Temporary Table with Constraints
```sql
CREATE TABLE #TempOrders (
    OrderID INT PRIMARY KEY,
    CustomerID INT NOT NULL,
    OrderDate DATE DEFAULT GETDATE(),
    TotalAmount DECIMAL(18,2) CHECK (TotalAmount > 0)
);
```

### Temporary Table with Indexes
```sql
CREATE TABLE #TempEmployees (
    EmployeeID INT PRIMARY KEY,
    DepartmentID INT,
    Salary DECIMAL(18,2)
);

CREATE INDEX IX_TempEmployees_Dept ON #TempEmployees(DepartmentID);
```

## Global Temporary Tables

### Creating Global Temporary Table
```sql
CREATE TABLE ##GlobalTemp (
    ID INT,
    Name VARCHAR(50)
);
```

### Usage
- Visible to all sessions
- Useful for sharing data across connections
- Automatically dropped when last session closes

## Table Variables

### Declaring Table Variable
```sql
DECLARE @EmployeeTable TABLE (
    EmployeeID INT,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Salary DECIMAL(18,2)
);
```

### Inserting Data
```sql
INSERT INTO @EmployeeTable (EmployeeID, FirstName, LastName, Salary)
SELECT EmployeeID, FirstName, LastName, Salary
FROM Employees
WHERE DepartmentID = 5;
```

### Using Table Variable
```sql
SELECT * FROM @EmployeeTable WHERE Salary > 50000;
```

### Table Variable with Primary Key
```sql
DECLARE @EmployeeTable TABLE (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50)
);
```

### Table Variable with Multiple Constraints
```sql
DECLARE @Orders TABLE (
    OrderID INT PRIMARY KEY,
    CustomerID INT NOT NULL,
    OrderDate DATE DEFAULT GETDATE(),
    TotalAmount DECIMAL(18,2) CHECK (TotalAmount > 0),
    UNIQUE (CustomerID, OrderDate)
);
```

## Comparison: Temporary Tables vs Table Variables

### Temporary Tables
- ✅ Can have indexes
- ✅ Support statistics
- ✅ Can be altered (ALTER TABLE)
- ✅ Visible in tempdb
- ✅ Better for large datasets
- ✅ Can be used in dynamic SQL
- ❌ More overhead
- ❌ Requires explicit cleanup (or auto-cleanup)

### Table Variables
- ✅ Less overhead
- ✅ Automatic cleanup
- ✅ Scoped to batch/procedure
- ✅ No statistics overhead
- ❌ Limited indexing (only primary key/unique)
- ❌ No statistics
- ❌ Cannot be altered
- ❌ May be materialized to tempdb if large
- ❌ Not visible in tempdb system views

## Common Patterns

### Staging Data Pattern
```sql
-- Create staging table
CREATE TABLE #StagingData (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    SourceData NVARCHAR(MAX),
    Processed BIT DEFAULT 0
);

-- Load data
INSERT INTO #StagingData (SourceData)
SELECT DataColumn FROM SourceTable;

-- Process in batches
WHILE EXISTS (SELECT 1 FROM #StagingData WHERE Processed = 0)
BEGIN
    UPDATE TOP (100) #StagingData
    SET Processed = 1
    WHERE Processed = 0;
    
    -- Process batch
    -- ...
END

DROP TABLE #StagingData;
```

### Aggregation Pattern
```sql
-- Store intermediate aggregations
CREATE TABLE #DepartmentSummary (
    DepartmentID INT PRIMARY KEY,
    EmployeeCount INT,
    TotalSalary DECIMAL(18,2),
    AvgSalary DECIMAL(18,2)
);

INSERT INTO #DepartmentSummary (DepartmentID, EmployeeCount, TotalSalary, AvgSalary)
SELECT 
    DepartmentID,
    COUNT(*) AS EmployeeCount,
    SUM(Salary) AS TotalSalary,
    AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY DepartmentID;

-- Use for further processing
SELECT * FROM #DepartmentSummary WHERE AvgSalary > 50000;

DROP TABLE #DepartmentSummary;
```

### Data Transformation Pattern
```sql
CREATE TABLE #TransformedData (
    OriginalID INT,
    TransformedValue VARCHAR(100),
    ProcessedDate DATETIME DEFAULT GETDATE()
);

-- Transform and insert
INSERT INTO #TransformedData (OriginalID, TransformedValue)
SELECT 
    ID,
    UPPER(REPLACE(Value, ' ', '_'))
FROM SourceTable;

-- Use transformed data
SELECT * FROM #TransformedData;

DROP TABLE #TransformedData;
```

### Table Variable in Stored Procedure
```sql
CREATE PROCEDURE PROC_ProcessEmployees
    @DepartmentID INT
AS
BEGIN
    DECLARE @EmployeeList TABLE (
        EmployeeID INT PRIMARY KEY,
        FirstName VARCHAR(50),
        LastName VARCHAR(50)
    );
    
    -- Populate table variable
    INSERT INTO @EmployeeList (EmployeeID, FirstName, LastName)
    SELECT EmployeeID, FirstName, LastName
    FROM Employees
    WHERE DepartmentID = @DepartmentID;
    
    -- Use in joins
    SELECT 
        e.EmployeeID,
        e.FirstName,
        e.LastName,
        SUM(o.OrderAmount) AS TotalOrders
    FROM @EmployeeList e
    LEFT JOIN Orders o ON e.EmployeeID = o.EmployeeID
    GROUP BY e.EmployeeID, e.FirstName, e.LastName;
END
```

## Performance Considerations

### When to Use Temporary Tables
- Large datasets (thousands of rows)
- Need for indexes
- Complex queries requiring statistics
- Multiple operations on same data
- Need to share across batches in same session

### When to Use Table Variables
- Small datasets (typically < 100 rows)
- Simple lookups
- Single batch operations
- Minimal overhead desired
- Automatic cleanup needed

### Statistics and Optimization
```sql
-- Temporary tables can have statistics
CREATE TABLE #LargeTemp (
    ID INT,
    Value VARCHAR(100)
);

CREATE INDEX IX_LargeTemp_ID ON #LargeTemp(ID);

-- Update statistics
UPDATE STATISTICS #LargeTemp;

-- Table variables don't have statistics
-- Optimizer assumes 1 row for table variables
-- May cause suboptimal plans for larger datasets
```

## Dynamic SQL with Temporary Tables

### Creating in Dynamic SQL
```sql
DECLARE @SQL NVARCHAR(MAX);

SET @SQL = N'
    CREATE TABLE #DynamicTemp (
        ID INT,
        Name VARCHAR(50)
    );
    
    INSERT INTO #DynamicTemp VALUES (1, ''Test'');
    
    SELECT * FROM #DynamicTemp;
';

EXEC sp_executesql @SQL;
-- #DynamicTemp is automatically dropped after dynamic SQL completes
```

## Best Practices

### 1. **Choose Appropriate Type**
- Use table variables for small, simple data
- Use temporary tables for larger datasets or complex operations
- Consider performance implications

### 2. **Cleanup**
- Explicitly drop temporary tables when done
- Table variables clean up automatically
- Don't rely on automatic cleanup for temp tables

### 3. **Indexing**
- Create indexes on temporary tables for large datasets
- Table variables have limited indexing options
- Consider query patterns when creating indexes

### 4. **Naming**
- Use descriptive names
- Avoid conflicts with existing tables
- Consider scope (local vs global)

### 5. **Statistics**
- Update statistics on temporary tables if needed
- Be aware table variables don't have statistics
- May need OPTION (RECOMPILE) with table variables

### 6. **Scope Awareness**
- Understand session vs batch scope
- Local temp tables: session scope
- Table variables: batch/procedure scope
- Global temp tables: all sessions

## Common Scenarios

### Data Validation
```sql
CREATE TABLE #InvalidRecords (
    RecordID INT,
    ErrorMessage VARCHAR(500)
);

-- Validate and collect errors
INSERT INTO #InvalidRecords (RecordID, ErrorMessage)
SELECT ID, 'Invalid email format'
FROM SourceTable
WHERE Email NOT LIKE '%@%.%';

-- Report errors
SELECT * FROM #InvalidRecords;

DROP TABLE #InvalidRecords;
```

### Batch Processing
```sql
CREATE TABLE #BatchQueue (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    RecordID INT,
    Processed BIT DEFAULT 0
);

INSERT INTO #BatchQueue (RecordID)
SELECT ID FROM SourceTable;

WHILE EXISTS (SELECT 1 FROM #BatchQueue WHERE Processed = 0)
BEGIN
    DECLARE @RecordID INT;
    
    SELECT TOP 1 @RecordID = RecordID
    FROM #BatchQueue
    WHERE Processed = 0
    ORDER BY ID;
    
    -- Process record
    EXEC PROC_ProcessRecord @RecordID;
    
    UPDATE #BatchQueue
    SET Processed = 1
    WHERE RecordID = @RecordID;
END

DROP TABLE #BatchQueue;
```

### Hierarchical Data Processing
```sql
CREATE TABLE #EmployeeHierarchy (
    EmployeeID INT,
    ManagerID INT,
    Level INT,
    Processed BIT DEFAULT 0
);

-- Initial level
INSERT INTO #EmployeeHierarchy (EmployeeID, ManagerID, Level)
SELECT EmployeeID, ManagerID, 0
FROM Employees
WHERE ManagerID IS NULL;

-- Process levels
DECLARE @CurrentLevel INT = 0;

WHILE EXISTS (SELECT 1 FROM #EmployeeHierarchy WHERE Level = @CurrentLevel AND Processed = 0)
BEGIN
    -- Process current level
    UPDATE #EmployeeHierarchy
    SET Processed = 1
    WHERE Level = @CurrentLevel AND Processed = 0;
    
    -- Add next level
    INSERT INTO #EmployeeHierarchy (EmployeeID, ManagerID, Level)
    SELECT e.EmployeeID, e.ManagerID, @CurrentLevel + 1
    FROM Employees e
    INNER JOIN #EmployeeHierarchy h ON e.ManagerID = h.EmployeeID
    WHERE h.Level = @CurrentLevel
        AND NOT EXISTS (SELECT 1 FROM #EmployeeHierarchy WHERE EmployeeID = e.EmployeeID);
    
    SET @CurrentLevel = @CurrentLevel + 1;
END

DROP TABLE #EmployeeHierarchy;
```

## Related Concepts
- See `03_Queries.md` for CTEs (alternative to temp tables)
- See `10_Cursors.md` for cursor usage with temp tables
- See `01_Stored_Procedures.md` for temp tables in procedures
- See `12_Dynamic_SQL.md` for dynamic SQL with temp tables

