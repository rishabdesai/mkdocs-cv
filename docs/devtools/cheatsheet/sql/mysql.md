[:material-arrow-left: Back to CheatSheets](/devtools/cheatsheet/)


# mySQL

login to sql 
```sql
terminal> mysql -u root -h localhost -p
<<enter password>>
```

CREATE database
```sql
create database classwork.

show databases;
```

create user
```sql
create user dbuser@localhost identified by 'password';

# check user list
select user, host from mysql.user;

#give permission to the user
grant all privileges on claswork.* to dbuser@localhost;

flush privideges;

```

```sql
-- use a specific database (ex. classwork) 
use classwork;

-- show tables
show tables;

-- create table
create table students(rollno int, name varchar(20), marks double);

-- check table structure
describe students;

-- insert records in student table
insert into students values(1, "abc", 91.00);
insert into students values(2, "pqr", 81.00);
insert into students values(3, "xyz", 71.00);

-- display table content
select * from students;

```


update
```sql
select * from students;

update students set marks=75 where rollno=3;

update students set name ='aaa' where rollno=1;

update students set marks=marks+5 where marks <=75;
```


```sql
-- delete table - to delete one or more rows in a table
delete from table students where rollno=1;

-- truncate - delete all rows (truncate is faster than delete)

truncate table students;

-- drop - delete all rows as well as table structure

drop table students;
drop database classwork;

```

<details>
<summary>Click to expand tables - dept, emp</summary>

```sql
DROP TABLE IF EXISTS dept;
DROP TABLE IF EXISTS emp;

CREATE TABLE dept(deptno INT(4), dname VARCHAR(40), loc VARCHAR(40));

INSERT INTO dept VALUES (10,'ACCOUNTING','NEW YORK');
INSERT INTO dept VALUES (20,'RESEARCH','DALLAS');
INSERT INTO dept VALUES (30,'SALES','CHICAGO');
INSERT INTO dept VALUES (40,'OPERATIONS','BOSTON');

CREATE TABLE emp(empno INT(4), ename VARCHAR(40), job VARCHAR(40), mgr INT(4), hire DATE, sal DECIMAL(8,2), comm DECIMAL(8,2), deptno INT(4));

INSERT INTO emp VALUES (7369,'SMITH','CLERK',7902,'1980-12-17',800.00,NULL,20);
INSERT INTO emp VALUES (7499,'ALLEN','SALESMAN',7698,'1981-02-20',1600.00,300.00,30);
INSERT INTO emp VALUES (7521,'WARD','SALESMAN',7698,'1981-02-22',1250.00,500.00,30);
INSERT INTO emp VALUES (7566,'JONES','MANAGER',7839,'1981-04-02',2975.00,NULL,20);
INSERT INTO emp VALUES (7654,'MARTIN','SALESMAN',7698,'1981-09-28',1250.00,1400.00,30);
INSERT INTO emp VALUES (7698,'BLAKE','MANAGER',7839,'1981-05-01',2850.00,NULL,30);
INSERT INTO emp VALUES (7782,'CLARK','MANAGER',7839,'1981-06-09',2450.00,NULL,10);
INSERT INTO emp VALUES (7788,'SCOTT','ANALYST',7566,'1982-12-09',3000.00,NULL,20);
INSERT INTO emp VALUES (7839,'KING','PRESIDENT',NULL,'1981-11-17',5000.00,NULL,10);
INSERT INTO emp VALUES (7844,'TURNER','SALESMAN',7698,'1981-09-08',1500.00,0.00,30);
INSERT INTO emp VALUES (7876,'ADAMS','CLERK',7788,'1983-01-12',1100.00,NULL,20);
INSERT INTO emp VALUES (7900,'JAMES','CLERK',7698,'1981-12-03',950.00,NULL,30);
INSERT INTO emp VALUES (7902,'FORD','ANALYST',7566,'1981-12-03',3000.00,NULL,20);
INSERT INTO emp VALUES (7934,'MILLER','CLERK',7782,'1982-01-23',1300.00,NULL,10);

CREATE TABLE accounts (id INT, type CHAR(20), amount DOUBLE);

INSERT INTO accounts VALUES(1, 'Saving', 10000);
INSERT INTO accounts VALUES(2, 'Saving', 2000);
INSERT INTO accounts VALUES(3, 'Saving', 5000);
INSERT INTO accounts VALUES(4, 'Saving', 3000);

SELECT * FROM accounts;
```


