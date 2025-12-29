# JDBC (Java Database Connectivity)

## What is JDBC?
JDBC is a Java API for connecting and executing queries with databases. It provides methods to query and update data in a database.

## JDBC Architecture
1. **Application**: Java application
2. **JDBC API**: Java interfaces and classes
3. **JDBC Driver**: Database-specific implementation
4. **Database**: Actual database (MySQL, PostgreSQL, etc.)

## Setting Up JDBC

### Add Driver Dependency
```xml
<!-- Maven -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>
```

### Load Driver
```java
Class.forName("com.mysql.cj.jdbc.Driver");
// Or (Java 6+)
DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());
```

## Establishing Connection

### Connection URL Format
```java
// MySQL
String url = "jdbc:mysql://localhost:3306/mydb";

// PostgreSQL
String url = "jdbc:postgresql://localhost:5432/mydb";

// Oracle
String url = "jdbc:oracle:thin:@localhost:1521:xe";
```

### Creating Connection
```java
import java.sql.*;

String url = "jdbc:mysql://localhost:3306/mydb";
String username = "root";
String password = "password";

Connection connection = DriverManager.getConnection(url, username, password);
```

## Executing Queries

### Statement
```java
Statement statement = connection.createStatement();

// Execute query
ResultSet resultSet = statement.executeQuery("SELECT * FROM users");

// Execute update
int rowsAffected = statement.executeUpdate(
    "INSERT INTO users (name, email) VALUES ('John', 'john@example.com')");

// Execute any SQL
boolean isResultSet = statement.execute("SELECT * FROM users");
```

### PreparedStatement
```java
String sql = "INSERT INTO users (name, email) VALUES (?, ?)";
PreparedStatement pstmt = connection.prepareStatement(sql);

pstmt.setString(1, "John");
pstmt.setString(2, "john@example.com");
int rowsAffected = pstmt.executeUpdate();
```

### CallableStatement (Stored Procedures)
```java
String sql = "{call get_user(?, ?)}";
CallableStatement cstmt = connection.prepareCall(sql);

cstmt.setInt(1, 123);
cstmt.registerOutParameter(2, Types.VARCHAR);
cstmt.execute();
String result = cstmt.getString(2);
```

## Processing Results

### ResultSet
```java
String sql = "SELECT id, name, email FROM users";
Statement statement = connection.createStatement();
ResultSet rs = statement.executeQuery(sql);

while (rs.next()) {
    int id = rs.getInt("id");
    String name = rs.getString("name");
    String email = rs.getString("email");
    
    System.out.println("ID: " + id + ", Name: " + name + ", Email: " + email);
}

// Or by index
int id = rs.getInt(1);
String name = rs.getString(2);
```

### ResultSet Types
```java
// Forward only (default)
Statement stmt = connection.createStatement(
    ResultSet.TYPE_FORWARD_ONLY, 
    ResultSet.CONCUR_READ_ONLY);

// Scrollable
Statement stmt = connection.createStatement(
    ResultSet.TYPE_SCROLL_INSENSITIVE, 
    ResultSet.CONCUR_READ_ONLY);

// Updatable
Statement stmt = connection.createStatement(
    ResultSet.TYPE_SCROLL_INSENSITIVE, 
    ResultSet.CONCUR_UPDATABLE);
```

## CRUD Operations

### Create (INSERT)
```java
String sql = "INSERT INTO users (name, email) VALUES (?, ?)";
PreparedStatement pstmt = connection.prepareStatement(sql);

pstmt.setString(1, "John");
pstmt.setString(2, "john@example.com");
int rows = pstmt.executeUpdate();
```

### Read (SELECT)
```java
String sql = "SELECT * FROM users WHERE id = ?";
PreparedStatement pstmt = connection.prepareStatement(sql);
pstmt.setInt(1, 1);

ResultSet rs = pstmt.executeQuery();
if (rs.next()) {
    String name = rs.getString("name");
    String email = rs.getString("email");
}
```

