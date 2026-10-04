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






[:material-arrow-left: Back to CheatSheets](/devtools/cheatsheet/)