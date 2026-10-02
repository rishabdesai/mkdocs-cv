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
#use a specific database (ex. classword) 
use classwork;

#show tables
show tables;

#create table
create table students(rollno int, name varchar(20), marks double);

#check table structure
describe students;

#insert records in student table
insert into students values(1, "abc", 91.00);
insert into students values(2, "pqr", 81.00);
insert into students values(3, "xyz", 71.00);

#display table content
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
#delete table - to delete one or more rows in a table
delete from table students where rollno=1;

#truncate - delete all rows (truncate is faster than delete)

truncate table students;

#drop - delete all rows as well as table structure

drop table students;
drop database classwork;

```



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


```sql


```






[:material-arrow-left: Back to CheatSheets](/devtools/cheatsheet/)