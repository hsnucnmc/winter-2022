---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Database: MySQL

siriuskoan

---

# Outline

- Relational and Non-relational Database
- SQL
  - Basic Statements
  - Primary Key and Foreign Key
- MySQL
  - Installation
  - `mysqldump`

---

# Relational and Non-relational Database

There are two types of database, one is relational and the other one is non-relational.

- Relational Database (also called Relational Database Management System, RDBMS, or SQL)
  - It stores data in tables. The data in tables and rows is called as records.
  - It is structured and confined in format.
  - The data is easy to navigate.
  - Some popular examples: MySQL, MariaDB, PostgreSQL

---
layout: two-cols
---

# Relational and Non-relational Database

::left::

- Non-relational Database (also called NoSQL)
  - No tables, no rows, less structured.
  - More flexibility and adaptability.
  - Some popular examples: MongoDB, Redis


::right::

![](/nosql.png)

<!--

https://docs.microsoft.com/zh-tw/azure/architecture/data-guide/big-data/non-relational-data

-->

---

# SQL

SQL, standing for Structured Query Language, is a language for managing data in relational database management system.

The statement consists of clause, expression and predicate (bool), and ends with `;`.

SQL keywords are case insensitive.

---

# SQL

For example, `UPDATE users SET level = level * 2 WHERE name = 'siriuskoan'` is a simple SQL statement.

- `UPDATE users` is `UPDATE` clause.
- `SET level = level * 2` is `SET` clause.
- `level * 2` is an expression.
- `WHERE name = 'siriuskoan'` is `WHERE` clause.
- `name = 'siriuskoan'` is predicate.
- `'siriuskoan'` is expression.

---

# SQL - Basic Statements

CRUD means Create, Read, Update and Delete, they are the operations can be done to data.

We will talk about CRUD commands in SQL.

We will use the following table called `users` to do operations.

![](/db-sample.png)

---

# SQL - Basic Statements

`WHERE` statement yields boolean value, and it cannot be used alone.

It is a filter, so it can yields more than one records.

If there are many conditions, the predicate can be concatenated with logical operators `AND`, `OR`, `NOT`.

For example,
- `WHERE username = 'siriuskoan'` will return `true` when the `username` column of this record is `siriuskoan`.
- `WHERE age < 18` will return `true` when the `age` column of a record is less than `18`;
- `WHERE age < 18 AND age > 65` will return `true` when the `age` column of a record is less than `18` OR greater than `65`.

---

# SQL - Basic Statements

`SELECT` statement is "R" in CRUD. It can select data from a table.

The syntax is

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

`WHERE` clause can be omitted.

For example,
- `SELECT * FROM users` will yield the original table.
- `SELECT username FROM users` will yield the three usernames.
- `SELECT id FROM users WHERE username = 'siriuskoan'` will yield `1` since the `id` column of the record whose `username` column is `siriuskoan`.

---

# SQL - Basic Statements

`INSERT INTO` statement is "C" in CRUD, it can insert new record to a table.

The syntax is

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

`(column1, column2, ...)` can be omitted if all columns are assigned.

For example,
- `INSERT INTO users (username, level) VALUES ('cat', 100000)` inserts a record whose `id` is `4` (by `AUTOINCREMENT`), `username` is `cat` and `level` is `100000`.

<!--

We will talk about `AUTOINCREMENT` later.

-->

---

# SQL - Basic Statements

`UPDATE` statement is "U" in CRUD, it can modify the existing records in a table.

The syntax is

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

`WHERE` clause can be omitted, but all records will be updated.

For example,
- `UPDATE users SET level = 10 WHERE username = 'siriuskoan'` updates the `level` column to `10` of the record whose `username` column is `siriuskoan`.
- `UPDATE users SET level = level + 100 WHERE id > 1` updates the `level` column to the original value plus `10` of the record whose `id` column is greater than `1`.

---

# SQL - Basic Statements

`DELETE` statement is "D" in CRUD, it can remove existing records in a table.

The syntax is

```sql
DELETE FROM table_name WHERE condition;
```

`WHERE` clause can be omitted, but **all records will be deleted**.

For example,
- `DELETE FROM users WHERE username = 'cat'` removes the fourth row since its `username` column is `cat`.

---

# SQL - Basic Statements

`CREATE DATABASE` statement can create a new database.

The syntax is

```sql
CREATE DATABASE new_database_name;
```

`DROP DATABASE` statement can drop (delete) an existing database. It means all records, all tables and the database will be removed.

The syntax is

```sql
DROP DATABASE database_name;
```

<!--

After talking about CRUD of data, let's talk about CRUD of database and table, but not all of CRUD.

-->

---

# SQL - Basic Statements

`CREATE TABLE` statement is used to create a new table in a database. We have to specify the structure of this table.

The syntax is

