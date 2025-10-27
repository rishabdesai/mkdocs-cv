

[⬅ Back](sql.md)

## Sub-divisions

| DDL Commands | DML Commands | DQL Commands | DCL Commands | DTL/TCL Commands          |
| ------------ | ------------ | ------------ | ------------ | ------------------------- |
| CREATE       | INSERT       | SELECT       | GRANT        | BEGIN / START TRANSACTION |
| Drop         | UPDATE       |              | REVOKE       | COMMIT                    |
| ALTER        | DELETE       |              |              | ROLLBACK                  |
| RENAME       | MERGE        |              |              | SAVEPOINT                 |
| TRUNCATE     |              |              |              | RELEASE SAVEPOINT         |
| COMMENT      |              |              |              | SET TRANSACTION           |




## data-types
-in postgresql

| Numeric Types             | Character Types | Date/Time Types | Boolean Type | JSON/XML Types | Other Common Types  |
| ------------------------- | --------------- | --------------- | ------------ | -------------- | ------------------- |
| SMALLINT (int2)           | CHAR(n)         | DATE            | BOOLEAN      | JSON           | SERIAL / BIGSERIAL  |
| INTEGER (int, int4)       | VARCHAR(n)      | TIME            |              | JSONB          | UUID                |
| BIGINT (int8)             | TEXT            | TIMESTAMP       |              | XML            | ARRAY               |
| DECIMAL(p,s)              |                 | TIMESTAMPTZ     |              |                | BYTEA (binary data) |
| NUMERIC(p,s)              |                 | INTERVAL        |              |                | MONEY               |
| REAL (float4)             |                 |                 |              |                | CIDR / INET (IP)    |
| DOUBLE PRECISION (float8) |                 |                 |              |                | HSTORE (key/value)  |

[⬅ Back](sql.md)


[Basic Definations - (External Site)](https://drishabh.wordpress.com/2020/03/27/sql/)

## where-clause

- Where clause is used for searhing
- searching takes place in server HD
- WHERE clause is used to rescrict the rows from DB server HD to server RAM

[⬅ Back](sql.md#WHERE)

[:material-arrow-left: Back to CheatSheets](/devtools/cheatsheet/)

