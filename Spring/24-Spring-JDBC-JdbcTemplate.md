# Spring JDBC and `JdbcTemplate`

**`JdbcTemplate`** removes boilerplate around **`Connection` / `PreparedStatement` / `ResultSet`** and translates **`SQLException`** into **`DataAccessException`** hierarchy.

---

## Basic queries

```java
int count = jdbcTemplate.queryForObject("select count(*) from users", Integer.class);

List<String> names = jdbcTemplate.query(
    "select name from users where active = ?",
    (rs, rowNum) -> rs.getString("name"),
    true);
```

---

## Updates

```java
jdbcTemplate.update("update users set last_login = ? where id = ?", Instant.now(), userId);
```

---

## `NamedParameterJdbcTemplate`

Named parameters improve readability for queries with many parameters:

```java
MapSqlParameterSource p = new MapSqlParameterSource()
    .addValue("id", id)
    .addValue("status", "ACTIVE");
namedTemplate.update("update orders set status = :status where id = :id", p);
```

---

## Batch operations

`jdbcTemplate.batchUpdate` for bulk inserts/updates.

---

## Transactions

Pair with **`DataSourceTransactionManager`** and `@Transactional` on service layer.

---

## Spring Framework 6+ `JdbcClient`

Fluent API over `JdbcTemplate` — see Boot doc in this repo.

---

## Next

- [Spring ORM and JPA Integration](25-Spring-ORM-and-JPA-Integration.md)
