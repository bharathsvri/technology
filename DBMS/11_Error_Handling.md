# Error Handling - Database Documentation

## Overview
Error Handling in SQL Server involves detecting, managing, and responding to errors that occur during database operations. Proper error handling ensures data integrity, provides meaningful error messages, and allows for graceful recovery from exceptional situations.

## Error Handling Mechanisms

### 1. **TRY...CATCH Blocks**
- Modern error handling approach
- Catches and handles exceptions
- Allows controlled error response

### 2. **@@ERROR System Function**
- Legacy error handling
- Returns error number of last statement
- Must be checked immediately after statement

### 3. **RAISERROR Statement**
- Generates custom error messages
- Can set error severity
- Can include parameters

### 4. **THROW Statement**
- Modern replacement for RAISERROR
- Simpler syntax
- Automatically sets error information

## TRY...CATCH Blocks

### Basic Syntax
```sql
BEGIN TRY
    -- SQL statements that might cause errors
END TRY
BEGIN CATCH
    -- Error handling code
END CATCH
```

### Simple Example
```sql
BEGIN TRY
    INSERT INTO Employees (EmployeeID, FirstName, LastName)
    VALUES (1, 'John', 'Doe');
END TRY
BEGIN CATCH
    PRINT 'Error occurred: ' + ERROR_MESSAGE();
END CATCH
```

### Error Information Functions
```sql
BEGIN TRY
    -- Operations that might fail
    INSERT INTO Employees (EmployeeID, FirstName) VALUES (1, 'John');
END TRY
BEGIN CATCH
    SELECT 
        ERROR_NUMBER() AS ErrorNumber,
        ERROR_SEVERITY() AS ErrorSeverity,
        ERROR_STATE() AS ErrorState,
        ERROR_PROCEDURE() AS ErrorProcedure,
        ERROR_LINE() AS ErrorLine,
        ERROR_MESSAGE() AS ErrorMessage;
END CATCH
```

### Error Information Details
- **ERROR_NUMBER()**: Returns error number
- **ERROR_SEVERITY()**: Returns error severity (1-25)
- **ERROR_STATE()**: Returns error state number
- **ERROR_PROCEDURE()**: Returns procedure/trigger name where error occurred
- **ERROR_LINE()**: Returns line number where error occurred
- **ERROR_MESSAGE()**: Returns complete error message text

## Nested TRY...CATCH

### Nested Blocks
```sql
BEGIN TRY
    BEGIN TRY
        -- Inner operation
        INSERT INTO Table1 VALUES (1);
    END TRY
    BEGIN CATCH
        -- Handle inner error
        PRINT 'Inner error: ' + ERROR_MESSAGE();
        -- Re-throw if needed
        THROW;
    END CATCH
    
    -- Outer operation
    INSERT INTO Table2 VALUES (2);
END TRY
BEGIN CATCH
    -- Handle outer error
    PRINT 'Outer error: ' + ERROR_MESSAGE();
END CATCH
```

## Error Handling in Stored Procedures

### Procedure with Error Handling
```sql
CREATE PROCEDURE PROC_InsertEmployee
    @EmployeeID INT,
    @FirstName VARCHAR(50),
    @LastName VARCHAR(50)
AS
BEGIN
    SET NOCOUNT ON;
    
    BEGIN TRY
        BEGIN TRANSACTION;
        
        -- Validate input
        IF @FirstName IS NULL OR @LastName IS NULL
        BEGIN
            RAISERROR('First name and last name are required', 16, 1);
            RETURN;
        END
        
        -- Insert employee
        INSERT INTO Employees (EmployeeID, FirstName, LastName)
        VALUES (@EmployeeID, @FirstName, @LastName);
        
        COMMIT TRANSACTION;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;
        
        DECLARE @ErrorMessage NVARCHAR(4000) = ERROR_MESSAGE();
        DECLARE @ErrorNumber INT = ERROR_NUMBER();
        DECLARE @ErrorSeverity INT = ERROR_SEVERITY();
        DECLARE @ErrorState INT = ERROR_STATE();
        
        -- Log error
        INSERT INTO ErrorLog (ErrorNumber, ErrorMessage, ErrorProcedure, ErrorLine, ErrorDate)
        VALUES (@ErrorNumber, @ErrorMessage, ERROR_PROCEDURE(), ERROR_LINE(), GETDATE());
        
        -- Re-throw error
        RAISERROR(@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH
END
```

## RAISERROR Statement

### Basic Syntax
```sql
RAISERROR (message_string, severity, state [, arguments])
```

### Simple RAISERROR
```sql
RAISERROR('An error occurred', 16, 1);
```

