---
title: 'Batch Operations'
weight: 3
--- 

> Batching allows you to **group multiple SQL statements** and send them to the database in **one round trip**, rather than sending each individually.



##  Why Use Batching?

* Reduces **network latency**
* Improves **write performance**

```java
public void createAll(List<Student> newStudents) throws SQLException {
    final String insertSQL = "INSERT INTO student(name) VALUES (?)";

        try(Connection connection = dataSource.getConnection();
            PreparedStatement preparedStatement = connection.prepareStatement(insertSQL)) {
            for (Student student:newStudents) {
                preparedStatement.setString(1, student.name());
                
                preparedStatement.addBatch();
            }

            preparedStatement.executeBatch();
    }
}
```

### ❌ **You cannot mix different SQL queries in a single `PreparedStatement` batch**.

Each `PreparedStatement` is bound to **one SQL query**.

---

### 🔍 What does that mean?

This is **invalid**:

```java
PreparedStatement stmt = conn.prepareStatement("INSERT INTO Movie (title) VALUES (?)");
stmt.setString(1, "Inception");
stmt.addBatch();

stmt = conn.prepareStatement("INSERT INTO User (username) VALUES (?)");
stmt.setString(1, "nexa");
stmt.addBatch();  // ❌ Wrong: new statement, separate batch
```

Each `addBatch()` belongs only to its own statement.

---

### ✅ What you CAN do

#### Option 1: **Same SQL → One Batch**

```java
PreparedStatement stmt = conn.prepareStatement("INSERT INTO Movie (title) VALUES (?)");
stmt.setString(1, "A");
stmt.addBatch();
stmt.setString(1, "B");
stmt.addBatch();
stmt.executeBatch();  // ✅ all for Movie
```

#### Option 2: **Different SQL → Separate Statements**

```java
PreparedStatement movieStmt = conn.prepareStatement("INSERT INTO Movie (title) VALUES (?)");
PreparedStatement userStmt = conn.prepareStatement("INSERT INTO User (username) VALUES (?)");

movieStmt.setString(1, "Inception");
movieStmt.addBatch();

userStmt.setString(1, "nexa");
userStmt.addBatch();

movieStmt.executeBatch(); // ✅ movie batch
userStmt.executeBatch();  // ✅ user batch
```

#### Option 3: **Use `Statement` (not `PreparedStatement`) for different queries**

```java
Statement stmt = conn.createStatement();
stmt.addBatch("INSERT INTO Movie (title) VALUES ('Inception')");
stmt.addBatch("INSERT INTO User (username) VALUES ('nexa')");
stmt.executeBatch();  // ✅ allowed because Statement is free-form
```

> ⚠️ But using `Statement` is unsafe for dynamic inputs (SQL injection risk).
> Use `PreparedStatement` for safety.

---

### ✅ Summary

| Batch With          | Can Mix Different SQL? | Safe for User Input? |
| ------------------- | ---------------------- | -------------------- |
| `PreparedStatement` | ❌ No                   | ✅ Yes                |
| `Statement`         | ✅ Yes                  | ❌ No (if dynamic)    |



All good ?!. What about we get an invalid student in between

```java
@Test
void testCreateAll() throws SQLException {
    List<Student> students = new ArrayList<>();

    for (int i = 0; i < 50; i++) {
        students.add(new Student(null, "madasamy"));
    }
    // Insering Invalid Record
    students.set(4, new Student(null, null));

    long time = System.currentTimeMillis();
    try {
            studentDao.createAll(students);
            Assertions.assertEquals(50, studentDao.count(), "Multiple Student Creation Failed");
    } catch (SQLException e) {
        Assertions.assertEquals(0, studentDao.count(), "Multiple Student Creation corrupted the DB");
    }
    System.out.println("Created in " + (System.currentTimeMillis() - time) + " milliseconds");
}
```

Ideally you will expect eigther add all or nothing. How to do that. Lets see next


