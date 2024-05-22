---
title: 'World of Database Developer'
weight: 1
--- 

Lets consider you have a postgress database running in machine with below table

```sql
CREATE TABLE movie (
    id int PRIMARY KEY,
    title VARCHAR(80),
    directed_by VARCHAR(80),
    genre VARCHAR(150),
    year_of_release NUMERIC(4),
    rating NUMERIC(2,1),
    created_by VARCHAR(80),
    created_at TIMESTAMP,
    modified_by VARCHAR(80),
    modified_at TIMESTAMP
);
```

First, We need to create a movie record. So We try below with the resources (IDE).

1. We connect to the database.
2. We prepare statement (INSERT) with SQL and Movie Record VALUES
3. We execute the statement
4. We close the Connection

Lets see that same through JDBC

```java
String sql = "INSERT INTO MOVIES VALUE()";

try (Connection connection = dataSource.getConnection();
       PreparedStatement ps = connection.prepareStatement()) {

      ps.setInt(1, postId);

      ps.execute();

  } catch (SQLException e) {
      //...
  }
```

Next, We need to query a movie record. So We try below with the resources (IDE).

1. We connect to the database.
2. We prepare Statement (SELECT) with SQL and WHERE movie ID is so and so
3. We execute the query to the database
4. We iterate result set
5. We close the Connection

Lets see that same through JDBC

```java
String sql = "SELECT * FROM MOBVIE  WHERE ID = ?";

try (Connection connection = dataSource.getConnection();
       PreparedStatement ps = connection.prepareStatement(sql)) {

      ps.setInt(1, movieId);

      ResultSet resultSet = ps.executeQuery()

      while(resultSet.next()) {
        //...
      }

  } catch (SQLException e) {
      //...
  }
```

Keep this in mind. When we work with JDBC, Our approch shoud be from Database to Java. (unlike ORM where approch From Java to Database). Enough of basic thory. Lets get started with some real coding.