### RAISERROR with Parameters
```sql
DECLARE @EmployeeID INT = 123;
RAISERROR('Employee ID %d not found', 16, 1, @EmployeeID);
```

### RAISERROR with Severity Levels
```sql
-- Information (severity 0-10)
RAISERROR('Information message', 10, 1) WITH NOWAIT;

-- User error (severity 11-16)
RAISERROR('User error occurred', 16, 1);

-- System error (severity 17-19)
RAISERROR('System error occurred', 17, 1) WITH LOG;

-- Fatal error (severity 20-25)
RAISERROR('Fatal error occurred', 20, 1) WITH LOG;
```

### RAISERROR Options
```sql
-- WITH NOWAIT: Send message immediately
RAISERROR('Processing...', 10, 1) WITH NOWAIT;

-- WITH LOG: Write to error log
RAISERROR('Error logged', 16, 1) WITH LOG;

-- WITH SETERROR: Set @@ERROR
RAISERROR('Error message', 16, 1) WITH SETERROR;
```

## THROW Statement

### Basic Syntax
```sql
THROW [error_number, message, state];
```

### Simple THROW
```sql
THROW 50000, 'An error occurred', 1;
```

### THROW with Error Information
```sql
BEGIN TRY
    -- Operation that might fail
    INSERT INTO Employees VALUES (1, 'John');
END TRY
BEGIN CATCH
    -- Re-throw with original error information
    THROW;
END CATCH
```

### THROW vs RAISERROR
- **THROW**: Simpler, modern, preserves error information
- **RAISERROR**: More options, legacy, requires error number

## @@ERROR System Function

### Legacy Error Handling
```sql
INSERT INTO Employees (EmployeeID, FirstName) VALUES (1, 'John');

IF @@ERROR <> 0
BEGIN
    PRINT 'Error occurred: ' + CAST(@@ERROR AS VARCHAR);
    ROLLBACK TRANSACTION;
    RETURN;
END
```

### Limitations
- Must check immediately after statement
- Reset to 0 after successful statement
- Doesn't capture error details
- Not recommended for new code

## Error Handling Patterns

### Transaction Rollback Pattern
```sql
BEGIN TRY
    BEGIN TRANSACTION;
    
    -- Multiple operations
    INSERT INTO Table1 VALUES (1);
    UPDATE Table2 SET Column1 = 'Value';
    DELETE FROM Table3 WHERE ID = 5;
    
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    IF @@TRANCOUNT > 0
        ROLLBACK TRANSACTION;
    
    -- Log error
    DECLARE @ErrorMsg NVARCHAR(4000) = ERROR_MESSAGE();
    PRINT 'Transaction rolled back: ' + @ErrorMsg;
    
    THROW;
END CATCH
```

### Validation Pattern
```sql
CREATE PROCEDURE PROC_ValidateAndProcess
    @EmployeeID INT,
    @Salary DECIMAL(18,2)
AS
BEGIN
    BEGIN TRY
        -- Validate input
        IF @EmployeeID IS NULL
        BEGIN
            THROW 50001, 'Employee ID is required', 1;
            RETURN;
        END
        
        IF @Salary < 0
        BEGIN
            THROW 50002, 'Salary cannot be negative', 1;
            RETURN;
        END
        
        -- Check existence
        IF NOT EXISTS (SELECT 1 FROM Employees WHERE EmployeeID = @EmployeeID)
        BEGIN
            THROW 50003, 'Employee not found', 1;
            RETURN;
        END
        
        -- Process
        UPDATE Employees SET Salary = @Salary WHERE EmployeeID = @EmployeeID;
    END TRY
    BEGIN CATCH
        DECLARE @ErrorNumber INT = ERROR_NUMBER();
        DECLARE @ErrorMessage NVARCHAR(4000) = ERROR_MESSAGE();
        
        -- Handle specific errors
        IF @ErrorNumber = 50001 OR @ErrorNumber = 50002 OR @ErrorNumber = 50003
        BEGIN
            PRINT 'Validation error: ' + @ErrorMessage;
        END
        ELSE
        BEGIN
            -- Unexpected error
            PRINT 'Unexpected error: ' + @ErrorMessage;
            THROW;
        END
    END CATCH
END
```

