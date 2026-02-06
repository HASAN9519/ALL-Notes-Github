## Data manipulation language 

#### Data manipulation language statements or DML statements are used to read and modify data; five basic SQL commands or DML statements are used to create a table, insert data to populate table, select data from table, update data in table and delete data from table 

## Data Definition Language

#### Data Definition Language or DDL statements are used to define, change or drop database objects such as tables; Common DDL statement types include CREATE, ALTER, TRUNCATE and DROP; CREATE is used for creating tables and defining its columns; ALTER is used for altering tables including adding and dropping columns and modifying their data types; TRUNCATE is used for deleting data in a table but not table itself and DROP is used for deleting table

## SELECT Statement

#### syntax of SELECT statement
    SELECT column1, column2, ... FROM table_name WHERE condition;

#### <> means not equal
    SELECT Title, ProductionCompany, Locations, ReleaseYear FROM FilmLocations WHERE Writer<>"James Cameron";

#### COUNT, DISTINCT, LIMIT
    SELECT COUNT(*) FROM FilmLocations;
    SELECT COUNT(Locations) FROM FilmLocations WHERE Writer="James Cameron";
    SELECT DISTINCT Title FROM FilmLocations;
    SELECT DISTINCT Title, ReleaseYear FROM FilmLocations WHERE ReleaseYear>=2001;
    SELECT COUNT(DISTINCT Distributor) FROM FilmLocations WHERE Actor1="Clint Eastwood";
    SELECT * FROM FilmLocations LIMIT 25;

#### Retrieve the first 15 rows from the "FilmLocations" table starting from row 11
    SELECT * FROM FilmLocations LIMIT 15 OFFSET 10;
    SELECT DISTINCT Title FROM FilmLocations WHERE ReleaseYear=2015 LIMIT 10;

#### INSERT, UPDATE, DELETE
    INSERT INTO Instructor(ins_id, lastname, firstname, city, country)
        VALUES(8, 'Ryan', 'Steve', 'Barlby', 'GB'), (9, 'Sannareddy', 'Ramesh', 'Hyderabad', 'IN');

    UPDATE Instructor SET city='Dubai', country='AE' WHERE ins_id=5;
    DELETE FROM instructor WHERE ins_id = 6;

#### CREATE tables
    CREATE TABLE PETSALE(
        ID INTEGER NOT NULL,
        PET CHAR(20),
        SALEPRICE DECIMAL(6,2),
        PROFIT DECIMAL(6,2),
        SALEDATE DATE
        );

#### ALTER, DROP and Truncate tables
#### ALTER using ADD COLUMN
    ALTER TABLE table_name ADD COLUMN column_name data_type column_constraint;
    ALTER TABLE PETSALE ADD COLUMN QUANTITY INTEGER;

#### ALTER using DROP COLUMN
    ALTER TABLE table_name DROP COLUMN column_name;
    ALTER TABLE PETSALE DROP COLUMN PROFIT;

#### ALTER using ALTER COLUMN
    ALTER TABLE table_name ALTER COLUMN column_name SET DATA TYPE data_type;

#### Changing data type & length of PET column to VARCHAR(20)
    ALTER TABLE PETSALE ALTER COLUMN PET SET DATA TYPE VARCHAR(20); 

#### ALTER using RENAME COLUMN
    ALTER TABLE table_name RENAME COLUMN current_column_name TO new_column_name;
    ALTER TABLE PETSALE RENAME COLUMN PET TO ANIMAL;

#### Remove all rows from PET table using TRUNCATE
    TRUNCATE TABLE PET IMMEDIATE;

#### Delete PET table using DROP
    DROP TABLE PET;

## LIKE predicate to search string patterns

#### percent sign is called a wildcard character and used to substitute other characters
#### percent sign can be placed before the pattern, after the pattern or both before and after the pattern
    SELECT F_NAME, L_NAME FROM EMPLOYEES WHERE ADDRESS LIKE '%Elgin,IL%';
    SELECT F_NAME, L_NAME FROM EMPLOYEES WHERE B_DATE LIKE '197%';

