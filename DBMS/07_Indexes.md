# Indexes - Database Documentation

## Overview
An Index is a database structure that improves the speed of data retrieval operations on a database table. Indexes are created on columns to allow faster searching, sorting, and filtering. While indexes speed up SELECT queries, they can slow down INSERT, UPDATE, and DELETE operations because the index must be maintained.

## Key Characteristics

### 1. **Performance Enhancement**
- Dramatically speeds up data retrieval
- Reduces the need for full table scans
- Enables efficient sorting and filtering

### 2. **Storage Overhead**
- Requires additional storage space
- Must be maintained during data modifications
- Can impact write performance

### 3. **Automatic Usage**
- Query optimizer automatically uses indexes
- No code changes required to benefit from indexes
- Optimizer chooses best index for each query

## Types of Indexes

### 1. **Clustered Index**
- Determines the physical order of data in a table
- Only one clustered index per table
- Automatically created for PRIMARY KEY (unless non-clustered specified)
- Data is stored in sorted order based on index key

### 2. **Non-Clustered Index**
- Separate structure from table data
- Contains pointers to actual data rows
- Multiple non-clustered indexes allowed per table
- Faster for lookups but requires additional I/O

### 3. **Unique Index**
- Ensures uniqueness of indexed column(s)
- Automatically created for UNIQUE constraints
- Can be clustered or non-clustered

### 4. **Composite Index**
- Index on multiple columns
- Order of columns matters
- Useful for queries filtering on multiple columns

### 5. **Covering Index**
- Contains all columns needed for a query
- Eliminates need to access base table
- Improves query performance significantly

### 6. **Filtered Index**
- Index with WHERE clause
- Only indexes subset of rows
- Reduces index size and maintenance overhead

## Creating Indexes

### Basic Syntax
```sql
CREATE [UNIQUE] [CLUSTERED | NONCLUSTERED] INDEX index_name
ON table_name (column1 [ASC | DESC], column2 [ASC | DESC], ...)
[INCLUDE (column3, column4, ...)]
[WHERE condition]
[WITH (index_options)]
```

### Clustered Index
```sql
CREATE CLUSTERED INDEX IX_Employees_EmployeeID
ON Employees (EmployeeID);
```

### Non-Clustered Index
```sql
CREATE NONCLUSTERED INDEX IX_Employees_LastName
ON Employees (LastName);
```

### Composite Index
```sql
CREATE NONCLUSTERED INDEX IX_Employees_Department_Salary
ON Employees (DepartmentID, Salary DESC);
```

### Unique Index
```sql
CREATE UNIQUE NONCLUSTERED INDEX IX_Employees_Email
ON Employees (Email);
```

### Covering Index with INCLUDE
```sql
CREATE NONCLUSTERED INDEX IX_Employees_Department_Covering
ON Employees (DepartmentID)
INCLUDE (FirstName, LastName, Email, Salary);
```

### Filtered Index
```sql
CREATE NONCLUSTERED INDEX IX_Employees_ActiveHighSalary
ON Employees (DepartmentID, Salary)
WHERE Status = 'Active' AND Salary > 50000;
```

## Index Options

### FILLFACTOR
Controls how full index pages are when created.

```sql
CREATE NONCLUSTERED INDEX IX_Employees_LastName
ON Employees (LastName)
WITH (FILLFACTOR = 80);
```

### PAD_INDEX
Applies FILLFACTOR to intermediate index pages.

```sql
CREATE NONCLUSTERED INDEX IX_Employees_LastName
ON Employees (LastName)
WITH (FILLFACTOR = 80, PAD_INDEX = ON);
```

### SORT_IN_TEMPDB
Sorts index in tempdb during creation.

```sql
CREATE NONCLUSTERED INDEX IX_Employees_LastName
ON Employees (LastName)
WITH (SORT_IN_TEMPDB = ON);
```

### ONLINE
Allows index operations while table is in use.

```sql
CREATE NONCLUSTERED INDEX IX_Employees_LastName
ON Employees (LastName)
WITH (ONLINE = ON);
```

## Index Maintenance

### Rebuild Index
```sql
ALTER INDEX IX_Employees_LastName ON Employees
REBUILD;
```

### Rebuild All Indexes on Table
```sql
ALTER INDEX ALL ON Employees
REBUILD;
```

### Reorganize Index
```sql
ALTER INDEX IX_Employees_LastName ON Employees
REORGANIZE;
```

### Update Statistics
```sql
UPDATE STATISTICS Employees;
UPDATE STATISTICS Employees IX_Employees_LastName;
```

## Viewing Indexes

### List All Indexes on Table
```sql
SELECT 
    i.name AS IndexName,
    i.type_desc AS IndexType,
    i.is_unique AS IsUnique,
    i.is_primary_key AS IsPrimaryKey,
    COL_NAME(ic.object_id, ic.column_id) AS ColumnName,
    ic.is_included_column AS IsIncluded
FROM sys.indexes i
INNER JOIN sys.index_columns ic ON i.object_id = ic.object_id AND i.index_id = ic.index_id
WHERE i.object_id = OBJECT_ID('Employees')
ORDER BY i.index_id, ic.key_ordinal;
```

### Index Fragmentation
```sql
SELECT 
    OBJECT_NAME(ips.object_id) AS TableName,
    i.name AS IndexName,
    ips.avg_fragmentation_in_percent,
    ips.page_count,
    ips.avg_page_space_used_in_percent
FROM sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, 'DETAILED') ips
INNER JOIN sys.indexes i ON ips.object_id = i.object_id AND ips.index_id = i.index_id
WHERE ips.avg_fragmentation_in_percent > 10
ORDER BY ips.avg_fragmentation_in_percent DESC;
```

