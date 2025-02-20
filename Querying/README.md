# Querying

> Daniel Camacho

# Content

- [Why?](#why-databases-are-needed)
- [Database](#database)
- [DBMS](#database-managment-system) 
- [SQL](#sql-structured-query-language)
    - [Querying](#querying)
        - [SELECT](#select)
        - [LIMIT](#limit)
        - [WHERE](#where)
        - [NOT](#not)
        - [Logical Operators](#logical-operators)
        - [NULL](#null)
        - [LIKE](#like)
        - [Greater and Less than](#greater---lesser-operators)
            - [BETWEEN ... AND ...](#between--and-)
        - [ORDER BY](#order-by)
        - [Agreggate Functions](#agreggate-functions)
        - [DISTINCT](#distinct)
    - [Pattern Summary](#patterns-summary)

----

# Why Databases are needed?

The necessity of moving from spreadsheets to databases comes from the necessities of:

- Scale
- Frequency 
- Speed

Being the most common but of course there are others.

# Database

A collection of data organized for creating, reading, updating, and deleting. 

# Database Managment System

Software via which you can interact with a database.

**Examples:**

- MySQL
- Oracle
- PostgreSQL
- SQLite

Being this obviously a nonexhaustive list

# SQL (Structured Query Language)

A language via which you can create, read, update and delete data in a database. 


## Querying

**Example:**

- What's the most-liked post on our plataform?
- Is our number of daily users growing or shrinking?
- Which songs are most like the song a user just played?


To do an exercise we start with the terminal, going to your file (**Using SQLite**)

````
> ls
> sqlite3 longlist.db 
sqlite >
````

or any database that you have in this case is used [longlist.db](Data/longlist.db)

Now let's answer **What data is in our database?**

### SELECT

````SQL
SELECT * FROM "longlist";
````

With * you can select all the elements of the table, where we specify the table with ``FROM``

Let's just select an specific column

````SQL
SELECT "title" FROM "longlist";
````

Now another column

````SQL
SELECT "title", "author" FROM "longlist";
````

**Note:** In SQL "" double quotes are use a identifiers of columns and Strings with '' single quotes


### LIMIT

Limits the number of rows that we can select, for example

````SQL
SELECT "title" FROM "longlist" LIMIT 10;
````

### WHERE

We just get back rows where the conditions of the where is true.

````SQL
SELECT "title", "author" FROM "longlist" WHERE "year" = 2023;
````

As 2023 is an integer does not have quotes.

It's common using operators with ``WHERE`` like

- = (equal)
- != (not equal)
- <> (not equal)


For example

````SQL
SELECT "title", "format" FROM "longlist" WHERE "format" != 'hardcover';
````

### NOT 

Negate a condition

````SQL
SELECT "title", "format" FROM "longlist" WHERE NOT "format" = 'hardcover';
````


### Logical Operators

- AND
- OR
- ()

To make compound conditionals, and with parenthesis we can determine the order of the conditions.

For Example

````SQL
SELECT "title", "author" FROM "longlist" WHERE "year" = 2022
   OR "year" = 2023;
````


With Parenthesis

````SQL
SELECT
 "title",
 "format"
 FROM "longlist"
 WHERE ("year" = 2022 OR year = 2023)
 AND "format" != 'hardcover';
````

### NULL

Missing value

It can be conditionated like 

- IS NULL
- IS NOT NULL

````SQL
SELECT "title", "translator"
FROM "longlist"
WHERE "translator" IS NULL;
````

Or 

````SQL
SELECT "title", "translator"
FROM "longlist"
WHERE "translator" IS NOT NULL;
````

### LIKE

To find id some element of exists in the registers.

The use of ``LIKE`` is common with the operators

- % (Match any character around a string)
- _ (Match any single character in an string)

For example

````SQL
SELECT "title" FROM "longlist"
WHERE "title" LIKE '%love%';
````


This query gives titles where Love is somewhere at the string.

````SQL
SELECT "title" FROM "longlist"
WHERE "title" LIKE 'The%';
````

This query gives titles where 'The' starts at the beginnig.

````SQL
SELECT "title" FROM "longlist"
WHERE "title" LIKE 'P_re';
````

The result of the query is 'Pyre'

**Note:** ``LIKE`` is case-insesitive  

### Greater - Lesser operators

- \>
- <
- \>=
- <=

````SQL
SELECT "title" FROM "longlist"
WHERE "year" >= 2019 AND "year" <= 2022;
````



#### BETWEEN ... AND ...

````SQL
SELECT "title",
"year"
FROM "longlist"
WHERE "year" BETWEEN 2019 AND 2022;
````


````SQL
SELECT "title",
"rating",
"votes"
FROM "longlist"
WHERE "rating" > 4.0 AND "votes" > 10000;
````

### ORDER BY

Allows us as its suggests order the results of the query by some column itself.


Let's answer: What are the top 10 books in the DB?

For this is useful the keywords

- ORDER  BY ... ASC
- ORDER BY ... DESC  

````SQL
SELECT
"title",
"rating"
FROM "longlist"
ORDER BY "rating" DESC LIMIT 10;
````

One way to break ties is adding more conditions, to know the order adding conditions.

````SQL
SELECT
"title",
"rating",
"votes"
FROM "longlist"
ORDER BY "rating" DESC, "votes" DESC
LIMIT 10;
````

### Agreggate Functions

- COUNT
- AVG
- MIN 
- MAX 
- SUM

Example for the average

````SQL
SELECT AVG("rating") FROM "longlist";
````

Rounding the result would be

````SQL
SELECT ROUND(AVG("rating")) FROM "longlist";
````

We can specify the number of decimals

````SQL
SELECT ROUND(AVG("rating"), 2) FROM "longlist";
````

Naming columns using ``AS``

````SQL
SELECT ROUND(AVG("rating"), 2) AS 'average rating'  FROM "longlist";
````

If we want to know the total number of rows

````SQL
SELECT COUNT(*) FROM "longlist";
````

### DISTINCT 

````SQL
SELECT DISTINCT "publisher" FROM "longlist";
````

Eliminates rows that are the same from the query.

````SQL
SELECT COUNT(DISTINCT "publisher") FROM "longlist";
````

Its the number of unique publishers 


In SQLite the keyboard to kill the terminal is 

````
.quit
````


## Patterns Summary

**Select Data**

````SQL
SELECT column FROM table;
````

**Aggregate Functions**

````SQL
SELECT AVG(column) FROM table;
````

**WHERE**

````SQL
SELECT column FROM table
WHERE condition;
````

**Logical Operators**

````SQL
SELECT column FROM table
WHERE condition0 OR condition1;
````

**LIKE**


````SQL
SELECT column FROM table
WHERE column LIKE pattern;
````

**ORDER BY**


````SQL
SELECT column FROM table
WHERE condition 
ORDER BY column;
````

**LIMIT**

Adding LIMIT to the last pattern

````SQL
SELECT column FROM table
WHERE condition 
ORDER BY column
LIMIT number;
````