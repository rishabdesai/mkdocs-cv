# SQL

- SQL = Structured Query Language
  
---
- [Sub-divison in SQL](sql_info.md#Sub-divisions)
- [Data types in SQL](sql_info.md#data-types)


## Basic Commands

### CREATE
> To Create a table.
```sql
CREATE TABLE emp1 (
empno CHAR(4),
ename VARCHAR(30),
sal NUMERIC(10,2),
city VARCHAR(30),
deptno SMALLINT
);
```

### Insert
> Adds new data into an existing table.
```sql
insert into emp1 values (‘1′,’Adams’,1000.0,’Mumbai’,10);
insert into emp1 values (‘2′,’Black’,2000.0,’Delhi’,10);
insert into emp1 values (‘3′,’Allen’,2500.0,’Mumbai’,20);
insert into emp1 values (‘4′,’King’,3000.0,’Delhi’,30);
insert into emp1 values (‘5′,’Ford’,4000.0,’Mumbai’,40);
```

### SELECT
> Used to fetch data from one or more tables.
```sql
SELECT * FROM emp1;
```

#### WHERE
> To restrict the rows
```sql
SELECT * FROM emp1
where deptno =10;
```
- [more about WHERE clause](sql_info.md#where-clause)

#### Relational Operator
>
```sql
< , > , <= , >=, <>, !=, = 
```
```sql
SELECT * from emp1 
where sal > 2000;
```

#### Logical Operator
>
```sql
NOT, AND, OR 
```
```sql
SELECT * from emp1 
where sal > 2000 and sal < 3000;
```
#### Computed Column
> Computed column never get saved in Harddisk.
```sql
SELECT ename, sal, sal*12 as annual_sal from emp1;
```

#### Arithmetic Operator
> 
```sql
+, - , * , /
```

### DISTINCT
> To Supress the duplicates .
```sql
SELECT DISTINCT job FROM emp1;
```

#### ORDER BY clause
> Order by clause is the last clause in the select statement.
```sql
SELECT ename, sal, deptno from emp1
ORDER by deptno;
```



