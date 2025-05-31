---
title: 'Data Access Methods'
weight: 2
--- 

In this chapter, we’ll **write a test case first**, then **implement the corresponding DAO method using JDBC**. This TDD-style flow will help reinforce *intentional design* and *clear validation*.

---

## ✅ `save(User user)` — Save or Update a User

### 🔍 Test Case: Insert New User

```java
@Test
void shouldInsertNewUser() {
    var user = new User(null, "alice@mail.com", "secret", "USER");
    var saved = userDao.save(user);

    assertNotNull(saved.id());
    assertEquals("alice@mail.com", saved.useremail());
}
```

### 🔍 Test Case: Update Existing User

```java
@Test
void shouldUpdateExistingUser() {
    var user = userDao.save(new User(null, "bob@mail.com", "1234", "USER"));
    var updated = new User(user.id(), "bob@mail.com", "newpass", "ADMIN");

    var saved = userDao.save(updated);

    assertEquals("newpass", saved.password());
    assertEquals("ADMIN", saved.role());
}
```

### ✅ Implementation

```java
@Override
public User save(final User user) {
    String sql = user.id() == null ?
            "INSERT INTO user (useremail, password, role) VALUES (?, ?, ?)" :
            "UPDATE user SET useremail = ?, password = ?, role = ? WHERE id = ?";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS)) {

        stmt.setString(1, user.useremail());
        stmt.setString(2, user.password());
        stmt.setString(3, user.role());

        if (user.id() != null) {
            stmt.setInt(4, user.id());
            stmt.executeUpdate();
            return user;
        } else {
            stmt.executeUpdate();
            try (ResultSet rs = stmt.getGeneratedKeys()) {
                if (rs.next()) {
                    return new User(rs.getInt(1), user.useremail(), user.password(), user.role());
                }
            }
        }
    } catch (SQLException e) {
        throw new RuntimeException(e);
    }
    return null;
}
```

---

## 🔍 `findAll()` — Retrieve All Users

### 🧪 Test Case

```java
@Test
void shouldFindAllUsers() {
    userDao.save(new User(null, "a@mail.com", "a", "USER"));
    userDao.save(new User(null, "b@mail.com", "b", "ADMIN"));

    var users = userDao.findAll();

    assertEquals(2, users.size());
}
```

### ✅ Implementation

```java
@Override
public List<User> findAll() {
    String sql = "SELECT id, useremail, password, role FROM user";
    List<User> users = new ArrayList<>();
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql);
         ResultSet rs = stmt.executeQuery()) {
        while (rs.next()) {
            users.add(new User(
                    rs.getInt("id"),
                    rs.getString("useremail"),
                    rs.getString("password"),
                    rs.getString("role")
            ));
        }
    } catch (SQLException e) {
        throw new RuntimeException(e);
    }
    return users;
}
```

---

## 🔍 `findById(int id)`

### 🧪 Test Case

```java
@Test
void shouldFindUserById() {
    var saved = userDao.save(new User(null, "chris@mail.com", "123", "USER"));

    var result = userDao.findById(saved.id());

    assertTrue(result.isPresent());
    assertEquals("chris@mail.com", result.get().useremail());
}
```

### ✅ Implementation

```java
@Override
public Optional<User> findById(final int id) {
    String sql = "SELECT id, useremail, password, role FROM user WHERE id = ?";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        stmt.setInt(1, id);
        try (ResultSet rs = stmt.executeQuery()) {
            if (rs.next()) {
                return Optional.of(new User(
                        rs.getInt("id"),
                        rs.getString("useremail"),
                        rs.getString("password"),
                        rs.getString("role")
                ));
            }
        }
    } catch (SQLException e) {
        throw new RuntimeException(e);
    }
    return Optional.empty();
}
```

---

## 🔍 `deleteById(int id)`

### 🧪 Test Case

```java
@Test
void shouldDeleteUserById() {
    var user = userDao.save(new User(null, "delete@mail.com", "x", "USER"));

    userDao.deleteById(user.id());

    assertTrue(userDao.findById(user.id()).isEmpty());
}
```

### ✅ Implementation

```java
@Override
public void deleteById(final int id) {
    String sql = "DELETE FROM user WHERE id = ?";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        stmt.setInt(1, id);
        stmt.executeUpdate();
    } catch (SQLException e) {
        throw new RuntimeException(e);
    }
}
```

---

## 🔍 `findByUseremail(String email)`

### 🧪 Test Case

```java
@Test
void shouldFindByUseremail() {
    userDao.save(new User(null, "find@mail.com", "123", "USER"));

    var result = userDao.findByUseremail("find@mail.com");

    assertTrue(result.isPresent());
    assertEquals("find@mail.com", result.get().useremail());
}
```

### ✅ Implementation

```java
@Override
public Optional<User> findByUseremail(final String email) {
    String sql = "SELECT id, useremail, password, role FROM user WHERE useremail = ?";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        stmt.setString(1, email);
        try (ResultSet rs = stmt.executeQuery()) {
            if (rs.next()) {
                return Optional.of(new User(
                        rs.getInt("id"),
                        rs.getString("useremail"),
                        rs.getString("password"),
                        rs.getString("role")
                ));
            }
        }
    } catch (SQLException e) {
        throw new RuntimeException(e);
    }
    return Optional.empty();
}
```

---

## 🔍 `existsByUseremail(String email)`

### 🧪 Test Case

```java
@Test
void shouldCheckIfUserExistsByUseremail() {
    userDao.save(new User(null, "check@mail.com", "x", "USER"));

    assertTrue(userDao.existsByUseremail("check@mail.com"));
    assertFalse(userDao.existsByUseremail("ghost@mail.com"));
}
```

### ✅ Implementation

```java
@Override
public boolean existsByUseremail(final String email) {
    String sql = "SELECT 1 FROM user WHERE useremail = ?";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        stmt.setString(1, email);
        try (ResultSet rs = stmt.executeQuery()) {
            return rs.next();
        }
    } catch (SQLException e) {
        throw new RuntimeException(e);
    }
}
```

---

## 🔍 `count()`

### 🧪 Test Case

```java
@Test
void shouldReturnUserCount() {
    userDao.save(new User(null, "one@mail.com", "1", "USER"));
    userDao.save(new User(null, "two@mail.com", "2", "ADMIN"));

    assertEquals(2, userDao.count());
}
```

### ✅ Implementation

```java
@Override
public long count() {
    String sql = "SELECT COUNT(*) FROM user";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql);
         ResultSet rs = stmt.executeQuery()) {
        if (rs.next()) {
            return rs.getLong(1);
        }
    } catch (SQLException e) {
        throw new RuntimeException(e);
    }
    return 0;
}
```

---

## 🎯 Summary

| Feature         | ✅ Status       |
| --------------- | -------------- |
| Insert          | ✔️ Test + Impl |
| Update          | ✔️ Test + Impl |
| Find All        | ✔️ Test + Impl |
| Find by ID      | ✔️ Test + Impl |
| Delete by ID    | ✔️ Test + Impl |
| Find by Email   | ✔️ Test + Impl |
| Exists by Email | ✔️ Test + Impl |
| Count All       | ✔️ Test + Impl |

You’ve now walked through every method in a real-world JDBC `UserDao` using **test-first development**. This practice not only guarantees correctness but also encourages thoughtful design.

---

Would you like the next chapter to focus on:

* **JDBC error handling patterns**, or
* **Transaction and batch insert examples**?
