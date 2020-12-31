---
title: PostgreSQL
category: Databases
tags: [Featured, WIP]
updated: 2020-12-29
weight: -1
---

Intro
-------------------------------------

- Login to `postgres` user account as superuser: `sudo -iu postgres`
- Access the prompt with `psql`.
- You can do it all in one step with `sudo -u postgres psql`.
- Connect to a different database with `psql -d <database name>`.


### psql commands

| Command                            | Description             |
| ---------------------------------- | ----------------------- |
| `\q`                               | Quit                    |
| `\l`                               | List databases          |
| `\du`                              | List all roles          |
| `\dt`                              | List all tables         |
| `\dn`                              | List all schemas        |
| `\d table_name`                    | Details table           |
| `\i file`                          | Run file                |
| `\c`                               | Connect to new database |
| `\conninfo`                        | Current connection info |


### Creating new Role

```shell
sudo -iu postgres
```
{: .-setup}

```shell
createuser --interactive --pwprompt
```

### Changing password

```sql
ALTER USER user_name
WITH PASSWORD 'new_password';
```


Databases
-------------------------------------

### Creating

```shell
sudo -iu postgres
```
{: .-setup}

```shell
createdb rkma
```

### Dropping

```shell
sudo -iu postgres
```
{: .-setup}

```shell
dropdb rkma
```

Or

```shell
sudo -u postgres psql
```
{: .-setup}

```sql
DROP DATABASE IF EXISTS <name>;
```


Tables
-------------------------------------

### Creating

```sql
CREATE TABLE IF NOT EXISTS <table> (
    <column> <data_type> <TableConstraint> DEFAULT <default_value>,
    <another_column> <data_type> <TableConstraint> DEFAULT <default_value>,
    foreign key (<column>) references <table>(<column>)
)
```

#### Examples

```sql
CREATE TABLE movies (
    id INTEGER PRIMARY KEY,
    title TEXT,
    director TEXT,
    year INTEGER,
    length_minutes INTEGER
);

create table master_plan(
    id serial primary key,
    the_date date,
    title varchar(100),
    description text
);
```

### Data types

