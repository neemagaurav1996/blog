---
layout: post
title:  Some useful Postgres Functions & Clauses
date:   2021-06-09 01:15:17 +0530
categories: [Blogging, Article]
permalink: useful-postgres-functions
tags: postgres postgresql sql database sql-functions functions
image: images/article-images/postgres-functions.png
image_alt: postgres-functions
comments: true
canonical_url: https://gaurav-neema.medium.com/some-useful-postgres-functions-clauses-6c64fbfc587f
display_image: images/postgres.svg
description: PostgreSQL (or Postgres) is one of the most advanced and powerful object-relational databases out there. Postgres can handle big tables with a large number of rows easily.<br><br>Let’s look at some of the functions and clauses of Postgres which I personally find very useful.</p>
duration: 4 min read
clean_date: May 09, 2021
---

## {{page.title}}

<div class="article-info muted-text">
    <span class="published-on">Published on <a rel="noopener" href="{{ page.canonical_url }}" target="_blank">Medium</a> on {{page.clean_date}}</span>
    <span class="duration"><i class="icon-clock"></i> {{page.duration}}</span>
</div>

<figure>
    <img class="article-image" src="{{ page.image }}" alt="{{ page.image_alt }}" width="200">
</figure>

PostgreSQL (or Postgres) is one of the most advanced and powerful object-relational databases out there. Postgres can handle big tables with a large number of rows easily.

It’s been a couple of years since I started using PostgreSQL as the main database. One thing which is clear to me is that if you use Postgres wisely, it does much of the back-end work at the database level.

Let’s look at some of the functions and clauses of Postgres which I personally find very useful.

<!--more-->

### **1. SIMILAR TO**

Suppose you want to run a `LIKE`query, but you want to do it for multiple expressions. Consider the following query which will select all the companies which **contain** apple or google or microsoft in the`column_name`:

```
SELECT * FROM companies 
WHERE (
        company_name LIKE '%apple%' 
        OR company_name LIKE '%google%' 
        OR company_name LIKE '%microsoft%'
    );
```

You can rewrite the above `LIKE`query using `SIMILAR_TO `so that you don’t have to write multiple OR conditions:

```
SELECT * FROM companies 
WHERE company_name SIMILAR TO '%(apple|google|microsoft)%';
```

### **2. DISTINCT ON**

No doubt, `DISTINCT` is a very useful keyword in SQL which is used to selected distinct values from a column.

`DISTINCT ON` is perfect when you have groups of data that are similar and want to get a single record out of each group, based on a specific ordering.

Suppose we want to find the employee with the highest salary in each department. The naive solution would be to create a subquery that selects department and max salary from that department and then finding the employee based on the subquery:

```
SELECT  * FROM employees 
WHERE
(department, salary) IN (
    SELECT department, MAX(salary) FROM employees
    GROUP BY department
);
```

This can be done much easier by `DISTINCT ON`:

```
SELECT 
    DISTINCT ON (department) * FROM employees 
ORDER BY department, salary DESC;
```

### **3. Aggregate Functions**

The aggregation functions are one of my favorites and I use them very often. Aggregate functions compute a single result from a set of input values. These are often used along with the `GROUP BY`.

- **ARRAY_AGG**

This query will return department and array of employee names for each department:

```
SELECT 
    department, ARRAY_AGG(emp_name) 
FROM employees
GROUP BY department;
```

- **JSON_AGG**

This will return `JSON`of employees for each department

- **STRING_AGG**

`STRING_AGG` will return the string of employee names separated by the delimiter for each department:

```
SELECT 
    department, STRING_AGG(emp_name, ' | ') 
FROM employees
GROUP BY department;
```

### 4. EXTRACT

The`EXTRACT()` function retrieves a field such as a year, month, and day from a date/time value.

Suppose you want to find the joining year for each employee, but you have the field with the `joining_date`, this query will do the job:

```
SELECT 
    emp_name, EXTRACT(YEAR FROM joining_date)
FROM employees;
```

You can extract a lot of stuff from the date/time value using [postgresql-extract](https://www.postgresqltutorial.com/postgresql-extract/).

### 5. CASE

The PostgreSQL `CASE `expression is similar to if/else statement in another programming language.

Suppose you want to tag the employees in 3 categories (Low, Medium, High) based on their salaries:

```
SELECT emp_name,
       CASE
           WHEN salary < 30000 THEN 'Low'
           WHEN (salary >= 30000 AND salary < 70000) THEN 'Medium'
           WHEN salary >= 70000 THEN 'High'
       END category
FROM employees;
```

We can also use CASE along with aggregate functions. Suppose you want to count the number of employees in each category:

```
SELECT
   COUNT(CASE WHEN salary < 30000 THEN 1 END) AS "# Low",
   COUNT(CASE WHEN (salary >= 30000 AND salary < 70000) THEN 1 END) AS "# Medium",
   COUNT(CASE WHEN salary >= 70000 THEN 1 END) AS "# High",
FROM employees;
```

### 6. CAST

`CAST` operator is used to convert one data type to another. Following is the syntax of `CAST`: `CAST ( expression AS target_type )`;

The following query will convert the date string to date object:

```
SELECT 
    CAST ('2020-09-25' AS DATE),
    CAST ('25-SEPT-2020' AS DATE);
```

### 7. SUBSTRING

SUBSTRING function returns part of a string. The syntax is: 

`SUBSTRING(string, start_postition, length)`

```
SELECT SUBSTRING ('PostgreSQL', 1, 8); -- PostgreS 
SELECT SUBSTRING ('PostgreSQL', 8); -- SQL
```

### 8. String Concatenation Functions

String concatenation functions of Postgres can turn to be of great help.

- **Concatenation Operator `||`**

Suppose you want to get the full names of the employees by combining the first name and last names:

```
SELECT 
    (first_name || ' ' || last_name) AS "Full Name" 
FROM employees;
```

If any of the value is `NULL`, it will return `NULL`.

- **CONCAT**

Postgres also have a `CONCAT` function that can be used to combine two or more strings. The syntax is `CONCAT(string_1, string_2, ...)`

The above query can be re-written as:

```
SELECT 
    CONCAT(first_name, ' ', last_name) AS "Full Name" 
FROM employees;
```

Unlike the concatenation operator `||`, the `CONCAT` function ignores `NULL` arguments. So if the last_name is NULL, it will return first_name as full_name.

- **CONCAT_WS**

`CONCAT_WS` (Concat With Separator) concatenates strings into one separated by a particular separator. The syntax is: `CONCAT_WS(separator, string_1, string_2, ...)`. It also ignores the `NULL` values.

The following query will concatenate the first_name and last_name separated by a pipe (`|`):

```
SELECT 
    CONCAT_WS('|', first_name, last_name) AS "Full Name" 
FROM employees;
```

These were some useful PostgreSQL functions, operators, and expressions. Though I have covered some of them which I ended up using the most, there are many such expressions in Postgres. Let me know which is your favorite function in the response section down here.

Feel free to connect me on [LinkedIn](https://www.linkedin.com/in/gaurav-neema-088bb3121/) or [Twitter](https://twitter.com/gaurav_neema).
