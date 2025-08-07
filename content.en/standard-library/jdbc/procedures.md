---
title: 'Stored Procedure'
weight: 3
--- 

> A **Stored Procedure** is a precompiled group of one or more SQL statements stored in the database. It can accept input parameters (`IN`), return output values (`OUT`), or both (`INOUT`). Stored procedures allow applications to delegate logic to the database server, leading to:

* Reduced application complexity
* Improved performance due to precompiled execution
* Easier maintenance and reuse of SQL logic

Stored procedures can simplify Java application logic by offloading reusable SQL code to the database layer. With JDBC’s `CallableStatement`, interacting with `IN`, `OUT`, and `INOUT` parameters is straightforward. By mastering this integration, Java developers can write more efficient, maintainable, and scalable database-driven applications.

```sql
{CALL procedure_name(?, ?, ?)}
```

Each `?` corresponds to a parameter (IN, OUT, or INOUT).

## IN Parameters

An `IN` parameter is used to pass a value from Java to the stored procedure.


```sql
DELIMITER //
CREATE PROCEDURE add_student(IN student_name VARCHAR(255))
BEGIN
    INSERT INTO student(name) VALUES (student_name);
END //
DELIMITER ;
```

```java
try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS);
     CallableStatement stmt = conn.prepareCall("{CALL add_student(?)}")) {

    stmt.setString(1, "John Doe");
    stmt.execute();
    System.out.println("Student added.");
}
```

## OUT Parameters

An `OUT` parameter is used to return a value from the stored procedure to Java.

```sql
DELIMITER //
CREATE PROCEDURE get_student_name(IN student_id INT, OUT student_name VARCHAR(255))
BEGIN
    SELECT name INTO student_name FROM student WHERE id = student_id;
END //
DELIMITER ;
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


## INOUT Parameters

An `INOUT` parameter takes a value from Java, possibly modifies it in the stored procedure, and returns the modified value back.

```sql
DELIMITER //
CREATE PROCEDURE append_suffix(INOUT student_name VARCHAR(255))
BEGIN
    SET student_name = CONCAT(student_name, ' - Verified');
END //
DELIMITER ;
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

In JDBC, whether you use `**CallableStatement**` or `**PreparedStatement**` depends on whether you're calling a **stored procedure** or a **stored function**, and how the database supports it.


### ✅ **When to Use `CallableStatement`**

Use `CallableStatement` when:

* You're calling a **stored procedure**
* The procedure has **OUT** or **INOUT** parameters
* You want to use JDBC’s `{call ...}` syntax

**Example:**

```java
CallableStatement stmt = conn.prepareCall("{call my_procedure(?, ?, ?)}");
stmt.setString(1, "val");
stmt.registerOutParameter(2, Types.INTEGER);
stmt.execute();
```

---

### ✅ **When to Use `PreparedStatement`**

Use `PreparedStatement` when:

* You're calling a **stored function** that **returns a value**
* You treat the function like a **SQL expression** (`SELECT my_function(?, ?)`)

**Example:**

```java
PreparedStatement stmt = conn.prepareStatement("SELECT my_function(?, ?)");
```

---

### 🤔 Why Not Use `CallableStatement` for Functions?

Technically, JDBC **does allow** calling functions using `CallableStatement`, but:

* The syntax is awkward and inconsistent across DBs.
* PostgreSQL doesn’t support `{? = call function(...)}` natively in JDBC unless the driver or tooling wraps it.
* Using `SELECT function(...)` is much more **idiomatic and portable** in PostgreSQL.


| Use Case                        | JDBC API            | Notes                              |
| ------------------------------- | ------------------- | ---------------------------------- |
| Stored Procedure                | `CallableStatement` | Supports IN, OUT, INOUT            |
| Stored Function (returns value) | `PreparedStatement` | Simpler and cleaner for PostgreSQL |


## Best Practices

* Always close JDBC resources (prefer try-with-resources).
* Validate stored procedure execution results where possible.
* Avoid business logic inside stored procedures unless performance requires it.
* Use clear naming for procedures and parameters.
* Manage exception handling in both SQL and Java sides.





