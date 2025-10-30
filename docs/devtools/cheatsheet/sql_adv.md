[⬅ Back](sql.md)

# SQL Advance Concepts and Commands

## Stored Routine
> Sql statements that can be stored in database server which can be called number of times.
- Two TYpes
  - S.P. (Stored Procedure)
  - User Defined Functions
  
### Stored Procedure (SP)
> Set of sql statements and procedural logic that can perform operations such as 
> INSERT, UPDATE, DELETE and Querying the data. 

!!! info "Create table 'tempp'"
    ```
    create table tempp (
    fir numeric,
    sec character(15) );

    insert into tempp values(1,'one');
    insert into tempp values(2,'Two');
    
    ```

#### Insert

=== ":material-database: PostgreSQL (default)"

    ```sql
    CREATE OR REPLACE PROCEDURE insert_tempp(
        insert_fir numeric,
        insert_sec character(15)
    )
    LANGUAGE plpgsql
    AS $$
    BEGIN
    INSERT INTO tempp(fir, sec)
    VALUES(insert_fir, insert_sec);
    END;
    $$;

    -- call 
    call insert_tempp(999,'wow999');
    ```

=== "MySQL"
    ```sql
    DELIMITER $$

    CREATE PROCEDURE insert_tempp(
        IN insert_fir NUMERIC,
        IN insert_sec CHAR(15)
    )
    BEGIN
        INSERT INTO tempp(fir, sec)
        VALUES (insert_fir, insert_sec);
    END $$

    DELIMITER ;

    -- call
    CALL insert_tempp(999, 'wow999');
    ```

=== "SQL Server"
    ```sql
    CREATE PROCEDURE insert_tempp
        @insert_fir NUMERIC,
        @insert_sec CHAR(15)
    AS
    BEGIN
        INSERT INTO tempp (fir, sec)
        VALUES (@insert_fir, @insert_sec);
    END;

    -- call
    EXEC insert_tempp 999, 'wow999';
    ```

---

#### Update

=== ":material-database: PostgreSQL (default)"
    ```sql
    CREATE OR REPLACE PROCEDURE update_tempp(
        update_fir numeric,
        update_sec character(15)
    )
    LANGUAGE plpgsql
    AS $$
    BEGIN
    update tempp
    set sec =update_sec
    WHERE fir = update_fir;
    END;
    $$;

	-- call
    call update_tempp(1,'one_one');
    ```

=== "MySQL"
    ```sql
    DELIMITER $$

    CREATE PROCEDURE update_tempp(
        IN update_fir NUMERIC,
        IN update_sec CHAR(15)
    )
    BEGIN
        UPDATE tempp
        SET sec = update_sec
        WHERE fir = update_fir;
    END $$

    DELIMITER ;

    -- call
    CALL update_tempp(1, 'one_one');
    ```

=== "SQL Server"
    ```sql
    CREATE PROCEDURE update_tempp
        @update_fir NUMERIC,
        @update_sec CHAR(15)
    AS
    BEGIN
        UPDATE tempp
        SET sec = @update_sec
        WHERE fir = @update_fir;
    END;

    -- call
    EXEC update_tempp 1, 'one_one';
    ```


#### SELECT

=== ":material-database: PostgreSQL (default)"
    ```sql
    CREATE OR REPLACE PROCEDURE select_tempp()
    LANGUAGE plpgsql
    AS $$
    BEGIN
        RAISE NOTICE 'Displaying records from tempp table:';
        PERFORM * FROM tempp;
    END;
    $$;

    -- call
    CALL select_tempp();
    ```   

=== "MySQL"
    ```sql
    DELIMITER $$

    CREATE PROCEDURE select_tempp()
    BEGIN
        SELECT * FROM tempp;
    END $$

    DELIMITER ;

    -- call
    CALL select_tempp();
    ```

=== "SQL Server"
    ```sql
    CREATE PROCEDURE select_tempp
    AS
    BEGIN
        SELECT * FROM tempp;
    END;

    -- call
    EXEC select_tempp;
    ```

---


#### DELETE

=== ":material-database: PostgreSQL (default)"
    ```sql
    CREATE OR REPLACE PROCEDURE delete_tempp(
        delete_fir NUMERIC
    )
    LANGUAGE plpgsql
    AS $$
    BEGIN
        DELETE FROM tempp
        WHERE fir = delete_fir;
    END;
    $$;

    -- call
    CALL delete_tempp(999);
    ```

=== "MySQL"
    ```sql
    DELIMITER $$

    CREATE PROCEDURE delete_tempp(
        IN delete_fir NUMERIC
    )
    BEGIN
        DELETE FROM tempp
        WHERE fir = delete_fir;
    END $$

    DELIMITER ;

    -- call
    CALL delete_tempp(999);
    ```

=== "SQL Server"
    ```sql
    CREATE PROCEDURE delete_tempp
        @delete_fir NUMERIC
    AS
    BEGIN
        DELETE FROM tempp
        WHERE fir = @delete_fir;
    END;

    -- call
    EXEC delete_tempp 999;
    ```

---

