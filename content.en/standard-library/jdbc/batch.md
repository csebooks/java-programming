---
title: 'Batch Operations'
weight: 3
--- 

> Batching allows you to **group multiple SQL statements** and send them to the database in **one round trip**, rather than sending each individually.

```java
public void createAll(List<User> newUsers) throws SQLException {
    final String insertSQL = "INSERT INTO `user`(useremail,role) VALUES (?,?)";

        try(Connection connection = dataSource.getConnection();
            PreparedStatement preparedStatement = connection.prepareStatement(insertSQL)) {
            for (User user:newUsers) {
                preparedStatement.setString(1, user.useremail());
                preparedStatement.setString(2, user.role());
                preparedStatement.addBatch();
            }

            preparedStatement.executeBatch();
    }
}
```

## ⚡ Why Use Batching?

* Reduces **network latency**
* Improves **write performance**

```java
@Test
void testCreateAll() throws SQLException {
    List<User> users = new ArrayList<>();

    for (int i = 0; i < 50; i++) {
        users.add(new User(null, "madasamy"+i+"@email.com","Employee"));
    }
    // Insering Invalid Record
    users.set(4, new User(null, null,"Employee"));

    long time = System.currentTimeMillis();
    try {
            userDao.createAll(users);
            Assertions.assertEquals(50, userDao.count(), "Multiple User Creation Failed");
    } catch (SQLException e) {
        Assertions.assertEquals(0, userDao.count(), "Multiple User Creation corrupted the DB");
    }
    System.out.println("Created in " + (System.currentTimeMillis() - time) + " milliseconds");
}
```


