# SQL Server Agent: Jobs, Steps, and Alerts

**SQL Server Agent** schedules recurring work: backups, ETL, index maintenance, and custom T-SQL/PowerShell/CmdExec steps.

## Core Objects
- **Job**: named container with **steps**, **schedules**, and optional **notifications**.
- **Step**: ordered action (often T-SQL or SSIS package execution).
- **Schedule**: time-based or event-based trigger.
- **Alert**: respond to **performance conditions**, **SQL events**, or **WMI** signals.

## Example: Create a Simple Job (Outline)
Typically done in **SSMS** GUI or `sp_add_job`, `sp_add_jobstep`, `sp_add_schedule`, `sp_attach_schedule`—production scripts should be **idempotent** and **source-controlled**.

## Permissions
- **`SQLAgentUserRole`**, **`SQLAgentOperatorRole`**, **`SQLAgentReaderRole`** control who can start jobs and see history.
- Principle of least privilege for operators.

## Observability
- **Job history** (`msdb` tables `sysjobhistory`) feeds dashboards.
- Log failures to **Database Mail** profiles for on-call routing.

## Pitfalls
- Overlapping schedules causing **concurrent** runs when the job is not re-entrant.
- **CmdExec** security surface—prefer T-SQL or controlled SSIS where possible.

## Related Topics
- `18_Backup_and_Restore_SQL_Server.md`
- `11_Error_Handling.md`
