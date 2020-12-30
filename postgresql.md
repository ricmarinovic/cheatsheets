---
title: PostgreSQL
category: Databases
tags: [Featured, WIP]
updated: 2020-12-29
weight: -1
intro: |
    what is postgresql?
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

## Tables

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

See: [ALTER TABLE](https://www.postgresql.org/docs/9.4/sql-altertable.html)

### Renaming columns

```sql
ALTER TABLE table
RENAME COLUMN <column_name> TO <new_column_name>;
```

See: [ALTER TABLE](https://www.postgresql.org/docs/9.4/sql-altertable.html)

###  Foreign Key

```sql
ALTER TABLE <table_name>
ADD FOREIGN KEY (<column id>) REFERENCES lojas(id);
```

See: [ALTER TABLE](https://www.postgresql.org/docs/9.4/sql-altertable.html)


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
