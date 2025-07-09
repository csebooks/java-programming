---
title: 'Setup'
weight: 1
--- 

Lets Create a project for jdbc practice

```shell
git clone https://github.com/example/java-ref.git
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

### Records

Create the file: `src/main/java/com/example/model/Student.java`

```java
public record Student(Integer id, String name) {}
```

Create the file: `src/main/java/com/example/model/Mark.java`

```java
public record Mark(String subject, Integer score) {}
```

Create the file: `src/main/java/com/example/model/MarkSheet.java`

```java
public record MarkSheet(Student student, List<Mark> marks) {}
```

### Data Access Object

Create the file: `src/main/java/com/example/dao/StudentDao.java`

```java
public class StudentDao {

    private final DataSource dataSource;

    public StudentDao(final DataSource theDataSource) {
        this.dataSource = theDataSource;
    }

    public Student save(final Student student) throws SQLException {
        // New Student
        if(student.id() == null) {
            final String insertSql = "INSERT INTO student (name) VALUES (?)";
            throw new UnsupportedOperationException("Insert is yet to be implemented");
        } else { // Existing student
            final String updateSql = "UPDATE student SET name = ? WHERE id = ?";
            throw new UnsupportedOperationException("Update is yet to be implemented");
        }

    }

    public List<Student> findAll() throws SQLException {
        final String selectSql = "SELECT * FROM student";
        throw new UnsupportedOperationException("findAll is yet to be implemented");
    }

    public Optional<Student> findById(final int id) throws SQLException {
        final String selectSql = "SELECT * FROM student where id = ?";
        throw new UnsupportedOperationException("findById is yet to be implemented");
    }

    public void deleteById(final int id) throws SQLException {
        final String deleteSql = "DELETE FROM student WHERE id = ?";
        throw new UnsupportedOperationException("deleteById is yet to be implemented");
    }

    public void deleteAll() throws SQLException {
        final String deleteSql = "DELETE FROM student";
        throw new UnsupportedOperationException("deleteAll is yet to be implemented");
    }

    public long count() throws SQLException {
        final String countSql = "SELECT COUNT(*) FROM student";
        throw new UnsupportedOperationException("count is yet to be implemented");
    }

    public void createAll(List<Student> students) throws SQLException {
        throw new UnsupportedOperationException("createAll is yet to be implemented");
    }

    public boolean create(MarkSheet marksheet) {
        throw new UnsupportedOperationException("create Marksheet is yet to be implemented");
    }
}
```

### Data Access Object Test

Create the file: `src/test/java/com/example/dao/StudentDaoTest.java`

```java
class StudentDaoTest {

    private final StudentDao studentDao;

    StudentDaoTest() throws SQLException {
        JdbcDataSource ds = new JdbcDataSource();
        ds.setURL("jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1");
        ds.setUser("sa");

        try (Connection conn = ds.getConnection();
             Statement stmt = conn.createStatement()) {
            stmt.execute("""
                CREATE TABLE student (
                id INT AUTO_INCREMENT PRIMARY KEY,
                name VARCHAR(255) NOT NULL
                );
                CREATE TABLE mark (
                    student_id INT,
                    subject VARCHAR(255) NOT NULL,
                    mark INT NOT NULL,
                    PRIMARY KEY (student_id, subject),
                    FOREIGN KEY (student_id) REFERENCES student(id)
                );
            """);
        }

        studentDao = new StudentDao(ds);
    }

    @AfterEach
    void cleanUp() throws SQLException {
        studentDao.deleteAll();
    }
}
```

Next up: **Let’s implement the actual DAO logic**.