[List of data types](https://www.postgresql.org/docs/9.5/datatype.html)
{: .-crosslink}

### Constraints

| Constraint | Description      |
| ---------- | ---------------- |
| `PRIMARY KEY`        | Used to identify a single row in the table |
| `UNIQUE`             | This means that the values in this column have to be unique, so you can't insert another row with the same value in this column as another row in the table |
| `NOT NULL`           | This means that the inserted value can not be `NULL` |
| `FOREIGN KEY`        | This is a consistency check which ensures that each value in this column corresponds to another value in a column in another table |
| `CHECK (expression)` | This is allows you to run a more complex expression to test whether the values inserted are valid |

See: [Constraints](https://www.postgresql.org/docs/9.4/ddl-constraints.html)

### Dropping

```sql
DROP TABLE IF EXISTS <table_name>;
```

### Renaming table

```sql
ALTER TABLE <table_name> RENAME TO <new_table_name>;
```

See: [ALTER TABLE](https://www.postgresql.org/docs/9.4/sql-altertable.html)


Altering tables
-------------------------------------

### Adding columns
{: .-prime}

```sql
ALTER TABLE <table name>
ADD <column_name> <data_type> <optional_table_constraint>
    DEFAULT <default_value>;
```

See: [ALTER TABLE](https://www.postgresql.org/docs/9.4/sql-altertable.html)

### Removing columns

```sql
ALTER TABLE <table_name> DROP <column_name>;
```

### Renaming columns

```sql
ALTER TABLE table
RENAME COLUMN <column_name> TO <new_column_name>;
```

###  Foreign Key

```sql
ALTER TABLE <table_name>
ADD FOREIGN KEY (<column id>) REFERENCES lojas(id);
```

### Index

```sql
CREATE INDEX <index name> ON <table>(<column);
ALTER TABLE <table name> DROP INDEX <index name>;
```

### Enum

```sql
CREATE TYPE <enum name> AS enum(<enum1>, <enum2>);
```


Values
-------------------------------------

### Insert

```sql
INSERT INTO table
(column1, column2)
VALUES
(value1, value2)
(value1, value2);
```

### Update

```sql
UPDATE table
SET column1 = new_value1, column2 = new_value2
WHERE <condition>;
```

**Do not forget the where condition!**

### Delete

```sql
DELETE FROM <table name>
WHERE <condition>;
```

**Do not forget the where condition!**


Queries
-------------------------------------

Queries run in this order:

- FROM + JOIN
- WHERE
- GROUP BY
- HAVING
- SELECT + DISTINCT (window functions)
- ORDER BY
- LIMIT

`GROUP BY` is executed after the `WHERE` clause. To filter results after grouping, use `HAVING`.

The `HAVING` clause constraints are written the same way as the `WHERE` clause constraints, and are applied to the grouped rows.

```sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression)
FROM mytable
JOIN another_table ON mytable.column = another_table.column
WHERE constraint_expression
GROUP BY column
HAVING constraint_expression
ORDER BY column ASC/DESC
LIMIT n OFFSET n;
```

### Filtering

```sql
SELECT <column_name>
FROM <table_name>
WHERE condition1 AND condition2 OR condition3
```

{: .-one-column}

### Filter operators

| Operator            | Condition                                              | SQL example                     |
| ------------------- | ----------------------------------------------------   | -----------------------------   |
| `=`, `!=`, `<`, `<=`, `>`, `>=` | Standard numerical operators               | `col_name != 4`                 |
| `BETWEEN … AND …`     | Number is within range of two values (inclusive)     | `col_name BETWEEN 1.5 AND 10.5` |
| `NOT BETWEEN … AND …` | Number is not within range of two values (inclusive) | `col_name NOT BETWEEN 1 AND 10` |
| `IN (…)`              | Number exists in a list                              | `col_name IN (2, 4, 6)`         |
| `NOT IN (…)`          | Number does not exist in a list                      | `col_name NOT IN (1, 3, 5)`     |
{: .-headers}

When working with text data the following operators are supported:

| Operator   | Condition                      | SQL example                                                           |
| ---------- | ------------------------------ | --------------------------------------------------------------------- |
| `=`          | Case sensitive exact string comparison (*notice the single equals*) | `col_name = "abc"` |
| `!=` or `<>`   | Case sensitive exact string inequality comparison | `col_name != "abcd"` |
| `LIKE`       | Case insensitive exact string comparison | `col_name LIKE "ABC"` |
| `NOT LIKE`   | Case insensitive exact string inequality comparison | `col_name NOT LIKE "ABCD"` |
| `%`          | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) | `col_name LIKE "%AT%"<br>(matches "AT", "ATTIC", "CAT" or even "BATS")` |
| `_`          | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) | `col_name LIKE "AN_"<br>(matches "AND", but not "AN")` |
| `IN (…)`     | String exists in a list | `col_name IN ("A", "B", "C")` |
| `NOT IN (…)` | String does not exist in a list | `col_name NOT IN ("D", "E", "F")` |
{: .-headers}

### Aggregate functions

| Function               | Description                                                        |
| ---------------------- | -----------                                                        |
| `AVG(expression)`      | the average (arithmetic mean) of all non-null input values         |
| `COUNT(expression)`    | number of input rows for which the value of expression is not null |
| `MAX(expression)`      | maximum value of expression across all non-null input values       |
| `MIN(expression)`      | minimum value of expression across all non-null input values       |
| `SUM(expression)`      | sum of expression across all non-null input values                 |
{: .-headers}

See: [Aggregate Functions](https://www.postgresql.org/docs/9.4/functions-aggregate.html)

### Sorting

```sql
SELECT DISTINCT column
FROM table
WHERE condition
ORDER BY column ASC/DESC
LIMIT n OFFSET m;
```

### Multi table queries

```sql
SELECT column, another_table_column
FROM table
INNER JOIN another_table ON table.at_id = another_table.id;
```

Possible joins: `LEFT`, `RIGHT`, `FULL`.

See also: [Say no to Venn Diagrams on Join](https://blog.jooq.org/2016/07/05/say-no-to-venn-diagrams-when-explaining-joins/)


Subqueries
-------------------------------------
{: .-one-column}

![Subqueries](https://pbs.twimg.com/media/EGCeRwfX0AAb9pP?format=jpg&name=large)

Materialized Views
-------------------------------------

[Reference](https://medium.com/@rebo_dood/the-benefits-of-materialized-views-and-how-to-use-them-in-your-ruby-on-rails-project-4ac1b5432881)


Common queries
-------------------------------------

### Listing rows with duplicated values

```sql
SELECT title
FROM tracks
GROUP BY title
HAVING count(id) > 1;
```

```sql
SELECT a.*
FROM tracks a
JOIN (
    SELECT title, count(id)
    FROM tracks
    GROUP BY title
    HAVING count(id) > 1) b ON a.title = b.title
ORDER BY title;
```


Resources
-------------------------------------

### Videos

- [Things every developer absolutely, positively needs to know about database indexing](https://www.youtube.com/watch?v=HubezKbFL7E)
