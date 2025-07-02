---
title: 'JDBC'
weight: 11

--- 

> JDBC (Java Database Connectivity) is a Java API designed to facilitate connection and execution of queries with relational databases compatible with SQL. It is an integral part of Java SE (Java Standard Edition). JDBC API relies on JDBC drivers to establish connections with databases.

# Steps

When you interact from Java to Relational Databases it consists of below steps.

1. Create SQL Query
2. You make a Database **Connection**
3. You create **Statement** from the  **Connection**
4. Execute the **Statement** and get the **ResultSet**
5. Iterate the **ResultSet** to get the database records
6. Close all the **ResultSet**, **Statement** and **Connection** _in sequence_


```goat
         +-----------+                                +-------------+
         |   Java    |                                |   RDBMS     |
         +-----------+                                +-------------+
               |                                             |
  (1) Create SQL Query                                       |
               |-------------------------------------------->|
               |                                             |
  (2) Open Database Connection                               |
               |================= JDBC Driver ===============|
               |                                             |
  (3) Create Statement from Connection                       |
               |-------------------------------------------->|
               |                                             |
  (4) Execute Statement                                      |
               |-------------------------------------------->|
               |                                Process SQL, Return ResultSet
               |<--------------------------------------------|
               |                                             |
  (5) Iterate ResultSet (read data)                         |
               |<============================================|
               |                                             |
  (6) Close ResultSet, Statement, and Connection             |
               |-------------------------------------------->|
               |                                             |
         +-----------+                                +-------------+
         |   Java    |                                |   RDBMS     |
         +-----------+                                +-------------+

```

Simple. Isnt it. In the next chapters lets see these concepts with examples

