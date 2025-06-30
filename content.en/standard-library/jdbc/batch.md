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

---

## Insert Multiple Users As Bulk



## 🧠 Best Practices

* Use **`executeBatch()`** with `PreparedStatement` to avoid SQL injection
* Use batching with **transactions** when you need **safety**
* Handle exceptions carefully — partial inserts can be tricky

