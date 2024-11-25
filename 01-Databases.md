# Databases

## Summary

| Query | Description |
| --- | --- |
| ```FROM information_schema.tables``` | Retrieve tables info in the database |
| ```FROM information_schema.columns``` | Retrieve columns info in the database |

## Introduction

A relational database is made up from Tables (Entities) and
relationships between them.

### Tables

The code to **create a table** is the following.

```postgresql
-- Structure
CREATE TABLE table_name (
    column_a data_type,
    column_b data_type,
    column_c data_type,
);

-- Example
CREATE TABLE weather (
    clouds text,
    temperature numeric,
    weather_station char(5),
);
```

To **modify** the table, ie. add or remove columns, rename,
delete table etc:

```postgresql
-- add column
ALTER TABLE table_name
ADD COLUMN column_name data_type;

-- remove column
ALTER TABLE table_name
DROP COLUMN column_name;

-- rename column
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;

-- delete table
DROP TABLE table_name;
```

**Migrate** information from one table to another. This is helpful
when getting the information consolidated in a table into
the tables that were created.

```postgresql
-- Structure
INSERT INTO table_name (column_a, column_b)
VALUES  ('value_a', 'value_b');

-- Example using a query
INSERT INTO professors 
SELECT DISTINCT firstname, lastname, university_shortname 
FROM university_professors;
```
