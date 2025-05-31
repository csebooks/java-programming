---
title: 'Setup'
weight: 1
--- 

Lets start our JDBC journey with the basic java project

```shell
git clone https://github.com/techatpark/java-ref.git
cd java-ref
./mvnw clean package
```

Lets add jdbc driver ( in this case lets use `h2` ) to `pom.xml`

```xml
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <version>${h2.version}</version>
    </dependency>
```

add modules to `module-info`

```java
    requires java.sql;
    requires java.naming;
```

We are planning store user information in our database which is stored at 

```sql
CREATE TABLE `user` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `useremail` VARCHAR(255) NOT NULL,
    `password` VARCHAR(255) NOT NULL,
    `role` VARCHAR(50),
    PRIMARY KEY (`id`)
);
```

we need a java record to store this in the jvm. Create `src/main/java/com/techatpark/model/User.java`

```java
package com.techatpark.model;

/**
 * User record to store users.
 * @param id
 * @param useremail
 * @param password
 * @param role
 */
public record User(int id,
               String useremail,
               String password,
               String role) {
}
```

we need a java dao (Data Access Object) to store this in the jvm. Create `src/main/java/com/techatpark/dao/UserDao.java`

```java
package com.techatpark.dao;

import com.techatpark.model.User;

import java.util.List;
import java.util.Optional;

/**
 * Data Access Object for users table.
 */
public interface UserDao {
    /**
     * Save Users.
     * Update if exists.
     * @param user
     * @return createdUser
     */
    User save(User user);

    /**
     * Find all users.
     * @return users
     */
    List<User> findAll();

    /**
     * Find a user by ID.
     * @param id
     * @return user
     */
    Optional<User> findById(int id);

    /**
     * Delete user with given id.
     * @param id
     */
    void deleteById(int id);

    /**
     * Find User by Email.
     * @param useremail
     * @return user
     */
    Optional<User> findByUseremail(String useremail);

    /**
     * Check existence by email.
     * @param useremail
     * @return isAvailable
     */
    boolean existsByUseremail(String useremail);

    /**
     * Count No of Users.
     * @return userCount
     */
    long count();
}

```

we need a java dao implemenatation. Create `src/main/java/com/techatpark/dao/impl/UserDaoImpl.java`

```java
package com.techatpark.dao.impl;

import com.techatpark.dao.UserDao;
import com.techatpark.model.User;

import javax.sql.DataSource;
import java.util.List;
import java.util.Optional;

/**
 * User Dao JDBC Implementation.
 */
public class UserDaoImpl implements UserDao {

    /**
     * Data source for RDBMS.
     */
    private final DataSource dataSource;

    /**
     * Created UserDao Impl with Datasource.
     * @param theDataSource
     */
    public UserDaoImpl(final DataSource theDataSource) {
        this.dataSource = theDataSource;
    }

    /**
     * Save Users.
     * Update if exists.
     *
     * @param user
     * @return createdUser
     */
    @Override
    public User save(final User user) {
        return null;
    }

    /**
     * Find all users.
     *
     * @return users
     */
    @Override
    public List<User> findAll() {
        return List.of();
    }

    /**
     * Find a user by ID.
     *
     * @param id
     * @return user
     */
    @Override
    public Optional<User> findById(final int id) {
        return Optional.empty();
    }

    /**
     * Delete user with given id.
     *
     * @param id
     */
    @Override
    public void deleteById(final int id) {

    }

    /**
     * Find User by Email.
     *
     * @param useremail
     * @return user
     */
    @Override
    public Optional<User> findByUseremail(final String useremail) {
        return Optional.empty();
    }

    /**
     * Check existence by email.
     *
     * @param useremail
     * @return isAvailable
     */
    @Override
    public boolean existsByUseremail(final String useremail) {
        return false;
    }

    /**
     * Count No of Users.
     *
     * @return userCount
     */
    @Override
    public long count() {
        return 0;
    }
}

```

We will use below testcase to test the dao `src/test/java/com/techatpark/dao/UserDaoTest.java`

```java
package com.techatpark.dao;

import com.techatpark.dao.impl.UserDaoImpl;
import org.h2.jdbcx.JdbcDataSource;
import org.junit.jupiter.api.Test;

import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;

class UserDaoTest {

    private final UserDao userDao;

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

    @Test
    void save() {
        System.out.printf("Hello Test");
    }
}
```