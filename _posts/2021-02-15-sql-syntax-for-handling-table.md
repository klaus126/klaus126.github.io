---
date: 2021-02-15 20:43:56
layout: post
title: "SQL Syntax for handling TABLE"
subtitle: Handling TABLE with SQL
description:
image: https://images.unsplash.com/photo-1579226905180-636b76d96082?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1534&q=80
optimized_image: https://images.unsplash.com/photo-1579226905180-636b76d96082?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1534&q=80
category: database
tags:
    - SQL
    - database
    - database-table
author: Klaus
paginate: false
---



# TABLE



## SQL Syntax for handling TABLE

- **CREATE TABLE** - creates a new table
- **INSERT INTO** - inserts new data into a database
- **UPDATE** - updates data in a database
- **REPLACE INTO** - replace data in a database
- **INSERT OR IGNORE** - Insert data. If data already exists, ignore it.
- **DELETE** - deletes data from a database
- **ALTER TABLE** - modifies a table
- **DROP TABLE** - deletes a table



### CREATE TABLE

- creates a new table

```sql
-- CREATE TABLE 테이블명 (칼럼명 칼럼데이터타입, 칼럼명 칼럼데이터타입 );
CREATE TABLE mytable (id INT, name VARCHAR(255), debut DATE);

-- PRIMARY KEY / AUTOINCREMENT 지정
CREATE TABLE mytable2 (id INTEGER PRIMARY KEY AUTOINCREMENT, name VARCHAR(255), debut DATE);
```



### INSERT INTO

- inserts new data into a database

```sql
-- mytable2에 데이터 넣기
INSERT INTO mytable2 (name, debut) VALUES ('hae', '2020-03-16');

-- 들어간 데이터 확인
SELECT * FROM mytalbe2
```



### UPDATE

- updates data in a database

```sql
-- UPDATE 테이블명 SET 변경할칼럼명 = '변경할칼럼값' WHERE 조건
UPDATE mytable2
SET debut = '2010-09-01'
WHERE id = 1234
```



### REPLACE INTO

- replace data in a database

```sql
REPLACE INTO mytable2 (id, name, debut) VALUES (6, 'fefe', '2019-04-11');
```



### INSERT OR IGNORE

- Insert data. If data already exists, ignore it.

```sql
-- 있으면 무시하고, 없으면 INSERT : 가장 많이 쓰여지는 것중 하나
INSERT OR IGNORE INTO mytable2 (id, name, debut) VALUES (1, 'fepe', '2019-04-13');

-- INSERT를 하는데 중복값이 있다면, 에러발생
```



### DELETE

- deletes data from a database

``` sql
-- DELETE FROM 테이블명 WHERE 조건
DELETE FROM mytable2 WHERE id=1;
```



### ALTER TABLE

- modifies a table

```sql
-- ALTER TABLE 테이블명 RENAME TO 변경할테이블명;
ALTER TABLE mytable2 RENAME TO players; -- mytable2가 players로 대체됨

-- ALTER TABLE 테이블명 ADD COLUMN 추가할칼럼명;
ALTER TABLE players ADD COLUM DOB date;
```



### DROP TABLE

- deletes a table

```sql
-- DROP TABLE 테이블명;
DROP TABLE mytable; -- mytable이 사라짐
```





## Logical Operators 

| **Operator** | **Description**                                              |
| ------------ | ------------------------------------------------------------ |
| =            | Equal*Example – `FirstName = John`*                          |
| !=           | Not equal. **Note:** In some versions of SQL this operator may be written as <> |
| >            | Greater than*Example –* *`Price > 20`*                       |
| <            | Less than*Example –* `WHERE`` Price < 20`                    |
| >=           | Greater than or equal*Example –* `WHERE Price >= 20`         |
| <=           | Less than or equal*Example –* `WHERE Price <= 20`            |
| BETWEEN      | Between an inclusive range `SELECT *column_name(s)*FROM *table_name*``WHERE *column_name* BETWEEN *value1* AND *value2;*` |
| LIKE         | Search for a pattern *Example –*  `SELECT *column_name(s)*``FROM *table_name*``WHERE *column_name* LIKE *pattern*;` |
| IN           | To specify multiple possible values for a column `SELECT *column_name(s)*FROM *table_name*``WHERE *column_name* IN (*value1*,*value2*,...);` |

 












