# Week 7 - SQL

Welcome to CS50! This is my review for Week 7's content about SQL.

## Lecture

1. In SQL, we have a **CRUD** paradigm, where
   * **C** stands for **CREATE**, sometimes includes **INSERT**
   * **R** stands for **READ**
   * **U** stands for **UPDATE**
   * **D** stands for **DELETE**, sometimes includes **DROP**
2. In SQL, we use the tradition that all the key words, like **SELECT**..., should be **captalized**. This is just to make our code easy to look at. Not captalizing the key words won't affect the execution of our SQL code.
3. Show the selected columns from the table in the terminal

{% code lineNumbers="true" %}
```sql
SELECT columns FROM table;
```
{% endcode %}

4. To define a variable in SQL

{% code lineNumbers="true" %}
```sql
SELECT COUNT(*) AS n FROM table;
```
{% endcode %}

5. Insert into table

{% code lineNumbers="true" %}
```sql
INSERT INTO table (column, ...) VALUES(value, ...);
```
{% endcode %}

6. Delete the **whole** table

{% code lineNumbers="true" %}
```sql
DELETE FROM table;
```
{% endcode %}

{% hint style="info" %}
This command will delete the whole table, which sometimes may cause horror story to happen.
{% endhint %}

7\. An Interesting SQL attack: SQL Injection Attack\
Hackers will use the property of the _comment_ (--) in SQL to hack into the user's account without knowing the password.

```python
rows = db.execute("SELECT * FROM users WHERE username = '{username}' AND password = '{password}'")
```

If the hacker input `malan@harvard'--` as the `username`

```python
    rows = db.execute("SELECT * FROM users WHERE username = `malan@harvard.edu'--' AND password = '{password}'")
```

In this case, the hacker doesn’t need to know the password but he can enter the account. To avoid this problem, we should use the code below:

```python
    from cs50 import SQL
    rows = db.execute("SELECT * FROM users WHERE username = ? AND password = ?", username, password)
