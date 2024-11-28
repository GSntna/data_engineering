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

### Integrity constraints

To ensure that the data follows a certain structure, rules can
be enforeced into a database: pre-defined model, data types,
relationships, etc.

1. **Attribute constraints**, eg. data types on columns
2. **Key constraints**, eg. primary keys
3. **Referential integrity constraints**, enforced through foreign
keys.

#### Most common data types

| Data type | Description |
| --------- | ----------- |
| ```text``` | character strings of any length |
| ```varchar(x)``` | a maximum of *x* characters |
| ```char(x)``` | a fixed-length string of *x* characters |
| ```boolean``` | ```TRUE```, ```FALSE``` or ```NULL``` values |
| ```date```, ```time``` and ```timestamp``` | various formats for date and time calculations |
| ```numeric``` | arbitrary precission numbers, eg. ```3.1457``` |
| ```integer``` | whole numbers in a range of ```[-2,147'483,648, -2,147'483,647]``` |

Example:

```postgresql
-- creation
CREATE TABLE students (
    ssn integer, 
    name varchar(64),
    dob date,
    average_grade numeric(3, 2) -- precision of 3 and a scale of 2; eg. 5.54
    tuition_paid boolean
);

-- modify
ALTER TABLE students
ALTER COLUMN name
TYPE varchar(128)
-- Turns 5.54 into 6, not 5, before type conversion
USING ROUND(average_grade);
-- this last part allows to use a function before changing the type
```

#### Not-null and unique constraints

* Not-null: doesn't allow the column to hold null values
* Unique: disallow duplicate values

```postgresql
-- creating attribute with not-null and unique constraint
CREATE TABLE table_name (
    column_name1 integer NOT NULL,
    column_name1 integer UNIQUE
);

-- after creation not-null
ALTER TABLE table_name
ALTER COLUMN column_name1
SET NOT NULL;

-- after creation unique
ALTER TABLE table_name
ADD CONSTRAINT some_name UNIQUE(column_name2);
```

#### Keys

Uniquely identifies each record on a database. This makes it
easier to reference those records from other tables.

* **Key**: an attribute that can identify a record uniquely
  * The combination of tow or more attributes might be a key on
    itself if the combinations are unique

* **Primary key**: it must be:
  * One per database table, chosen from candidate keys
  * Uniquely identifies records
  * Unique and not-null constraints both apply
  * Primary keys are time invariant (must hold for current and
  future data on the table)
  * Can have more than one column. It's better to use as few
  columns as possible

```postgresql
-- create table setting one column as primary key
CREATE TABLE table_name (
key_col type PRIMARY KEY,
column_b type
);

-- using more than one column as primary key
CREATE TABLE example (
a int,
b int,
c int,
PRIMARY KEY (a, c)
);

-- add one after creation
ALTER TABLE table_name
ADD CONSTRAINT some_name PRIMARY KEY (column_name)
-- this just makes the column_name the primary key
```

* **Surrogate key**: diffrent from the primary key, it isn't
tied to any attribute. It is "artificially" generated in the
sense that it is only a SERIAL column, assigning a number from
the sequence to each record.

```postgresql
-- Add the new serial column to the table
ALTER TABLE table_name 
ADD COLUMN id SERIAL;

-- Make id (the serial column) a primary key
ALTER TABLE professors 
ADD CONSTRAINT professors_pkey PRIMARY KEY (id);
```

#### Relationships

* **Foreign key**: they are designated columns that point to
a primary key of another table. 
  * Each value of the FK must exist in the PK of the other
  table (FK Constraint or "referential integrity")
  * It can hold duplicates and ```NULL``` values

##### Specifying foreign keys in a table

```postgresql
-- from creation
CREATE TABLE table_a (
    a int PRIMARY KEY
    -- allow only values from pk column in table_b
    b varchar(255) REFERENCES table_b (pk_table_b)
); -- This will return an error when trying to add a different value

-- modify
ALTER TABLE table_a
ADD CONSTRAINT a_fkey FOREIGN KEY (b_id) REFERENCES table_b(pk_table_b)

-- add a foreign key using other columns*
UPDATE affiliations
SET professor_id = professors.id
FROM professors
WHERE affiliations.firstname = professors.firstname AND affiliations.lastname = professors.lastname;
```
\* In this example, supose the affiliations table has the 
professors ```firstname``` and ```lastname``` columns.
Professors table has ```firstname```, ```lastname``` and an
```id``` column. The query updates an empty ```professor_id```
column in the affiliations table using the ```firstname``` and
```lastname``` columns to look for ```professors.id```. 

#### Referential integrity violations

The records in column ```fk``` in table b should always point to
the values in column ```pk``` in table a.

The violation of this rule can be handled in different ways:

```postgresql
CREATE TABLE a (
    id INTEGER PRIMARY KEY,
    ...,
    b_id INTEGER REFERENCES b (id) ON DELETE NO ACTION -- default
);
```
```ON DELETE...```

* ```... NO ACTION```: Throw an error
* ```... CASCADE```: Delete all referencing records
* ```... RESTRICT```: Throw an error
* ```... SET NULL```: Set the referencing column to ```NULL```
* ```... SET DEFAULT```: Set the referencing column to its default
value

```postgresql
-- Identify table constraints
SELECT constraint_name, table_name, constraint_type
FROM information_schema.table_constraints
WHERE constraint_type = 'FOREIGN KEY';
```