# Views - Database Documentation

## Overview
A View is a virtual table based on the result of a SQL statement. Views contain rows and columns like a real table, but the data is dynamically retrieved from underlying tables when the view is queried. Views provide a way to simplify complex queries, enhance security, and abstract the underlying table structure.

## Key Characteristics

### 1. **Virtual Table**
- Views don't store data physically
- Data is computed dynamically from underlying tables
- Changes to base tables are immediately reflected in views

### 2. **Simplified Access**
- Hide complex joins and calculations
- Present data in a user-friendly format
- Reduce query complexity for end users

### 3. **Security**
- Restrict access to specific columns or rows
- Hide sensitive data from users
- Provide controlled data access

### 4. **Data Abstraction**
- Abstract underlying table structure
- Allow schema changes without affecting applications
- Provide consistent interface

## Types of Views

### 1. **Simple Views**
Based on a single table with no aggregation or grouping.

### 2. **Complex Views**
Based on multiple tables with joins, aggregations, or subqueries.

### 3. **Indexed Views (Materialized Views)**
Views with a unique clustered index that physically stores data.

### 4. **Partitioned Views**
Views that combine data from multiple tables (horizontal partitioning).

## Creating Views

### Basic Syntax
```sql
CREATE VIEW [schema_name.]view_name
[WITH {ENCRYPTION | SCHEMABINDING | VIEW_METADATA}]
AS
SELECT column1, column2, ...
FROM table_name
[WHERE condition]
[WITH CHECK OPTION]
```

### Simple View Example
```sql
CREATE VIEW VW_ActiveEmployees
AS
SELECT 
    EmployeeID,
    FirstName,
    LastName,
    Email,
    DepartmentID
FROM Employees
WHERE Status = 'Active';
```

### Usage
```sql
SELECT * FROM VW_ActiveEmployees;
```

### Complex View with Joins
```sql
CREATE VIEW VW_EmployeeDepartment
AS
SELECT 
    e.EmployeeID,
    e.FirstName,
    e.LastName,
    e.Email,
    e.Salary,
    d.DepartmentName,
    d.Location
FROM Employees e
INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID
WHERE e.Status = 'Active';
```

### View with Aggregation
```sql
CREATE VIEW VW_DepartmentSummary
AS
SELECT 
    d.DepartmentID,
    d.DepartmentName,
    COUNT(e.EmployeeID) AS EmployeeCount,
    AVG(e.Salary) AS AverageSalary,
    SUM(e.Salary) AS TotalSalary
FROM Departments d
LEFT JOIN Employees e ON d.DepartmentID = e.DepartmentID
GROUP BY d.DepartmentID, d.DepartmentName;
```

## View Options

### ENCRYPTION
Encrypts the view definition.

```sql
CREATE VIEW VW_EncryptedView
WITH ENCRYPTION
AS
SELECT * FROM Employees;
```

### SCHEMABINDING
Prevents changes to underlying objects that would affect the view.

```sql
CREATE VIEW VW_SchemaBoundView
WITH SCHEMABINDING
AS
SELECT 
    EmployeeID,
    FirstName,
    LastName
FROM dbo.Employees;
```

### VIEW_METADATA
Returns view metadata instead of base table metadata.

```sql
CREATE VIEW VW_WithMetadata
WITH VIEW_METADATA
AS
SELECT * FROM Employees;
```

### WITH CHECK OPTION
Ensures data modifications through the view meet the view's WHERE clause.

```sql
CREATE VIEW VW_ActiveEmployeesOnly
AS
SELECT * FROM Employees
WHERE Status = 'Active'
WITH CHECK OPTION;
```

## Modifying Views

### ALTER VIEW
```sql
ALTER VIEW VW_ActiveEmployees
AS
SELECT 
    EmployeeID,
    FirstName,
    LastName,
    Email,
    DepartmentID,
    HireDate
FROM Employees
WHERE Status = 'Active' AND HireDate >= '2020-01-01';
```

### DROP VIEW
```sql
DROP VIEW [IF EXISTS] view_name;
```

## Updatable Views

### Conditions for Updatable Views
- Based on a single table
- No aggregation functions
- No GROUP BY or HAVING clauses
- No DISTINCT
- No computed columns (unless updatable)

### INSERT Through View
```sql
CREATE VIEW VW_SimpleEmployees
AS
SELECT EmployeeID, FirstName, LastName, Email
FROM Employees;

INSERT INTO VW_SimpleEmployees (FirstName, LastName, Email)
VALUES ('John', 'Doe', 'john.doe@company.com');
```

### UPDATE Through View
```sql
UPDATE VW_SimpleEmployees
SET Email = 'newemail@company.com'
WHERE EmployeeID = 123;
```

### DELETE Through View
```sql
DELETE FROM VW_SimpleEmployees
WHERE EmployeeID = 123;
```

## Indexed Views (Materialized Views)

