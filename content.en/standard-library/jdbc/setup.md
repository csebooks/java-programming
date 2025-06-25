---
title: 'Setup'
weight: 1
--- 

Lets Create a project for jdbc practice

```shell
git clone https://github.com/techatpark/java-ref.git
cd java-ref
./mvnw clean package
```

### JDBC Driver

We'll use in-memory and embedded database `H2 database` for simplicity. In realtime projects you will be using clinet server databases like `postgress`.

Add the following dependency to your `pom.xml`:

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>${h2.version}</version>
</dependency>
```

### Modules

Update your `module-info.java` with:

```java
requires java.sql;
requires java.naming;
```

### `User` Record

Create the file: `src/main/java/com/techatpark/model/User.java`

```java
package com.techatpark.model;

public record User(Integer id,
               String useremail,
               String role) {
}
```

### Data Access Object

Create the file: `src/main/java/com/techatpark/dao/UserDao.java`

```java
public class UserDao {

    private final DataSource dataSource;

    public UserDao(final DataSource theDataSource) {
        this.dataSource = theDataSource;
    }

    public User save(final User user) throws SQLException {
        // New User
        if(user.id() == null) {
            final String insertSql = "INSERT INTO `user` (useremail, role) VALUES (?, ?)";
            throw new UnsupportedOperationException("Insert is yet to be implemented");
        } else { // Existing user0
            final String updateSql = "UPDATE `user` SET useremail = ?, role = ? WHERE id = ?";
            throw new UnsupportedOperationException("Update is yet to be implemented");
        }

    }

    public List<User> findAll() throws SQLException {
        final String selectSql = "SELECT * FROM `user`";
        throw new UnsupportedOperationException("findAll is yet to be implemented");
    }

    public Optional<User> findById(final int id) throws SQLException {
        final String selectSql = "SELECT * FROM `user` where id = ?";
        throw new UnsupportedOperationException("findById is yet to be implemented");
    }

    public void deleteById(final int id) throws SQLException {
        final String deleteSql = "DELETE FROM `user` WHERE id = ?";
        throw new UnsupportedOperationException("deleteById is yet to be implemented");
    }

    public void deleteAll() throws SQLException {
        final String deleteSql = "DELETE FROM `user`";
        throw new UnsupportedOperationException("deleteAll is yet to be implemented");
    }

    public long count() throws SQLException {
        final String countSql = "SELECT COUNT(*) FROM `user`";
        throw new UnsupportedOperationException("count is yet to be implemented");
    }
}
```

### Data Access Object Test

Create the file: `src/test/java/com/techatpark/dao/UserDaoTest.java`

```java
import org.h2.jdbcx.JdbcDataSource;
import org.junit.jupiter.api.AfterEach;

import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;

class UserDaoTest {

    private final UserDao userDao;

    UserDaoTest() throws SQLException {
        JdbcDataSource ds = new JdbcDataSource();
        ds.setURL("jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1");
        ds.setUser("sa");

        try (Connection conn = ds.getConnection();
             Statement stmt = conn.createStatement()) {
            stmt.execute("""
                CREATE TABLE `user` (
                    id INT AUTO_INCREMENT PRIMARY KEY,
                    useremail VARCHAR(255) NOT NULL,
                    role VARCHAR(50)
                )
            """);
        }

        userDao = new UserDao(ds);
    }

    @AfterEach
    void cleanUp() throws SQLException {
        userDao.deleteAll();
    }
}
```

Next up: **Let’s implement the actual DAO logic**.