```

## Section

1. Some basic jargons in Database:
   * **Database**: A collection of data organized for **c**reating, **r**eading, **u**pdating, and **d**eleting. (This is **CRUD** principle)
   * **Database Management System (DBMS)**: Software via which you can interact with a data base. And below are some examples:
     * MySQL
     * Oracle
     * PostgreSQL
     * SQLite\
       ...
   * **SQL**: A **language** via which you can **c**reate, **r**ead, **u**pdate, and **d**elete data in a database.
2. Design Principles for a database:
   * Create one table for each **entity** in your dataset. For example, you should create **two** tables for the **books and authors**.
   * All tables should have a **primary key**. This is a unique id that differentiate one row from every other row.
   * The information in the table should depend on the **primary key** only.
3. Basic **SQL** Commands:

* Create a table:

{% code lineNumbers="true" %}
```sql
CREATE TABLE table_name (
    column0 INTEGER,
    column1 TEXT,
    column2 NUMERIC,
    column3 REAL,
    PRIMARY KEY(column0)
);
```
{% endcode %}

{% hint style="info" %}
In this code, `column0` and so on is just the **name** for one specific column. Remember that you should also define the **PRIMARY KEY** for this table, which abides by our designing principle stated above.
{% endhint %}

* Create a joint/associative table:

{% code lineNumbers="true" %}
```sql
CREATE TABLE table_name (
    column0 INTEGER,
    column1 INTEGER,
    PRIMARY KEY(column0, column1)
    FOREIGN KEY(column0) REFERENCES other_table_name(column0),
    FOREIGN KEY(column1) REFERENCES other_table_name(column1)
);
```
{% endcode %}

{% hint style="info" %}
In this code, `column0` and `column1` are the **foreign keys** that reference to the **primary keys** in other tables.
{% endhint %}

* Delete a table:

{% code lineNumbers="true" %}
```sql
DROP TABLE table_name;
```
{% endcode %}

* Insert:

{% code lineNumbers="true" %}
```sql
INSERT INTO table (column0, column1)
VALUES(value0, value1);
```
{% endcode %}

{% hint style="info" %}
When inserting, you **don’t need** to include the `id`/`PRIMARY KEY` column.
{% endhint %}

* Delete:

{% code lineNumbers="true" %}
```sql
DELETE FROM table WHERE condition;
```
{% endcode %}

{% hint style="info" %}
Note that `WHERE` is usaully used to filter rows. It should appear right after you `SELECT` your columns.
{% endhint %}

* Show basic information about a table (Usually the **column** information):

{% code lineNumbers="true" %}
```sql
.schema table_name
```
{% endcode %}

* Show all the tables in the database:

{% code lineNumbers="true" %}
```sql
.tables
```
{% endcode %}

* Order the result you see:

{% code lineNumbers="true" %}
```sql
SELECT * FROM table ORDER BY column_name;
```
{% endcode %}

{% hint style="info" %}
Note that `ORDER BY` is usually at the end of the statement. You can specify the ordering by add `DESC` (descending) and `ASC` (ascending) right after the `column_name` after `ORDER BY`.
{% endhint %}

*   Aggregate functions:

    * `COUNT(*)`: Count the number of rows in the table.
    * `SUM(column_name)`: Sum the values in the column.
    * `MAX(column_name)`: Find the maximum value in the column.
    * `MIN(column_name)`: Find the minimum value in the column.
    * `AVG(column_name)`: Find the average value in the column.
    * Usage:

    <pre class="language-sql" data-line-numbers><code class="lang-sql"><strong>SELECT AVG(column_name) FROM table;
    </strong></code></pre>

{% hint style="info" %}
This will output a number, which is the average of a specifc column from the table.
{% endhint %}

## Shorts

1. All our queries will refer to **rows** of the table since we specify all of the **columns** in that table.
2. In a joint table, it is okay to establish a **joint primary key** - a combination of two columns that is always guaranteed to be unique. Otherwise, there will be duplicates if we only specify one column as the `PRIMARY KEY`
3. In SQL, it is usally to define the `PRIMARY KEY` column as `INTEGER`. Otherwise, you need to configure that column to **autoincrement**.

## Problem Set 7

### [Songs](https://cs50.harvard.edu/x/2024/psets/7/songs/)

Nothing more to note down.

### [Movies](https://cs50.harvard.edu/x/2024/psets/7/movies/)

1. In `ORDER BY`, we can specify more than one parameters and its order matters. For example, `ORDER BY column1 DESC, column2 ASC` will first sort the table by `column1` in descending order and then by `column2` in ascending order.
2. `JOIN` must appear before `WHERE`. For example,

{% code lineNumbers="true" %}
```sql
SELECT title
FROM movies
JOIN ratings ON ratings.movie_id = id
WHERE id IN (
    ---
);
```
{% endcode %}

3. We can use `INTERSECT` to get the intersection of two **columns**. For example,

{% code lineNumbers="true" %}
```sql
SELECT movie_id
FROM stars
WHERE person_id IN (
    SELECT people.id
    FROM people
    WHERE name = 'Bradley Cooper'
)
-- INTERSECT two columns
INTERSECT
SELECT movie_id
FROM stars
WHERE person_id IN (
    SELECT people.id
    FROM people
    WHERE name = 'Jennifer Lawrence'
)
```
{% endcode %}

4. In `WHERE` statement, you can specify more than one condition and use the `AND` or `OR` to connect them. For example,

{% code lineNumbers="true" %}
```sql
SELECT title
FROM movies
WHERE year = 2014 AND title = 'Harry Potter%'
```
{% endcode %}

5. In SQL, we can use `%` to match any string. For example, `'Harry Potter%'` will match any string that starts with `'Harry Potter'`.

### [Fiftyville](https://cs50.harvard.edu/x/2024/psets/7/fiftyville/)

#### **Things to notice in the problem statement**

1. In the `phone_calls` table, the unit of `duration` is **seconds**.

#### **Divide and Conquer**

```
Fiftyville Algorithm:
1. Get the description from the crime scene
2. Get the three interviews
3. Analyze these three interviews
```

#### **Useful Snippts**

Will update at the end of this course. My `log.sql` is quite long