#### BETWEEN helps to retrieve rows in a range of values 
    SELECT * FROM EMPLOYEES WHERE (SALARY BETWEEN 60000 AND 70000) AND DEP_ID = 5;

#### IN operator allows to specify a set of values in a WHERE clause
    SELECT * FROM EMPLOYEES WHERE DEP_ID IN (2,5);
    SELECT * FROM EMPLOYEES WHERE SEX IN ('M','F');

#### ORDER BY helps sorting
    SELECT F_NAME, L_NAME, DEP_ID FROM EMPLOYEES ORDER BY DEP_ID;
    SELECT F_NAME, L_NAME, DEP_ID FROM EMPLOYEES ORDER BY DEP_ID DESC, L_NAME DESC;

#### In SQL Query D and E are aliases for table names to simply write D.COLUMN_NAME rather than full form DEPARTMENTS.COLUMN_NAME
    SELECT D.DEP_NAME, E.F_NAME, E.L_NAME FROM EMPLOYEES as E, DEPARTMENTS as D WHERE E.DEP_ID = D.DEPT_ID_DEP ORDER BY D.DEP_NAME, E.L_NAME DESC;

#### GROUP BY clause groups a result into subsets that has matching values for one or more columns
#### GROUP BY DEP_ID retrieves number of employees in the department For each department ID 
    SELECT DEP_ID, COUNT(*) AS Total FROM EMPLOYEES GROUP BY DEP_ID;

#### GROUP BY DEP_ID retrieves number of employees in the department and average employee salary in the department 
    SELECT DEP_ID, COUNT(*) AS Total, AVG(SALARY) AS AVG_SALARY FROM EMPLOYEES GROUP BY DEP_ID ORDER BY AVG_SALARY;

#### HAVING clause works only with GROUP BY clause
    SELECT DEP_ID, COUNT(*) AS NUM_EMPLOYEES, AVG(SALARY) AS AVG_SALARY FROM EMPLOYEES GROUP BY DEP_ID HAVING count(*) < 4 ORDER BY AVG_SALARY;

#### Built-in Database Functions
#### Aggregate Functions only be used like below if it outputs only one column, for multiple output columns Sub-queries needed 
    select SUM(COST) AS SUM_OF_COST from PETRESCUE;
    select MAX(QUANTITY) from PETRESCUE;
    select AVG(COST) from PETRESCUE;
    select AVG(COST/QUANTITY) from PETRESCUE where ANIMAL = 'Dog';

#### Scalar and String Functions
    select ROUND(COST) from PETRESCUE;
    select DISTINCT(UCASE(ANIMAL)) from PETRESCUE;
    select * from PETRESCUE where LCASE(ANIMAL) = 'cat';

#### Date and Time Functions
    select DAY(RESCUEDATE) from PETRESCUE where ANIMAL = 'Cat';
    select SUM(QUANTITY) from PETRESCUE where MONTH(RESCUEDATE)='05'; 
    select SUM(QUANTITY) from PETRESCUE where DAY(RESCUEDATE)='14';
    select DAYOFWEEK(RescueDate) from PetRescue where Animal = ‘Dog’;
    select (RESCUEDATE + 3 DAYS) from PETRESCUE;
    select (CURRENT DATE - RESCUEDATE) from PETRESCUE;

#### Sub-queries and Nested SELECTs
    select EMP_ID, F_NAME, L_NAME, SALARY from employees where SALARY < (select AVG(SALARY) from employees);
    select EMP_ID, SALARY, ( select MAX(SALARY) from employees ) AS MAX_SALARY from employees;
    select * from ( select EMP_ID, F_NAME, L_NAME, DEP_ID from employees) AS EMP4ALL;

#### Multiple Tables
    select * from employees where JOB_ID IN (select JOB_IDENT from jobs where JOB_TITLE= 'Jr. Designer');
    select JOB_TITLE,MAX_SALARY,JOB_IDENT from jobs where JOB_IDENT IN (select JOB_ID from employees where YEAR(B_DATE)>1976 and SEX='F' );

#### Multiple Tables with Implicit Joins
    select * from employees, jobs;
    select * from employees E, jobs J where E.JOB_ID = J.JOB_IDENT;
    select E.EMP_ID,E.F_NAME,E.L_NAME, J.JOB_TITLE from employees E, jobs J where E.JOB_ID = J.JOB_IDENT;

