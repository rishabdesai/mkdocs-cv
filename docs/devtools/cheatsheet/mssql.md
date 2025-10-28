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
### Sub queries / Inner query / Nested query
```sql
SELECT * FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees)
```

- Types of sub-query:
  - single row - Return one row, one column [ WHERE, HAVING ]
  - Multiple row - Returns many rows (one column) [ IN, ANY, ALL ]
  - Correlated - depends on outer query [WHERE, SELECT]
  - Inline view - inside FROM, acts like temp table [ FROM ]


```sql
-- correlated subquery
-- find emp with the highest salary in the dept

select *
from employees e1
where salary = (
    select max(salary)
    from employees e2
    where e2.dept = e1.dept
);

-- another way
select * from employees where
salary in (select max(salary) from employees group by dept);
```

```SQL
-- inline view subquery
-- find dept whose avg salary is above 50,000; 
select dept, avg_sal
from (
select dept, avg(salary) as avg_sal
from employees
group by dept
) as dept_avg 
where avg_sal >50000;
```

### String functions
> CONCAT, CONCAT_WS, SUBSTRING, LEFT, RIGHT, LEN
> UPPER, LOWER, TRIM, LTRIM, RTRIM, REPLACE, CHAINDEX

### DATE functions

### ALTER TABLE

```sql
-- add a column
ALTER TABLE employees
ADD phone VARCHAR(15);

-- remove column
ALTER TABLE employees
DROP COLUMN phone;

-- change datatype of column
ALTER TABLE employees
ALTER COLUMN lname VARCHAR(150) NOT NULL;

-- change column name
EXEC sp_rename
'employees.fname', 'first_name' , 'COLUMN';


-- ADD or DROP a constraint

-- add default constraint as default_dept
ALTER TABLE employees
ADD CONSTRAINT default_dept DEFAULT 'Trainee'
FOR dept;
-- add unique constraint
ALTER TABLE employees
ADD UNIQUE (email_id);
```

### CHECK constraint
```sql
CREATE TABLE emp (
name varchar(100),
salary desimal(10,2) CHECK(salary>0)
)

-- drop a constraint
ALTER TABLE employees
DROP CONSTRAINT {constraint_name}
```
### Relationships
> primary key and foreign key

- ONE TO ONE, ONE TO MANY, MANY TO ONE, MANY TO MANY


### Joins

- cross join : every row from one table combile with every row or another table
- Inner join : matching rows of both tables based on specified columns
- Outer join ; left outer join, right outer join, full outer join.
- self join : table joined to itself

### OUTER APPLY
> it is used to join each row from one table(left side) to the results of a table-valued function of subquery(right side).

- ex: for each customer, show most recent order only (if they have one). If hey have no orders, still show the customer name.


### UNION, UNION ALL, EXCEPTS
- Use to combine result of two or more select statements into a single result set
- UNION removes duplicates, while UNION ALL shows the duplicates.
- EXCEPTS returns row from first query that do not exists in the second query.
- 

### VIEWS 
- a virtual table that shows data from a saved query. It does not store data, just displays if from other tables.

### Window function

### CTE (Common Table Expression)
- is a temp result set that you can define within a query to simplify complex sql statement.

### Stored Routine
- an sql statement or a set of SQL statement that can be stored on database server which can be called number of times.

### Stored Procedure
- set of sql statement & procedural logic that can perform operations such as INSERT, UPDATE, DELETE AND  QUERY the data.
- SP without parameter
- SP with input parameter
- SP with input and output parameter
  
### ITVF (inline table-valued function)
>

### Index

### Triggers




[⬅ Back](sql.md)
