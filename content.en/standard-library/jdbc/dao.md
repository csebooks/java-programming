---
title: 'Data Access Methods'
weight: 2
--- 

In this chapter, we’ll **write a test case first**, then **implement the corresponding DAO method using JDBC**. This TDD-style flow will help reinforce *intentional design* and *clear validation*.

## Create User

### Insert New User

```java
@Test
void save() {
    // 1. Construct a new User
    User user = new User(null, "Madasamy", "Password", "Employee");

    // 2. Create the user in the DB
    User createdUser = userDao.save(user);

    // 3. Verify that User is has auto generated id.
    Assertions.assertNotNull(createdUser.id(), "User Creation Failed");
}
```

### Implementation

```java
public User save(final User user) throws SQLException {

    User createdUser = null;
        
    String insertSql = "INSERT INTO `user` (useremail, password, role) VALUES (?, ?, ?)";

    try (Connection conn = dataSource.getConnection();
            PreparedStatement stmt = conn.prepareStatement(insertSql, Statement.RETURN_GENERATED_KEYS)) {

        stmt.setString(1, user.useremail());
        stmt.setString(2, user.password());
        stmt.setString(3, user.role());

        // Talk to the Database and Execute the SQL.
        stmt.executeUpdate();

        // Obtain User Id
        try (ResultSet rs = stmt.getGeneratedKeys()) {
            if (rs.next()) {
                createdUser = new User(rs.getInt(1), user.useremail(), user.password(), user.role());
            }
        }
    }

    return createdUser;
}
```

## 🔍 Update User

```java
    @Test
    void save() throws SQLException {
        // 1. Construct a new User
        User user = new User(null, "Madasamy@Email.com", "Password", "Employee");

        // 2. Create the user in the DB
        User createdUser = userDao.save(user);

        // 3. Verify that User is has auto generated id.
        Assertions.assertNotNull(createdUser.id(), "User Creation Failed");

        // 4. Change Email Value
        User userToUpdate = new User(createdUser.id(), "Mithra@Email.com", createdUser.password(), createdUser.role());

        // 5. Update the user in the DB
        User updatedUser = userDao.save(userToUpdate);

        // 6. Verify that User has updated Email
        Assertions.assertEquals("Mithra@Email.com",updatedUser.useremail(), "User Update Failed");

    }
```

### Implementation

```java
public User save(final User user) throws SQLException {

        User createdUser = null;

        // New User
        if (user.id() == null) {
            String insertSql = "INSERT INTO `user` (useremail, password, role) VALUES (?, ?, ?)";

            try (Connection conn = dataSource.getConnection();
                 PreparedStatement stmt = conn.prepareStatement(insertSql, Statement.RETURN_GENERATED_KEYS)) {

                stmt.setString(1, user.useremail());
                stmt.setString(2, user.password());
                stmt.setString(3, user.role());

                // Talk to the Database and Execute the SQL.
                stmt.executeUpdate();

                // Obtain User Id
                try (ResultSet rs = stmt.getGeneratedKeys()) {
                    if (rs.next()) {
                        createdUser = new User(rs.getInt(1), user.useremail(), user.password(), user.role());
                    }
                }
            }
        } else { // Existing User
            String updateSql = "UPDATE `user` SET useremail = ?, password = ?, role = ? WHERE id = ?";

            try (Connection conn = dataSource.getConnection();
                 PreparedStatement stmt = conn.prepareStatement(updateSql)) {

                stmt.setString(1, user.useremail());
                stmt.setString(2, user.password());
                stmt.setString(3, user.role());

                stmt.setInt(4, user.id());

                stmt.executeUpdate();

                createdUser = new User(user.id(), user.useremail(), user.password(), user.role());
            }
        }

        return createdUser;
    }
```

---

## Retrieve All Users

### Test Case

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

## Retrive an User

### Test Case

```java
@Test
void shouldFindUserById() {
    var saved = userDao.save(new User(null, "chris@mail.com", "123", "USER"));

    var result = userDao.findById(saved.id());

    assertTrue(result.isPresent());
    assertEquals("chris@mail.com", result.get().useremail());
}
```

### Implementation

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

## Delete an User

### Test Case

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

## No of Users

### Test Case

```java
@Test
void shouldReturnUserCount() {
    userDao.save(new User(null, "one@mail.com", "1", "USER"));
    userDao.save(new User(null, "two@mail.com", "2", "ADMIN"));

    assertEquals(2, userDao.count());
}
```

### Implementation

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

