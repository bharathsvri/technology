# Triggers - Database Documentation

## Overview
A Trigger is a special type of stored procedure that automatically executes in response to certain events on a table or view. Triggers are used to enforce business rules, maintain data integrity, audit changes, and implement complex validation logic that cannot be handled by constraints.

## Key Characteristics

### 1. **Automatic Execution**
- Triggers fire automatically when specified events occur
- Cannot be called directly like stored procedures
- Execute as part of the transaction that caused the trigger

### 2. **Event-Driven**
- Respond to INSERT, UPDATE, DELETE operations
- Can fire BEFORE or AFTER the operation (depending on database system)
- SQL Server supports AFTER and INSTEAD OF triggers

### 3. **Transaction Scope**
- Execute within the same transaction as the triggering statement
- Can roll back the entire transaction if needed
- Changes are atomic with the triggering operation

## Types of Triggers

### 1. **AFTER Triggers (FOR Triggers)**
- Fire after the triggering action completes
- Most common type in SQL Server
- Can access the final state of data

### 2. **INSTEAD OF Triggers**
- Fire instead of the triggering action
- Can be used on views to make them updatable
- Replaces the original operation

### 3. **DML Triggers**
- Respond to Data Manipulation Language events (INSERT, UPDATE, DELETE)

### 4. **DDL Triggers**
- Respond to Data Definition Language events (CREATE, ALTER, DROP)

## AFTER Triggers

### Basic Syntax
```sql
CREATE TRIGGER trigger_name
ON table_name
AFTER {INSERT | UPDATE | DELETE}
AS
BEGIN
    -- Trigger logic
END
```

### INSERT Trigger Example
```sql
CREATE TRIGGER TRG_Employees_Insert
ON Employees
AFTER INSERT
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Log the insert
    INSERT INTO EmployeeAudit (EmployeeID, Action, ActionDate, UserName)
    SELECT 
        EmployeeID,
        'INSERT',
        GETDATE(),
        SYSTEM_USER
    FROM inserted;
END
```

### UPDATE Trigger Example
```sql
CREATE TRIGGER TRG_Employees_Update
ON Employees
AFTER UPDATE
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Log changes
    INSERT INTO EmployeeAudit (EmployeeID, Action, OldValue, NewValue, ActionDate)
    SELECT 
        i.EmployeeID,
        'UPDATE',
        d.Salary AS OldSalary,
        i.Salary AS NewSalary,
        GETDATE()
    FROM inserted i
    INNER JOIN deleted d ON i.EmployeeID = d.EmployeeID
    WHERE i.Salary != d.Salary;
END
```

### DELETE Trigger Example
```sql
CREATE TRIGGER TRG_Employees_Delete
ON Employees
AFTER DELETE
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Archive deleted records
    INSERT INTO EmployeesArchive
    SELECT *, GETDATE() AS DeletedDate, SYSTEM_USER AS DeletedBy
    FROM deleted;
END
```

## INSERTED and DELETED Tables

### INSERTED Table
- Contains new rows for INSERT and UPDATE operations
- Available in INSERT and UPDATE triggers
- Represents the new state of data

### DELETED Table
- Contains old rows for DELETE and UPDATE operations
- Available in DELETE and UPDATE triggers
- Represents the old state of data

### Example: Tracking All Changes
```sql
CREATE TRIGGER TRG_Employees_AllChanges
ON Employees
AFTER INSERT, UPDATE, DELETE
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Handle INSERT
    IF EXISTS (SELECT * FROM inserted) AND NOT EXISTS (SELECT * FROM deleted)
    BEGIN
        INSERT INTO ChangeLog (TableName, Action, RecordID, ChangeDate)
        SELECT 'Employees', 'INSERT', EmployeeID, GETDATE()
        FROM inserted;
    END
    
    -- Handle UPDATE
    IF EXISTS (SELECT * FROM inserted) AND EXISTS (SELECT * FROM deleted)
    BEGIN
        INSERT INTO ChangeLog (TableName, Action, RecordID, ChangeDate)
        SELECT 'Employees', 'UPDATE', EmployeeID, GETDATE()
        FROM inserted;
    END
    
    -- Handle DELETE
    IF EXISTS (SELECT * FROM deleted) AND NOT EXISTS (SELECT * FROM inserted)
    BEGIN
        INSERT INTO ChangeLog (TableName, Action, RecordID, ChangeDate)
        SELECT 'Employees', 'DELETE', EmployeeID, GETDATE()
        FROM deleted;
    END
END
```

## INSTEAD OF Triggers

### Syntax
```sql
CREATE TRIGGER trigger_name
ON {table_name | view_name}
INSTEAD OF {INSERT | UPDATE | DELETE}
AS
BEGIN
    -- Trigger logic that replaces the operation
END
```

