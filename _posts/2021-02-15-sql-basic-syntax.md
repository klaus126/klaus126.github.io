---
date: 2021-02-15 12:50:41
layout: post
title: "SQL Basic Syntax"
subtitle: SQL Syntax 101
description:
image: https://images.unsplash.com/photo-1571293905964-01d9fc58d30c?ixlib=rb-1.2.1&ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&auto=format&fit=crop&w=2547&q=80
optimized_image: https://images.unsplash.com/photo-1571293905964-01d9fc58d30c?ixlib=rb-1.2.1&ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&auto=format&fit=crop&w=2547&q=80
category: database
tags: 
    - database
    - sql-101
author: Klaus
paginate: false
---


# SQL basic



### 101

``` sql
SELECT [colum1,column2] 
FROM [데이터 테이블명] 
WHERE [조건/필터] 
ORDER BY [정렬 기준 칼럼명] DESC LIMIT [몇개까지 보여질지];
```



### SELECT : 어떤것을 가지고 올 것인지 / FROM : 어디서 가져올 것인지 / LIMIT : 얼마나 가져올 것인지

``` sql
-- SELECT : 어떤것을 가지고 올 것인지
-- FROM : 어디서 가져올 것인지
-- LIMIT : 얼마나 가져올 것인지

-- SELECT [Colum명] FROM [Data Table명] 
SELECT yearID FROM Salaries LIMIT 10;
SELECT yearID, playerID, salary FROM Salaries LIMIT 10;
```



### ORDER BY : 

``` sql
SELECT * FROM Salaries ORDER BY salary DESC LIMIT 20; 
SELECT * FROM Salaries ORDER BY salary ASC LIMIT 20; 
-- DESC : 내림차순 / ASC : 오름차순
```



### WHERE : 조건(필터링)

``` sql
-- Salaries 테이블에서, playerID가 'rodrial01'인 플레이어의 정보를 yearID 기준으로 오름차순해서 보여줘라 
SELECT * FROM Salaries WHERE playerID = 'rodrial01' ORDER BY yearID ASC;

-- Salaries 테이블에서, yearID가 '2010'인 플레이어의 정보를 salary기준으로 내림차순 20개
SELECT * FROM Salaries WHERE yearID = '2010' ORDER BY salary DESC LIMIT 20;
```

``` sql
-- Salaries 테이블에서, yearID가 '2010', lgID가 'AL'인 정보를 salary기준으로 내림차순해서 20개를 보여줘라
SELECT * FROM Salaries 
WHERE yearID = '2010' 
AND lgID = 'AL' -- AND로 추가해서 필터링 할 수 있다.
ORDER BY salary DESC 
LIMIT 20;

-- teamID
SELECT * FROM Salaries
WHERE teamID='SEA'
ORDER BY salary DESC
LIMIT 20;
```





###  SUM, AVG

``` sql
-- playerID가 'rodrial01'인 사람의 Salaries를 SUM 해서 보여줘라.
SELECT SUM(salary)
FROM Salaries
WHERE playerID = 'rodrial01';
```

``` sql
-- rodrial01의 연도별 연봉
SELECT 
	yearID,
  SUM(salary) AS year_sum_salary_of_rodrial01
FROM 
	Salaries
WHERE 
	playerID = 'rodrial01'
GROUP BY
	yearID
ORDER BY 
	yearID ASC;
```





### Concat Function, DISTINCT, GROUP BY

#### Concat

``` sql
-- nameFirst + nameLast : SQLite ver.
SELECT nameFirst || ' ' || nameLast AS name FROM People LIMIT 10;

```

#### DISTINCT : unique contents

``` sql
-- Unique filter
SELECT COUNT(*) FROM People;

-- Unique filter : by nameFirst + nameLast
SELECT COUNT(DISTINCT(nameFirst || ' ' || nameLast)) FROM People;
```

#### GROUP BY

``` sql
SELECT nameFirst || ' ' || nameLast AS name, COUNT(*)
FROM People
GROUP BY name
HAVING COUNT(*) > 1
ORDER BY COUNT(*) DESC;

-- GROUP BY : MySQL ver

```



