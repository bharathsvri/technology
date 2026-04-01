# Transactions - Database Documentation

## Overview
A Transaction is a sequence of database operations that are treated as a single unit of work. Transactions ensure data integrity by following the ACID properties: Atomicity, Consistency, Isolation, and Durability. All operations in a transaction either succeed together or fail together.

## ACID Properties

### 1. **Atomicity**
- All operations in a transaction complete successfully, or none do
- If any operation fails, the entire transaction is rolled back
- No partial updates are allowed

### 2. **Consistency**
- Database remains in a consistent state before and after transaction
- All constraints and rules are enforced
- Data integrity is maintained

### 3. **Isolation**
- Concurrent transactions don't interfere with each other
- Each transaction sees a consistent snapshot of data
- Isolation levels control visibility of uncommitted changes

### 4. **Durability**
- Committed changes are permanent
- Survives system failures
- Written to persistent storage

## Transaction Syntax

### Basic Transaction
```sql
BEGIN TRANSACTION;

-- SQL statements
INSERT INTO Employees (FirstName, LastName) VALUES ('John', 'Doe');
UPDATE Departments SET EmployeeCount = EmployeeCount + 1 WHERE DepartmentID = 5;

-- Commit or Rollback
COMMIT TRANSACTION;
-- OR
ROLLBACK TRANSACTION;
```

### Named Transaction
```sql
BEGIN TRANSACTION TransferFunds;

UPDATE Accounts SET Balance = Balance - 1000 WHERE AccountID = 1;
UPDATE Accounts SET Balance = Balance + 1000 WHERE AccountID = 2;

COMMIT TRANSACTION TransferFunds;
```

### Transaction with Error Handling
```sql
BEGIN TRY
    BEGIN TRANSACTION;
    
    UPDATE Accounts SET Balance = Balance - 1000 WHERE AccountID = 1;
    UPDATE Accounts SET Balance = Balance + 1000 WHERE AccountID = 2;
    
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    IF @@TRANCOUNT > 0
        ROLLBACK TRANSACTION;
    
    THROW;
END CATCH
```

## Transaction States

### 1. **Active**
- Transaction is executing
- Operations are being performed
- Not yet committed or rolled back

### 2. **Committed**
- All operations completed successfully
- Changes are permanent
- Transaction is complete

### 3. **Rolled Back**
- Transaction failed or was cancelled
- All changes are undone
- Database returns to previous state

### 4. **Aborted**
- Transaction encountered an error
- Automatic rollback occurs
- System handles cleanup

## Isolation Levels

### READ UNCOMMITTED
- Lowest isolation level
- Allows dirty reads
- No locks acquired
- Fastest but least safe

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SELECT * FROM Employees;
```

### READ COMMITTED (Default)
- Prevents dirty reads
- Allows non-repeatable reads
- Shared locks held until read completes
- Most common isolation level

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
SELECT * FROM Employees;
```

### REPEATABLE READ
- Prevents dirty reads and non-repeatable reads
- Allows phantom reads
- Locks held for duration of transaction
- More restrictive than READ COMMITTED

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SELECT * FROM Employees;
```

### SERIALIZABLE
- Highest isolation level
- Prevents all concurrency issues
- Most restrictive
- Can cause deadlocks

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
SELECT * FROM Employees;
```

### SNAPSHOT
- Uses row versioning
- No blocking reads
- Consistent snapshot for transaction
- Requires READ_COMMITTED_SNAPSHOT or ALLOW_SNAPSHOT_ISOLATION

```sql
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;
SELECT * FROM Employees;
```

## Transaction Patterns

### Explicit Transaction
```sql
BEGIN TRANSACTION;

-- Multiple operations
INSERT INTO Orders (OrderDate, CustomerID) VALUES (GETDATE(), 123);
DECLARE @OrderID INT = SCOPE_IDENTITY();
INSERT INTO OrderItems (OrderID, ProductID, Quantity) VALUES (@OrderID, 456, 2);

COMMIT TRANSACTION;
```

### Nested Transactions
```sql
BEGIN TRANSACTION OuterTran;

    BEGIN TRANSACTION InnerTran;
    INSERT INTO Table1 VALUES (1);
    COMMIT TRANSACTION InnerTran;

    BEGIN TRANSACTION InnerTran2;
    INSERT INTO Table2 VALUES (2);
    COMMIT TRANSACTION InnerTran2;

COMMIT TRANSACTION OuterTran;
```

**Note:** SQL Server doesn't support true nested transactions. Inner commits don't actually commit until outer transaction commits.

### Savepoints
```sql
BEGIN TRANSACTION;

INSERT INTO Table1 VALUES (1);
SAVE TRANSACTION SavePoint1;

INSERT INTO Table2 VALUES (2);
SAVE TRANSACTION SavePoint2;

INSERT INTO Table3 VALUES (3);

-- Rollback to savepoint
ROLLBACK TRANSACTION SavePoint2;
-- Table3 insert is rolled back, Table1 and Table2 remain

COMMIT TRANSACTION;
```

## Common Patterns

