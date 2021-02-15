---
date: 2021-02-15 20:52:26
layout: post
title: "PRIMARY KEY in SQL"
subtitle: How to use PRIMARY KEY
description:
image:  /assets/postimg/photo-1600201172395-c8165f448b39.webp
optimized_image: /assets/postimg/photo-1600201172395-c8165f448b39.webp
category: database
tags: 
    - database
    - sql-101
author: Klaus
paginate: false
---



# PRIMARY KEY in SQL



## SQL PRIMARY KEY constraint

The **PRIMARY KEY** constraint uniquely identifies each record in a table.
PRIMARY KEY **must contain UNIQUE values** also, **cannot have NULL values.**
A table can have **only one** PRIMARY KEY, and in the table, this PRIMARY KEY can consist of single or multiple columns (fields).

 - **Must conatin UNIQUE VALUES**
 - **Cannot contain NULL valuse**
 - A table can have **ONLY ONE PRIMARY KEY**



#### ex. SQL PRIMARY KEY when CREATE TABLE

```sql
CREATE TABLE Members (
	ID int NOT NULL PRIMARY KEY,
  name varchar(50) NOT NULL,
  age int
);
```



#### SQL PRIMARY KEY on ALTER TABLE

- If you want to define a PRIMARY KEY constraint on multiple columns, `CONSTRAINT col_name PRIMARY KEY (col_1, col2)`
- The `PK_` prefix is a common way to name a PRIMARY KEY

``` sql
CREATE TABLE Members (
	ID int NOT NULL,
  name varchar(50) NOT NULL,
  age int,
  CONSTRAINT PK_Members PRIMARY KEY (ID, name)
);
```

> - The `PK_Members` is only one PRIMARY KEY for the **Members table**
> - The VALUE of the PRIMARY KEY is made up of two columns (ID + name)



If you want to create a PRIMARY KEY constraint on the specific column when the table is already created, you can use `ALTER TABLE`

```sql
ALTER TABLE Members
ADD CONSTRAINT PK_Members PRIMARY KEY (ID, name)
```

> When you use the ALTER TABLE statement to add a PRIMARY KEY, the PRIMARY KEY column must already have been declared to NOT NULL when the table was first created.





DROP a PRIMARY KEY Constraint

```sql
ALTER TABLE Members
DROP CONSTRAINT PK_Members
```














