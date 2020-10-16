---
layout: post
title:  "Databases"
date:   2020-10-16 21:03:36 +0530
---

SQL vs NoSQL:

## What is SQL and NoSQL?
  * SQL
    * This is a **S**tructured **Q**uery **L**anguage and it is a programming language whos intent in on facilitating the retrieval of specific information from databases. In other words, SQL is the langauge of databases.
    * SQL's are specific so you need to know ahead of time what type of data you are going to be putting into the database. A Schema is used to deifne your data.
    * Atomic - Meaning that the data sent to the database is waited until all the data is into the database. Great for authentication/validation of types of data.

  * NoSQL
    * A NoSQL Schema can have semi to no design structure. Thus, they can be much more flexible with the type of data they can work with.
    * You need to wait for the data to push though all the different types of nodes before the data can finally be submitted to the whole database.
    
## When to use SQL vs NoSQL?
  * SQL
    * Use SQL when you want exact structure to your objects and you know that the objects will be consistent throughout time and not change.
  
  *NoSQL
    * Use NoSQL when your data objects may change their children often. For example tracking a workout plan has different types of data. Jumprope may not have the same data points as lifting weights so in this case NoSQL would be great for saving that abstract and changing data.
    
## Differences  and Advantages between SQL and NoSQL
  - SQL stores data in tables while noSQL stores data in JSON format that has no structure besides the structure within the JSON.
  - A big difference is that in NoSQL schemas are dynamic while in SQL they are static making it harder to change the data in SQL.
  - In SQL, schemas are rigid and bound to relationships. In NoSQL, schemas are more flexible and not bound.
  - SQL can be helpful in designing complex queries while NoSQL has no interface to compare the complex queries.
 

    
