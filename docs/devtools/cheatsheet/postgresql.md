[⬅ Back](sql.md)

# PostgreSQL specific queries
[Ref](https://www.youtube.com/watch?v=cnzka7kF5Zk)


### List all database
``` 
-- on psql
\l 
-- in pgadmin
SELECT datname FROM pg_database;
```
### Change database
```
\c <db_name>;
```

### List tables inside the database
```
\dt;
```

### table details
```
\d <table_name>;
```

### describe views
```
\dv 
```