### Update
```java
String sql = "UPDATE users SET email = ? WHERE id = ?";
PreparedStatement pstmt = connection.prepareStatement(sql);

pstmt.setString(1, "newemail@example.com");
pstmt.setInt(2, 1);
int rows = pstmt.executeUpdate();
```

### Delete
```java
String sql = "DELETE FROM users WHERE id = ?";
PreparedStatement pstmt = connection.prepareStatement(sql);

pstmt.setInt(1, 1);
int rows = pstmt.executeUpdate();
```

## Transactions

### Manual Transaction Control
```java
connection.setAutoCommit(false);

try {
    // Multiple operations
    statement.executeUpdate("INSERT INTO users ...");
    statement.executeUpdate("UPDATE accounts ...");
    
    connection.commit(); // Commit transaction
} catch (SQLException e) {
    connection.rollback(); // Rollback on error
} finally {
    connection.setAutoCommit(true);
}
```

## Connection Pooling

### Using HikariCP
```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:mysql://localhost:3306/mydb");
config.setUsername("root");
config.setPassword("password");
config.setMaximumPoolSize(10);

HikariDataSource dataSource = new HikariDataSource(config);
Connection connection = dataSource.getConnection();
```

## Complete Example
```java
import java.sql.*;

public class JDBCDemo {
    private static final String URL = "jdbc:mysql://localhost:3306/mydb";
    private static final String USERNAME = "root";
    private static final String PASSWORD = "password";
    
    public static void main(String[] args) {
        try (Connection connection = DriverManager.getConnection(
                URL, USERNAME, PASSWORD)) {
            
            // Create table
            createTable(connection);
            
            // Insert
            insertUser(connection, "John", "john@example.com");
            
            // Select
            selectUsers(connection);
            
            // Update
            updateUser(connection, 1, "john.doe@example.com");
            
            // Delete
            deleteUser(connection, 1);
            
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    
    private static void createTable(Connection conn) throws SQLException {
        String sql = "CREATE TABLE IF NOT EXISTS users (" +
                    "id INT AUTO_INCREMENT PRIMARY KEY," +
                    "name VARCHAR(100)," +
                    "email VARCHAR(100)" +
                    ")";
        try (Statement stmt = conn.createStatement()) {
            stmt.execute(sql);
        }
    }
    
    private static void insertUser(Connection conn, String name, String email) 
            throws SQLException {
        String sql = "INSERT INTO users (name, email) VALUES (?, ?)";
        try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setString(1, name);
            pstmt.setString(2, email);
            pstmt.executeUpdate();
        }
    }
    
    private static void selectUsers(Connection conn) throws SQLException {
        String sql = "SELECT * FROM users";
        try (Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {
            
            while (rs.next()) {
                System.out.println("ID: " + rs.getInt("id") +
                                 ", Name: " + rs.getString("name") +
                                 ", Email: " + rs.getString("email"));
            }
        }
    }
    
    private static void updateUser(Connection conn, int id, String email) 
            throws SQLException {
        String sql = "UPDATE users SET email = ? WHERE id = ?";
        try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setString(1, email);
            pstmt.setInt(2, id);
            pstmt.executeUpdate();
        }
    }
    
    private static void deleteUser(Connection conn, int id) 
            throws SQLException {
        String sql = "DELETE FROM users WHERE id = ?";
        try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setInt(1, id);
            pstmt.executeUpdate();
        }
    }
}
```

## Best Practices
1. Always use PreparedStatement to prevent SQL injection
2. Use try-with-resources for automatic resource management
3. Close connections, statements, and result sets
4. Use connection pooling for production
5. Handle SQLException properly
6. Use transactions for multiple related operations
7. Don't store passwords in code - use configuration files
8. Use batch operations for multiple inserts/updates
9. Set appropriate timeouts
10. Use connection pooling libraries (HikariCP, C3P0)

