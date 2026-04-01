# Constraints - Database Documentation

## Overview
Constraints are rules enforced on data columns in database tables. They ensure data integrity, accuracy, and reliability by restricting the type of data that can be stored in a table. Constraints can be defined at the column level or table level.

## Key Characteristics

### 1. **Data Integrity**
- Enforce business rules at database level
- Prevent invalid data entry
- Ensure data consistency

### 2. **Automatic Enforcement**
- Checked automatically by database engine
- Cannot be bypassed by application code
- Consistent across all applications

### 3. **Performance Impact**
- Some constraints create indexes automatically
- Validation overhead on INSERT/UPDATE operations
- Generally minimal performance impact

## Types of Constraints

### 1. **PRIMARY KEY**
- Uniquely identifies each row in a table
- Cannot contain NULL values
- Automatically creates clustered unique index
- Only one primary key per table

### 2. **FOREIGN KEY**
- Ensures referential integrity
- Links data between tables
- Prevents orphaned records
- Can cascade updates/deletes

### 3. **UNIQUE**
- Ensures all values in column are unique
- Allows NULL values (only one NULL allowed)
- Creates non-clustered unique index
- Multiple unique constraints per table

### 4. **CHECK**
- Validates data against a condition
- Can reference multiple columns
- Enforces business rules
- Cannot reference other tables

### 5. **NOT NULL**
- Prevents NULL values in column
- Column-level constraint only
- Simplest form of constraint

### 6. **DEFAULT**
- Provides default value when not specified
- Applied only on INSERT
- Can use functions or constants

## PRIMARY KEY Constraint

### Column Level
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50)
);
```

### Table Level
```sql
CREATE TABLE Employees (
    EmployeeID INT,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    CONSTRAINT PK_Employees PRIMARY KEY (EmployeeID)
);
```

### Composite Primary Key
```sql
CREATE TABLE OrderItems (
    OrderID INT,
    ProductID INT,
    Quantity INT,
    CONSTRAINT PK_OrderItems PRIMARY KEY (OrderID, ProductID)
);
```

### Adding Primary Key to Existing Table
```sql
ALTER TABLE Employees
ADD CONSTRAINT PK_Employees PRIMARY KEY (EmployeeID);
```

### Dropping Primary Key
```sql
ALTER TABLE Employees
DROP CONSTRAINT PK_Employees;
```

## FOREIGN KEY Constraint

### Column Level
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    DepartmentID INT REFERENCES Departments(DepartmentID),
    FirstName VARCHAR(50)
);
```

### Table Level
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    DepartmentID INT,
    FirstName VARCHAR(50),
    CONSTRAINT FK_Employees_Departments 
        FOREIGN KEY (DepartmentID) 
        REFERENCES Departments(DepartmentID)
);
```

### Foreign Key with ON DELETE CASCADE
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    DepartmentID INT,
    FirstName VARCHAR(50),
    CONSTRAINT FK_Employees_Departments 
        FOREIGN KEY (DepartmentID) 
        REFERENCES Departments(DepartmentID)
        ON DELETE CASCADE
);
```

### Foreign Key with ON DELETE SET NULL
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    ManagerID INT,
    FirstName VARCHAR(50),
    CONSTRAINT FK_Employees_Manager 
        FOREIGN KEY (ManagerID) 
        REFERENCES Employees(EmployeeID)
        ON DELETE SET NULL
);
```

### Foreign Key with ON UPDATE CASCADE
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    DepartmentID INT,
    CONSTRAINT FK_Employees_Departments 
        FOREIGN KEY (DepartmentID) 
        REFERENCES Departments(DepartmentID)
        ON UPDATE CASCADE
);
```

### Adding Foreign Key to Existing Table
```sql
ALTER TABLE Employees
ADD CONSTRAINT FK_Employees_Departments 
    FOREIGN KEY (DepartmentID) 
    REFERENCES Departments(DepartmentID);
```

### Dropping Foreign Key
```sql
ALTER TABLE Employees
DROP CONSTRAINT FK_Employees_Departments;
```

## UNIQUE Constraint

### Column Level
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Email VARCHAR(100) UNIQUE,
    FirstName VARCHAR(50)
);
```

### Table Level
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Email VARCHAR(100),
    FirstName VARCHAR(50),
    CONSTRAINT UQ_Employees_Email UNIQUE (Email)
);
```

