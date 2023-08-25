# Selecting Data with SQL

## Introduction

As a data scientist, the SQL query you'll likely use most often is `SELECT`. This lesson introduces how to use `SELECT` to subset and transform the columns of a database table.

## Objectives

You will be able to:

- Retrieve a subset of columns from a table
- Create an alias in a SQL query
- Use SQL CASE statements to transform selected columns
- Use built-in SQL functions to transform selected columns

## The Data

Below, we connect to a SQLite database using the Python `sqlite3` library ([documentation here](https://docs.python.org/3/library/sqlite3.html)):


```python
import sqlite3 
conn = sqlite3.connect('data.sqlite')
```

The database that you've just connected to is the same database you have seen previously, containing data about orders, employees, etc. Here's an overview of the database:  

<img src="https://curriculum-content.s3.amazonaws.com/data-science/images/Database-Schema.png">

For this first section we'll be focusing on the `employees` table.

If we want to get all information about the employee records, we might do something like this (`*` means all columns):


```python
import pandas as pd
pd.read_sql("""SELECT * FROM employees;""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>employeeNumber</th>
      <th>lastName</th>
      <th>firstName</th>
      <th>extension</th>
      <th>email</th>
      <th>officeCode</th>
      <th>reportsTo</th>
      <th>jobTitle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1002</td>
      <td>Murphy</td>
      <td>Diane</td>
      <td>x5800</td>
      <td>dmurphy@classicmodelcars.com</td>
      <td>1</td>
      <td></td>
      <td>President</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1056</td>
      <td>Patterson</td>
      <td>Mary</td>
      <td>x4611</td>
      <td>mpatterso@classicmodelcars.com</td>
      <td>1</td>
      <td>1002</td>
      <td>VP Sales</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1076</td>
      <td>Firrelli</td>
      <td>Jeff</td>
      <td>x9273</td>
      <td>jfirrelli@classicmodelcars.com</td>
      <td>1</td>
      <td>1002</td>
      <td>VP Marketing</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1088</td>
      <td>Patterson</td>
      <td>William</td>
      <td>x4871</td>
      <td>wpatterson@classicmodelcars.com</td>
      <td>6</td>
      <td>1056</td>
      <td>Sales Manager (APAC)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1102</td>
      <td>Bondur</td>
      <td>Gerard</td>
      <td>x5408</td>
      <td>gbondur@classicmodelcars.com</td>
      <td>4</td>
      <td>1056</td>
      <td>Sale Manager (EMEA)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1143</td>
      <td>Bow</td>
      <td>Anthony</td>
      <td>x5428</td>
      <td>abow@classicmodelcars.com</td>
      <td>1</td>
      <td>1056</td>
      <td>Sales Manager (NA)</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1165</td>
      <td>Jennings</td>
      <td>Leslie</td>
      <td>x3291</td>
      <td>ljennings@classicmodelcars.com</td>
      <td>1</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1166</td>
      <td>Thompson</td>
      <td>Leslie</td>
      <td>x4065</td>
      <td>lthompson@classicmodelcars.com</td>
      <td>1</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1188</td>
      <td>Firrelli</td>
      <td>Julie</td>
      <td>x2173</td>
      <td>jfirrelli@classicmodelcars.com</td>
      <td>2</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1216</td>
      <td>Patterson</td>
      <td>Steve</td>
      <td>x4334</td>
      <td>spatterson@classicmodelcars.com</td>
      <td>2</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1286</td>
      <td>Tseng</td>
      <td>Foon Yue</td>
      <td>x2248</td>
      <td>ftseng@classicmodelcars.com</td>
      <td>3</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1323</td>
      <td>Vanauf</td>
      <td>George</td>
      <td>x4102</td>
      <td>gvanauf@classicmodelcars.com</td>
      <td>3</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1337</td>
      <td>Bondur</td>
      <td>Loui</td>
      <td>x6493</td>
      <td>lbondur@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1370</td>
      <td>Hernandez</td>
      <td>Gerard</td>
      <td>x2028</td>
      <td>ghernande@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1401</td>
      <td>Castillo</td>
      <td>Pamela</td>
      <td>x2759</td>
      <td>pcastillo@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1501</td>
      <td>Bott</td>
      <td>Larry</td>
      <td>x2311</td>
      <td>lbott@classicmodelcars.com</td>
      <td>7</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1504</td>
      <td>Jones</td>
      <td>Barry</td>
      <td>x102</td>
      <td>bjones@classicmodelcars.com</td>
      <td>7</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1611</td>
      <td>Fixter</td>
      <td>Andy</td>
      <td>x101</td>
      <td>afixter@classicmodelcars.com</td>
      <td>6</td>
      <td>1088</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1612</td>
      <td>Marsh</td>
      <td>Peter</td>
      <td>x102</td>
      <td>pmarsh@classicmodelcars.com</td>
      <td>6</td>
      <td>1088</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1619</td>
      <td>King</td>
      <td>Tom</td>
      <td>x103</td>
      <td>tking@classicmodelcars.com</td>
      <td>6</td>
      <td>1088</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1621</td>
      <td>Nishi</td>
      <td>Mami</td>
      <td>x101</td>
      <td>mnishi@classicmodelcars.com</td>
      <td>5</td>
      <td>1056</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1625</td>
      <td>Kato</td>
      <td>Yoshimi</td>
      <td>x102</td>
      <td>ykato@classicmodelcars.com</td>
      <td>5</td>
      <td>1621</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1702</td>
      <td>Gerard</td>
      <td>Martin</td>
      <td>x2312</td>
      <td>mgerard@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
  </tbody>
</table>
</div>



### Quick Note on String Syntax

When working with strings, you may have previously seen a `'string'`, a `"string"`, a `'''string'''`, or a `"""string"""`. While all of these are strings, the triple quotes have the added functionality of being able to use multiple lines within the same string as well as to use single quotes within the string. Sometimes, SQL queries can be much longer than others, in which case it's helpful to use new lines for readability. Here's the same example, this time with the string spread out onto multiple lines:


```python
pd.read_sql("""
SELECT *
  FROM employees;
""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>employeeNumber</th>
      <th>lastName</th>
      <th>firstName</th>
      <th>extension</th>
      <th>email</th>
      <th>officeCode</th>
      <th>reportsTo</th>
      <th>jobTitle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1002</td>
      <td>Murphy</td>
      <td>Diane</td>
      <td>x5800</td>
      <td>dmurphy@classicmodelcars.com</td>
      <td>1</td>
      <td></td>
      <td>President</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1056</td>
      <td>Patterson</td>
      <td>Mary</td>
      <td>x4611</td>
      <td>mpatterso@classicmodelcars.com</td>
      <td>1</td>
      <td>1002</td>
      <td>VP Sales</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1076</td>
      <td>Firrelli</td>
      <td>Jeff</td>
      <td>x9273</td>
      <td>jfirrelli@classicmodelcars.com</td>
      <td>1</td>
      <td>1002</td>
      <td>VP Marketing</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1088</td>
      <td>Patterson</td>
      <td>William</td>
      <td>x4871</td>
      <td>wpatterson@classicmodelcars.com</td>
      <td>6</td>
      <td>1056</td>
      <td>Sales Manager (APAC)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1102</td>
      <td>Bondur</td>
      <td>Gerard</td>
      <td>x5408</td>
      <td>gbondur@classicmodelcars.com</td>
      <td>4</td>
      <td>1056</td>
      <td>Sale Manager (EMEA)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1143</td>
      <td>Bow</td>
      <td>Anthony</td>
      <td>x5428</td>
      <td>abow@classicmodelcars.com</td>
      <td>1</td>
      <td>1056</td>
      <td>Sales Manager (NA)</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1165</td>
      <td>Jennings</td>
      <td>Leslie</td>
      <td>x3291</td>
      <td>ljennings@classicmodelcars.com</td>
      <td>1</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1166</td>
      <td>Thompson</td>
      <td>Leslie</td>
      <td>x4065</td>
      <td>lthompson@classicmodelcars.com</td>
      <td>1</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1188</td>
      <td>Firrelli</td>
      <td>Julie</td>
      <td>x2173</td>
      <td>jfirrelli@classicmodelcars.com</td>
      <td>2</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1216</td>
      <td>Patterson</td>
      <td>Steve</td>
      <td>x4334</td>
      <td>spatterson@classicmodelcars.com</td>
      <td>2</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1286</td>
      <td>Tseng</td>
      <td>Foon Yue</td>
      <td>x2248</td>
      <td>ftseng@classicmodelcars.com</td>
      <td>3</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1323</td>
      <td>Vanauf</td>
      <td>George</td>
      <td>x4102</td>
      <td>gvanauf@classicmodelcars.com</td>
      <td>3</td>
      <td>1143</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1337</td>
      <td>Bondur</td>
      <td>Loui</td>
      <td>x6493</td>
      <td>lbondur@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1370</td>
      <td>Hernandez</td>
      <td>Gerard</td>
      <td>x2028</td>
      <td>ghernande@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1401</td>
      <td>Castillo</td>
      <td>Pamela</td>
      <td>x2759</td>
      <td>pcastillo@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1501</td>
      <td>Bott</td>
      <td>Larry</td>
      <td>x2311</td>
      <td>lbott@classicmodelcars.com</td>
      <td>7</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>16</th>
      <td>1504</td>
      <td>Jones</td>
      <td>Barry</td>
      <td>x102</td>
      <td>bjones@classicmodelcars.com</td>
      <td>7</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>17</th>
      <td>1611</td>
      <td>Fixter</td>
      <td>Andy</td>
      <td>x101</td>
      <td>afixter@classicmodelcars.com</td>
      <td>6</td>
      <td>1088</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>18</th>
      <td>1612</td>
      <td>Marsh</td>
      <td>Peter</td>
      <td>x102</td>
      <td>pmarsh@classicmodelcars.com</td>
      <td>6</td>
      <td>1088</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>19</th>
      <td>1619</td>
      <td>King</td>
      <td>Tom</td>
      <td>x103</td>
      <td>tking@classicmodelcars.com</td>
      <td>6</td>
      <td>1088</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>20</th>
      <td>1621</td>
      <td>Nishi</td>
      <td>Mami</td>
      <td>x101</td>
      <td>mnishi@classicmodelcars.com</td>
      <td>5</td>
      <td>1056</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>21</th>
      <td>1625</td>
      <td>Kato</td>
      <td>Yoshimi</td>
      <td>x102</td>
      <td>ykato@classicmodelcars.com</td>
      <td>5</td>
      <td>1621</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>22</th>
      <td>1702</td>
      <td>Gerard</td>
      <td>Martin</td>
      <td>x2312</td>
      <td>mgerard@classicmodelcars.com</td>
      <td>4</td>
      <td>1102</td>
      <td>Sales Rep</td>
    </tr>
  </tbody>
</table>
</div>



Unlike in Python, whitespace indentation in SQL is not used to indicate scope or any other important information. Therefore this:

```sql
SELECT *
  FROM employees;
```

(with two spaces in front of `FROM`)

is identical to this:

```sql
SELECT *
FROM employees;
```

(with zero spaces in front of `FROM`)

as far as SQL is concerned. However we will be aligning the right edge of the SQL keywords, using a "river" of whitespace down the center to improve legibility in this lesson, following [this style guide](https://www.sqlstyle.guide/). You will see multi-line SQL written with various different indentation styles, and you will want to check with your employer to learn what their style guide is.

## Retrieving a Subset of Columns

Once we know what the column names are for a given table, we can select specific columns rather than using `*` to select all of them. This is achieved by replacing the `*` with the names of the columns, separated by commas.

For example, if we just wanted to select the last and first names of the employees:


```python
pd.read_sql("""
SELECT lastName, firstName
  FROM employees;
""", conn).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>lastName</th>
      <th>firstName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Murphy</td>
      <td>Diane</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Patterson</td>
      <td>Mary</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Firrelli</td>
      <td>Jeff</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Patterson</td>
      <td>William</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bondur</td>
      <td>Gerard</td>
    </tr>
  </tbody>
</table>
</div>



We can also specify the columns in a different order than they appear in the database, in order to reorder the columns in the resulting dataframe:


```python
pd.read_sql("""
SELECT firstName, lastName
  FROM employees;
""", conn).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Diane</td>
      <td>Murphy</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mary</td>
      <td>Patterson</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Jeff</td>
      <td>Firrelli</td>
    </tr>
    <tr>
      <th>3</th>
      <td>William</td>
      <td>Patterson</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gerard</td>
      <td>Bondur</td>
    </tr>
  </tbody>
</table>
</div>



Additionally, we can use **aliases** (`AS` keyword) to change the column names in our query result:


```python
pd.read_sql("""
SELECT firstName AS name
  FROM employees;
""", conn).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Diane</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mary</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Jeff</td>
    </tr>
    <tr>
      <th>3</th>
      <td>William</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gerard</td>
    </tr>
  </tbody>
</table>
</div>



Note: the `AS` keyword is technically optional when assigning an alias in SQL, so you may see examples that don't include it. In other words, you could just say `SELECT firstName name` and it would work the same as `SELECT firstName AS name`. However we recommend being more explicit and including the `AS`, so that it's clearer what your code is doing.

## Using SQL `CASE` Statements

`CASE` statements appear very frequently in SQL technical interview questions. They are a type of conditional statement, similar to `if` statements in Python. Whereas Python uses the keywords `if`, `elif`, and `else`, SQL uses `CASE`, `WHEN`, `THEN`, `ELSE`, and `END`.

`CASE` indicates that a conditional statement has begun, and `END` indicates that it has ended.

`WHEN` is similar to `if`, and then instead of a colon and an indented block, `THEN` indicates what should happen if the condition is true. After the first `THEN` has executed, it skips to the end, so each subsequent `WHEN` is more like `elif` in Python.

`ELSE` is essentially the same as `else` in Python.

### `CASE` to Bin Column Values

One of the most common use cases for `CASE` statements is to bin the column values. This is true for both numeric and categorical columns.

In the example below, we use the `jobTitle` field to bin all employees into `role` categories based on whether or not their job title is "Sales Rep":


```python
pd.read_sql("""
SELECT firstName, lastName, jobTitle,
       CASE
       WHEN jobTitle = "Sales Rep" THEN "Sales Rep"
       ELSE "Not Sales Rep"
       END AS role
  FROM employees;
""", conn).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>jobTitle</th>
      <th>role</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Diane</td>
      <td>Murphy</td>
      <td>President</td>
      <td>Not Sales Rep</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mary</td>
      <td>Patterson</td>
      <td>VP Sales</td>
      <td>Not Sales Rep</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Jeff</td>
      <td>Firrelli</td>
      <td>VP Marketing</td>
      <td>Not Sales Rep</td>
    </tr>
    <tr>
      <th>3</th>
      <td>William</td>
      <td>Patterson</td>
      <td>Sales Manager (APAC)</td>
      <td>Not Sales Rep</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gerard</td>
      <td>Bondur</td>
      <td>Sale Manager (EMEA)</td>
      <td>Not Sales Rep</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Anthony</td>
      <td>Bow</td>
      <td>Sales Manager (NA)</td>
      <td>Not Sales Rep</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>Sales Rep</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Leslie</td>
      <td>Thompson</td>
      <td>Sales Rep</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Julie</td>
      <td>Firrelli</td>
      <td>Sales Rep</td>
      <td>Sales Rep</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Steve</td>
      <td>Patterson</td>
      <td>Sales Rep</td>
      <td>Sales Rep</td>
    </tr>
  </tbody>
</table>
</div>



### `CASE`  to Make Values Human-Readable

Another typical way to use `CASE` is to translate the column values into something that your eventual audience will understand. This is especially true of data that is entered into the database as a "code" or "ID" rather than a human-readable name.

In the example below, we use a `CASE` statement with multiple `WHEN`s in order to transform the `officeCode` column into an `office` column that uses a more meaningful name for the office:


```python
pd.read_sql("""
SELECT firstName, lastName, officeCode,
       CASE
       WHEN officeCode = "1" THEN "San Francisco, CA"
       WHEN officeCode = "2" THEN "Boston, MA"
       WHEN officeCode = "3" THEN "New York, NY"
       WHEN officeCode = "4" THEN "Paris, France"
       WHEN officeCode = "5" THEN "Tokyo, Japan"
       END AS office
  FROM employees;
""", conn).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>officeCode</th>
      <th>office</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Diane</td>
      <td>Murphy</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mary</td>
      <td>Patterson</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Jeff</td>
      <td>Firrelli</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>3</th>
      <td>William</td>
      <td>Patterson</td>
      <td>6</td>
      <td>None</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gerard</td>
      <td>Bondur</td>
      <td>4</td>
      <td>Paris, France</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Anthony</td>
      <td>Bow</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Leslie</td>
      <td>Thompson</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Julie</td>
      <td>Firrelli</td>
      <td>2</td>
      <td>Boston, MA</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Steve</td>
      <td>Patterson</td>
      <td>2</td>
      <td>Boston, MA</td>
    </tr>
  </tbody>
</table>
</div>



Note that because **we did not specify a name for `officeCode` "6"**, and did not include an `ELSE`, the associated office value for William Patterson is `NULL` (represented as `None` in Python).

There is also a shorter syntax possible if all of the `WHEN`s are just checking if a value is equal to another value (e.g. in this case where we are repeating `officeCode =` over and over). Instead we can specify `officeCode` right after `CASE`, then only specify the potential matching values:


```python
pd.read_sql("""
SELECT firstName, lastName, officeCode,
       CASE officeCode
       WHEN "1" THEN "San Francisco, CA"
       WHEN "2" THEN "Boston, MA"
       WHEN "3" THEN "New York, NY"
       WHEN "4" THEN "Paris, France"
       WHEN "5" THEN "Tokyo, Japan"
       END AS office
  FROM employees;
""", conn).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>officeCode</th>
      <th>office</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Diane</td>
      <td>Murphy</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mary</td>
      <td>Patterson</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Jeff</td>
      <td>Firrelli</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>3</th>
      <td>William</td>
      <td>Patterson</td>
      <td>6</td>
      <td>None</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gerard</td>
      <td>Bondur</td>
      <td>4</td>
      <td>Paris, France</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Anthony</td>
      <td>Bow</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Leslie</td>
      <td>Thompson</td>
      <td>1</td>
      <td>San Francisco, CA</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Julie</td>
      <td>Firrelli</td>
      <td>2</td>
      <td>Boston, MA</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Steve</td>
      <td>Patterson</td>
      <td>2</td>
      <td>Boston, MA</td>
    </tr>
  </tbody>
</table>
</div>



## Using Built-in SQL Functions

Similar to the [Python built-in functions](https://docs.python.org/3/library/functions.html), SQL also has built-in functions. The available functions will differ somewhat by the type of SQL you are using, but in general you should be able to find functions for:

* String manipulation
* Math operations
* Date and time operations

For SQLite in particular, if you are looking for a built-in function, start by checking the [core functions](https://www.sqlite.org/lang_corefunc.html) page, [mathematical functions](https://www.sqlite.org/lang_mathfunc.html) page, and/or [date and time functions](https://www.sqlite.org/lang_datefunc.html) page.

### Built-in SQL Functions for String Manipulation

#### `length`

Let's start with an example of a SQL built-in function that is very similar to one we have in Python: `length` ([documentation here](https://www.sqlite.org/lang_corefunc.html#length)). This works very similarly to the `len` built-in function in Python. For a string, it returns the number of characters.

If we wanted to find the length of the first names of all employees, that would look like this:


```python
pd.read_sql("""
SELECT length(firstName) AS name_length
  FROM employees;
""", conn).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



#### `upper`

Now let's say we wanted to return all of the employee names in all caps. Similar to the Python string method, this SQL function is called `upper` ([documentation here](https://www.sqlite.org/lang_corefunc.html#upper)). However, since it's a built-in function and not a method, the syntax looks like:

```sql
upper(column_name)
```

and not `column_name.upper()`.

As you get more comfortable with Python and SQL, distinctions like this will get more intuitive, but for now don't worry if you have to look it up every time!

Here is an example using `upper`:


```python
pd.read_sql("""
SELECT upper(firstName) AS name_in_all_caps
  FROM employees;
""", conn).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name_in_all_caps</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>DIANE</td>
    </tr>
    <tr>
      <th>1</th>
      <td>MARY</td>
    </tr>
    <tr>
      <th>2</th>
      <td>JEFF</td>
    </tr>
    <tr>
      <th>3</th>
      <td>WILLIAM</td>
    </tr>
    <tr>
      <th>4</th>
      <td>GERARD</td>
    </tr>
  </tbody>
</table>
</div>



#### `substr`

Another form of string manipulation you might need is finding a substring (subset of a string). In Python, we do this with string slicing. In SQL, there is a built-in function that does this instead. For SQLite specifically, this is called `substr` ([documentation here](https://www.sqlite.org/lang_corefunc.html#substr)).

Let's say we wanted just the first initial (first letter of the first name) for each employee:


```python
pd.read_sql("""
SELECT substr(firstName, 1, 1) AS first_initial
  FROM employees;
""", conn).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>first_initial</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>D</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M</td>
    </tr>
    <tr>
      <th>2</th>
      <td>J</td>
    </tr>
    <tr>
      <th>3</th>
      <td>W</td>
    </tr>
    <tr>
      <th>4</th>
      <td>G</td>
    </tr>
  </tbody>
</table>
</div>



If we wanted to add a `.` after each first initial, we could use the SQLite `||` (concatenate) operator. This works similarly to `+` with strings in Python:


```python
pd.read_sql("""
SELECT substr(firstName, 1, 1) || "." AS first_initial
  FROM employees;
""", conn).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>first_initial</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>D.</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M.</td>
    </tr>
    <tr>
      <th>2</th>
      <td>J.</td>
    </tr>
    <tr>
      <th>3</th>
      <td>W.</td>
    </tr>
    <tr>
      <th>4</th>
      <td>G.</td>
    </tr>
  </tbody>
</table>
</div>



We can also combine multiple column values, not just string literals. For example, below we combine the first and last name:


```python
pd.read_sql("""
SELECT firstName || lastName AS full_name
  FROM employees;
""", conn).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>full_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>DianeMurphy</td>
    </tr>
    <tr>
      <th>1</th>
      <td>MaryPatterson</td>
    </tr>
    <tr>
      <th>2</th>
      <td>JeffFirrelli</td>
    </tr>
    <tr>
      <th>3</th>
      <td>WilliamPatterson</td>
    </tr>
    <tr>
      <th>4</th>
      <td>GerardBondur</td>
    </tr>
  </tbody>
</table>
</div>



Hmm, that looks a bit odd. Let's concatenate those column values with a space (`" "`) string literal:


```python
pd.read_sql("""
SELECT firstName || " " || lastName AS full_name
  FROM employees;
""", conn).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>full_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Diane Murphy</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Mary Patterson</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Jeff Firrelli</td>
    </tr>
    <tr>
      <th>3</th>
      <td>William Patterson</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gerard Bondur</td>
    </tr>
  </tbody>
</table>
</div>



That looks better!

### Built-in SQL Functions for Math Operations

For these examples, let's switch over to using the `orderDetails` table:


```python
pd.read_sql("""SELECT * FROM orderDetails;""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>productCode</th>
      <th>quantityOrdered</th>
      <th>priceEach</th>
      <th>orderLineNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>S18_1749</td>
      <td>30</td>
      <td>136.00</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10100</td>
      <td>S18_2248</td>
      <td>50</td>
      <td>55.09</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10100</td>
      <td>S18_4409</td>
      <td>22</td>
      <td>75.46</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10100</td>
      <td>S24_3969</td>
      <td>49</td>
      <td>35.29</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10101</td>
      <td>S18_2325</td>
      <td>25</td>
      <td>108.06</td>
      <td>4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2991</th>
      <td>10425</td>
      <td>S24_2300</td>
      <td>49</td>
      <td>127.79</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2992</th>
      <td>10425</td>
      <td>S24_2840</td>
      <td>31</td>
      <td>31.82</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2993</th>
      <td>10425</td>
      <td>S32_1268</td>
      <td>41</td>
      <td>83.79</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2994</th>
      <td>10425</td>
      <td>S32_2509</td>
      <td>11</td>
      <td>50.32</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2995</th>
      <td>10425</td>
      <td>S50_1392</td>
      <td>18</td>
      <td>94.92</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>2996 rows × 5 columns</p>
</div>



#### `round`

Let's say we wanted to round the price to the nearest dollar. We could use the SQL `round` function ([documentation here](https://www.sqlite.org/lang_corefunc.html#round)), which is very similar to the the Python `round`:


```python
pd.read_sql("""
SELECT round(priceEach) AS rounded_price
  FROM orderDetails;
""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>rounded_price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>136.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>55.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>75.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>108.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2991</th>
      <td>128.0</td>
    </tr>
    <tr>
      <th>2992</th>
      <td>32.0</td>
    </tr>
    <tr>
      <th>2993</th>
      <td>84.0</td>
    </tr>
    <tr>
      <th>2994</th>
      <td>50.0</td>
    </tr>
    <tr>
      <th>2995</th>
      <td>95.0</td>
    </tr>
  </tbody>
</table>
<p>2996 rows × 1 columns</p>
</div>



#### `CAST`

The previous result looks ok, but it's returning floating point numbers. What if we want integers instead?

In Python, we might apply the `int` built-in function. In SQLite, we can use a `CAST` expression ([documentation here](https://www.sqlite.org/lang_expr.html#castexpr)):


```python
pd.read_sql("""
SELECT CAST(round(priceEach) AS INTEGER) AS rounded_price_int
  FROM orderDetails;
""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>rounded_price_int</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>136</td>
    </tr>
    <tr>
      <th>1</th>
      <td>55</td>
    </tr>
    <tr>
      <th>2</th>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35</td>
    </tr>
    <tr>
      <th>4</th>
      <td>108</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2991</th>
      <td>128</td>
    </tr>
    <tr>
      <th>2992</th>
      <td>32</td>
    </tr>
    <tr>
      <th>2993</th>
      <td>84</td>
    </tr>
    <tr>
      <th>2994</th>
      <td>50</td>
    </tr>
    <tr>
      <th>2995</th>
      <td>95</td>
    </tr>
  </tbody>
</table>
<p>2996 rows × 1 columns</p>
</div>



#### Basic Math Operations

Just like when performing math operations with Python, you don't always need to use a function. Sometimes all you need is an operator like `+`, `-`, `/`, or `*`. For example, below we multiply the price times the quantity ordered to find the total price:


```python
pd.read_sql("""
SELECT priceEach * quantityOrdered AS total_price
  FROM orderDetails;
""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4080.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2754.50</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1660.12</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1729.21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2701.50</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>2991</th>
      <td>6261.71</td>
    </tr>
    <tr>
      <th>2992</th>
      <td>986.42</td>
    </tr>
    <tr>
      <th>2993</th>
      <td>3435.39</td>
    </tr>
    <tr>
      <th>2994</th>
      <td>553.52</td>
    </tr>
    <tr>
      <th>2995</th>
      <td>1708.56</td>
    </tr>
  </tbody>
</table>
<p>2996 rows × 1 columns</p>
</div>



### Built-in SQL Functions for Date and Time Operations

For these examples, we'll look at yet another table within the database, this time the `orders` table:


```python
pd.read_sql("""SELECT * FROM orders;""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>orderDate</th>
      <th>requiredDate</th>
      <th>shippedDate</th>
      <th>status</th>
      <th>comments</th>
      <th>customerNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>2003-01-06</td>
      <td>2003-01-13</td>
      <td>2003-01-10</td>
      <td>Shipped</td>
      <td></td>
      <td>363</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10101</td>
      <td>2003-01-09</td>
      <td>2003-01-18</td>
      <td>2003-01-11</td>
      <td>Shipped</td>
      <td>Check on availability.</td>
      <td>128</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10102</td>
      <td>2003-01-10</td>
      <td>2003-01-18</td>
      <td>2003-01-14</td>
      <td>Shipped</td>
      <td></td>
      <td>181</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10103</td>
      <td>2003-01-29</td>
      <td>2003-02-07</td>
      <td>2003-02-02</td>
      <td>Shipped</td>
      <td></td>
      <td>121</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10104</td>
      <td>2003-01-31</td>
      <td>2003-02-09</td>
      <td>2003-02-01</td>
      <td>Shipped</td>
      <td></td>
      <td>141</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>321</th>
      <td>10421</td>
      <td>2005-05-29</td>
      <td>2005-06-06</td>
      <td></td>
      <td>In Process</td>
      <td>Custom shipping instructions were sent to ware...</td>
      <td>124</td>
    </tr>
    <tr>
      <th>322</th>
      <td>10422</td>
      <td>2005-05-30</td>
      <td>2005-06-11</td>
      <td></td>
      <td>In Process</td>
      <td></td>
      <td>157</td>
    </tr>
    <tr>
      <th>323</th>
      <td>10423</td>
      <td>2005-05-30</td>
      <td>2005-06-05</td>
      <td></td>
      <td>In Process</td>
      <td></td>
      <td>314</td>
    </tr>
    <tr>
      <th>324</th>
      <td>10424</td>
      <td>2005-05-31</td>
      <td>2005-06-08</td>
      <td></td>
      <td>In Process</td>
      <td></td>
      <td>141</td>
    </tr>
    <tr>
      <th>325</th>
      <td>10425</td>
      <td>2005-05-31</td>
      <td>2005-06-07</td>
      <td></td>
      <td>In Process</td>
      <td></td>
      <td>119</td>
    </tr>
  </tbody>
</table>
<p>326 rows × 7 columns</p>
</div>



What if we wanted to know how many days there are between the `requiredDate` and the `orderDate` for each order? Intuitively you might try something like this:


```python
pd.read_sql("""
SELECT requiredDate - orderDate
  FROM orders;
""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>requiredDate - orderDate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>321</th>
      <td>0</td>
    </tr>
    <tr>
      <th>322</th>
      <td>0</td>
    </tr>
    <tr>
      <th>323</th>
      <td>0</td>
    </tr>
    <tr>
      <th>324</th>
      <td>0</td>
    </tr>
    <tr>
      <th>325</th>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>326 rows × 1 columns</p>
</div>



Clearly that didn't work.

It turns out that we need to specify that we want the difference in *days*. One way to do this is using the `julianday` function ([documentation here](https://www.sqlite.org/lang_datefunc.html)):


```python
pd.read_sql("""
SELECT julianday(requiredDate) - julianday(orderDate) AS days_from_order_to_required
  FROM orders;
""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>days_from_order_to_required</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>321</th>
      <td>8.0</td>
    </tr>
    <tr>
      <th>322</th>
      <td>12.0</td>
    </tr>
    <tr>
      <th>323</th>
      <td>6.0</td>
    </tr>
    <tr>
      <th>324</th>
      <td>8.0</td>
    </tr>
    <tr>
      <th>325</th>
      <td>7.0</td>
    </tr>
  </tbody>
</table>
<p>326 rows × 1 columns</p>
</div>



If we wanted to select the order dates as well as dates 1 week after the order dates, that would look like this:


```python
pd.read_sql("""
SELECT orderDate, date(orderDate, "+7 days") AS one_week_later
  FROM orders;
""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderDate</th>
      <th>one_week_later</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2003-01-06</td>
      <td>2003-01-13</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2003-01-09</td>
      <td>2003-01-16</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2003-01-10</td>
      <td>2003-01-17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2003-01-29</td>
      <td>2003-02-05</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2003-01-31</td>
      <td>2003-02-07</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>321</th>
      <td>2005-05-29</td>
      <td>2005-06-05</td>
    </tr>
    <tr>
      <th>322</th>
      <td>2005-05-30</td>
      <td>2005-06-06</td>
    </tr>
    <tr>
      <th>323</th>
      <td>2005-05-30</td>
      <td>2005-06-06</td>
    </tr>
    <tr>
      <th>324</th>
      <td>2005-05-31</td>
      <td>2005-06-07</td>
    </tr>
    <tr>
      <th>325</th>
      <td>2005-05-31</td>
      <td>2005-06-07</td>
    </tr>
  </tbody>
</table>
<p>326 rows × 2 columns</p>
</div>



You can also use the `strftime` function, which is very similar to the [Python version](https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior). This is useful if you want to split apart a date or time value into different sub-parts. For example, here we extract the year, month, and day of month from the order date:


```python
pd.read_sql("""
SELECT orderDate,
       strftime("%m", orderDate) AS month,
       strftime("%Y", orderDate) AS year,
       strftime("%d", orderDate) AS day
  FROM orders;
""", conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderDate</th>
      <th>month</th>
      <th>year</th>
      <th>day</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2003-01-06</td>
      <td>01</td>
      <td>2003</td>
      <td>06</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2003-01-09</td>
      <td>01</td>
      <td>2003</td>
      <td>09</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2003-01-10</td>
      <td>01</td>
      <td>2003</td>
      <td>10</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2003-01-29</td>
      <td>01</td>
      <td>2003</td>
      <td>29</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2003-01-31</td>
      <td>01</td>
      <td>2003</td>
      <td>31</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>321</th>
      <td>2005-05-29</td>
      <td>05</td>
      <td>2005</td>
      <td>29</td>
    </tr>
    <tr>
      <th>322</th>
      <td>2005-05-30</td>
      <td>05</td>
      <td>2005</td>
      <td>30</td>
    </tr>
    <tr>
      <th>323</th>
      <td>2005-05-30</td>
      <td>05</td>
      <td>2005</td>
      <td>30</td>
    </tr>
    <tr>
      <th>324</th>
      <td>2005-05-31</td>
      <td>05</td>
      <td>2005</td>
      <td>31</td>
    </tr>
    <tr>
      <th>325</th>
      <td>2005-05-31</td>
      <td>05</td>
      <td>2005</td>
      <td>31</td>
    </tr>
  </tbody>
</table>
<p>326 rows × 4 columns</p>
</div>



Now that we are finished with our queries, we can close the database connection.


```python
conn.close()
```

## Summary

In this lesson, you saw how to execute several kinds of SQL `SELECT` queries. First, there were examples of specifying the selection of particular columns, rather than always using `SELECT *` to select all columns. Then you saw some examples of how to use `CASE` to transform column values using conditional logic. Finally, we walked through how to use built-in SQL functions, particularly for string, numeric, and date/time fields.