</details>



```sql
-- UNION operator is to combien the results of two queries
-- both queries must have same number of columns
-- it automatically deletes duplicate records
-- to retain duplicate use UNION ALL operator

(select deptno, sum(sal) from emp
group by deptno)
UNION
(select null, sum(sal) from emp);

-- better version of above
select deptno, sum(sal) from emp
group by deptno
with rollup;

```

### Joins

<details>
<summary>Click to expand tables - depts, emps, addr, meeting, emp_meeting</summary>

```sql
DROP TABLE IF EXISTS depts;
DROP TABLE IF EXISTS emps;
DROP TABLE IF EXISTS addr; 
DROP TABLE IF EXISTS meeting;
DROP TABLE IF EXISTS emp_meeting;

CREATE TABLE depts (deptno INT, dname VARCHAR(20));
INSERT INTO depts VALUES (10, 'DE');
INSERT INTO depts VALUES (20, 'QA');
INSERT INTO depts VALUES (30, 'OP');
INSERT INTO depts VALUES (40, 'AC');

CREATE TABLE emps (empno INT, ename VARCHAR(20), deptno INT, mgr INT);
INSERT INTO emps VALUES (1, 'Amar', 10, 4);
INSERT INTO emps VALUES (2, 'Ram', 10, 3);
INSERT INTO emps VALUES (3, 'Narang', 20, 4);
INSERT INTO emps VALUES (4, 'Nitin', 50, 5);
INSERT INTO emps VALUES (5, 'Samar', 50, NULL);

CREATE TABLE addr(empno INT, tal VARCHAR(20), dist VARCHAR(20));
INSERT INTO addr VALUES (1, 'kol', 'Kolkata');
INSERT INTO addr VALUES (2, 'mum', 'Mumbai');
INSERT INTO addr VALUES (3, 'pun', 'Pune');
INSERT INTO addr VALUES (4, 'nas', 'Nashik');
INSERT INTO addr VALUES (5, 'nag', 'Nagpur');

CREATE TABLE meeting (meetno INT, topic VARCHAR(20), venue VARCHAR(20));
INSERT INTO meeting VALUES (100, 'Scheduling', 'Director Cabin');
INSERT INTO meeting VALUES (200, 'Annual meet', 'Board Room');
INSERT INTO meeting VALUES (300, 'App Design', 'Co-director Cabin');

CREATE TABLE emp_meeting (meetno INT, empno INT);
INSERT INTO emp_meeting VALUES (100, 3);
INSERT INTO emp_meeting VALUES (100, 4);
INSERT INTO emp_meeting VALUES (200, 1);
INSERT INTO emp_meeting VALUES (200, 2);
INSERT INTO emp_meeting VALUES (200, 3);
INSERT INTO emp_meeting VALUES (200, 4);
INSERT INTO emp_meeting VALUES (200, 5);
INSERT INTO emp_meeting VALUES (300, 1);
INSERT INTO emp_meeting VALUES (300, 2);
INSERT INTO emp_meeting VALUES (300, 4);

```


</details>




#### cross join 
- Cartesian Join/ Cross Join:
- It is a join without a WHERE clause.
- Every row in driving table(outer table) is combined with each and every row of driven (inner table) table.
- practical use – payroll printing.

```sql
select e.ename, d.dname from  emps e
cross join depts d;
```

#### Inner join
- inner join is used to return the rows from both tables that satisfy the join condition using ON.
- Non matching rows from both tables are skipped
- If the join condition contains equality check, it is reffered as equi-join, otherwise it is non-equi-join.
  
```sql

select e.ename, d.dname from emps e
inner join depts d on e.deptno = d.deptno

```

#### Outer Join

- a. Left Outer Join: It shows matching rows of both the tables plus non-matching rows of outer table.
- b. Right Outer Join: It is opposite of Left outer join.
- c. Full Outer Join: It shows matching rows of both the tables plus non-matching rows of both the tables.

