# Database Concepts Documentation

This documentation provides comprehensive coverage of all major database concepts including stored procedures, functions, queries, and related database objects.

## Documentation Files

### Core Concepts

1. **[01_Stored_Procedures.md](01_Stored_Procedures.md)**
   - Overview of stored procedures
   - Syntax and structure
   - Parameters and execution
   - Control-of-flow statements
   - Transaction management
   - Best practices

2. **[02_Functions.md](02_Functions.md)**
   - Scalar functions
   - Table-valued functions
   - Inline and multi-statement functions
   - Function options and best practices

3. **[03_Queries.md](03_Queries.md)**
   - SELECT statements
   - WHERE, JOIN, GROUP BY clauses
   - Subqueries and CTEs
   - Window functions
   - INSERT, UPDATE, DELETE
   - Best practices

4. **[04_Recurring_JV_Procedures.md](04_Recurring_JV_Procedures.md)**
   - Specific documentation for Recurring Journal Voucher procedures
   - Table structures
   - Stored procedure details
   - Workflow and best practices

5. **[05_Views.md](05_Views.md)**
   - Creating and using views
   - Simple and complex views
   - Indexed views
   - Updatable views
   - Security through views

6. **[06_Triggers.md](06_Triggers.md)**
   - AFTER triggers
   - INSTEAD OF triggers
   - DDL triggers
   - INSERTED and DELETED tables
   - Common patterns

### Advanced Concepts

7. **[07_Indexes.md](07_Indexes.md)**
   - Clustered and non-clustered indexes
   - Composite and covering indexes
   - Filtered indexes
   - Index maintenance
   - Performance optimization

8. **[08_Constraints.md](08_Constraints.md)**
   - PRIMARY KEY, FOREIGN KEY
   - UNIQUE, CHECK constraints
   - NOT NULL, DEFAULT
   - Constraint management

9. **[09_Transactions.md](09_Transactions.md)**
   - ACID properties
   - Transaction syntax
   - Isolation levels
   - Deadlocks
   - Best practices

10. **[10_Cursors.md](10_Cursors.md)**
    - Types of cursors
    - Cursor syntax and usage
    - Scrollable cursors
    - Alternatives to cursors
    - Performance considerations

11. **[11_Error_Handling.md](11_Error_Handling.md)**
    - TRY...CATCH blocks
    - RAISERROR and THROW
    - Error information functions
    - Error handling patterns
    - Best practices

12. **[12_Dynamic_SQL.md](12_Dynamic_SQL.md)**
    - Building dynamic SQL
    - Parameterized queries
    - SQL injection prevention
    - Security best practices
    - Common patterns

13. **[13_Temporary_Tables.md](13_Temporary_Tables.md)**
    - Local and global temporary tables
    - Table variables
    - Comparison and use cases
    - Performance considerations

14. **[14_Security.md](14_Security.md)**
    - Logins and users
    - Roles and permissions
    - Schemas
    - Row-level security
    - Encryption
    - Best practices

15. **[15_Table_Partitioning.md](15_Table_Partitioning.md)**
    - Partition functions and schemes
    - Sliding windows and `SWITCH` patterns
    - Query elimination and maintenance trade-offs
    - Columnstore note for analytics workloads

16. **[16_JSON_in_SQL_Server.md](16_JSON_in_SQL_Server.md)**
    - `FOR JSON` shaping for APIs
    - `OPENJSON` with explicit schemas
    - `ISJSON` constraints and computed-column indexing

17. **[17_Execution_Plans_and_Query_Store.md](17_Execution_Plans_and_Query_Store.md)**
    - Reading estimated vs actual plans
    - Statistics, parameter sniffing concepts
    - Query Store for regressions and plan history

18. **[18_Backup_and_Restore_SQL_Server.md](18_Backup_and_Restore_SQL_Server.md)**
    - Full, differential, and transaction log backups
    - Recovery models, point-in-time restore, verification with `CHECKSUM`

19. **[19_Locking_Blocking_Deadlocks.md](19_Locking_Blocking_Deadlocks.md)**
    - Lock modes, isolation interactions, blocking chains
    - Deadlock detection (`1205`), diagnosing with DMVs and Extended Events

20. **[20_MERGE_Statement_SQL_Server.md](20_MERGE_Statement_SQL_Server.md)**
    - Upsert and sync patterns with `MERGE`
    - Concurrency, triggers, and performance caveats

21. **[21_Temporal_Tables_SQL_Server.md](21_Temporal_Tables_SQL_Server.md)**
    - System-versioned tables and history stores
    - `FOR SYSTEM_TIME AS OF` queries and retention planning

22. **[22_Replication_and_HA_Overview.md](22_Replication_and_HA_Overview.md)**
    - Log shipping, Always On AGs, FCIs, replication families
    - Choosing HA vs DR strategies at a high level