#### Create a View
    CREATE VIEW EMPSALARY AS SELECT EMP_ID, F_NAME, L_NAME, B_DATE, SEX, SALARY FROM EMPLOYEES;

#### run a view after creating
    SELECT * FROM EMPSALARY;

#### Update a View
    CREATE OR REPLACE VIEW EMPSALARY AS SELECT EMP_ID, F_NAME, L_NAME, B_DATE, SEX, JOB_TITLE, MIN_SALARY, MAX_SALARY
        FROM EMPLOYEES, JOBS WHERE EMPLOYEES.JOB_ID = JOBS.JOB_IDENT;

#### Drop a View
    DROP VIEW EMPSALARY;

#### VIEWS done on IBM DB2 Database
    CREATE VIEW CHICAGO_School_Details(School_Name, Safety_Rating, Family_Rating, Environment_Rating, Instruction_Rating, 
        Leaders_Rating, Teachers_Rating) AS SELECT NAME_OF_SCHOOL, Safety_Icon, Family_Involvement_Icon, Environment_Icon,
        Instruction_Icon, Leaders_Icon, Teachers_Icon FROM CHICAGO_PUBLIC_SCHOOLS;

## Joins

#### INNER JOIN matches results from two tables and returns only rows that match
    select E.F_NAME,E.L_NAME, JH.START_DATE from EMPLOYEES as E INNER JOIN JOB_HISTORY as JH on E.EMP_ID=JH.EMPL_ID 
        where E.DEP_ID ='5';
    select E.F_NAME,E.L_NAME, JH.START_DATE, J.JOB_TITLE from EMPLOYEES as E INNER JOIN JOB_HISTORY as JH on 
        E.EMP_ID=JH.EMPL_ID INNER JOIN JOBS as J on E.JOB_ID=J.JOB_IDENT where E.DEP_ID ='5';

#### LEFT OUTER JOIN matches all rows from the left table & any matching rows from the right table
    select E.EMP_ID,E.L_NAME,E.DEP_ID,D.DEP_NAME from EMPLOYEES AS E 
        LEFT OUTER JOIN DEPARTMENTS AS D ON E.DEP_ID=D.DEPT_ID_DEP where YEAR(E.B_DATE) < 1980;

#### RIGHT OUTER JOIN matches all rows from the right table & any matching rows from the left table
    select E.EMP_ID,E.L_NAME,E.DEP_ID,D.DEP_NAME from EMPLOYEES AS E 
        RIGHT OUTER JOIN DEPARTMENTS AS D ON E.DEP_ID=D.DEPT_ID_DEP AND YEAR(E.B_DATE) < 1980;

#### FULL OUTER JOIN returns all rows from both tables
    select E.F_NAME,E.L_NAME,D.DEPT_ID_DEP, D.DEP_NAME from EMPLOYEES AS E 
        FULL OUTER JOIN DEPARTMENTS AS D ON E.DEP_ID=D.DEPT_ID_DEP AND E.SEX = 'M';

#### SELF JOIN compare rows within the same table
    SELECT column_name(s) FROM table1 T1, table1 T2 WHERE condition;

#### CROSS JOIN is used to generate a paired combination of each row of first table with each row of second table
    SELECT DEPT_ID_DEP, LOCT_ID FROM DEPARTMENTS CROSS JOIN LOCATIONS;