```sql
-- Left outer join
select e.ename, d.dname from emps e 
left outer join depts d 
on e.deptno = d.deptno

-- right outer join
select e.ename, d.dname from emps e 
right outer join depts d 
on e.deptno = d.deptno

-- same output we can get using left join - swapping table positions in join
SELECT e.ename, d.dname FROM depts d
LEFT JOIN emps e ON e.deptno = d.deptno;

-- full outer join (not available in mysql - use set operator - union and union all)

* UNION ALL
	* duplicated rows are retained.
* UNION
	* duplicated rows are omitted.

-- Union all
(SELECT e.ename, d.dname FROM emps e
LEFT JOIN depts d ON e.deptno = d.deptno)
UNION ALL
(SELECT e.ename, d.dname FROM emps e
RIGHT JOIN depts d ON e.deptno = d.deptno);

-- union
(SELECT e.ename, d.dname FROM emps e
LEFT JOIN depts d ON e.deptno = d.deptno)
UNION
(SELECT e.ename, d.dname FROM emps e
RIGHT JOIN depts d ON e.deptno = d.deptno);
-- same output as full outer join

```
#### self join
- when join is done on same table, then it is know on self join. the both columns in condition belongs to the same table.
- self join may be inner join or outer join

```sql
SELECT e.ename, m.ename AS mname FROM emps m
INNER JOIN emps e ON e.mgr = m.empno;

SELECT e.ename, m.ename AS mname FROM emps m
RIGHT JOIN emps e ON e.mgr = m.empno;

```

#### USING keyword in Join
* Specify equi-join condition
* When joined column names are same in both tables
* Can be used for inner or outer joins.

```SQL
SELECT ename, dname FROM emps e
INNER JOIN depts d USING (deptno);
-- USING (deptno) ---> ON e.deptno = d.deptno (equi-join)
-- this can be done only if joined column name is same in both the tables.

SELECT ename, dname FROM emps e
LEFT OUTER JOIN depts d USING (deptno);
-- USING (deptno) ---> ON e.deptno = d.deptno (equi-join)
```

#### Natural Join
Automatically join two tables on columns whose names are same (in both table) with equality condition.

```SQL
DESCRIBE emps;

DESCRIBE depts;

SELECT ename, dname FROM emps e
NATURAL JOIN depts d;
-- emps e NATURAL JOIN depts d --> INNER JOIN depts d ON e.deptno = d.deptno;

SELECT ename, dname FROM emps e
NATURAL LEFT JOIN depts d;
-- emps e NATURAL LEFT JOIN depts d --> LEFT JOIN depts d ON e.deptno = d.deptno;
```
### Sub queries
- sub-query is query within query. 
- Typically it work with select statements
- For each row of outer query result, sub-query is executed once.

<details>
<summary>Click to expand tables for sub-query - dept, emp
</summary>

```sql
DROP TABLE IF EXISTS dept;
DROP TABLE IF EXISTS emp;

CREATE TABLE dept(deptno INT(4), dname VARCHAR(40), loc VARCHAR(40));

INSERT INTO dept VALUES (10,'ACCOUNTING','NEW YORK');
INSERT INTO dept VALUES (20,'RESEARCH','DALLAS');
INSERT INTO dept VALUES (30,'SALES','CHICAGO');
INSERT INTO dept VALUES (40,'OPERATIONS','BOSTON');

CREATE TABLE emp(empno INT(4), ename VARCHAR(40), job VARCHAR(40), mgr INT(4), hire DATE, sal DECIMAL(8,2), comm DECIMAL(8,2), deptno INT(4));

INSERT INTO emp VALUES (7369,'SMITH','CLERK',7902,'1980-12-17',800.00,NULL,20);
INSERT INTO emp VALUES (7499,'ALLEN','SALESMAN',7698,'1981-02-20',1600.00,300.00,30);
INSERT INTO emp VALUES (7521,'WARD','SALESMAN',7698,'1981-02-22',1250.00,500.00,30);
INSERT INTO emp VALUES (7566,'JONES','MANAGER',7839,'1981-04-02',2975.00,NULL,20);
INSERT INTO emp VALUES (7654,'MARTIN','SALESMAN',7698,'1981-09-28',1250.00,1400.00,30);
INSERT INTO emp VALUES (7698,'BLAKE','MANAGER',7839,'1981-05-01',2850.00,NULL,30);
INSERT INTO emp VALUES (7782,'CLARK','MANAGER',7839,'1981-06-09',2450.00,NULL,10);
INSERT INTO emp VALUES (7788,'SCOTT','ANALYST',7566,'1982-12-09',3000.00,NULL,20);
INSERT INTO emp VALUES (7839,'KING','PRESIDENT',NULL,'1981-11-17',5000.00,NULL,10);
INSERT INTO emp VALUES (7844,'TURNER','SALESMAN',7698,'1981-09-08',1500.00,0.00,30);
INSERT INTO emp VALUES (7876,'ADAMS','CLERK',7788,'1983-01-12',1100.00,NULL,20);
INSERT INTO emp VALUES (7900,'JAMES','CLERK',7698,'1981-12-03',950.00,NULL,30);
INSERT INTO emp VALUES (7902,'FORD','ANALYST',7566,'1981-12-03',3000.00,NULL,20);
INSERT INTO emp VALUES (7934,'MILLER','CLERK',7782,'1982-01-23',1300.00,NULL,10);

```