``` sql
-- 각 팀의 연도별 salary 총합을 구하고, 내림차순으로 표시
SELECT 
	teamID, 
	yearID,
	SUM(salary) AS total_salary
FROM
	Salaries
GROUP BY 
	teamID, 
  yearID
ORDER BY 
	teamID,
	yearID ASC;  
```





### JOIN

#### JOIN

``` sql
-- 각 선수의 풀네임을 만들고 연봉별 내림차순으로 만드시오.
SELECT
        t2.nameFirst || ' ' || t2.nameLast AS name,
        t1.salary
    FROM
        Salaries t1
    JOIN
        People t2 ON t2.playerID = t1.playerID
    ORDER BY salary DESC
    LIMIT 20;
```

``` sql
-- 2010년의 연봉이 높은 순서대로 팀/이름/연봉을 출력하시오.
-- Top paid player for each team in 2010
    SELECT
        t2.teamID,
        t1.nameFirst || ' ' || t1.nameLast AS name,
        MAX(salary) -- 가장 높은 샐러리
    FROM
        People t1
    JOIN
        Salaries t2 ON t2.playerID = t1.playerID
    WHERE
        t2.yearID = 2010        
    GROUP BY
        t2.teamID
    ORDER BY
        MAX(salary) DESC;
```





#### INNER JOIN

``` sql
SELECT <fields>
FROM TableA A
INNER JOIN TableB B
ON A.key = B.key
```



#### LEFT JOIN : B에는 없을수도 있으나, A에 있다면 다 가져오겠다.

``` sql
SELECT <fields>
FROM TableA A
LEFT JOIN TableB B
ON A.key = B.key
```

``` sql
SELECT <fields>
FROM TableA A
LEFT JOIN TableB B
ON A.key = B.key
WHERE B.key IS NULL -- B에 null값이 있다면 빼줘.
```

``` sql
-- 전체 플레이어(People t1) 중에서 올스타이력(AllstarFull t2)이 있는 플레이어ID를 뽑고, COUNT(*)순으로 내림차순으로 정렬
SELECT
    t1.playerID,
    COUNT(*)
FROM
    People t1
LEFT JOIN
    AllstarFull t2 ON t2.playerID = t1.playerID
GROUP BY 1
ORDER BY COUNT(*) DESC LIMIT 20;
```

``` sql
-- 선수 이름별로 올스타 선정된 횟수 내림차순으로 정리
SELECT
    t2.playerID,
    t1.nameFirst || ' ' || t1.nameLast AS name,
    COUNT(*)
FROM
    People t1
LEFT JOIN
    AllstarFull t2 ON t2.playerID = t1.playerID
GROUP BY 1
ORDER BY COUNT(*) DESC LIMIT 20; 
```



### LEFT JOIN 3개 이상 할 경우

``` sql
-- 선수 이름별로 올스타 선정된 횟수, 연봉 / 올스타 횟수별 내림차순으로 정리
SELECT
    t2.playerID,
    t1.nameFirst || ' ' || t1.nameLast AS name,
    t3.salary,
    COUNT(*)
FROM
    People t1
LEFT JOIN
    AllstarFull t2 ON t2.playerID = t1.playerID
LEFT JOIN
    Salaries t3 ON t3.playerID = t1.playerID
GROUP BY 1
ORDER BY COUNT(*) DESC LIMIT 20; 
```

``` sql
-- 선수 이름별로 올스타 선정된 횟수, 연봉/ 연봉별 내림차순으로 정리
SELECT
    t2.playerID,
    t1.nameFirst || ' ' || t1.nameLast AS name,
    t3.salary,
    COUNT(*)
FROM
    People t1
LEFT JOIN
    AllstarFull t2 ON t2.playerID = t1.playerID
LEFT JOIN
    Salaries t3 ON t3.playerID = t1.playerID
GROUP BY 1
ORDER BY salary DESC LIMIT 20; 
```