```sql
CREATE TABLE table_name (
    column1 data_type,
    column2 data_type,
    ...
);
```

---

# SQL - Basic Statements

For example,

```sql
CREATE TABLE users (
    id INT,
    username VARCHAR(255),
    level INT
);
```

The statement can create a table with the same columns as the original table.

<!--

But not totally the same, we will talk about that later.

-->

---

# SQL - Basic Statements

`DROP TABLE` statement can remove an existing table, including all records in it.

The syntax is

```sql
DROP TABLE table_name;
```

For example,

- `DROP TABLE users` will remove the original table.

---

# SQL - Basic Statements

`ALTER TABLE` statement is used to "alter" an existing table by adding, removing or modifying columns.

To add a column, the syntax is

```sql
ALTER TABLE table_name
ADD column_name data_type [DEFAULT default_value];
```

For example,
- `ALTER TABLE users ADD age int DEFAULT 0;` will add a new column `age` whose data type is `INT` with default value `0` to `users` table.

<!--

Modifying columns does not mean change its value but change its data type.

-->

---

# SQL - Basic Statements

To drop a column, the syntax is

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

For example,
- `ALTER TABLE users DROP COLUMN age` will deletes the `age` column.

---

# SQL - Basic Statements

To modify a column, the syntax is

```sql
ALTER TABLE table_name
MODIFY COLUMN column_name data_type;
```

For example,
- `ALTER TABLE users MODIFY COLUMN age varchar(255)` changes the data type of `age` column from `INT` to `VARCHAR(255)`;

---

# SQL - Primary Key and Foreign Key

When creating database, we can set some constraints such as `UNIQUE`, `NOT NULL` and `DEFAULT` which we have mentioned above.

In addition to them, `PRIMARY KEY` and `FOREIGN KEY` are also important constraints, and we will discuss them.

For example (from w3school),

```sql
CREATE TABLE Orders (
    OrderID INT NOT NULL PRIMARY KEY,
    OrderNumber INT NOT NULL,
    PersonID INT FOREIGN KEY REFERENCES Persons(PersonID)
);
```

---

# SQL - Primary Key and Foreign Key

- Primary Key (PK)
  - `PRIMARY KEY` constraint uniquely identifies each record in a table.
  - For example, `id` can be the primary key since it is unique.
  - A table can have only one primary key, but it can consists of many columns.

- Foreign Key (FK)
  - `FOREIGN KEY` constraint is a field in one table, and the field refers to the primary key in another table.
  - For example, `id` is the PK in table `users`, then there can be a FK called `owner` which refers to `id` column in `users` in table `cars`.

<!--

We will see some examples when talking about database normalization.

-->

---

# MySQL - Primary Key and Foreign Key

Finally, let's see the schema of the original table.

```sql
CREATE TABLE users (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(255) NOT NULL UNIQUE,
    level INT NOT NULL
);
```

<!--

`AUTO_INCREMENT` is often used in PK, the value will be incremented by one for every new record.

-->

---

# MySQL

MySQL is a relational database management system. It is also the "M" in LAMP.

Though MySQL is an open source software originally, it is a commercial software now.

MariaDB is one of its fork, and it is open source.

<!--

LAMP: Linux, Apache, MySQL, PHP

-->

---

# MySQL - Installation

1. Install MySQL.

   `$ sudo apt install mysql-server`

2. Check whether MySQL is running.

   `$ service mysql status`

3. Do installation.

   `$ sudo mysql_secure_installation`

4. It will ask you some questions such as whether you want to remove test database, and let you set new root password.

5. Login MySQL and see whether it works correctly.

   `$ mysql -u root -p`

<!--

I answer yes for all questions.

-->

---

# MySQL - Installation

After installation, you can try to modify its config file `/etc/mysql/my.cnf`. There are some config you may want to check such as port, log location, etc.

After you login the server, you can try to enter some statement and see whether it works.

Let's create a database and a table now.

```sql
CREATE DATABASE users;
CREATE TABLE users (
    id INT NOT NULL AUTO_INCREMENT,
    username VARCHAR(255) NOT NULL UNIQUE,
    level INT NOT NULL
);
```

---

# MySQL - `mysqldump`

`mysqldump` is a tool for database backup.

- Backup
  - `mysqldump -h localhost -u root -p users > users_db_backup.sql` - Backup `users` database on localhost.
  - `mysqldump -h localhost -u root -p users users > users_table_backup.sql` - Backup table `users` of database `users` on localhost.
  - `mysqldump -u root -p --all-databases > backup.sql` - Backup all databases.

- Restoration
  - `mysql -u root -p < users_db_backup.sql` - Restore database `users`.
  - `mysql -u root -p users < users_table_backup.sql` - Restore table `users` to database `users`.
  - `mysql -u root -p < backup.sql` - Restore all databases.

