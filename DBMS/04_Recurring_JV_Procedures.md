# Recurring Journal Voucher (JV) Stored Procedures - Documentation

## Overview
This document describes the stored procedures related to Recurring Journal Vouchers (RJV) in the General Ledger system. These procedures handle the creation, scheduling, and management of recurring journal entries.

## Database Tables

### GL_RECURRING_JV_HDR
Header table storing recurring journal voucher master information.

**Key Fields:**
- POID (Long) - Primary key
- RecurringJVNumber - Unique identifier for the recurring JV
- Description - Description of the recurring entry
- Frequency - Recurrence frequency (Monthly, Quarterly, etc.)
- StartDate - When the recurring entries should start
- EndDate - When the recurring entries should end
- Status - Active/Inactive status
- CreatedBy, CreatedDate - Audit fields
- ModifiedBy, ModifiedDate - Audit fields

### GL_RECURRING_JV_DTL
Detail table storing individual line items for the recurring journal voucher.

**Key Fields:**
- POID (Long) - Primary key
- RecurringJVHdrPOID (Long) - Foreign key to header
- LineNumber - Line sequence number
- AccountCode - GL Account code
- DebitAmount (BigDecimal) - Debit amount
- CreditAmount (BigDecimal) - Credit amount
- Description - Line item description
- CostCenter - Optional cost center
- ProjectCode - Optional project code

### GL_RECURRING_JV_MONTH_DTL
Month-wise schedule table storing when each recurring entry should be generated.

**Key Fields:**
- POID (Long) - Primary key
- RecurringJVHdrPOID (Long) - Foreign key to header
- ScheduleMonth - Target month for generation
- ScheduleYear - Target year for generation
- Status - Pending/Generated/Cancelled
- GeneratedDate - When the JV was actually generated
- GeneratedJVNumber - Reference to the generated journal voucher

## Stored Procedures

### PROC_GL_RJV_CREATE_SCHEDULE

**Purpose:** Creates the monthly schedule for a recurring journal voucher based on the frequency and date range specified in the header.

**Parameters:**
```sql
@RecurringJVHdrPOID BIGINT,  -- Header POID
@StartDate DATE,              -- Start date for scheduling
@EndDate DATE,                -- End date for scheduling
@Frequency VARCHAR(50)        -- Frequency (Monthly, Quarterly, etc.)
```

**Functionality:**
- Generates schedule entries in GL_RECURRING_JV_MONTH_DTL
- Creates one entry per month/period based on frequency
- Sets initial status as 'Pending'
- Validates date ranges and frequency
- Handles edge cases (leap years, month-end dates)

**Example Usage:**
```sql
EXEC PROC_GL_RJV_CREATE_SCHEDULE
    @RecurringJVHdrPOID = 12345,
    @StartDate = '2024-01-01',
    @EndDate = '2024-12-31',
    @Frequency = 'Monthly';
```

**Expected Behavior:**
- Creates 12 schedule entries (one for each month) in GL_RECURRING_JV_MONTH_DTL
- Each entry has ScheduleMonth and ScheduleYear populated
- Status is set to 'Pending'
- GeneratedDate and GeneratedJVNumber remain NULL until JV is generated

### PROC_GL_RJV_DELETE_SCHEDULE

**Purpose:** Deletes schedule entries for a recurring journal voucher. Used when modifying or cancelling a recurring JV.

**Parameters:**
```sql
@RecurringJVHdrPOID BIGINT,  -- Header POID
@DeleteGenerated BIT = 0     -- Whether to delete already generated entries (0 = No, 1 = Yes)
```

**Functionality:**
- Deletes schedule entries from GL_RECURRING_JV_MONTH_DTL
- By default, only deletes 'Pending' entries
- If @DeleteGenerated = 1, also deletes 'Generated' entries
- Validates that entries exist before deletion
- Logs deletion activity

**Example Usage:**
```sql
-- Delete only pending entries
EXEC PROC_GL_RJV_DELETE_SCHEDULE
    @RecurringJVHdrPOID = 12345,
    @DeleteGenerated = 0;

-- Delete all entries including generated ones
EXEC PROC_GL_RJV_DELETE_SCHEDULE
    @RecurringJVHdrPOID = 12345,
    @DeleteGenerated = 1;
```

**Expected Behavior:**
- Removes schedule entries based on criteria
- Updates audit fields if soft delete is implemented
- Returns count of deleted records

### PROC_GL_RJV_CREATE_JV

**Purpose:** Generates an actual journal voucher from a scheduled recurring entry. This procedure creates the JV header and detail lines based on the recurring JV template.

**Parameters:**
```sql
@RecurringJVHdrPOID BIGINT,      -- Header POID
@ScheduleMonth INT,               -- Month to generate
@ScheduleYear INT,                -- Year to generate
@GeneratedJVNumber VARCHAR(50) OUTPUT,  -- Generated JV number
@ResultCode INT OUTPUT,           -- Result code (0 = Success, non-zero = Error)
@ResultMessage VARCHAR(500) OUTPUT  -- Result message
```

**Functionality:**
- Retrieves header and detail information from recurring JV tables
- Creates new JV header in GL_JV_HDR (or equivalent)
- Creates JV detail lines in GL_JV_DTL (or equivalent)
- Updates GL_RECURRING_JV_MONTH_DTL with generated information
- Sets status to 'Generated'
- Handles validation and error scenarios
- Ensures debit and credit amounts balance

