---
title: 'Batch Operations'
weight: 3
--- 

> Batching allows you to **group multiple SQL statements** and send them to the database in **one round trip**, rather than sending each individually.



## ⚡ Why Use Batching?

* Reduces **network latency**
* Improves **write performance**

```java
public void createAll(List<Student> newStudents) throws SQLException {
    final String insertSQL = "INSERT INTO student(name) VALUES (?)";

        try(Connection connection = dataSource.getConnection();
            PreparedStatement preparedStatement = connection.prepareStatement(insertSQL)) {
            for (Student student:newStudents) {
                preparedStatement.setString(1, student.name());
                
                preparedStatement.addBatch();
            }

            preparedStatement.executeBatch();
    }
}
```

```java
@Test
void testCreateAll() throws SQLException {
    List<Student> students = new ArrayList<>();

    for (int i = 0; i < 50; i++) {
        students.add(new Student(null, "madasamy"));
    }
    // Insering Invalid Record
    students.set(4, new Student(null, null));

    long time = System.currentTimeMillis();
    try {
            studentDao.createAll(students);
            Assertions.assertEquals(50, studentDao.count(), "Multiple Student Creation Failed");
    } catch (SQLException e) {
        Assertions.assertEquals(0, studentDao.count(), "Multiple Student Creation corrupted the DB");
    }
    System.out.println("Created in " + (System.currentTimeMillis() - time) + " milliseconds");
}
```


