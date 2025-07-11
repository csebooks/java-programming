---
title: 'Summary'
weight: 5
--- 

Although JDBC serves as the fundamental interface for communicating with databases from Java, most applications do not interact with it directly. Instead, they utilize the following technologies:

### SQL Mappers

SQL Mappers translate SQL statements into Java objects. They abstract the complexities of core JDBC classes from developers while allowing the flexibility to execute any SQL statement and retrieve results as Java objects. Examples include `Spring JDBC or MyBatis`.

### JPA (Java Persistence API)

JPA (Java Persistence API) provides Object-Relational Mapping, enabling the mapping of Java objects to database records. While offering simplicity from the Java perspective, database developers often criticize this approach for concealing useful features (such as stored procedures) that might be necessary. Examples are `Spring JPA and Hibernate`.

### Compile Time ORM

A relatively new entrant to the market, Compile Time ORM attempts to combine features of both SQL Mappers and JPA, achieving significant performance improvements through compile-time code generation. Examples include `JOOQ and SQL Components`.

### Relevance of JDBC

Despite the diminished direct use of JDBC in modern applications, understanding JDBC remains important in the following scenarios:

1. Developing Performance-Sensitive Applications (e.g., Data Engineering)
2. Any Java FRamework you use for Database Application use JDBC under the hood. Clear understanding makes you profeciant and flexible developer
3. Maintaining or developing Legacy CRM/ERP Applications
4. Exploring advanced framework functionalities and use cases