### Creating Indexed View
```sql
CREATE VIEW VW_EmployeeDepartmentIndexed
WITH SCHEMABINDING
AS
SELECT 
    e.EmployeeID,
    e.DepartmentID,
    COUNT_BIG(*) AS EmployeeCount,
    SUM(e.Salary) AS TotalSalary
FROM dbo.Employees e
GROUP BY e.EmployeeID, e.DepartmentID;

-- Create unique clustered index
CREATE UNIQUE CLUSTERED INDEX IX_VW_EmployeeDepartment
ON VW_EmployeeDepartmentIndexed (EmployeeID, DepartmentID);
```

### Benefits
- Improved query performance
- Data is physically stored
- Automatic maintenance when base tables change

### Limitations
- Requires SCHEMABINDING
- Base tables must be in same database
- Certain restrictions on SELECT statement

## System Views

### INFORMATION_SCHEMA Views
```sql
-- List all views
SELECT 
    TABLE_SCHEMA,
    TABLE_NAME
FROM INFORMATION_SCHEMA.VIEWS;

-- View definition
SELECT VIEW_DEFINITION
FROM INFORMATION_SCHEMA.VIEWS
WHERE TABLE_NAME = 'VW_ActiveEmployees';
```

### System Catalog Views
```sql
-- List views
SELECT 
    s.name AS SchemaName,
    v.name AS ViewName,
    v.create_date,
    v.modify_date
FROM sys.views v
INNER JOIN sys.schemas s ON v.schema_id = s.schema_id;
```

### View Dependencies
```sql
EXEC sp_depends 'VW_EmployeeDepartment';
```

### View Definition
```sql
EXEC sp_helptext 'VW_ActiveEmployees';
```

## Common Patterns

### Security View
```sql
CREATE VIEW VW_EmployeePublicInfo
AS
SELECT 
    EmployeeID,
    FirstName,
    LastName,
    DepartmentName
FROM Employees e
INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID;
-- Excludes sensitive columns like Salary, SSN
```

### Calculated Columns View
```sql
CREATE VIEW VW_EmployeeCalculations
AS
SELECT 
    EmployeeID,
    FirstName,
    LastName,
    Salary,
    Salary * 12 AS AnnualSalary,
    CASE 
        WHEN Salary > 80000 THEN 'High'
        WHEN Salary > 50000 THEN 'Medium'
        ELSE 'Low'
    END AS SalaryCategory
FROM Employees;
```

### Filtered View
```sql
CREATE VIEW VW_CurrentYearEmployees
AS
SELECT *
FROM Employees
WHERE YEAR(HireDate) = YEAR(GETDATE());
```

### Union View
```sql
CREATE VIEW VW_AllPeople
AS
SELECT 'Employee' AS PersonType, FirstName, LastName, Email
FROM Employees
UNION ALL
SELECT 'Contractor' AS PersonType, FirstName, LastName, Email
FROM Contractors;
```

## Best Practices

1. **Naming Convention**: Use consistent prefix (e.g., VW_)
2. **Documentation**: Include comments explaining view purpose
3. **Performance**: Consider indexed views for frequently accessed data
4. **Security**: Use views to restrict column/row access
5. **Simplicity**: Keep views focused and simple
6. **Avoid Nesting**: Limit view nesting depth
7. **SCHEMABINDING**: Use when possible for stability
8. **Check Option**: Use WITH CHECK OPTION for data integrity

## Performance Considerations

1. **Query Optimization**: Views are optimized like regular queries
2. **Indexed Views**: Can significantly improve performance
3. **View Nesting**: Deep nesting can impact performance
4. **Statistics**: Keep statistics updated on base tables
5. **Execution Plans**: Analyze execution plans for complex views

## Security

### Grant Permissions
```sql
GRANT SELECT ON VW_ActiveEmployees TO username;
GRANT INSERT, UPDATE ON VW_SimpleEmployees TO username;
```

### Row-Level Security
```sql
CREATE VIEW VW_MyEmployees
AS
SELECT *
FROM Employees
WHERE ManagerID = USER_ID();
```

## Common Use Cases

### 1. **Simplifying Complex Queries**
Hide complex joins and calculations from end users.

### 2. **Data Security**
Restrict access to sensitive columns or rows.

### 3. **Backward Compatibility**
Maintain view interface when underlying tables change.

### 4. **Reporting**
Create views optimized for reporting needs.

### 5. **Data Integration**
Combine data from multiple sources.

## Limitations

1. **No Parameters**: Views cannot accept parameters (use table-valued functions)
2. **Limited Updates**: Complex views may not be updatable
3. **Performance**: Complex views can be slower than direct queries
4. **Dependencies**: Changes to base tables can break views

## Related Concepts
- See `01_Stored_Procedures.md` for Stored Procedures
- See `02_Functions.md` for User-Defined Functions (including Table-Valued Functions)
- See `03_Queries.md` for SQL Query concepts

