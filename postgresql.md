---
title: PostgreSQL
category: Databases
updated: 2020-12-28
weight: -1
---

### Data types

[Data types](https://www.postgresql.org/docs/9.5/datatype.html)
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
