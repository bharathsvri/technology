# Security - Database Documentation

## Overview
Database Security involves protecting data from unauthorized access, ensuring data integrity, and controlling what users can do within the database. SQL Server provides a comprehensive security model with users, roles, permissions, and encryption capabilities.

## Security Principals

### 1. **Logins**
- Server-level principals
- Allow connection to SQL Server instance
- Windows or SQL Server authentication

### 2. **Users**
- Database-level principals
- Mapped to logins
- Own database objects
- Receive permissions

### 3. **Roles**
- Groups of permissions
- Server roles and database roles
- Simplify permission management

### 4. **Schemas**
- Containers for database objects
- Provide namespace separation
- Owned by users or roles

## Creating Logins

### SQL Server Login
```sql
CREATE LOGIN LoginName
WITH PASSWORD = 'StrongPassword123!';
```

### Windows Login
```sql
CREATE LOGIN [DOMAIN\Username]
FROM WINDOWS;
```

### Login with Options
```sql
CREATE LOGIN LoginName
WITH PASSWORD = 'StrongPassword123!',
    DEFAULT_DATABASE = MyDatabase,
    DEFAULT_LANGUAGE = us_english,
    CHECK_EXPIRATION = ON,
    CHECK_POLICY = ON;
```

### Altering Login
```sql
ALTER LOGIN LoginName
WITH PASSWORD = 'NewPassword123!',
    DEFAULT_DATABASE = AnotherDatabase;
```

### Disabling/Enabling Login
```sql
ALTER LOGIN LoginName DISABLE;
ALTER LOGIN LoginName ENABLE;
```

### Dropping Login
```sql
DROP LOGIN LoginName;
```

## Creating Users

### Basic User
```sql
CREATE USER UserName
FOR LOGIN LoginName;
```

### User with Default Schema
```sql
CREATE USER UserName
FOR LOGIN LoginName
WITH DEFAULT_SCHEMA = dbo;
```

### User Without Login (Contained User)
```sql
CREATE USER UserName
WITH PASSWORD = 'Password123!';
```

### Altering User
```sql
ALTER USER UserName
WITH DEFAULT_SCHEMA = Sales;
```

### Dropping User
```sql
DROP USER UserName;
```

## Server Roles

### Fixed Server Roles
- **sysadmin**: Full system administration
- **serveradmin**: Server configuration
- **securityadmin**: Security administration
- **processadmin**: Process management
- **setupadmin**: Linked servers
- **bulkadmin**: Bulk operations
- **diskadmin**: Disk management
- **dbcreator**: Database creation
- **public**: Default role (all logins)

### Adding Login to Server Role
```sql
ALTER SERVER ROLE sysadmin ADD MEMBER LoginName;
```

### Removing from Server Role
```sql
ALTER SERVER ROLE sysadmin DROP MEMBER LoginName;
```

## Database Roles

### Fixed Database Roles
- **db_owner**: Full database control
- **db_accessadmin**: User management
- **db_securityadmin**: Security management
- **db_ddladmin**: DDL operations
- **db_backupoperator**: Backup operations
- **db_datareader**: Read all data
- **db_datawriter**: Write all data
- **db_denydatareader**: Deny read access
- **db_denydatawriter**: Deny write access
- **public**: Default role (all users)

### Adding User to Database Role
```sql
ALTER ROLE db_datareader ADD MEMBER UserName;
```

### Creating Custom Database Role
```sql
CREATE ROLE RoleName;
ALTER ROLE RoleName ADD MEMBER UserName;
```

### Dropping Role
```sql
DROP ROLE RoleName;
```

## Permissions

### Object Permissions
- **SELECT**: Read data
- **INSERT**: Add data
- **UPDATE**: Modify data
- **DELETE**: Remove data
- **EXECUTE**: Execute stored procedures/functions
- **REFERENCES**: Reference in foreign keys
- **ALTER**: Modify object structure
- **CONTROL**: Full control
- **TAKE OWNERSHIP**: Take ownership

### Granting Permissions

#### Grant SELECT
```sql
GRANT SELECT ON Employees TO UserName;
```

#### Grant Multiple Permissions
```sql
GRANT SELECT, INSERT, UPDATE ON Employees TO UserName;
```

#### Grant on Schema
```sql
GRANT SELECT ON SCHEMA::Sales TO UserName;
```

#### Grant Execute
```sql
GRANT EXECUTE ON PROC_GetEmployees TO UserName;
```

#### Grant with Grant Option
```sql
GRANT SELECT ON Employees TO UserName WITH GRANT OPTION;
```

### Revoking Permissions
```sql
REVOKE SELECT ON Employees FROM UserName;
```

### Denying Permissions
```sql
DENY SELECT ON Employees TO UserName;
```

### Viewing Permissions
```sql
-- Object permissions
SELECT 
    pr.name AS PrincipalName,
    pr.type_desc AS PrincipalType,
    pe.permission_name,
    pe.state_desc,
    OBJECT_NAME(pe.major_id) AS ObjectName
FROM sys.database_permissions pe
INNER JOIN sys.database_principals pr ON pe.grantee_principal_id = pr.principal_id
WHERE pe.major_id = OBJECT_ID('Employees');
```

## Schemas

### Creating Schema
```sql
CREATE SCHEMA Sales;
```

### Schema with Owner
```sql
CREATE SCHEMA Sales AUTHORIZATION UserName;
```

### Altering Schema
```sql
ALTER SCHEMA Sales TRANSFER dbo.Employees;
```

### Dropping Schema
```sql
DROP SCHEMA Sales;
```

### Default Schemas
```sql
-- Set default schema for user
ALTER USER UserName WITH DEFAULT_SCHEMA = Sales;

-- User can access objects without schema prefix
SELECT * FROM Employees;  -- Uses Sales.Employees
```