### Composite Unique Constraint
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    CONSTRAINT UQ_Employees_Name UNIQUE (FirstName, LastName)
);
```

### Adding Unique Constraint
```sql
ALTER TABLE Employees
ADD CONSTRAINT UQ_Employees_Email UNIQUE (Email);
```

### Dropping Unique Constraint
```sql
ALTER TABLE Employees
DROP CONSTRAINT UQ_Employees_Email;
```

## CHECK Constraint

### Column Level
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Salary DECIMAL(18,2) CHECK (Salary > 0),
    Age INT CHECK (Age >= 18 AND Age <= 65)
);
```

### Table Level
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Salary DECIMAL(18,2),
    Bonus DECIMAL(18,2),
    CONSTRAINT CK_Employees_Salary CHECK (Salary > 0),
    CONSTRAINT CK_Employees_Bonus CHECK (Bonus >= 0 AND Bonus <= Salary)
);
```

### Multi-Column CHECK Constraint
```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    OrderDate DATE,
    ShipDate DATE,
    CONSTRAINT CK_Orders_Dates CHECK (ShipDate >= OrderDate)
);
```

### Complex CHECK Constraint
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Email VARCHAR(100),
    CONSTRAINT CK_Employees_Email CHECK (
        Email LIKE '%@%.%' 
        AND LEN(Email) >= 5
        AND Email NOT LIKE '@%'
        AND Email NOT LIKE '%@'
    )
);
```

### Adding CHECK Constraint
```sql
ALTER TABLE Employees
ADD CONSTRAINT CK_Employees_Salary CHECK (Salary > 0);
```

### Dropping CHECK Constraint
```sql
ALTER TABLE Employees
DROP CONSTRAINT CK_Employees_Salary;
```

## NOT NULL Constraint

### Column Definition
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    Email VARCHAR(100) NULL
);
```

### Adding NOT NULL to Existing Column
```sql
-- First update existing NULL values
UPDATE Employees SET FirstName = '' WHERE FirstName IS NULL;

-- Then add constraint
ALTER TABLE Employees
ALTER COLUMN FirstName VARCHAR(50) NOT NULL;
```

### Removing NOT NULL
```sql
ALTER TABLE Employees
ALTER COLUMN FirstName VARCHAR(50) NULL;
```

## DEFAULT Constraint

### Column Level
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    HireDate DATE DEFAULT GETDATE(),
    Status VARCHAR(20) DEFAULT 'Active',
    CreatedDate DATETIME DEFAULT GETDATE()
);
```

### Table Level
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    HireDate DATE,
    Status VARCHAR(20),
    CONSTRAINT DF_Employees_HireDate DEFAULT GETDATE() FOR HireDate,
    CONSTRAINT DF_Employees_Status DEFAULT 'Active' FOR Status
);
```

### Default with Function
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    EmployeeCode VARCHAR(20) DEFAULT 'EMP' + CAST(NEWID() AS VARCHAR(36)),
    CreatedDate DATETIME DEFAULT GETDATE(),
    ModifiedDate DATETIME DEFAULT GETDATE()
);
```

### Adding DEFAULT Constraint
```sql
ALTER TABLE Employees
ADD CONSTRAINT DF_Employees_Status DEFAULT 'Active' FOR Status;
```

### Dropping DEFAULT Constraint
```sql
ALTER TABLE Employees
DROP CONSTRAINT DF_Employees_Status;
```

## Viewing Constraints

### List All Constraints
```sql
SELECT 
    OBJECT_NAME(parent_object_id) AS TableName,
    name AS ConstraintName,
    type_desc AS ConstraintType,
    definition
FROM sys.check_constraints
UNION ALL
SELECT 
    OBJECT_NAME(parent_object_id) AS TableName,
    name AS ConstraintName,
    'DEFAULT' AS ConstraintType,
    definition
FROM sys.default_constraints
UNION ALL
SELECT 
    OBJECT_NAME(parent_object_id) AS TableName,
    name AS ConstraintName,
    'PRIMARY_KEY' AS ConstraintType,
    NULL AS definition
FROM sys.key_constraints
WHERE type = 'PK'
UNION ALL
SELECT 
    OBJECT_NAME(parent_object_id) AS TableName,
    name AS ConstraintName,
    'FOREIGN_KEY' AS ConstraintType,
    NULL AS definition
FROM sys.foreign_keys
UNION ALL
SELECT 
    OBJECT_NAME(parent_object_id) AS TableName,
    name AS ConstraintName,
    'UNIQUE' AS ConstraintType,
    NULL AS definition
FROM sys.key_constraints
WHERE type = 'UQ';
```

