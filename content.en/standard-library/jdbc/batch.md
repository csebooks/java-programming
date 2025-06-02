---
title: 'Batch Operations'
weight: 3
--- 

> Batching allows you to **group multiple SQL statements** and send them to the database in **one round trip**, rather than sending each individually.

---

## ⚡ Why Use Batching?

* Reduces **network latency**
* Improves **write performance**

---

## Use Case: Insert Multiple Users Faster

We’ll add a new method: `saveAllBatch(List<User> users)`

This will insert all users **as a batch** but **without transaction rollback** behavior.

---

## 🧪 Test Case 1: All Inserts Succeed

```java
@Test
void shouldInsertAllUsersUsingBatch() {
    var users = List.of(
        new User(null, "batch1@mail.com", "pass1", "USER"),
        new User(null, "batch2@mail.com", "pass2", "ADMIN")
    );

    var saved = userDao.saveAllBatch(users);

    assertEquals(2, saved.size());
}
```

---

## 🧪 Test Case 2: Partial Inserts on Error

```java
@Test
void shouldInsertPartialDataIfOneFailsInBatch() {
    // Insert one to cause a duplicate
    userDao.save(new User(null, "dupe@mail.com", "pass", "USER"));

    var users = List.of(
        new User(null, "batch-ok@mail.com", "p", "USER"),
        new User(null, "dupe@mail.com", "x", "ADMIN") // duplicate
    );

    assertThrows(RuntimeException.class, () -> userDao.saveAllBatch(users));

    // Only the first one might have been inserted
    assertTrue(userDao.existsByUseremail("batch-ok@mail.com"));
}
```

Notice: Unlike the transaction version, this does **not rollback** already-inserted rows.

---

## Method: `saveAllBatch(List<User>)`

```java
@Override
public List<User> saveAllBatch(final List<User> users) {
    String sql = "INSERT INTO user (useremail, password, role) VALUES (?, ?, ?)";

    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS)) {

        for (User user : users) {
            stmt.setString(1, user.useremail());
            stmt.setString(2, user.password());
            stmt.setString(3, user.role());
            stmt.addBatch();
        }

        stmt.executeBatch(); // Executes all batched inserts

        List<User> savedUsers = new ArrayList<>();
        try (ResultSet rs = stmt.getGeneratedKeys()) {
            int index = 0;
            while (rs.next()) {
                int id = rs.getInt(1);
                User original = users.get(index++);
                savedUsers.add(new User(id, original.useremail(), original.password(), original.role()));
            }
        }

        return savedUsers;

    } catch (SQLException e) {
        throw new RuntimeException("Batch insert failed", e);
    }
}
```

---

## 🔍 Batching vs Transaction

| Feature   | Batch Only               | Batch + Transaction    |
| --------- | ------------------------ | ---------------------- |
| Speed     | Fast                   | ⚠️ Slightly slower     |
| Atomicity | ❌ No rollback            | Rollback supported   |
| Use case  | Bulk insert, logs, audit | Critical multi-row ops |

---

## 🧠 Best Practices

* Use **`executeBatch()`** with `PreparedStatement` to avoid SQL injection
* Use batching with **transactions** when you need **safety**
* Handle exceptions carefully — partial inserts can be tricky

---

## Summary

* Batching is a performance boost for **bulk inserts**
* Combine it with transactions when you need **both speed and safety**
* JDBC makes it easy — just use `.addBatch()` and `.executeBatch()`
