[⬅ Back](sql.md)

# MS SQL specific queries
[Ref](https://www.youtube.com/watch?v=4bTt1JbAp3M)

### List existing database
```SQL
select name from sys.databases;

-- another command
exec sp_databases;
```

### CREATE DATABASE
> Create new database.
```sql
CREATE DATABASE school_db;

-- use databse
 USE school_db;

 -- show present working database name
 SELECT DB_NAME();
```

### CRUD (Create, Read, Updade, Delete)

### CREATE TABLE
> To Create a table.
```sql
CREATE TABLE students (
    student_id int,
    name varchar(100),
    age int,
    grade int
);
```

### CREATE TABLE
> To Create a table.
```sql
CREATE TABLE students (
    student_id int,
    name varchar(100),
    age int,
    grade int
);

-- checking existing table
EXEC sp_help 'students';
```

### INSERT
> insert data in table.
```sql
INSERT INTO students (student_id,name,age,grade)
VALUES (101,'Raju',10,5);
```

### SELECT
> Reading data from table.
```sql
SELECT * from students;
```

### SELECT
> Reading data from table.
```sql
SELECT * from students;
SELECT name from students;
```

### UPDATE
> update value.
```sql
UPDATE students
SET grade = 12
WHERE student_id=103;
```

### DELETE
> delete value.
```sql
DELETE from students
WHERE student_id=104;
```

### TRUNCATE 
> Truncate is similar to delete but without select clause.
> It removes all rows from table.
```sql
TRUNCATE TABLE students;

```

### Datatypes
> - Numeric : INT, BIGINT, FLOAT, DECIMAL/NUMERIC
> - String : VARCHAR, CHAR
> - Date : DATE
> - Date Time : DATETIME
> - Boolean : BIT(0/1)

### Constraint 

> - PRIMARY KEY,  NOT NULL, DEFAULT, IDENTITY, UNIQUE
```sql
CREATE TABLE Customers (
customer_id INT IDENTITY(100,1) PRIMARY KEY,
customer_name VARCHAR(100) NOT NULL,
email VARCHAR(100) UNIQUE
created_at DATETIME DEFAULT GETDATE()
);
```

### Clauses
> * WHERE = Filter rows based on condition
> * DISTINCT = Remove duplicate row and result set
> * ORDER BY = Sort result by one or more columns
> * LIKE & NOT LIKE = Finds patterns in text (using wildcards % & _ )
> * TOP = Limits no. of rows returned (ONLY IN MSSQL)

### Relational Operators
> '>, >=, <, <=, <>, !=, =

### Logical Operators
> * AND = when both condition are true
> * OR = when either of the conditon is true
> * IN, NOT IN, BETWEEN, IS NULL

### CASE
```sql
SELECT fname, lname, salary,
CASE
    WHEN salary > 100000 THEN 'High'
    WHEN salary >= 80000 AND salary <=100000 THEN 'Med'
    ELSE 'Std'
END AS salary_band

FROM employees;
```

### Aggregate Functions
> COUNT, SUM, MIN, MAX, AVG

### GROUP BY
```sql
select dept, count(*) from employees
group by dept;
```

### HAVING clause
```sql
SELECT dept, sum(salary) as total
FROM employees
GROUP BY dept
HAVING sum(salary) > 200000;
```

### GROUP BY ROLLUP
> it is extension of group by clause that generates subtotals and a grand total for a set of columns.
```sql
select dept,count(emp_id) as count 
from employees
group by rollup (dept);
```

### COALESCE
> returns the first non-null value 
```sql
select coalesce(dept, 'Total') ,count(emp_id) as count 
from employees
group by rollup (dept);
```
### Sub queries











[⬅ Back](sql.md)