**Example Usage:**
```sql
DECLARE @JVNumber VARCHAR(50);
DECLARE @ResultCode INT;
DECLARE @ResultMessage VARCHAR(500);

EXEC PROC_GL_RJV_CREATE_JV
    @RecurringJVHdrPOID = 12345,
    @ScheduleMonth = 1,
    @ScheduleYear = 2024,
    @GeneratedJVNumber = @JVNumber OUTPUT,
    @ResultCode = @ResultCode OUTPUT,
    @ResultMessage = @ResultMessage OUTPUT;

SELECT @JVNumber AS GeneratedJVNumber, @ResultCode AS ResultCode, @ResultMessage AS ResultMessage;
```

**Expected Behavior:**
- Creates actual JV with unique JV number
- Copies all detail lines from recurring template
- Updates schedule entry status to 'Generated'
- Populates GeneratedDate and GeneratedJVNumber
- Returns success/error status

**Note:** This procedure may be unused or replaced by application-level logic. Verify current implementation.

## Data Type Requirements

### POID Fields
- **Type:** BIGINT (Long)
- **Usage:** All primary keys and foreign keys use POID
- **Example:** RecurringJVHdrPOID, RecurringJVDetailPOID

### Amount Fields
- **Type:** DECIMAL(18,2) or DECIMAL(19,2) (BigDecimal)
- **Usage:** DebitAmount, CreditAmount
- **Precision:** Sufficient for financial calculations
- **Example:** 123456789.99

## Workflow

### Creating a Recurring JV

1. **Insert Header**
   ```sql
   INSERT INTO GL_RECURRING_JV_HDR (RecurringJVNumber, Description, Frequency, StartDate, EndDate, Status)
   VALUES ('RJV-2024-001', 'Monthly Rent', 'Monthly', '2024-01-01', '2024-12-31', 'Active');
   ```

2. **Insert Detail Lines**
   ```sql
   INSERT INTO GL_RECURRING_JV_DTL (RecurringJVHdrPOID, LineNumber, AccountCode, DebitAmount, CreditAmount)
   VALUES 
       (@HdrPOID, 1, '5000', 10000.00, 0.00),
       (@HdrPOID, 2, '2000', 0.00, 10000.00);
   ```

3. **Create Schedule**
   ```sql
   EXEC PROC_GL_RJV_CREATE_SCHEDULE
       @RecurringJVHdrPOID = @HdrPOID,
       @StartDate = '2024-01-01',
       @EndDate = '2024-12-31',
       @Frequency = 'Monthly';
   ```

### Generating Scheduled JVs

1. **Query Pending Schedules**
   ```sql
   SELECT *
   FROM GL_RECURRING_JV_MONTH_DTL
   WHERE Status = 'Pending'
       AND ScheduleYear = YEAR(GETDATE())
       AND ScheduleMonth = MONTH(GETDATE());
   ```

2. **Generate JV for Each Schedule**
   ```sql
   EXEC PROC_GL_RJV_CREATE_JV
       @RecurringJVHdrPOID = @HdrPOID,
       @ScheduleMonth = @Month,
       @ScheduleYear = @Year,
       @GeneratedJVNumber = @JVNumber OUTPUT,
       @ResultCode = @ResultCode OUTPUT,
       @ResultMessage = @ResultMessage OUTPUT;
   ```

### Modifying a Recurring JV

1. **Delete Existing Schedule**
   ```sql
   EXEC PROC_GL_RJV_DELETE_SCHEDULE
       @RecurringJVHdrPOID = @HdrPOID,
       @DeleteGenerated = 0;
   ```

2. **Update Header/Details**
   ```sql
   UPDATE GL_RECURRING_JV_HDR
   SET Description = 'Updated Description',
       ModifiedDate = GETDATE()
   WHERE POID = @HdrPOID;
   ```

3. **Recreate Schedule**
   ```sql
   EXEC PROC_GL_RJV_CREATE_SCHEDULE
       @RecurringJVHdrPOID = @HdrPOID,
       @StartDate = @NewStartDate,
       @EndDate = @NewEndDate,
       @Frequency = @Frequency;
   ```

## Best Practices

1. **Transaction Management**
   - Wrap schedule creation/deletion in transactions
   - Ensure atomicity of JV generation

2. **Validation**
   - Validate debit/credit balance before creating schedule
   - Check date ranges are valid
   - Verify account codes exist

3. **Error Handling**
   - Use TRY...CATCH blocks in procedures
   - Return meaningful error messages
   - Log errors for troubleshooting

4. **Performance**
   - Index on RecurringJVHdrPOID in detail and schedule tables
   - Index on Status, ScheduleYear, ScheduleMonth in schedule table
   - Consider batch processing for multiple JV generations

5. **Audit Trail**
   - Maintain CreatedBy, CreatedDate, ModifiedBy, ModifiedDate
   - Log all schedule changes
   - Track JV generation history

## Related Concepts
- See `01_Stored_Procedures.md` for general stored procedure concepts
- See `02_Functions.md` for user-defined functions
- See `03_Queries.md` for SQL query concepts