23. **[23_SQL_Server_Agent_Jobs_and_Alerts.md](23_SQL_Server_Agent_Jobs_and_Alerts.md)**
    - Jobs, steps, schedules, and operator alerts
    - Permissions and failure monitoring in `msdb`

24. **[24_Columnstore_Indexes_Overview.md](24_Columnstore_Indexes_Overview.md)**
    - Column-oriented storage, batch mode concept
    - When columnstore helps analytics vs OLTP trade-offs

25. **[25_Synonyms_and_Cross_Database_References.md](25_Synonyms_and_Cross_Database_References.md)**
    - Synonyms for stable object aliases
    - Cross-database queries and linked-server caution

26. **[26_Window_Functions_SQL_Server.md](26_Window_Functions_SQL_Server.md)**
    - `OVER`, `PARTITION BY`, ranking and analytic functions
    - Running totals and performance considerations

27. **[27_Common_Table_Expressions_CTEs.md](27_Common_Table_Expressions_CTEs.md)**
    - Readable subqueries; recursive hierarchies with `MAXRECURSION`

28. **[28_Isolation_Levels_and_Read_Phenomena.md](28_Isolation_Levels_and_Read_Phenomena.md)**
    - Dirty / non-repeatable / phantom reads vs isolation levels
    - Snapshot and locking trade-offs in SQL Server

29. **[29_Normal_Forms_and_Denormalization.md](29_Normal_Forms_and_Denormalization.md)**
    - 1NF–3NF intent; when denormalize for read performance

30. **[30_Heap_vs_Clustered_Index_Structure.md](30_Heap_vs_Clustered_Index_Structure.md)**
    - Heaps vs clustered tables; clustering key trade-offs

31. **[31_Index_Maintenance_REBUILD_and_REORGANIZE.md](31_Index_Maintenance_REBUILD_and_REORGANIZE.md)**
    - Fragmentation thresholds; `REBUILD` vs `REORGANIZE`

32. **[32_PIVOT_UNPIVOT_and_Cross_Tab_Queries.md](32_PIVOT_UNPIVOT_and_Cross_Tab_Queries.md)**
    - Cross-tab reporting; dynamic pivot awareness

33. **[33_Covering_Indexes_and_INCLUDE_Columns.md](33_Covering_Indexes_and_INCLUDE_Columns.md)**
    - Covering indexes; `INCLUDE` at leaf vs wide keys

34. **[34_Parameter_Sniffing_and_OPTION_RECOMPILE.md](34_Parameter_Sniffing_and_OPTION_RECOMPILE.md)**
    - Plan reuse pitfalls; `OPTION (RECOMPILE)` and alternatives

35. **[35_SARGable_Predicates_and_Index_Seekability.md](35_SARGable_Predicates_and_Index_Seekability.md)**
    - Predicate shapes that enable index seeks; common anti-patterns

36. **[36_Hash_vs_Merge_Join_Sketch.md](36_Hash_vs_Merge_Join_Sketch.md)**
    - Hash vs merge vs nested loops intuition; estimate sensitivity

37. **[37_WAITFOR_Delay_and_Time_Dependent_SQL.md](37_WAITFOR_Delay_and_Time_Dependent_SQL.md)**
    - `WAITFOR` for tests; avoid production sleeps holding locks

## Quick Reference

### Stored Procedures
- Create: `CREATE PROCEDURE`
- Execute: `EXEC procedure_name`
- Parameters: `@param datatype`

### Functions
- Scalar: Returns single value
- Table-valued: Returns table
- Usage: `SELECT dbo.function_name()`

### Queries
- SELECT: Retrieve data
- JOIN: Combine tables
- WHERE: Filter rows
- GROUP BY: Aggregate data

### Indexes
- Clustered: Physical data order
- Non-clustered: Separate structure
- Create: `CREATE INDEX`

### Constraints
- PRIMARY KEY: Unique identifier
- FOREIGN KEY: Referential integrity
- CHECK: Data validation

### Transactions
- Begin: `BEGIN TRANSACTION`
- Commit: `COMMIT TRANSACTION`
- Rollback: `ROLLBACK TRANSACTION`

## Best Practices Summary

1. **Always use parameterized queries** to prevent SQL injection
2. **Implement proper error handling** with TRY...CATCH blocks
3. **Use transactions** for operations that must be atomic
4. **Index appropriately** for query performance
5. **Follow naming conventions** consistently
6. **Document code** with comments
7. **Test thoroughly** before deployment
8. **Follow security best practices** (least privilege, encryption)
9. **Optimize for performance** (avoid cursors when possible)
10. **Maintain data integrity** with constraints

## Related Topics

- Each document references related concepts in other files
- See individual files for detailed examples and patterns
- All concepts work together in real-world database applications

## Notes

- All examples use SQL Server syntax
- Adjust syntax for other database systems as needed
- POID fields should be BIGINT (Long)
- Amount fields should be DECIMAL(18,2) or DECIMAL(19,2) (BigDecimal)
- See `04_Recurring_JV_Procedures.md` for specific Recurring JV implementation details

