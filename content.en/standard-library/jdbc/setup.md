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

### JDBC Driver (H2)

We'll use the H2 database for this example. Add the following dependency to your `pom.xml`:

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
package com.techatpark.dao;

import com.techatpark.model.User;

import javax.sql.DataSource;
import java.util.List;
import java.util.Optional;

public class UserDao {

    private final DataSource dataSource;

    public UserDao(final DataSource theDataSource) {
        this.dataSource = theDataSource;
    }
    
    public User save(final User user) {
        return null;
    }
    
    public List<User> findAll() {
        return List.of();
    }
    
    public Optional<User> findById(final int id) {
        return Optional.empty();
    }
    
    public void deleteById(final int id) {
    }
    
    public void deleteAll() {
    }
    
    public long count() {
        return 0;
    }
}
```

### Data Access Object Test

Create the file: `src/test/java/com/techatpark/dao/UserDaoTest.java`

```java
package com.techatpark.dao;

import org.h2.jdbcx.JdbcDataSource;
import org.junit.jupiter.api.BeforeEach;

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

    @BeforeEach
    void init() {
        userDao.deleteAll();
    }
}
```

Next up: **Let’s implement the actual DAO logic**.
