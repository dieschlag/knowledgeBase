TODO: choose order and apply it

- SQL: Structured Query Language
- Create database
```shell-session
mysql> CREATE DATABASE database_name;
```
- Show databases:
```shell-session
mysql> SHOW DATABASES;
```
- Use database:
```shell-session
mysql> USE thm_bookmarket_db;
```
- Remove db:
```shell-session
mysql> DROP database database_name;
```
- Create table:
```shell-session
mysql> CREATE TABLE example_table_name (
    example_column1 data_type,
    example_column2 data_type,
    example_column3 data_type
);
```
- Show tables:
```shell-session
mysql> SHOW TABLES;
```
- Get column informations in a table:
```shell-session
mysql> DESCRIBE table_name;
```
- Alter a table:
```shell-session
mysql> ALTER TABLE book_inventory
ADD page_count INT;
```
- Remove table:
```shell-session
mysql> DROP TABLE table_name;
```
- CRUD: Create/Read/Update/Delete

- Create:
```shell-session
mysql> INSERT INTO table_name (column_1, column_2)
    VALUES (value_1, value_2);
```
- Read: get all infos
```shell-session
SELECT * FROM table_name;
```
- Read: get specific column infos
```shell-session
mysql> SELECT column_1, column_2 FROM table_name;
```

- Read from multiple columns:
``` mysql
SELECT name,address,city,postcode from customers UNION SELECT company,address,city,postcode from suppliers;
```
- Update a value (row) in table:
```shell-session
mysql> UPDATE table_name
    SET value_name = value
    WHERE id = item_id;
```
- Delete:
```shell-session
DELETE FROM table_name WHERE id = item_id;
```

- Clauses:
	- `DISTINCT`: removes duplicates
		- `SELECT DISTINCT ...`
	- `GROUP BY`: regroupes answers that have same \<column_name> value
		- `SELECT ... FROM ... GROUP BY column_name`
	- `ORDER BY`
		- `... FROM ... ORDER BY column_name ASC/DESC`
		- `HAVING`: filters based on condition, applies after GROUP BY, if present
			- `HAVING name LIKE '%Hack%'`
- Operators:
	- `LIKE`: filter for specific patterns
	- `AND`: to link several booleans
	- `OR`: same
	- `NOT`: to exclude
	- `BETWEEN`: define range
	- `=`
	- `!=`
	- `<`
	- `>`
	- `<=`
	- `>=`
- `CONCAT()`: to add strings together
	- Ex:
```shell-session
mysql> SELECT CONCAT(name, " is a type of ", category, " book.") AS book_info FROM books;
+------------------------------------------------------------------+
| book_info                                                         |
+------------------------------------------------------------------+
| Android Security Internals is a type of Defensive Security book. |
| Bug Bounty Bootcamp is a type of Offensive Security book.        |
| Car Hacker's Handbook is a type of Offensive Security book.      |
| Designing Secure Software is a type of Defensive Security book.  |
| Ethical Hacking is a type of Offensive Security book.            |
+------------------------------------------------------------------+
```
- `GROUP_CONCAT()`: concatenate data from multiple rows into 1 field
	- Ex:
```shell-session
mysql> SELECT category, GROUP_CONCAT(name SEPARATOR ", ") AS books
    FROM books
    GROUP BY category;
+--------------------+-------------------------------------------------------------+
| category           | books                                                       |
+--------------------+-------------------------------------------------------------+
| Defensive Security | Android Security Internals, Designing Secure Software       |
| Offensive Security | Bug Bounty Bootcamp, Car Hacker's Handbook, Ethical Hacking |
+--------------------+-------------------------------------------------------------+
```
- `SUBSTRING()`: gets substring query from querried string
```shell-session
mysql> SELECT SUBSTRING(published_date, 1, 4) AS published_year FROM books;
```
- `LENGTH()`: returns the number of character in a string
```shell-session
mysql> SELECT LENGTH(name) AS name_length FROM books;
```
- `COUNT()`: returns number of records
```shell-session
mysql> SELECT COUNT(*) AS total_books FROM books;
```
- `SUM()`: sums all values
```shell-session
mysql> SELECT SUM(price) AS total_price FROM books;
```
- `MAX()`: returns maximum
```shell-session
mysql> SELECT MAX(published_date) AS latest_book FROM books;
```
- `MIN()`: returns minimum
```shell-session
mysql> SELECT MIN(published_date) AS earliest_book FROM books;
```
- Add comment in query: use `--` inside query, everything after that will be considered as a comment, useful for [[SQL Injection]].