---
title: 'Transactions'
weight: 4
--- 

> In production systems, **atomicity** is critical. What if you want to insert multiple students and one of them violates a constraint? Without a transaction, you'd end up with **partial data** — a serious integrity issue. Transactions solve this by allowing you to **commit** or **rollback** changes as a group.

---

## Scenario: Atomic Insert of Multiple Students

Let’s say we want to insert two students — **both must be saved, or neither**. This is a perfect case for transaction handling.

---

##  Test Case 1: Successful Transaction

```java
@Test
void shouldSaveAllStudentsInOneTransaction() {
    var students = List.of(
        new Student(null, "keerthanasri", "p1"),
        new Student(null, "sathashini", "p2")
    );

    var saved = studentDao.saveAllAtomic(students);

    assertEquals(2, saved.size());
}
```

---

## Test Case 2: Rollback on Constraint Violation

```java
@Test
void shouldRollbackWhenAnyStudentFails() {
    // Precondition: Create a student with this email so we get a conflict
    studentDao.save(new Student(null, "sathashini", "x"));

    var students = List.of(
        new Student(null, "guruprasath", "v"),
        new Student(null, "maadasamy", "e") // duplicate
    );

    assertThrows(RuntimeException.class, () -> studentDao.saveAllAtomic(students));

    // Verify: nothing inserted
    assertEquals(1, studentDao.count());
}
```

---

## Method: `saveAllAtomic(List<Student>)`

```java
@Override
public List<Student> saveAllAtomic(final List<Student> students) {
    String sql = "INSERT INTO student (name, password) VALUES (?, ?)";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS)) {

        conn.setAutoCommit(false); // Begin transaction

        for (Student student : students) {
            stmt.setString(1, student.name());
            stmt.setString(2, student.password());
            
            stmt.addBatch();
        }

        stmt.executeBatch(); // May throw exception on constraint violation

        List<Student> savedStudents = new ArrayList<>();
        try (ResultSet rs = stmt.getGeneratedKeys()) {
            int index = 0;
            while (rs.next()) {
                int id = rs.getInt(1);
                Student original = students.get(index++);
                savedStudents.add(new Student(id, original.name(), original.password()));
            }
        }

        conn.commit(); // Commit transaction
        return savedStudents;

    } catch (SQLException e) {
        try {
            dataSource.getConnection().rollback(); // Rollback on error
        } catch (SQLException ignore) {}
        throw new RuntimeException("Transaction failed", e);
    }
}
```

---

##  Why This Matters

Without `conn.setAutoCommit(false)`, every insert is committed **immediately**. If one insert fails, the others stay in the DB — violating the "all-or-nothing" guarantee.

| Step                   | What It Does                  |
| ---------------------- | ----------------------------- |
| `setAutoCommit(false)` | Starts a manual transaction   |
| `executeBatch()`       | Executes all inserts together |
| `commit()`             | Finalizes all changes         |
| `rollback()`           | Undoes everything on error    |

---

## Summary

* Transactions ensure **atomic operations**
* Use them when:

  * Multiple rows are involved
  * Data consistency is critical
  * Any failure should **abort all changes**