#### Joins done on IBM DB2 Database
    select CCD.CASE_NUMBER, CCD.PRIMARY_TYPE, CD.COMMUNITY_AREA_NUMBER from 
        CHICAGO_CRIME_DATA as CCD INNER JOIN CENSUS_DATA as CD on 
        CCD.COMMUNITY_AREA_NUMBER=CD.COMMUNITY_AREA_NUMBER
        where CD.COMMUNITY_AREA_NUMBER ='18';

    select CCD.CASE_NUMBER, CCD.PRIMARY_TYPE, CCD.LOCATION_DESCRIPTION, CD.COMMUNITY_AREA_NAME from 
        CHICAGO_CRIME_DATA as CCD LEFT OUTER JOIN CENSUS_DATA as CD
        on CCD.COMMUNITY_AREA_NUMBER=CD.COMMUNITY_AREA_NUMBER
        where CCD.LOCATION_DESCRIPTION LIKE '%SCHOOL%';

    select CCD.CASE_NUMBER,CCD.COMMUNITY_AREA_NUMBER, CD.COMMUNITY_AREA_NAME from 
        CHICAGO_CRIME_DATA as CCD FULL OUTER JOIN CENSUS_DATA as CD
        on CCD.COMMUNITY_AREA_NUMBER=CD.COMMUNITY_AREA_NUMBER
        where CD.COMMUNITY_AREA_NAME IN ('Oakland','Armour Square','Edgewater','CHICAGO');

## Stored Procedures

#### Stored Procedures is set of SQL statements that are stored and executed on database server
#### RETRIEVE_ALL routine will contain an SQL query to retrieve all records from PETSALE table
    --#SET TERMINATOR @
    CREATE PROCEDURE RETRIEVE_ALL       -- Name of this stored procedure routine

    LANGUAGE SQL                        -- Language used in this routine 
    READS SQL DATA                      -- This routine will only read data from the table
    DYNAMIC RESULT SETS 1               -- Maximum possible number of result-sets to be returned to caller query
    BEGIN 

        DECLARE C1 CURSOR               -- CURSOR C1 will handle result-set by retrieving records row by row from table
        WITH RETURN FOR                 -- This routine will return retrieved records as a result-set to caller query
        SELECT * FROM PETSALE;          -- Query to retrieve all records from the table
        
        OPEN C1;                        -- Keeping CURSOR C1 open so that result-set can be returned to caller query

    END
    @                                   -- Routine termination character

#### call RETRIEVE_ALL routine
    CALL RETRIEVE_ALL;      -- Caller query

#### drop stored procedure
    DROP PROCEDURE RETRIEVE_ALL;

#### stored procedure to write/modify data in a table
    --#SET TERMINATOR @
    CREATE PROCEDURE UPDATE_SALEPRICE ( 
        IN Animal_ID INTEGER, IN Animal_Health VARCHAR(5) )     -- ( { IN/OUT type } { parameter-name } { data-type }, ... )

    LANGUAGE SQL                                                -- Language used in this routine
    MODIFIES SQL DATA                                           -- This routine will only write/modify data in table

    BEGIN 

        IF Animal_Health = 'BAD' THEN                           -- Start of conditional statement
            UPDATE PETSALE
            SET SALEPRICE = SALEPRICE - (SALEPRICE * 0.25)
            WHERE ID = Animal_ID;
        
        ELSEIF Animal_Health = 'WORSE' THEN
            UPDATE PETSALE
            SET SALEPRICE = SALEPRICE - (SALEPRICE * 0.5)
            WHERE ID = Animal_ID;
            
        ELSE
            UPDATE PETSALE
            SET SALEPRICE = SALEPRICE
            WHERE ID = Animal_ID;

        END IF;                                                 -- End of conditional statement
        
    END
    @                                                           -- Routine termination character

#### calling procedure to write/modify data
    CALL UPDATE_SALEPRICE(1, 'BAD');        -- Caller query

#### SQL statement to update Leaders_Score Column in CHICAGO_PUBLIC_SCHOOLS table for school identified by in_School_ID to value in in_Leader_Score parameter
    --#SET TERMINATOR @
    CREATE PROCEDURE UPDATE_LEADERS_SCORE_TEST(
            IN in_School_ID INTEGER, IN in_Leader_Score INTEGER)       -- Name of this stored procedure routine

    LANGUAGE SQL                        -- Language used in this routine 
    MODIFIES SQL DATA                   -- This routine will only read data from table

    BEGIN 

            UPDATE CHICAGO_PUBLIC_SCHOOLS
            SET LEADERS_SCORE = in_Leader_Score
            WHERE SCHOOL_ID = in_School_ID;
            
    END
    @                                   -- Routine termination character

    CALL UPDATE_LEADERS_SCORE_TEST(610038, 67);

