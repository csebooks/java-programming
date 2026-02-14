---
title: 'Transactions'
weight: 4
--- 

> In production systems, **atomicity** is critical. Imagine you’re inserting multiple records — what if one fails? Should the others remain? The answer is a resounding **no**. You want **all-or-nothing** behavior. That’s where **transactions** come into play.

Let’s say we want to create a **Marksheet** for a student named *Madasamy*, who scored:

| Subject | Marks |
| ------- | ----- |
| DS      | 100   |
| DBA     | 98    |
| OS      | 75    |

This involves two tables:

* `student`: stores the student's name
* `mark`: stores the subject-wise marks with a foreign key to the student

Without a transaction, if one insert fails (say, due to a null or constraint violation), you could end up with **orphan student records** — corrupting the database state.

## Success Test Case

```java
@Test
void testSuccessfulMarksheetInsertion() {
    StudentDao studentDao = new StudentDao(dataSource);

    Student student = new Student(null, "Madasamy");
    List<Mark> marks = List.of(
        new Mark("DS", 100),
        new Mark("DBA", 98),
        new Mark("OS", 75)
    );

    MarkSheet marksheet = new MarkSheet(student, marks);
    boolean result = studentDao.create(marksheet);

    assertTrue(result); // Everything should be inserted
}
```

Remember we now have dtudent ids referred by mark table. so before we delete students we need to delete their marks first

```java
public void deleteAll() throws SQLException {
    final String deleteMarksSql = "DELETE FROM mark";
    try(Connection connectionection= dataSource.getConnection();
        PreparedStatement preparedStatement = connectionection.prepareStatement(deleteMarksSql)) {
        preparedStatement.executeUpdate();
    }

    final String deleteSql = "DELETE FROM student";
    try(Connection connectionection= dataSource.getConnection();
        PreparedStatement preparedStatement = connectionection.prepareStatement(deleteSql)) {
        preparedStatement.executeUpdate();
    }
}
```

## Failing Test Case (Rollback Demo)

```java
@Test
void testMarksheetInsertionFailsAndRollsBack() throws SQLException {

    Student student = new Student(null, "Parthiban");
    List<Mark> marks = List.of(
            new Mark("DS", 100),
            new Mark(null, 10), // This will violate NOT NULL constraint
            new Mark("OS", 75)
    );

    MarkSheet marksheet = new MarkSheet(student, marks);
    boolean result = studentDao.create(marksheet);

    assertFalse(result); // Should fail and rollback

    // Ensure student record is not present (i.e., rollback worked)
    try (Connection conn = ds.getConnection();
            PreparedStatement stmt = conn.prepareStatement("SELECT COUNT(*) FROM student WHERE name = ?")) {
        stmt.setString(1, "Parthiban");

        try (ResultSet rs = stmt.executeQuery()) {
            rs.next();
            int count = rs.getInt(1);
            assertEquals(0, count); // ✅ Ensure rollback worked
        }
    }
}
```


## Implementation

```java
public boolean create(MarkSheet marksheet) {
    try (Connection conn = dataSource.getConnection()) {
        conn.setAutoCommit(false); // Start transaction

        // Insert student
        String insertStudent = "INSERT INTO student(name) VALUES (?)";
        try (PreparedStatement stmt = conn.prepareStatement(insertStudent, Statement.RETURN_GENERATED_KEYS)) {
            stmt.setString(1, marksheet.student().name());
            stmt.executeUpdate();

            try (ResultSet rs = stmt.getGeneratedKeys()) {
                if (rs.next()) {
                    int studentId = rs.getInt(1);

                    // Insert marks
                    String insertMark = "INSERT INTO mark(student_id, subject, mark) VALUES (?, ?, ?)";
                    try (PreparedStatement markStmt = conn.prepareStatement(insertMark)) {
                        for (Mark mark : marksheet.marks()) {
                            markStmt.setInt(1, studentId);
                            markStmt.setString(2, mark.subject());
                            markStmt.setInt(3, mark.score());
                            markStmt.addBatch();
                        }
                        markStmt.executeBatch();
                    }

                    conn.commit(); // All good!
                    return true;
                } else {
                    conn.rollback();
                    return false;
                }
            }
        } catch (SQLException e) {
            conn.rollback(); // Something went wrong, rollback everything
            e.printStackTrace();
            return false;
        }

    } catch (SQLException e) {
        e.printStackTrace();
        return false;
    }
}

```

## How It Works

1. **`conn.setAutoCommit(false)`** – disables auto-commit, so we have manual control.
2. **Insert student** and **get generated ID**.
3. **Insert all subject marks** using a batch operation.
4. If **all inserts succeed**, call `conn.commit()` to persist changes.
5. If **any insert fails**, call `conn.rollback()` to undo everything.


## Why Transactions Matter

* Prevents **partial inserts** or **orphaned data**
* Ensures **data consistency** during failures
* Allows **batch operations** to be treated as a single logical unit


## Best Practices

| Principle      | JDBC Tip                                 |
| -------------- | ---------------------------------------- |
| Atomicity      | Use `conn.setAutoCommit(false)`          |
| Error Handling | Catch exceptions, then `conn.rollback()` |
| Final Success  | Call `conn.commit()` only if all succeed |
| Avoiding Leaks | Use try-with-resources for all JDBC APIs |


JDBC transactions are **not optional** when dealing with multi-step operations. Whether you’re inserting a `MarkSheet`, processing an e-commerce order, or handling user registration — you need to ensure either **everything happens**, or **nothing does**.

