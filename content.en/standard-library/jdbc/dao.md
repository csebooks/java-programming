---
title: 'Data Access Methods'
weight: 2
--- 

In this chapter, we’ll **write a test case first**, then **implement the corresponding DAO method using JDBC**. This TDD-style flow will help reinforce *intentional design* and *clear validation*.

## Create Student

### Insert New Student

```java
@Test
void testSave() throws SQLException {
    // 1. Create an Student
    Student student = studentDao.save(new Student(null, "madasamy"));

    // 2. Student should have a generated id
    Assertions.assertNotNull(student.id(), "Student Creation Failed");
}
```

### Implementation

```java
public Student save(final Student student) throws SQLException {
    if(student != null) {
        if ( student.id() == null ) {
            final String insertSQL = "INSERT INTO student(name) VALUES (?,?)";

            try(Connection connectionection = dataSource.getConnection();
                PreparedStatement preparedStatement = connectionection.prepareStatement(insertSQL, Statement.RETURN_GENERATED_KEYS)) {
                preparedStatement.setString(1, student.name());

                preparedStatement.executeUpdate();

                try(ResultSet resultSet = preparedStatement.getGeneratedKeys()) {
                    if(resultSet.next()) {
                        return new Student(resultSet.getInt(1),
                                student.name());
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

## Delete All Student

```java
@Test
void testDeleteAll() throws SQLException {
    studentDao.save(new Student(null, "sathashini"));
    studentDao.save(new Student(null, "sobhan"));

    studentDao.deleteAll();

    assertEquals(studentDao.count(), 0);
}
```

### Implementation

```java
public void deleteAll() throws SQLException {
    final String sql = "DELETE FROM student";
    try(Connection connectionection= dataSource.getConnection();
            PreparedStatement preparedStatement = connectionection.prepareStatement(sql)) {
        preparedStatement.executeUpdate();
    }
}
```

## No of Students

```java
@Test
void testCount() throws SQLException {
    studentDao.save(new Student(null, "guruprasath"));
    studentDao.save(new Student(null, "keerthanasri"));

    assertEquals(2, studentDao.count());
}
```

### Implementation

```java
public long count() throws SQLException {
    String sql = "SELECT COUNT(*) FROM student";
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


## Update Student

```java
@Test
void testSave() throws SQLException {
    // 1. Create an Student
    Student student = studentDao.save(new Student(null, "madasamy"));

    // 2. Student should have a generated id
    Assertions.assertNotNull(student.id(), "Student Creation Failed");

    // 3. Update the student email in the DB
    Student updatedStudent = studentDao.save(new Student(student.id(), "Mithra"));

    // 4. Verify that Student is the same
    Assertions.assertEquals(updatedStudent.id(),student.id(), "Student Update Failed");

    // 5. Verify that Student has updated Email
    Assertions.assertEquals("Mithra",updatedStudent.name(), "Student Update Failed");
}
```

### Implementation

```java
public Student save(final Student student) throws SQLException {
    if(student != null) {
        if ( student.id() == null ) {
            final String insertSQL = "INSERT INTO student(name) VALUES (?)";

            try(Connection connectionection = dataSource.getConnection();
                PreparedStatement preparedStatement = connectionection.prepareStatement(insertSQL, Statement.RETURN_GENERATED_KEYS)) {
                preparedStatement.setString(1, student.name());
                

                preparedStatement.executeUpdate();

                try(ResultSet resultSet = preparedStatement.getGeneratedKeys()) {
                    if(resultSet.next()) {
                        return new Student(resultSet.getInt(1),
                                student.name());
                    }
                }
            }
        } else {
            String updateSql = "UPDATE student SET name = ? WHERE id = ?";

            try (Connection connection = dataSource.getConnection();
                    PreparedStatement preparedstatement = connection.prepareStatement(updateSql)) {

                preparedstatement.setString(1, student.name());
               
                preparedstatement.setInt(2, student.id());

                preparedstatement.executeUpdate();

                return new Student(student.id(), student.name());
            }
        }
    }
    return null;
}
```

## Delete an Student

```java
@Test
void testDeleteStudentById() throws SQLException {
    var student = studentDao.save(new Student(null, "keerthanasri"));

    studentDao.deleteById(student.id());

    assertTrue(studentDao.findById(student.id()).isEmpty());
}
```

### Implementation

```java
public void deleteById(final int id) throws SQLException {
    String sql = "DELETE FROM student WHERE id = ?";
    try (Connection connection = dataSource.getConnection();
         PreparedStatement preparedstatement = connection.prepareStatement(sql)) {
        preparedstatement.setInt(1, id);
        preparedstatement.executeUpdate();
    } 
}
```

## Retrive an Student

```java
@Test
void testFindStudentById() throws SQLException {
    var saved = studentDao.save(new Student(null, "chris"));

    var result = studentDao.findById(saved.id());

    assertTrue(result.isPresent());
    assertEquals("chris", result.get().name());
}
```

### Implementation

```java
public Optional<Student> findById(final int id) throws SQLException {
    String sql = "SELECT id, name  FROM student WHERE id = ?";
    try (Connection connection = dataSource.getConnection();
         PreparedStatement preparedstatement = connection.prepareStatement(sql)) {
        preparedstatement.setInt(1, id);
        try (ResultSet rs = preparedstatement.executeQuery()) {
            if (rs.next()) {
                return Optional.of(new Student(
                        rs.getInt("id"),
                        rs.getString("name")
                        
                ));
            }
        }
    } 
    return Optional.empty();
}
```

## Retrieve All Students

```java
@Test
void testFindAll() throws SQLException {
    studentDao.save(new Student(null, "sathashini"));
    studentDao.save(new Student(null, "keerthanasri"));

    var students = studentDao.findAll();

    assertEquals(2, students.size());
}
```

### Implementation

```java
public List<Student> findAll() throws SQLException {
    String sql = "SELECT id, name FROM student";
    List<Student> students = new ArrayList<>();
    try (Connection connection = dataSource.getConnection();
         PreparedStatement preparedstatement = connection.prepareStatement(sql);
         ResultSet rs = preparedstatement.executeQuery()) {
        while (rs.next()) {
            students.add(new Student(
                    rs.getInt("id"),
                    rs.getString("name")
                    
            ));
        }
    } 
    return students;
}
```

### Insert Multiple Students

```java
@Test
void testCreateAll() throws SQLException {
    List<Student> students = new ArrayList<>();

    for (int i = 0; i < 50; i++) {
        students.add(new Student(null, "madasamy"));
    }
    
    long time = System.currentTimeMillis();
    studentDao.createAll(students);
    System.out.println("Created in " + (System.currentTimeMillis() - time) + " milliseconds");

    Assertions.assertEquals(50, studentDao.count(), "Multiple Student Creation Failed");
}
```

### Implementation

```java
    public void createAll(List<Student> students) throws SQLException {
        for (Student student:students) {
            this.save(student);
        }
    }
```

But do you think it is good from performance perspective ?! Remember every time you get new student in java it travels from one server to another. why dont we pack them and send in single trip ?