### Foreign Key Details
```sql
SELECT 
    fk.name AS ForeignKeyName,
    OBJECT_NAME(fk.parent_object_id) AS TableName,
    COL_NAME(fc.parent_object_id, fc.parent_column_id) AS ColumnName,
    OBJECT_NAME(fk.referenced_object_id) AS ReferencedTable,
    COL_NAME(fc.referenced_object_id, fc.referenced_column_id) AS ReferencedColumn,
    fk.delete_referential_action_desc AS DeleteAction,
    fk.update_referential_action_desc AS UpdateAction
FROM sys.foreign_keys fk
INNER JOIN sys.foreign_key_columns fc ON fk.object_id = fc.constraint_object_id
ORDER BY TableName, ForeignKeyName;
```

## Constraint Naming Conventions

### Recommended Prefixes
- **PK_** - Primary Key
- **FK_** - Foreign Key
- **UQ_** - Unique
- **CK_** - Check
- **DF_** - Default

### Examples
```sql
CONSTRAINT PK_Employees PRIMARY KEY (EmployeeID)
CONSTRAINT FK_Employees_Departments FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
CONSTRAINT UQ_Employees_Email UNIQUE (Email)
CONSTRAINT CK_Employees_Salary CHECK (Salary > 0)
CONSTRAINT DF_Employees_Status DEFAULT 'Active' FOR Status
```

## Disabling Constraints

### Disable CHECK Constraint
```sql
ALTER TABLE Employees
NOCHECK CONSTRAINT CK_Employees_Salary;
```

### Disable All Constraints
```sql
ALTER TABLE Employees
NOCHECK CONSTRAINT ALL;
```

### Re-enable Constraint
```sql
ALTER TABLE Employees
CHECK CONSTRAINT CK_Employees_Salary;
```

### Re-enable All Constraints
```sql
ALTER TABLE Employees
CHECK CONSTRAINT ALL;
```

## Best Practices

### 1. **Primary Keys**
- Use single column when possible
- Use INT or BIGINT for surrogate keys
- Consider GUID for distributed systems
- Always define primary key

### 2. **Foreign Keys**
- Index foreign key columns
- Use appropriate CASCADE options
- Document referential integrity rules
- Consider performance impact

### 3. **CHECK Constraints**
- Enforce business rules at database level
- Keep constraints simple and fast
- Document complex constraint logic
- Test constraint behavior

### 4. **Unique Constraints**
- Use for business keys (Email, SSN, etc.)
- Consider composite unique constraints
- Remember NULL handling (only one NULL allowed)

### 5. **Default Values**
- Provide sensible defaults
- Use functions for dynamic defaults
- Document default behavior
- Consider NULL vs default

## Common Patterns

### Audit Fields
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50) NOT NULL,
    CreatedDate DATETIME DEFAULT GETDATE() NOT NULL,
    CreatedBy VARCHAR(50) DEFAULT SYSTEM_USER NOT NULL,
    ModifiedDate DATETIME DEFAULT GETDATE() NOT NULL,
    ModifiedBy VARCHAR(50) DEFAULT SYSTEM_USER NOT NULL
);
```

### Status Validation
```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    Status VARCHAR(20) DEFAULT 'Pending',
    CONSTRAINT CK_Orders_Status CHECK (
        Status IN ('Pending', 'Processing', 'Shipped', 'Delivered', 'Cancelled')
    )
);
```

### Date Range Validation
```sql
CREATE TABLE Projects (
    ProjectID INT PRIMARY KEY,
    StartDate DATE NOT NULL,
    EndDate DATE,
    CONSTRAINT CK_Projects_Dates CHECK (
        EndDate IS NULL OR EndDate >= StartDate
    )
);
```

## Related Concepts
- See `07_Indexes.md` for indexes created by constraints
- See `03_Queries.md` for querying with constraints
- See `06_Triggers.md` for additional validation logic
- See `09_Transactions.md` for constraint enforcement in transactions