</details>


#### Single row sub-query
- sub-query returns single row

```sql
-- find emp with max sal.
SET @maxsal=(SELECT MAX(sal) FROM emp);
SELECT * FROM emp WHERE sal = @maxsal;

-- find emp with second highest sal.
SET @sal2 = (SELECT DISTINCT sal FROM emp ORDER BY sal DESC LIMIT 1,1);
SELECT * FROM emp WHERE sal = @sal2;
```


```sql
-- find emp with third highest sal.
SELECT * FROM emp WHERE sal = (SELECT DISTINCT sal FROM emp ORDER BY sal DESC LIMIT 2,1);

-- find emps having sal more than sal of all saleman
select * from emp where sal > (select max(sal) from emp where job="SALESMAN")

-- find emp having sal less than sal of any salesman
select * from emp where sal < (select max(sal) from emp where job="SALESMAN")
```

### Multi-row sub-query
- sub-query returns multiple rows
- usually it is compared in outer query using operators like IN, ANY, ALL
- IN operator checks for equality with results from sub-queries (like logical OR)
- ANY operator compares with all the result from sub-queries (like logical OR)
- ALL operator compares with all the results from sub-queries (like logical AND)

```sql
-- find emps having sal more than sal of all saleman
select * from emp where sal > ALL(select sal from emp where job = "SALESMAN")

-- find emp having sal less than sal of any salesman
select * from emp where sal < ANY(select sal from emp where job = "SALESMAN")

-- Find depts which has at least one emp.
SELECT * FROM dept WHERE deptno = ANY(SELECT deptno FROM emp);
-- deptno = 10 OR deptno = 20 OR deptno = 30
-- ANY operator can be used to check =, !=, >, <, >=, <=

SELECT * FROM dept WHERE deptno IN (SELECT deptno FROM emp);
-- deptno = 10 OR deptno = 20 OR deptno = 30
-- IN operator can be used to check "=" (equality) only

-- Find depts which doesn't have any emp.
SELECT * FROM dept WHERE deptno != ALL(SELECT deptno FROM emp);

SELECT * FROM dept WHERE deptno NOT IN (SELECT deptno FROM emp);

```

### Derived tables
--- Derived table is a virtual table returned from a **sub-query in FROM clause** of outer query. This is also referred as **Inline view**.

- advantages: more readable than joins and correlated subqueries, overcome limitations of GROUP BY.

-- tables used emp

```sql
-- categorize emps in 2 categories.
-- poor: sal < 1500
-- rich: sal > 2500
-- middle: 1500 <= sal <= 2500

SELECT empno, ename, sal, CASE
WHEN sal < 1500 THEN 'POOR'
WHEN sal > 2500 THEN 'RICH'
ELSE 'MIDDLE'
END AS category FROM emp;

-- count emps in each category
SELECT category, COUNT(empno)
FROM
(SELECT empno, ename, sal, CASE
WHEN sal < 1500 THEN 'POOR'
WHEN sal > 2500 THEN 'RICH'
ELSE 'MIDDLE'
END AS category FROM emp) AS emp_cat
GROUP BY category;

-- create view and use it.
CREATE VIEW v_empcategory AS
SELECT empno, ename, sal, CASE
WHEN sal < 1500 THEN 'POOR'
WHEN sal > 2500 THEN 'RICH'
ELSE 'MIDDLE'
END AS category FROM emp;

SELECT category, COUNT(empno)
FROM v_empcategory GROUP BY category;
```