### Index Usage Statistics
```sql
SELECT 
    OBJECT_NAME(s.object_id) AS TableName,
    i.name AS IndexName,
    s.user_seeks,
    s.user_scans,
    s.user_lookups,
    s.user_updates,
    s.last_user_seek,
    s.last_user_scan
FROM sys.dm_db_index_usage_stats s
INNER JOIN sys.indexes i ON s.object_id = i.object_id AND s.index_id = i.index_id
WHERE s.database_id = DB_ID()
    AND OBJECT_NAME(s.object_id) = 'Employees'
ORDER BY s.user_seeks + s.user_scans + s.user_lookups DESC;
```

## Dropping Indexes

### Drop Index
```sql
DROP INDEX IX_Employees_LastName ON Employees;
```

### Drop Multiple Indexes
```sql
DROP INDEX 
    IX_Employees_LastName,
    IX_Employees_Department
ON Employees;
```

## Best Practices

### 1. **Index Selection**
- Index columns frequently used in WHERE clauses
- Index columns used in JOIN conditions
- Index columns used in ORDER BY clauses
- Consider composite indexes for multiple column filters

### 2. **Index Design**
- Keep index key columns narrow
- Place most selective columns first in composite indexes
- Use INCLUDE for covering indexes
- Consider filtered indexes for subset queries

### 3. **Avoid Over-Indexing**
- Too many indexes slow down INSERT/UPDATE/DELETE
- Monitor index usage and remove unused indexes
- Balance between read and write performance

### 4. **Maintenance**
- Regularly rebuild or reorganize fragmented indexes
- Update statistics regularly
- Monitor index usage statistics
- Remove duplicate or overlapping indexes

### 5. **Column Selection**
- Avoid indexing columns with many NULL values
- Avoid indexing columns with low selectivity
- Consider data types (wider columns = larger indexes)

## Common Patterns

### Primary Key Index
```sql
-- Automatically creates clustered unique index
ALTER TABLE Employees
ADD CONSTRAINT PK_Employees PRIMARY KEY (EmployeeID);
```

### Foreign Key Index
```sql
-- Create index on foreign key for join performance
CREATE NONCLUSTERED INDEX IX_Employees_DepartmentID
ON Employees (DepartmentID);
```

### Search Index
```sql
-- Index for common search patterns
CREATE NONCLUSTERED INDEX IX_Employees_Search
ON Employees (LastName, FirstName)
INCLUDE (Email, Phone);
```

### Date Range Index
```sql
-- Index for date range queries
CREATE NONCLUSTERED INDEX IX_Employees_HireDate
ON Employees (HireDate)
INCLUDE (EmployeeID, FirstName, LastName);
```

## Performance Considerations

### Index Seek vs Index Scan
- **Index Seek**: Direct lookup using index (faster)
- **Index Scan**: Reading entire index (slower but sometimes necessary)

### Index Selectivity
- High selectivity = many unique values = better index
- Low selectivity = few unique values = less useful index

### Index Fragmentation
- Fragmentation occurs over time with data modifications
- Rebuild for fragmentation > 30%
- Reorganize for fragmentation 10-30%

### Missing Indexes
```sql
SELECT 
    migs.avg_total_user_cost * (migs.avg_user_impact / 100.0) * (migs.user_seeks + migs.user_scans) AS improvement_measure,
    'CREATE INDEX [missing_index_' + CONVERT(VARCHAR, mig.index_group_handle) + '_' + CONVERT(VARCHAR, mid.index_handle) + '_' + LEFT(PARSENAME(mid.statement, 1), 32) + ']'
    + ' ON ' + mid.statement
    + ' (' + ISNULL(mid.equality_columns, '')
    + CASE WHEN mid.equality_columns IS NOT NULL AND mid.inequality_columns IS NOT NULL THEN ',' ELSE '' END
    + ISNULL(mid.inequality_columns, '')
    + ')'
    + ISNULL(' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
    migs.*,
    mid.database_id,
    mid.[object_id]
FROM sys.dm_db_missing_index_groups mig
INNER JOIN sys.dm_db_missing_index_group_stats migs ON migs.group_handle = mig.index_group_handle
INNER JOIN sys.dm_db_missing_index_details mid ON mig.index_handle = mid.index_handle
WHERE migs.avg_total_user_cost * (migs.avg_user_impact / 100.0) * (migs.user_seeks + migs.user_scans) > 10
ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC;
```

## Indexed Views

### Creating Indexed View
```sql
CREATE VIEW VW_EmployeeDepartmentSummary
WITH SCHEMABINDING
AS
SELECT 
    DepartmentID,
    COUNT_BIG(*) AS EmployeeCount,
    SUM(Salary) AS TotalSalary
FROM dbo.Employees
GROUP BY DepartmentID;

-- Create unique clustered index
CREATE UNIQUE CLUSTERED INDEX IX_VW_EmployeeDepartmentSummary
ON VW_EmployeeDepartmentSummary (DepartmentID);
```

## Full-Text Indexes

### Creating Full-Text Index
```sql
CREATE FULLTEXT CATALOG ftCatalog AS DEFAULT;

CREATE FULLTEXT INDEX ON Employees (FirstName, LastName, Email)
KEY INDEX PK_Employees
ON ftCatalog;
```

### Full-Text Search
```sql
SELECT * FROM Employees
WHERE CONTAINS((FirstName, LastName), 'John');
```

## Related Concepts
- See `03_Queries.md` for query optimization
- See `01_Stored_Procedures.md` for procedure performance
- See `05_Views.md` for indexed views
- See `08_Constraints.md` for constraint-related indexes