!!! info "Create table 'emp20'"
    ```
    CREATE TABLE emp20 (
        ename VARCHAR(30),
        sal NUMERIC,
        job VARCHAR(15),
        deptno SMALLINT
        );

    insert into emp20 values('Scott',3000,'Clerk',10);
    insert into emp20 values('King',5000,'Manager',20);
        
    ```


#### Variables - '%type', '%rowtype', 'record'
 
=== "Variable1 (default)"
    ```sql
    CREATE OR REPLACE PROCEDURE variable1()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        x numeric;
    BEGIN
        x := 10;
        INSERT INTO tempp VALUES (x, 'variable1');
    END;
    $$;

    -- call
    call variable();

    ```

=== "Variable2"
    ```sql
    CREATE OR REPLACE PROCEDURE variable2()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        x numeric;
        y VARCHAR(15);
    BEGIN
        SELECT sal, job INTO x, y
        FROM emp20
        WHERE ename = 'King';

        INSERT INTO tempp VALUES (x, y);
    END;
    $$;
    -- call
    call variable2();
    ```

=== "Variable3"
    ```sql
    CREATE OR REPLACE PROCEDURE variable3()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        x emp20.sal%TYPE;
        y emp20.job%TYPE;
    BEGIN
        SELECT sal, job INTO x, y
        FROM emp20
        WHERE ename ILIKE 'king';

        INSERT INTO tempp VALUES (x, y);
    END;
    $$;
    -- call
    call variable3();
    ```

=== "Variable4"
    ```sql
    CREATE OR REPLACE PROCEDURE variable4()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        x emp20%ROWTYPE;
    BEGIN
        SELECT * INTO x
        FROM emp20
        WHERE ename = 'King';

        INSERT INTO tempp VALUES (x.sal, x.job);
    END;
    $$;
    -- call
    call variable4();
    ```

=== "Variable5"
    ```sql
    CREATE OR REPLACE PROCEDURE variable5()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        -- Define a composite record type
        pqr RECORD;

    BEGIN
        SELECT sal, job
        INTO pqr
        FROM emp20
        WHERE ename = 'King';

    INSERT INTO tempp VALUES (pqr.sal, pqr.job);
    END;
    $$;
    ```

#### Decision - if, if-elsif-else, if-else, case

=== "if"
    ```sql
    CREATE OR REPLACE PROCEDURE decision1()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        x numeric;
    BEGIN
        SELECT sal INTO x 
        FROM emp20 
        WHERE ename = 'King';

        IF x >= 5000 THEN
            INSERT INTO tempp VALUES (x, 'High Sal');
        END IF;
    END;
    $$;

    -- call
    call decision1();
    ```

=== "if-elsif-else"
    ```sql
    CREATE OR REPLACE PROCEDURE decision2()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        x numeric;
    BEGIN
        SELECT sal INTO x 
        FROM emp20 
        WHERE ename = 'King';

        IF x > 5000 THEN
            INSERT INTO tempp VALUES (x, 'High Sal');
        elsif x < 5000 then
            INSERT INTO tempp VALUES (x, 'Low Sal');
        else
            INSERT INTO tempp VALUES (x, 'Med Sal');
        
        END IF;
    END;
    $$;

    -- call
    call decision2();
    ```

=== "if-else"
    ```sql
    CREATE OR REPLACE PROCEDURE decision3()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        x numeric;
    BEGIN
        SELECT sal INTO x 
        FROM emp20 
        WHERE ename = 'King';

        IF x > 5000 THEN
            INSERT INTO tempp VALUES (x, 'High Sal');
        else
            IF x < 5000 then
                INSERT INTO tempp VALUES (x, 'Low Sal');
            else
                INSERT INTO tempp VALUES (x, 'Med Sal');
        
            END IF;
        END IF;
    END;
    $$;
    -- call
    call decision3();
    ```

=== "Boolean"
    ```sql
    -- for TRUE
    CREATE OR REPLACE PROCEDURE bool_test()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        x BOOLEAN;
    BEGIN
        x := TRUE;

        IF x THEN
            INSERT INTO tempp VALUES (1, 'hello');
        END IF;
    END;
    $$;

    -- call
    CALL bool_test();
    
    -- for FALSE
    CREATE OR REPLACE PROCEDURE bool_test1()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        x BOOLEAN;
    BEGIN
        x := FALSE;

        IF not x THEN
            INSERT INTO tempp VALUES (1, 'hello-false');
        END IF;
    END;
    $$;

    -- call
    CALL bool_test1();
    ```

=== "case"
    ```sql
    CREATE OR REPLACE PROCEDURE salary_case()
    LANGUAGE plpgsql
    AS $$
    DECLARE
        sal numeric;
        remark text;
    BEGIN
        -- Get salary of KING
        SELECT emp20.sal INTO sal
        FROM emp20
        WHERE ename ILIKE 'King';

        -- Use CASE to categorize salary
        remark := CASE
                    WHEN sal > 5000 THEN 'High Salary'
                    WHEN sal BETWEEN 3000 AND 4999 THEN 'Medium Salary'
                    ELSE 'Low Salary'
                END;

        INSERT INTO tempp VALUES (sal, remark);
    END;
    $$;
    -- call
    call salary_case();
    ```    

#### Loops - while, do-while

=== "while Loop"
    ```sql
    
    
    ```