#### ACID (Atomic, Consistent, Isolated and Durable) Transaction
#### COMMIT is used to permanently save the changes done in transactions in a table
#### ROLLBACK is used to undo transactions that have not been saved in a table
#### creating stored procedure routine named TRANSACTION_ROSE including TCL commands like COMMIT and ROLLBACK
    --#SET TERMINATOR @
    CREATE PROCEDURE TRANSACTION_ROSE                     -- Name of this stored procedure routine

    LANGUAGE SQL                                          -- Language used in this routine 
    MODIFIES SQL DATA                                     -- This routine will only write/modify data in table

    BEGIN
            DECLARE SQLCODE INTEGER DEFAULT 0;            -- Host variable SQLCODE declared and assigned 0
            DECLARE retcode INTEGER DEFAULT 0;            -- Local variable retcode with declared and assigned 0
            DECLARE CONTINUE HANDLER FOR SQLEXCEPTION     -- Handler tell the routine what to do when an error or warning occurs
            SET retcode = SQLCODE;                        -- Value of SQLCODE assigned to local variable retcode
            
            UPDATE BankAccounts
            SET Balance = Balance-200
            WHERE AccountName = 'Rose';
            
            UPDATE BankAccounts
            SET Balance = Balance+200
            WHERE AccountName = 'Shoe Shop';
            
            UPDATE ShoeShop
            SET Stock = Stock-1
            WHERE Product = 'Boots';
            
            UPDATE BankAccounts
            SET Balance = Balance-300
            WHERE AccountName = 'Rose';

            IF retcode < 0 THEN                           --  SQLCODE returns negative value for error, zero for success, positive value for warning
                ROLLBACK WORK;
            ELSE
                COMMIT WORK;

            END IF;
    END
    @                                                     -- Routine termination character

#### SQL IF statement to update Leaders_Icon field in CHICAGO_PUBLIC_SCHOOLS table for school identified by in_School_ID
    --#SET TERMINATOR @
    CREATE PROCEDURE UPDATE_LEADERS_SCORE(
            IN in_School_ID INTEGER, IN in_Leader_Score INTEGER)       -- Name of this stored procedure routine

    LANGUAGE SQL                        -- Language used in this routine 
    MODIFIES SQL DATA                   -- This routine will only read data from table

    BEGIN 

        IF in_Leader_Score >= 0 AND in_Leader_Score < 20 THEN                           -- Start of conditional statement
            UPDATE CHICAGO_PUBLIC_SCHOOLS
            SET LEADERS_SCORE = in_Leader_Score, LEADERS_ICON = 'Very weak'
            WHERE SCHOOL_ID = in_School_ID;
        ELSEIF in_Leader_Score >= 20 AND in_Leader_Score < 40 THEN                           
            UPDATE CHICAGO_PUBLIC_SCHOOLS
            SET LEADERS_SCORE = in_Leader_Score, LEADERS_ICON = 'Weak'
            WHERE SCHOOL_ID = in_School_ID;
        ELSEIF in_Leader_Score >= 40 AND in_Leader_Score < 60 THEN                           
            UPDATE CHICAGO_PUBLIC_SCHOOLS
            SET LEADERS_SCORE = in_Leader_Score, LEADERS_ICON = 'Average'
            WHERE SCHOOL_ID = in_School_ID;
        ELSEIF in_Leader_Score >= 60 AND in_Leader_Score < 80 THEN                           
            UPDATE CHICAGO_PUBLIC_SCHOOLS
            SET LEADERS_SCORE = in_Leader_Score, LEADERS_ICON = 'Strong'
            WHERE SCHOOL_ID = in_School_ID;
        ELSEIF in_Leader_Score >= 80 AND in_Leader_Score < 100 THEN                           
            UPDATE CHICAGO_PUBLIC_SCHOOLS
            SET LEADERS_SCORE = in_Leader_Score, LEADERS_ICON = 'Very strong'
            WHERE SCHOOL_ID = in_School_ID;
        ELSE
            ROLLBACK WORK;
        END IF;                                                 -- End of conditional statement
            COMMIT WORK;
    END
    @                                   -- Routine termination character