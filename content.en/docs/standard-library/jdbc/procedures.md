---
title: 'Stored Procedure'
weight: 3
--- 

> A **Stored Procedure** is a precompiled group of one or more SQL statements stored in the database. It can accept input parameters (`IN`), return output values (`OUT`), or both (`INOUT`). Stored procedures allow applications to delegate logic to the database server, leading to:

* Reduced application complexity
* Improved performance due to precompiled execution
* Easier maintenance and reuse of SQL logic

Stored procedures can simplify Java application logic by offloading reusable SQL code to the database layer. With JDBC’s `CallableStatement`, interacting with `IN`, `OUT`, and `INOUT` parameters is straightforward. By mastering this integration, Java developers can write more efficient, maintainable, and scalable database-driven applications.

**JDBC call syntax:**

```sql
{CALL procedure_name(?, ?, ?)}
```

Each `?` corresponds to a parameter (IN, OUT, or INOUT).

---

## IN Parameters

An `IN` parameter is used to pass a value from Java to the stored procedure.

**Example: MySQL/PostgreSQL**

```sql
CREATE OR REPLACE PROCEDURE add_student(IN student_name VARCHAR(255))
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO student(name) VALUES (student_name);
END;
$$;
```

```java
try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
     CallableStatement stmt = conn.prepareCall("{CALL add_student(?)}")) {

    stmt.setString(1, "John Doe");
    stmt.execute();
    System.out.println("Student added.");
}
```

---

## OUT Parameters

An `OUT` parameter is used to return a value from the stored procedure to Java.

```sql
CREATE OR REPLACE PROCEDURE get_student_name(
    IN student_id INT,
    OUT student_name VARCHAR(255)
)
LANGUAGE plpgsql
AS $$
BEGIN
    SELECT name INTO student_name
    FROM student
    WHERE id = student_id;
END;
$$;
```

```java
try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
     CallableStatement stmt = conn.prepareCall("{CALL get_student_name(?, ?)}")) {

    stmt.setInt(1, 1); // student_id
    stmt.registerOutParameter(2, Types.VARCHAR);
    stmt.execute();

    String name = stmt.getString(2);
    System.out.println("Student name: " + name);
}
```

---

## INOUT Parameters

An `INOUT` parameter takes a value from Java, possibly modifies it in the stored procedure, and returns the modified value back.

```sql
CREATE OR REPLACE PROCEDURE append_suffix(INOUT student_name VARCHAR(255))
LANGUAGE plpgsql
AS $$
BEGIN
    student_name := student_name || ' - Verified';
END;
$$;
```

```java
try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
     CallableStatement stmt = conn.prepareCall("{CALL append_suffix(?)}")) {

    stmt.setString(1, "Jane Doe");
    stmt.registerOutParameter(1, Types.VARCHAR);
    stmt.execute();

    String updatedName = stmt.getString(1);
    System.out.println("Updated name: " + updatedName);
}
```
## Batch Execution with CallableStatement

You can also perform **batch inserts/updates** using stored procedures via `CallableStatement`.
This works best with **only IN parameters**—OUT/INOUT parameters are not batch-friendly.

**PostgreSQL Example:**

```sql
CREATE OR REPLACE PROCEDURE insert_student_in(
    IN p_name TEXT
)
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO student (name)
    VALUES (p_name);
END;
$$;
```

**Java Batch Call:**

```java
try (Connection conn = dataSource.getConnection()) {
    conn.setAutoCommit(false);

    try (CallableStatement stmt = conn.prepareCall("{CALL insert_student_in(?)}")) {
        stmt.setString(1, "Alice");
        stmt.addBatch();

        stmt.setString(1, "Bob");
        stmt.addBatch();

        stmt.setString(1, "Charlie");
        stmt.addBatch();

        int[] results = stmt.executeBatch();
        conn.commit();

        System.out.println("Inserted rows: " + results.length);
    } catch (SQLException e) {
        conn.rollback();
        throw e;
    }
}
```

---

## CallableStatement vs PreparedStatement

| Use Case                        | JDBC API            | Notes                              |
| ------------------------------- | ------------------- | ---------------------------------- |
| Stored Procedure                | `CallableStatement` | Supports IN, OUT, INOUT            |
| Stored Function (returns value) | `PreparedStatement` | Simpler and cleaner for PostgreSQL |

**CallableStatement** is ideal for calling stored procedures:

```java
CallableStatement stmt = conn.prepareCall("{call my_procedure(?, ?, ?)}");
stmt.setString(1, "val");
stmt.registerOutParameter(2, Types.INTEGER);
stmt.execute();
```

**PreparedStatement** is better for stored functions in PostgreSQL:

```java
PreparedStatement stmt = conn.prepareStatement("SELECT my_function(?, ?)");
```

---

## Best Practices

* Always close JDBC resources (use try-with-resources).
* Disable autocommit for batch calls and commit explicitly.
* Avoid business logic in stored procedures unless performance dictates.
* Use descriptive procedure and parameter names.
* Catch and handle SQL exceptions both in Java and SQL.