### Example: Making View Updatable
```sql
CREATE VIEW VW_EmployeeDepartment
AS
SELECT 
    e.EmployeeID,
    e.FirstName,
    e.LastName,
    d.DepartmentName
FROM Employees e
INNER JOIN Departments d ON e.DepartmentID = d.DepartmentID;

-- Create INSTEAD OF trigger to allow updates
CREATE TRIGGER TRG_VW_EmployeeDepartment_Update
ON VW_EmployeeDepartment
INSTEAD OF UPDATE
AS
BEGIN
    SET NOCOUNT ON;
    
    UPDATE e
    SET 
        e.FirstName = i.FirstName,
        e.LastName = i.LastName
    FROM Employees e
    INNER JOIN inserted i ON e.EmployeeID = i.EmployeeID;
    
    -- Handle department name change by updating DepartmentID
    UPDATE e
    SET e.DepartmentID = d.DepartmentID
    FROM Employees e
    INNER JOIN inserted i ON e.EmployeeID = i.EmployeeID
    INNER JOIN Departments d ON d.DepartmentName = i.DepartmentName;
END
```

## DDL Triggers

### Syntax
```sql
CREATE TRIGGER trigger_name
ON {DATABASE | ALL SERVER}
AFTER {CREATE_TABLE | ALTER_TABLE | DROP_TABLE | ...}
AS
BEGIN
    -- Trigger logic
END
```

### Database-Level DDL Trigger
```sql
CREATE TRIGGER TRG_Database_DDL
ON DATABASE
AFTER CREATE_TABLE, ALTER_TABLE, DROP_TABLE
AS
BEGIN
    SET NOCOUNT ON;
    
    DECLARE @EventData XML = EVENTDATA();
    
    INSERT INTO DDLChangeLog (EventType, ObjectName, EventDate, UserName, EventData)
    VALUES (
        @EventData.value('(/EVENT_INSTANCE/EventType)[1]', 'NVARCHAR(100)'),
        @EventData.value('(/EVENT_INSTANCE/ObjectName)[1]', 'NVARCHAR(100)'),
        GETDATE(),
        SYSTEM_USER,
        @EventData
    );
END
```

### Server-Level DDL Trigger
```sql
CREATE TRIGGER TRG_Server_DDL
ON ALL SERVER
AFTER CREATE_DATABASE, ALTER_DATABASE, DROP_DATABASE
AS
BEGIN
    SET NOCOUNT ON;
    
    DECLARE @EventData XML = EVENTDATA();
    
    INSERT INTO ServerDDLLog (EventType, DatabaseName, EventDate, LoginName)
    VALUES (
        @EventData.value('(/EVENT_INSTANCE/EventType)[1]', 'NVARCHAR(100)'),
        @EventData.value('(/EVENT_INSTANCE/DatabaseName)[1]', 'NVARCHAR(100)'),
        GETDATE(),
        SYSTEM_USER
    );
END
```

## Common Patterns

### Audit Trail
```sql
CREATE TRIGGER TRG_AuditEmployees
ON Employees
AFTER INSERT, UPDATE, DELETE
AS
BEGIN
    SET NOCOUNT ON;
    
    DECLARE @Action VARCHAR(10);
    
    IF EXISTS (SELECT * FROM inserted) AND NOT EXISTS (SELECT * FROM deleted)
        SET @Action = 'INSERT';
    ELSE IF EXISTS (SELECT * FROM inserted) AND EXISTS (SELECT * FROM deleted)
        SET @Action = 'UPDATE';
    ELSE
        SET @Action = 'DELETE';
    
    INSERT INTO AuditLog (TableName, Action, RecordID, OldValues, NewValues, ChangedBy, ChangedDate)
    SELECT 
        'Employees',
        @Action,
        ISNULL(i.EmployeeID, d.EmployeeID),
        (SELECT d.* FOR JSON PATH) AS OldValues,
        (SELECT i.* FOR JSON PATH) AS NewValues,
        SYSTEM_USER,
        GETDATE()
    FROM inserted i
    FULL OUTER JOIN deleted d ON i.EmployeeID = d.EmployeeID;
END
```

### Enforcing Business Rules
```sql
CREATE TRIGGER TRG_Employees_SalaryValidation
ON Employees
AFTER INSERT, UPDATE
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Prevent salary decrease
    IF EXISTS (
        SELECT 1
        FROM inserted i
        INNER JOIN deleted d ON i.EmployeeID = d.EmployeeID
        WHERE i.Salary < d.Salary
    )
    BEGIN
        ROLLBACK TRANSACTION;
        RAISERROR('Salary cannot be decreased', 16, 1);
        RETURN;
    END
    
    -- Ensure salary is within department range
    IF EXISTS (
        SELECT 1
        FROM inserted i
        INNER JOIN Departments d ON i.DepartmentID = d.DepartmentID
        WHERE i.Salary < d.MinSalary OR i.Salary > d.MaxSalary
    )
    BEGIN
        ROLLBACK TRANSACTION;
        RAISERROR('Salary is outside department range', 16, 1);
        RETURN;
    END
END
```