### Retry Pattern
```sql
DECLARE @RetryCount INT = 0;
DECLARE @MaxRetries INT = 3;
DECLARE @Success BIT = 0;

WHILE @RetryCount < @MaxRetries AND @Success = 0
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;
        
        -- Operation that might fail
        UPDATE Accounts SET Balance = Balance - 1000 WHERE AccountID = 1;
        
        COMMIT TRANSACTION;
        SET @Success = 1;
    END TRY
    BEGIN CATCH
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;
        
        SET @RetryCount = @RetryCount + 1;
        
        IF @RetryCount >= @MaxRetries
        BEGIN
            THROW;
        END
        ELSE
        BEGIN
            WAITFOR DELAY '00:00:01';  -- Wait before retry
        END
    END CATCH
END
```

### Error Logging Pattern
```sql
CREATE TABLE ErrorLog (
    ErrorLogID INT IDENTITY(1,1) PRIMARY KEY,
    ErrorNumber INT,
    ErrorSeverity INT,
    ErrorState INT,
    ErrorProcedure NVARCHAR(128),
    ErrorLine INT,
    ErrorMessage NVARCHAR(4000),
    ErrorDate DATETIME DEFAULT GETDATE(),
    UserName NVARCHAR(128) DEFAULT SYSTEM_USER
);

CREATE PROCEDURE PROC_LogError
AS
BEGIN
    INSERT INTO ErrorLog (
        ErrorNumber,
        ErrorSeverity,
        ErrorState,
        ErrorProcedure,
        ErrorLine,
        ErrorMessage
    )
    VALUES (
        ERROR_NUMBER(),
        ERROR_SEVERITY(),
        ERROR_STATE(),
        ERROR_PROCEDURE(),
        ERROR_LINE(),
        ERROR_MESSAGE()
    );
END
```

### Using Error Logging
```sql
BEGIN TRY
    -- Operations
    INSERT INTO Employees VALUES (1, 'John');
END TRY
BEGIN CATCH
    EXEC PROC_LogError;
    THROW;
END CATCH
```

## Error Severity Levels

### Severity 0-10
- Informational messages
- Not actual errors
- Execution continues

### Severity 11-16
- User errors
- Can be handled in application
- Execution can continue

### Severity 17-19
- Resource errors
- Should be logged
- May require DBA attention

### Severity 20-25
- Fatal errors
- Connection terminated
- Requires immediate attention

## Custom Error Messages

### Creating Custom Error Messages
```sql
EXEC sp_addmessage 
    @msgnum = 50001,
    @severity = 16,
    @msgtext = 'Employee ID %d already exists',
    @lang = 'us_english';
```

### Using Custom Messages
```sql
RAISERROR(50001, 16, 1, 123);
```

### Viewing Custom Messages
```sql
SELECT * FROM sys.messages WHERE message_id = 50001;
```

## Error Handling Best Practices

### 1. **Always Use TRY...CATCH**
- Modern and reliable
- Captures all error information
- Allows proper cleanup

### 2. **Handle Transactions Properly**
- Always check @@TRANCOUNT before rollback
- Rollback in CATCH block
- Commit only on success

### 3. **Provide Meaningful Messages**
- Use descriptive error messages
- Include relevant context
- Help with debugging

### 4. **Log Errors**
- Log unexpected errors
- Include error details
- Track error frequency

### 5. **Don't Swallow Errors**
- Re-throw if you can't handle
- Don't hide errors silently
- Let calling code decide

### 6. **Validate Input**
- Check parameters early
- Provide clear validation messages
- Fail fast on invalid input

### 7. **Use Appropriate Severity**
- Use correct severity levels
- Don't use fatal severity for user errors
- Reserve high severity for critical issues

## Common Error Scenarios

### Constraint Violations
```sql
BEGIN TRY
    INSERT INTO Employees (EmployeeID, Email)
    VALUES (1, 'invalid-email');  -- Might violate CHECK constraint
END TRY
BEGIN CATCH
    IF ERROR_NUMBER() = 547  -- Constraint violation
    BEGIN
        PRINT 'Constraint violation: ' + ERROR_MESSAGE();
    END
    ELSE
    BEGIN
        THROW;
    END
END CATCH
```

### Deadlock Handling
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

### Timeout Handling
```sql
BEGIN TRY
    SET LOCK_TIMEOUT 5000;  -- 5 seconds
    
    UPDATE LargeTable SET Column1 = 'Value';
END TRY
BEGIN CATCH
    IF ERROR_NUMBER() = 1222  -- Lock timeout
    BEGIN
        PRINT 'Operation timed out. Please try again.';
    END
    ELSE
    BEGIN
        THROW;
    END
END CATCH
```

## Related Concepts
- See `01_Stored_Procedures.md` for error handling in procedures
- See `06_Triggers.md` for error handling in triggers
- See `09_Transactions.md` for transaction error handling
- See `10_Cursors.md` for cursor error handling

