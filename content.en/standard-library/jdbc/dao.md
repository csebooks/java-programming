---
title: 'Data Access Methods'
weight: 2
--- 

In this chapter, we’ll **write a test case first**, then **implement the corresponding DAO method using JDBC**. This TDD-style flow will help reinforce *intentional design* and *clear validation*.

## Create User

### Insert New User

```java
@Test
void testSave() throws SQLException {
    // 1. Create an User
    User user = userDao.save(new User(null, "madasamy@email.com","Employee"));

    // 2. User should have a generated id
    Assertions.assertNotNull(user.id(), "User Creation Failed");
}
```

### Implementation

```java
public User save(final User user) throws SQLException {
    if(user != null) {
        if ( user.id() == null ) {
            final String insertSQL = "INSERT INTO `user`(useremail,role) VALUES (?,?)";

            try(Connection connectionection = dataSource.getConnection();
                PreparedStatement preparedStatement = connectionection.prepareStatement(insertSQL, Statement.RETURN_GENERATED_KEYS)) {
                preparedStatement.setString(1, user.useremail());
                preparedStatement.setString(2, user.role());

                preparedStatement.executeUpdate();

                try(ResultSet resultSet = preparedStatement.getGeneratedKeys()) {
                    if(resultSet.next()) {
                        return new User(resultSet.getInt(1),
                                user.useremail(), user.role());
                    }
                }
            }
        } else {
            throw new UnsupportedOperationException("Update is yet to be implemented");
        }
    }
    return null;
}
```

## Delete All User

```java
@Test
void testDeleteAll() throws SQLException {
    userDao.save(new User(null, "delete@mail.com", "USER"));
    userDao.save(new User(null, "delete2@mail.com", "USER"));

    userDao.deleteAll();

    assertEquals(userDao.count(), 0);
}
```

### Implementation

```java
public void deleteAll() throws SQLException {
    final String sql = "DELETE FROM `user`";
    try(Connection connectionection= dataSource.getConnection();
            PreparedStatement preparedStatement = connectionection.prepareStatement(sql)) {
        preparedStatement.executeUpdate();
    }
}
```

## No of Users

```java
@Test
void testCount() throws SQLException {
    userDao.save(new User(null, "one@mail.com", "USER"));
    userDao.save(new User(null, "two@mail.com", "ADMIN"));

    assertEquals(2, userDao.count());
}
```

### Implementation

```java
public long count() throws SQLException {
    String sql = "SELECT COUNT(*) FROM user";
    try (Connection connection = dataSource.getConnection();
         PreparedStatement preparedstatement = connection.prepareStatement(sql);
         ResultSet rs = preparedstatement.executeQuery()) {
        if (rs.next()) {
            return rs.getLong(1);
        }
    } 
    return 0;
}
```

#### Question

> We see that there are return statements inside try block. will they close the resultset ?


## Update User

```java
@Test
void testSave() throws SQLException {
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
    if(user != null) {
        if ( user.id() == null ) {
            final String insertSQL = "INSERT INTO `user`(useremail,role) VALUES (?,?)";

            try(Connection connectionection = dataSource.getConnection();
                PreparedStatement preparedStatement = connectionection.prepareStatement(insertSQL, Statement.RETURN_GENERATED_KEYS)) {
                preparedStatement.setString(1, user.useremail());
                preparedStatement.setString(2, user.role());

                preparedStatement.executeUpdate();

                try(ResultSet resultSet = preparedStatement.getGeneratedKeys()) {
                    if(resultSet.next()) {
                        return new User(resultSet.getInt(1),
                                user.useremail(), user.role());
                    }
                }
            }
        } else {
            String updateSql = "UPDATE `user` SET useremail = ?, role = ? WHERE id = ?";

            try (Connection connection = dataSource.getConnection();
                    PreparedStatement preparedstatement = connection.prepareStatement(updateSql)) {

                preparedstatement.setString(1, user.useremail());
                preparedstatement.setString(2, user.role());

                preparedstatement.setInt(3, user.id());

                preparedstatement.executeUpdate();

                return new User(user.id(), user.useremail(), user.role());
            }
        }
    }
    return null;
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
    try (Connection connection = dataSource.getConnection();
         PreparedStatement preparedstatement = connection.prepareStatement(sql)) {
        preparedstatement.setInt(1, id);
        preparedstatement.executeUpdate();
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
    try (Connection connection = dataSource.getConnection();
         PreparedStatement preparedstatement = connection.prepareStatement(sql)) {
        preparedstatement.setInt(1, id);
        try (ResultSet rs = preparedstatement.executeQuery()) {
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
void testFindAll() throws SQLException {
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
    try (Connection connection = dataSource.getConnection();
         PreparedStatement preparedstatement = connection.prepareStatement(sql);
         ResultSet rs = preparedstatement.executeQuery()) {
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

### Insert Multiple Users

```java
@Test
void testCreateAll() throws SQLException {
    List<User> users = new ArrayList<>();

    for (int i = 0; i < 50; i++) {
        users.add(new User(null, "madasamy"+i+"@email.com","Employee"));
    }
    
    long time = System.currentTimeMillis();
    userDao.createAll(users);
    System.out.println("Created in " + (System.currentTimeMillis() - time) + " milliseconds");

    Assertions.assertEquals(50, userDao.count(), "Multiple User Creation Failed");
}
```

### Implementation

```java
    public void createAll(List<User> users) throws SQLException {
        for (User user:users) {
            this.save(user);
        }
    }
```

But do you think it is good from performance perspective ?! Remember every time you get new user in java it travels from one server to another. why dont we pack them and send in single trip ?