### Transfer Funds Pattern
```sql
BEGIN TRY
    BEGIN TRANSACTION;
    
    -- Validate source account
    IF NOT EXISTS (SELECT 1 FROM Accounts WHERE AccountID = @FromAccount AND Balance >= @Amount)
    BEGIN
        RAISERROR('Insufficient funds', 16, 1);
        RETURN;
    END
    
    -- Debit source account
    UPDATE Accounts 
    SET Balance = Balance - @Amount 
    WHERE AccountID = @FromAccount;
    
    -- Credit destination account
    UPDATE Accounts 
    SET Balance = Balance + @Amount 
    WHERE AccountID = @ToAccount;
    
    -- Log transaction
    INSERT INTO TransactionLog (FromAccount, ToAccount, Amount, TransactionDate)
    VALUES (@FromAccount, @ToAccount, @Amount, GETDATE());
    
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    IF @@TRANCOUNT > 0
        ROLLBACK TRANSACTION;
    
    DECLARE @ErrorMessage NVARCHAR(4000) = ERROR_MESSAGE();
    RAISERROR(@ErrorMessage, 16, 1);
END CATCH
```

### Batch Processing Pattern
```sql
BEGIN TRANSACTION;

DECLARE @Processed INT = 0;
DECLARE @ErrorCount INT = 0;

WHILE EXISTS (SELECT 1 FROM PendingOrders WHERE Status = 'Pending')
BEGIN
    BEGIN TRY
        DECLARE @OrderID INT;
        SELECT TOP 1 @OrderID = OrderID FROM PendingOrders WHERE Status = 'Pending';
        
        -- Process order
        EXEC PROC_ProcessOrder @OrderID;
        
        UPDATE PendingOrders SET Status = 'Processed' WHERE OrderID = @OrderID;
        SET @Processed = @Processed + 1;
    END TRY
    BEGIN CATCH
        SET @ErrorCount = @ErrorCount + 1;
        UPDATE PendingOrders SET Status = 'Error', ErrorMessage = ERROR_MESSAGE() WHERE OrderID = @OrderID;
    END CATCH
END

COMMIT TRANSACTION;

SELECT @Processed AS ProcessedCount, @ErrorCount AS ErrorCount;
```

## Transaction Control

### Check Transaction Count
```sql
SELECT @@TRANCOUNT AS TransactionCount;
```

### Transaction Status
```sql
-- Check if in transaction
IF @@TRANCOUNT > 0
BEGIN
    PRINT 'In transaction';
END
```

### Implicit Transactions
```sql
SET IMPLICIT_TRANSACTIONS ON;

-- Each statement starts a transaction
INSERT INTO Table1 VALUES (1);
-- Must explicitly commit or rollback

COMMIT;
SET IMPLICIT_TRANSACTIONS OFF;
```

## Distributed Transactions

### Begin Distributed Transaction
```sql
BEGIN DISTRIBUTED TRANSACTION;

-- Operations on multiple servers
UPDATE Server1.Database1.dbo.Table1 SET Column1 = 'Value';
UPDATE Server2.Database2.dbo.Table2 SET Column2 = 'Value';

COMMIT TRANSACTION;
```

## Best Practices

### 1. **Keep Transactions Short**
- Minimize lock duration
- Reduce deadlock probability
- Improve concurrency

### 2. **Error Handling**
- Always use TRY...CATCH
- Check @@TRANCOUNT before rollback
- Log errors appropriately

### 3. **Isolation Levels**
- Use lowest isolation level needed
- Understand concurrency implications
- Test with concurrent users

### 4. **Avoid Long-Running Transactions**
- Don't include user input in transaction
- Process in batches
- Use savepoints when appropriate

### 5. **Transaction Naming**
- Use descriptive names for debugging
- Helpful in error messages
- Easier to identify in logs

### 6. **Deadlock Prevention**
- Access tables in same order
- Keep transactions short
- Use appropriate isolation levels
- Consider deadlock priority

## Deadlocks

### Understanding Deadlocks
- Two transactions waiting for each other's locks
- SQL Server automatically detects and resolves
- One transaction is chosen as victim and rolled back

### Deadlock Prevention
```sql
-- Set deadlock priority
SET DEADLOCK_PRIORITY LOW;  -- More likely to be chosen as victim
SET DEADLOCK_PRIORITY NORMAL;
SET DEADLOCK_PRIORITY HIGH;  -- Less likely to be chosen as victim
```

### Deadlock Handling
```sql
BEGIN TRY
    BEGIN TRANSACTION;
    
    -- Operations that might cause deadlock
    
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    IF @@TRANCOUNT > 0
        ROLLBACK TRANSACTION;
    
    IF ERROR_NUMBER() = 1205  -- Deadlock victim
    BEGIN
        -- Retry logic
        WAITFOR DELAY '00:00:01';
        -- Retry transaction
    END
    ELSE
    BEGIN
        THROW;
    END
END CATCH
```

## Monitoring Transactions

### Active Transactions
```sql
SELECT 
    session_id,
    transaction_id,
    name AS TransactionName,
    transaction_begin_time,
    transaction_type,
    transaction_state
FROM sys.dm_tran_active_transactions t
INNER JOIN sys.dm_tran_session_transactions st ON t.transaction_id = st.transaction_id;
```

### Transaction Locks
```sql
SELECT 
    request_session_id,
    resource_type,
    resource_database_id,
    resource_associated_entity_id,
    request_mode,
    request_status
FROM sys.dm_tran_locks
WHERE request_session_id = @@SPID;
```

## Related Concepts
- See `01_Stored_Procedures.md` for transaction usage in procedures
- See `06_Triggers.md` for transaction scope in triggers
- See `10_Cursors.md` for cursor transactions
- See `11_Error_Handling.md` for error handling in transactions

