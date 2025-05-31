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
        <scope>test</scope>
    </dependency>
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

we need a java record to store this in the jvm. Create ``

```java
public record User(
    int id,
    String useremail,
    String password,
    String role
) {}
```

we need a java dao (Data Access Object) to store this in the jvm. Create ``

```java
import java.util.List;
import java.util.Optional;

public interface UserDao {

    // Save a user (create or update)
    User save(User user);

    // Find all users
    List<User> findAll();

    // Find a user by ID
    Optional<User> findById(int id);

    // Delete a user by ID
    void deleteById(int id);

    // Find a user by email
    Optional<User> findByUseremail(String useremail);

    // Check existence by email
    boolean existsByUseremail(String useremail);

    // Count all users
    long count();
}
```

We will use below testcase to test the dao

```java
import org.junit.jupiter.api.*;
import java.sql.*;
import java.util.*;

import static org.junit.jupiter.api.Assertions.*;

public class UserDaoTest {

    private static Connection connection;
    private static UserDaoImpl userDao;

    @BeforeAll
    static void setup() throws SQLException {
        connection = DriverManager.getConnection("jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1");
        Statement stmt = connection.createStatement();
        stmt.execute("""
            CREATE TABLE user (
                id INT AUTO_INCREMENT PRIMARY KEY,
                useremail VARCHAR(255) UNIQUE NOT NULL,
                password VARCHAR(255),
                role VARCHAR(50)
            )
        """);
        userDao = new UserDaoImpl(connection);
    }

    @Test
    void testSaveAndFindById() {
        User saved = userDao.save(new User(0, "test@example.com", "secret", "user"));
        assertNotNull(saved);
        assertTrue(saved.id() > 0);

        Optional<User> found = userDao.findById(saved.id());
        assertTrue(found.isPresent());
        assertEquals("test@example.com", found.get().useremail());
    }

    @Test
    void testFindAllAndCount() {
        userDao.save(new User(0, "a@example.com", "pass", "user"));
        userDao.save(new User(0, "b@example.com", "pass", "admin"));

        List<User> all = userDao.findAll();
        assertTrue(all.size() >= 2);

        long count = userDao.count();
        assertEquals(all.size(), count);
    }

    @Test
    void testExistsByUseremail() {
        userDao.save(new User(0, "exists@example.com", "pass", "user"));
        assertTrue(userDao.existsByUseremail("exists@example.com"));
        assertFalse(userDao.existsByUseremail("notfound@example.com"));
    }

    @Test
    void testDeleteById() {
        User u = userDao.save(new User(0, "delete@example.com", "pass", "user"));
        userDao.deleteById(u.id());

        Optional<User> deleted = userDao.findById(u.id());
        assertTrue(deleted.isEmpty());
    }

    @AfterAll
    static void cleanup() throws SQLException {
        connection.createStatement().execute("DROP TABLE user");
        connection.close();
    }
}
```