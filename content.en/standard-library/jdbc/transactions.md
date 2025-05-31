---
title: 'Transactions'
weight: 3
--- 

> In production systems, **atomicity** is critical. What if you want to insert multiple users and one of them violates a constraint? Without a transaction, you'd end up with **partial data** — a serious integrity issue. Transactions solve this by allowing you to **commit** or **rollback** changes as a group.

---

## ✅ Scenario: Atomic Insert of Multiple Users

Let’s say we want to insert two users — **both must be saved, or neither**. This is a perfect case for transaction handling.

---

## 🧪 Test Case 1: Successful Transaction

```java
@Test
void shouldSaveAllUsersInOneTransaction() {
    var users = List.of(
        new User(null, "t1@mail.com", "p1", "USER"),
        new User(null, "t2@mail.com", "p2", "ADMIN")
    );

    var saved = userDao.saveAllAtomic(users);

    assertEquals(2, saved.size());
}
```

---

## 🧪 Test Case 2: Rollback on Constraint Violation

```java
@Test
void shouldRollbackWhenAnyUserFails() {
    // Precondition: Create a user with this email so we get a conflict
    userDao.save(new User(null, "existing@mail.com", "x", "USER"));

    var users = List.of(
        new User(null, "valid@mail.com", "v", "USER"),
        new User(null, "existing@mail.com", "e", "ADMIN") // duplicate
    );

    assertThrows(RuntimeException.class, () -> userDao.saveAllAtomic(users));

    // Verify: nothing inserted
    assertEquals(1, userDao.count());
}
```

---

## ✅ Method: `saveAllAtomic(List<User>)`

```java
@Override
public List<User> saveAllAtomic(final List<User> users) {
    String sql = "INSERT INTO user (useremail, password, role) VALUES (?, ?, ?)";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS)) {

        conn.setAutoCommit(false); // Begin transaction

        for (User user : users) {
            stmt.setString(1, user.useremail());
            stmt.setString(2, user.password());
            stmt.setString(3, user.role());
            stmt.addBatch();
        }

        stmt.executeBatch(); // May throw exception on constraint violation

        List<User> savedUsers = new ArrayList<>();
        try (ResultSet rs = stmt.getGeneratedKeys()) {
            int index = 0;
            while (rs.next()) {
                int id = rs.getInt(1);
                User original = users.get(index++);
                savedUsers.add(new User(id, original.useremail(), original.password(), original.role()));
            }
        }

        conn.commit(); // Commit transaction
        return savedUsers;

    } catch (SQLException e) {
        try {
            dataSource.getConnection().rollback(); // Rollback on error
        } catch (SQLException ignore) {}
        throw new RuntimeException("Transaction failed", e);
    }
}
```

---

## 🔍 Why This Matters

Without `conn.setAutoCommit(false)`, every insert is committed **immediately**. If one insert fails, the others stay in the DB — violating the "all-or-nothing" guarantee.

| Step                   | What It Does                  |
| ---------------------- | ----------------------------- |
| `setAutoCommit(false)` | Starts a manual transaction   |
| `executeBatch()`       | Executes all inserts together |
| `commit()`             | Finalizes all changes         |
| `rollback()`           | Undoes everything on error    |

---

## ✅ Summary

* Transactions ensure **atomic operations**
* Use them when:

  * Multiple rows are involved
  * Data consistency is critical
  * Any failure should **abort all changes**