## Row-Level Security

### Creating Security Policy
```sql
-- Create filter function
CREATE FUNCTION dbo.fn_SecurityPolicy(@EmployeeID INT)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN
    SELECT 1 AS Result
    WHERE @EmployeeID = USER_ID()  -- User can only see their own records
       OR USER_ID() IN (SELECT ManagerID FROM Employees WHERE EmployeeID = @EmployeeID);  -- Or their manager
GO

-- Create security policy
CREATE SECURITY POLICY EmployeeSecurityPolicy
ADD FILTER PREDICATE dbo.fn_SecurityPolicy(EmployeeID) ON dbo.Employees
WITH (STATE = ON);
```

### Blocking Predicate
```sql
CREATE SECURITY POLICY EmployeeSecurityPolicy
ADD BLOCK PREDICATE dbo.fn_SecurityPolicy(EmployeeID) ON dbo.Employees
WITH (STATE = ON);
```

## Encryption

### Transparent Data Encryption (TDE)
```sql
-- Create master key
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'StrongPassword123!';

-- Create certificate
CREATE CERTIFICATE TDECertificate
WITH SUBJECT = 'TDE Certificate';

-- Create database encryption key
CREATE DATABASE ENCRYPTION KEY
WITH ALGORITHM = AES_256
ENCRYPTION BY SERVER CERTIFICATE TDECertificate;

-- Enable TDE
ALTER DATABASE MyDatabase
SET ENCRYPTION ON;
```

### Column-Level Encryption
```sql
-- Create master key
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'StrongPassword123!';

-- Create certificate
CREATE CERTIFICATE EncryptionCert
WITH SUBJECT = 'Column Encryption Certificate';

-- Create symmetric key
CREATE SYMMETRIC KEY ColumnKey
WITH ALGORITHM = AES_256
ENCRYPTION BY CERTIFICATE EncryptionCert;

-- Encrypt data
OPEN SYMMETRIC KEY ColumnKey DECRYPTION BY CERTIFICATE EncryptionCert;

UPDATE Employees
SET EncryptedSSN = ENCRYPTBYKEY(KEY_GUID('ColumnKey'), SSN);

CLOSE SYMMETRIC KEY ColumnKey;

-- Decrypt data
OPEN SYMMETRIC KEY ColumnKey DECRYPTION BY CERTIFICATE EncryptionCert;

SELECT 
    EmployeeID,
    CONVERT(VARCHAR, DECRYPTBYKEY(EncryptedSSN)) AS SSN
FROM Employees;

CLOSE SYMMETRIC KEY ColumnKey;
```

## Dynamic Data Masking

### Creating Masked Columns
```sql
ALTER TABLE Employees
ALTER COLUMN Email ADD MASKED WITH (FUNCTION = 'email()');

ALTER TABLE Employees
ALTER COLUMN SSN ADD MASKED WITH (FUNCTION = 'partial(0,"XXX-XX-",4)');

ALTER TABLE Employees
ALTER COLUMN Salary ADD MASKED WITH (FUNCTION = 'random(1000, 9999)');
```

### Granting Unmask Permission
```sql
GRANT UNMASK ON Employees TO UserName;
```

## Best Practices

### 1. **Principle of Least Privilege**
- Grant minimum necessary permissions
- Use roles to group permissions
- Regularly review permissions

### 2. **Strong Authentication**
- Enforce password policies
- Use Windows authentication when possible
- Enable password expiration

### 3. **Role-Based Access**
- Create custom roles for business functions
- Assign users to roles, not individual permissions
- Document role purposes

### 4. **Schema Organization**
- Use schemas to organize objects
- Separate by application or function
- Set appropriate default schemas

### 5. **Regular Auditing**
- Review user permissions regularly
- Remove unused accounts
- Monitor access patterns

### 6. **Encryption**
- Encrypt sensitive data at rest (TDE)
- Encrypt sensitive columns
- Protect encryption keys

### 7. **Security Policies**
- Implement row-level security when needed
- Use dynamic data masking for sensitive data
- Document security requirements

## Common Security Patterns

### Application User Pattern
```sql
-- Create login and user for application
CREATE LOGIN AppLogin WITH PASSWORD = 'StrongPassword123!';
CREATE USER AppUser FOR LOGIN AppLogin;

-- Create role for application
CREATE ROLE AppRole;

-- Grant necessary permissions
GRANT SELECT, INSERT, UPDATE ON SCHEMA::Sales TO AppRole;
GRANT EXECUTE ON SCHEMA::Sales TO AppRole;

-- Add user to role
ALTER ROLE AppRole ADD MEMBER AppUser;
```

### Read-Only User Pattern
```sql
CREATE LOGIN ReadOnlyLogin WITH PASSWORD = 'Password123!';
CREATE USER ReadOnlyUser FOR LOGIN ReadOnlyLogin;

ALTER ROLE db_datareader ADD MEMBER ReadOnlyUser;
```

### Schema-Based Security
```sql
-- Create schemas
CREATE SCHEMA Sales;
CREATE SCHEMA HR;

-- Create users
CREATE USER SalesUser FOR LOGIN SalesLogin;
CREATE USER HRUser FOR LOGIN HRLogin;

-- Grant schema access
GRANT SELECT, INSERT, UPDATE ON SCHEMA::Sales TO SalesUser;
GRANT SELECT, INSERT, UPDATE ON SCHEMA::HR TO HRUser;

-- Deny cross-schema access
DENY SELECT ON SCHEMA::HR TO SalesUser;
DENY SELECT ON SCHEMA::Sales TO HRUser;
```

## Related Concepts
- See `01_Stored_Procedures.md` for EXECUTE AS
- See `05_Views.md` for security through views
- See `08_Constraints.md` for data integrity