### Maintaining Denormalized Data
```sql
CREATE TRIGGER TRG_Employees_UpdateDepartmentCount
ON Employees
AFTER INSERT, UPDATE, DELETE
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Update employee count in Departments table
    UPDATE d
    SET EmployeeCount = (
        SELECT COUNT(*)
        FROM Employees e
        WHERE e.DepartmentID = d.DepartmentID
    )
    FROM Departments d
    WHERE d.DepartmentID IN (
        SELECT DISTINCT DepartmentID FROM inserted
        UNION
        SELECT DISTINCT DepartmentID FROM deleted
    );
END
```

### Cascading Updates
```sql
CREATE TRIGGER TRG_Departments_UpdateEmployees
ON Departments
AFTER UPDATE
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Update employee department names when department name changes
    IF UPDATE(DepartmentName)
    BEGIN
        UPDATE e
        SET e.DepartmentName = i.DepartmentName
        FROM Employees e
        INNER JOIN inserted i ON e.DepartmentID = i.DepartmentID
        INNER JOIN deleted d ON i.DepartmentID = d.DepartmentID
        WHERE i.DepartmentName != d.DepartmentName;
    END
END
```

## Best Practices

1. **Keep Triggers Simple**: Complex logic should be in stored procedures
2. **Set NOCOUNT ON**: Reduces network traffic
3. **Handle Multiple Rows**: Always assume multiple rows in inserted/deleted
4. **Error Handling**: Use TRY...CATCH blocks
5. **Transaction Management**: Be careful with ROLLBACK
6. **Performance**: Keep triggers fast; avoid cursors and loops
7. **Documentation**: Document trigger purpose and behavior
8. **Naming Convention**: Use consistent naming (e.g., TRG_ prefix)
9. **Avoid Recursion**: Be careful of trigger cascades
10. **Test Thoroughly**: Test with single and multiple row operations

## Performance Considerations

1. **Minimize Logic**: Keep trigger logic minimal
2. **Index Usage**: Ensure proper indexes on tables used in triggers
3. **Avoid Cursors**: Use set-based operations
4. **Batch Operations**: Consider impact on bulk operations
5. **Nested Triggers**: Control nested trigger execution
6. **Recursive Triggers**: Be aware of recursive trigger settings

## Disabling and Enabling Triggers

### Disable Trigger
```sql
DISABLE TRIGGER trigger_name ON table_name;
DISABLE TRIGGER ALL ON table_name;  -- Disable all triggers on table
```

### Enable Trigger
```sql
ENABLE TRIGGER trigger_name ON table_name;
ENABLE TRIGGER ALL ON table_name;  -- Enable all triggers on table
```

### Check Trigger Status
```sql
SELECT 
    name,
    is_disabled,
    is_instead_of_trigger
FROM sys.triggers
WHERE parent_id = OBJECT_ID('Employees');
```

## Modifying and Dropping Triggers

### ALTER TRIGGER
```sql
ALTER TRIGGER trigger_name
ON table_name
AFTER INSERT
AS
BEGIN
    -- Updated trigger logic
END
```

### DROP TRIGGER
```sql
DROP TRIGGER [IF EXISTS] trigger_name ON table_name;
```

## Viewing Triggers

### List All Triggers
```sql
SELECT 
    t.name AS TriggerName,
    OBJECT_NAME(t.parent_id) AS TableName,
    t.is_disabled,
    t.is_instead_of_trigger,
    t.create_date,
    t.modify_date
FROM sys.triggers t
WHERE t.parent_class = 1;  -- DML triggers
```

### View Trigger Definition
```sql
EXEC sp_helptext 'TRG_Employees_Insert';
```

### View Trigger Dependencies
```sql
EXEC sp_depends 'TRG_Employees_Insert';
```

## Security

### Grant Permissions
Users need appropriate permissions on the base table to cause triggers to fire. Triggers execute with the permissions of the user who caused the trigger to fire, not the trigger owner.

## Common Pitfalls

1. **Assuming Single Row**: Always handle multiple rows
2. **Infinite Loops**: Avoid recursive trigger calls
3. **Performance Impact**: Triggers execute for every row
4. **Hidden Logic**: Triggers can make debugging difficult
5. **Transaction Rollback**: Rolling back can affect entire transaction

## Related Concepts
- See `01_Stored_Procedures.md` for Stored Procedures
- See `02_Functions.md` for User-Defined Functions
- See `03_Queries.md` for SQL Query concepts
- See `05_Views.md` for Views (INSTEAD OF triggers)