```sql
-- count emps in each dept & each category
SELECT dname, empno, ename, sal, CASE
WHEN sal < 1500 THEN 'POOR'
WHEN sal > 2500 THEN 'RICH'
ELSE 'MIDDLE'
END AS category FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno;

SELECT dname, category, COUNT(empno)
FROM
(SELECT dname, empno, ename, sal, CASE
WHEN sal < 1500 THEN 'POOR'
WHEN sal > 2500 THEN 'RICH'
ELSE 'MIDDLE'
END AS category FROM emp e
INNER JOIN dept d ON e.deptno = d.deptno
) AS emp_cat
GROUP BY dname, category;

```


```sql
-- find max sal of each dept
SELECT deptno, MAX(sal) FROM emp
GROUP BY deptno;

-- find emp with max sal in each dept.
SELECT e.empno, e.ename, e.sal, e.deptno
FROM emp e
INNER JOIN
(SELECT deptno, MAX(sal) mxsal FROM emp
GROUP BY deptno) AS md
ON e.deptno = md.deptno
WHERE e.sal = md.mxsal;

-- for derived table, the alias also can be put at the end of query ex. (deptno, mxsal)
SELECT e.empno, e.ename, e.sal, e.deptno
FROM emp e
INNER JOIN
(SELECT deptno, MAX(sal) FROM emp
GROUP BY deptno) AS md (deptno, mxsal)
ON e.deptno = md.deptno
WHERE e.sal = md.mxsal;

-- using correlated sub-query 
-- correlated subquery = innter query is dependent on outer query
SELECT e.empno, e.ename, e.sal, e.deptno
FROM emp e 
WHERE e.sal = (SELECT MAX(sal) FROM emp me WHERE me.deptno = e.deptno);
```

### Lateral Derived Tables

```sql
-- display ename, sal & dname using join with derived table.
SELECT e.ename, e.sal, d.dname FROM emp e
JOIN LATERAL (SELECT dname FROM dept d WHERE d.deptno = e.deptno) AS d;

```

### Common Table Expressions
-- CTE is a virtual table returned from a SELECT query
- it can be used for CRUD operations, creating table or view
- types - Non recursive CTE, Recursive CTE
- Applications of CTE - Readable, better organization of large queries, non-reusable view, overcome limitation of GROUP BY, Recursion for hierarchical data.

```sql
-- find emp with max sal in each dept.
with md (deptno, mxsal) as
(SELECT deptno, MAX(sal) FROM emp
GROUP BY deptno) 
SELECT e.empno, e.ename, e.sal, e.deptno
FROM emp e
INNER JOIN md
ON e.deptno = md.deptno
WHERE e.sal = md.mxsal;
```


```sql
-- find avg of deptwise total sal.
WITH dept_total AS
(
SELECT deptno, SUM(sal) total FROM emp
GROUP BY deptno
)
SELECT AVG(total) FROM dept_total;

-- above is similar to (using derived table)
select avg(total) from 
(select deptno, sum(sal) total from emp
group by deptno) as dept_total;

```


```sql
-- Compare sal of each emp with avg sal in his dept
-- and avg sal for his job.

-- avg salary in job
select job, avg(sal) jobAvgSal from emp 
group by job;

-- avg salary in dept
select deptno, avg(sal) deptAvgSal from emp
group by deptno;

-- using derived table
select ename, e.sal, e.job, e.deptno, jobAvgSal, deptAvgSal
from emp e
join(select job, avg(sal) jobAvgSal from emp
group by job) as ej
on e.job = ej.job
join(select deptno, avg(sal) deptAvgSal from emp
group by deptno) as ed
on e.deptno = ed.deptno;


-- using CTE
with ej as (select job, avg(sal) jobAvgSal from emp
group by job),
ed as (select deptno, avg(sal) deptAvgSal from emp
group by deptno)
select ename, e.sal, e.job, e.deptno, jobAvgSal, deptAvgSal
from emp e 
join ej on e.job = ej.job
join ed on e.deptno = ed.deptno;
```










[:material-arrow-left: Back to CheatSheets](/devtools/cheatsheet/)