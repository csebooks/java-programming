---
title: 'Setup'
weight: 1
--- 

Here’s a simplified and cleaner version of your setup instructions. I’ve kept all the original content but made the language and formatting easier to follow:

---

## 🧪 Getting Started with JDBC – Step by Step

### 1. Clone the Starter Java Project

```shell
git clone https://github.com/techatpark/java-ref.git
cd java-ref
./mvnw clean package
```

---

### 2. Add JDBC Driver (H2) to `pom.xml`

We'll use the H2 database for this example. Add the following dependency to your `pom.xml`:

```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>${h2.version}</version>
</dependency>
```

---

### 3. Add Required Modules

Update your `module-info.java` with:

```java
requires java.sql;
requires java.naming;
```

---

### 4. Create the `user` Table

We’ll store user data using this SQL table:

```sql
CREATE TABLE `user` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `useremail` VARCHAR(255) NOT NULL,
    `password` VARCHAR(255) NOT NULL,
    `role` VARCHAR(50),
    PRIMARY KEY (`id`)
);
```

---

### 5. Create the `User` Java Record

Create the file: `src/main/java/com/techatpark/model/User.java`

```java
package com.techatpark.model;

/**
 * User record to store users.
 */
public record User(Integer id,
                   String useremail,
                   String password,
                   String role) {
}
```

---

### 6. Create the `UserDao` Interface

Create the file: `src/main/java/com/techatpark/dao/UserDao.java`

```java
package com.techatpark.dao;

import com.techatpark.model.User;

import java.util.List;
import java.util.Optional;

/**
 * Data Access Object for users table.
 */
public interface UserDao {
    User save(User user);
    List<User> findAll();
    Optional<User> findById(int id);
    void deleteById(int id);
    Optional<User> findByUseremail(String useremail);
    boolean existsByUseremail(String useremail);
    long count();
}
```

---

### 7. Implement the `UserDaoImpl` Class

Create the file: `src/main/java/com/techatpark/dao/impl/UserDaoImpl.java`

```java
package com.techatpark.dao.impl;

import com.techatpark.dao.UserDao;
import com.techatpark.model.User;

import javax.sql.DataSource;
import java.util.List;
import java.util.Optional;

/**
 * JDBC Implementation of UserDao.
 */
public class UserDaoImpl implements UserDao {

    private final DataSource dataSource;

    public UserDaoImpl(final DataSource theDataSource) {
        this.dataSource = theDataSource;
    }

    @Override
    public User save(final User user) {
        return null;
    }

    @Override
    public List<User> findAll() {
        return List.of();
    }

    @Override
    public Optional<User> findById(final int id) {
        return Optional.empty();
    }

    @Override
    public void deleteById(final int id) {
    }

    @Override
    public Optional<User> findByUseremail(final String useremail) {
        return Optional.empty();
    }

    @Override
    public boolean existsByUseremail(final String useremail) {
        return false;
    }

    @Override
    public long count() {
        return 0;
    }
}
```


### 8. Write a Basic Test for DAO

Create the file: `src/test/java/com/techatpark/dao/UserDaoTest.java`

```java
package com.techatpark.dao;

import com.techatpark.dao.impl.UserDaoImpl;
import com.techatpark.model.User;
import org.h2.jdbcx.JdbcDataSource;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;

class UserDaoTest {

    private final UserDao userDao;
    private User sampleUser;

    UserDaoTest() throws SQLException {
        JdbcDataSource ds = new JdbcDataSource();
        ds.setURL("jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1");
        ds.setUser("sa");
        ds.setPassword("");

        try (Connection conn = ds.getConnection();
             Statement stmt = conn.createStatement()) {
            stmt.execute("""
                CREATE TABLE `user` (
                    id INT AUTO_INCREMENT PRIMARY KEY,
                    useremail VARCHAR(255) UNIQUE NOT NULL,
                    password VARCHAR(255),
                    role VARCHAR(50)
                )
            """);
        }

        userDao = new UserDaoImpl(ds);
    }

    @BeforeEach
    void init() {
        sampleUser = new User(null, "Madasamy", "Password", "Employee");
    }

    @Test
    void save() {
        System.out.printf("Hello Test");
    }
}
```


Next up: **Let’s implement the actual DAO logic**.
