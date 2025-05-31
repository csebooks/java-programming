---
title: 'Data Access Methods'
weight: 2
--- 

In this chapter, we’ll **write a test case first**, then **implement the corresponding DAO method using JDBC**. This TDD-style flow will help reinforce *intentional design* and *clear validation*.

## Create User

### Insert New User

```java
@Test
void testsave() throws SQLException {
    // 1. Create an User
    User user = userDao.save(new User(null, "madasamy@email.com","Employee"));

    // 2. User should have a generated id
    Assertions.assertNotNull(user.id(), "User Creation Failed");
}
```

### Implementation

```java
public User save(final User user) throws SQLException {
    User createdUser = null;

    // New User
    if(user.id() == null) {
        String insertSQL = "INSERT INTO `user`(useremail, role) VALUES (?,?)";

        try(Connection connection = dataSource.getConnection();
            PreparedStatement ps = connection.prepareStatement(insertSQL, Statement.RETURN_GENERATED_KEYS)) {
            ps.setString(1, user.useremail());
            ps.setString(2, user.role());

            ps.executeUpdate();

            try (ResultSet rs = ps.getGeneratedKeys()) {
                if (rs.next()) {
                    createdUser = new User(rs.getInt(1), user.useremail(), user.role());
                }
            }
        }
    } else { // Existing User
        throw new UnsupportedOperationException("Update is yet to be implemented");
    }

    return createdUser;
}
```

## Update User

```java
@Test
void testsave() throws SQLException {
    // 1. Create an User
    User user = userDao.save(new User(null, "madasamy@email.com","Employee"));

    // 2. User should have a generated id
    Assertions.assertNotNull(user.id(), "User Creation Failed");

    // 3. Update the user email in the DB
    User updatedUser = userDao.save(new User(user.id(), "Mithra@Email.com", user.role()));

    // 4. Verify that User is the same
    Assertions.assertEquals(updatedUser.id(),user.id(), "User Update Failed");

    // 5. Verify that User has updated Email
    Assertions.assertEquals("Mithra@Email.com",updatedUser.useremail(), "User Update Failed");
}
```

### Implementation

```java
public User save(final User user) throws SQLException {
    User createdUser = null;

    // New User
    if(user.id() == null) {
        String insertSQL = "INSERT INTO `user`(useremail, role) VALUES (?,?)";

        try(Connection connection = dataSource.getConnection();
            PreparedStatement ps = connection.prepareStatement(insertSQL, Statement.RETURN_GENERATED_KEYS)) {
            ps.setString(1, user.useremail());
            ps.setString(2, user.role());

            ps.executeUpdate();

            try (ResultSet rs = ps.getGeneratedKeys()) {
                if (rs.next()) {
                    createdUser = new User(rs.getInt(1), user.useremail(), user.role());
                }
            }
        }
    } else { // Existing User
        String updateSql = "UPDATE `user` SET useremail = ?, role = ? WHERE id = ?";

        try (Connection conn = dataSource.getConnection();
                PreparedStatement stmt = conn.prepareStatement(updateSql)) {

            stmt.setString(1, user.useremail());
            stmt.setString(2, user.role());

            stmt.setInt(3, user.id());

            stmt.executeUpdate();

            createdUser = new User(user.id(), user.useremail(), user.role());
        }
    }

    return createdUser;
}
```

## No of Users

```java
@Test
void testcount() throws SQLException {
    userDao.save(new User(null, "one@mail.com", "USER"));
    userDao.save(new User(null, "two@mail.com", "ADMIN"));

    assertEquals(2, userDao.count());
}
```

### Implementation

```java
public long count() throws SQLException {
    String sql = "SELECT COUNT(*) FROM user";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql);
         ResultSet rs = stmt.executeQuery()) {
        if (rs.next()) {
            return rs.getLong(1);
        }
    } 
    return 0;
}
```

## Delete All User

```java
@Test
void testDeleteUserById() throws SQLException {
    var user1 = userDao.save(new User(null, "delete@mail.com", "USER"));
    var user2 = userDao.save(new User(null, "delete2@mail.com", "USER"));

    userDao.deleteAll();

    assertEquals(userDao.count(), 0);
}
```

### Implementation

```java
public void deleteAll() throws SQLException {
    String sql = "DELETE FROM user";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {

        stmt.executeUpdate();
    } 
}
```

## Delete an User

```java
@Test
void testDeleteUserById() throws SQLException {
    var user = userDao.save(new User(null, "delete@mail.com", "USER"));

    userDao.deleteById(user.id());

    assertTrue(userDao.findById(user.id()).isEmpty());
}
```

### Implementation

```java
public void deleteById(final int id) throws SQLException {
    String sql = "DELETE FROM user WHERE id = ?";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        stmt.setInt(1, id);
        stmt.executeUpdate();
    } 
}
```

## Retrive an User

```java
@Test
void testFindUserById() throws SQLException {
    var saved = userDao.save(new User(null, "chris@mail.com", "USER"));

    var result = userDao.findById(saved.id());

    assertTrue(result.isPresent());
    assertEquals("chris@mail.com", result.get().useremail());
}
```

### Implementation

```java
public Optional<User> findById(final int id) throws SQLException {
    String sql = "SELECT id, useremail, role FROM user WHERE id = ?";
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        stmt.setInt(1, id);
        try (ResultSet rs = stmt.executeQuery()) {
            if (rs.next()) {
                return Optional.of(new User(
                        rs.getInt("id"),
                        rs.getString("useremail"),
                        rs.getString("role")
                ));
            }
        }
    } 
    return Optional.empty();
}
```

## Retrieve All Users

```java
@Test
void testFindAllUsers() throws SQLException {
    userDao.save(new User(null, "a@mail.com", "USER"));
    userDao.save(new User(null, "b@mail.com", "ADMIN"));

    var users = userDao.findAll();

    assertEquals(2, users.size());
}
```

### Implementation

```java
public List<User> findAll() throws SQLException {
    String sql = "SELECT id, useremail, role FROM user";
    List<User> users = new ArrayList<>();
    try (Connection conn = dataSource.getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql);
         ResultSet rs = stmt.executeQuery()) {
        while (rs.next()) {
            users.add(new User(
                    rs.getInt("id"),
                    rs.getString("useremail"),
                    rs.getString("role")
            ));
        }
    } 
    return users;
}
```


