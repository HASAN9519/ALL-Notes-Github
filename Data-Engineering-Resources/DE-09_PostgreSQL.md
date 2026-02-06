## Commands for WCP (windows command prompt)

#### check version on WCP (windows command prompt)
    psql --version

#### other command on WCP
    psql --help

#### connect database named test in WCP any two ways listed below, command details can be found by command: psql --help
    psql -h localhost -p 5432 -U SHEHA test
    psql -h localhost -p 5432 -U postgres test

#### login to PostgreSQL Database using CMD
    psql -U postgres -W

#### exit psql shell
    \q

## Basic Commands on psql        

#### to get help
    \?

#### to get out of long result
    q 

#### to get all databases, to get info in details use \l+
    \l 
    \l+

#### Listing databases in PostgreSQL using SELECT statement
    SELECT datname FROM pg_database;

#### clear terminal in psql
    \! cls

#### to enable or disable expanded display
    \x

#### create database named test, recommended to use lowercase letters
    CREATE DATABASE test1;

#### connect to database test
    \c test

#### creating database test1, then deleting it for learning delete command
    CREATE DATABASE test1;
    DROP DATABASE test1;

#### creating table person without constraints in test database
    CREATE TABLE person (id INT, first_name VARCHAR(50), last_name VARCHAR(50), date_of_birth DATE);

#### view all relations between tables
    \d

#### view only list of tables not relations
    \dt

#### to view info about table named person
    \d person

#### deleting table person
    DROP TABLE person;

#### creating table person with constraints in test database, BIGSERIAL is a signed integer which auto increments
    CREATE TABLE person (id BIGSERIAL NOT NULL PRIMARY KEY, email VARCHAR(50), first_name VARCHAR(50) NOT NULL, 
        last_name VARCHAR(50) NOT NULL, gender VARCHAR(7) NOT NULL, date_of_birth DATE NOT NULL);

#### insert data into tables, id auto increments because of BIGSERIAL
    INSERT INTO person (first_name, last_name, gender, date_of_birth) VALUES ('Anne','Smith','Female',date '1988-01-09');
    INSERT INTO person (email, first_name, last_name, gender, date_of_birth) VALUES ('jack@gmail.com','Jack','Jones','Male',date '1990-12-31');

#### to view all records
    SELECT * FROM person;

#### Altering TABLE
#### Altering table person by adding column country_of_birth and changing column gender's data type length
    ALTER TABLE person ADD COLUMN country_of_birth VARCHAR(50);
    ALTER TABLE person ALTER COLUMN gender SET DATA TYPE VARCHAR(20);

#### To add a NOT NULL constraint to an existing table by using ALTER TABLE statement, column must not contain any NULL value before applying NOT NULL constraint otherwise it will through a error, So before adding NOT NULL constraint to country_of_birth column, first adding data to empty column
#### country_of_birth of table person using UPDATE statement
    UPDATE person SET country_of_birth='England' WHERE country_of_birth IS NULL;

#### now adding Not NULL constraint to column country_of_birth
    ALTER TABLE person ALTER COLUMN country_of_birth SET NOT NULL;

#### To drop a not null constraint from column country_of_birth if needed
    ALTER TABLE person ALTER COLUMN country_of_birth DROP NOT NULL;

#### generated more data in https://www.mockaroo.com/ and created a sql file named person.sql, then Downloaded it
#### constraints of sql file and column should be same as database table if it is executed in a existing table
#### inserting more sample data on person table by executing person.sql file
    \i C:/Users/SHEHA/Downloads/Database/person.sql

#### Remove all rows from person table using TRUNCATE
    TRUNCATE TABLE person;

    # to reset index value back to start after TRUNCATE, use \d to get SEQUENCE name, syntax of SEQUENCE name is: <table name>_<column name>_seq
    # syntax of ALTER SEQUENCE: ALTER SEQUENCE <table name>_<column name>_seq RESTART WITH <reset id>;
    # resetting SEQUENCE of person_id_seq, after execution of this command, new rows index will start with 1   
    ALTER SEQUENCE person_id_seq RESTART WITH 1;

## ORDER BY

#### ORDER BY ascending and descending order
    SELECT * FROM person ORDER BY country_of_birth;
    SELECT * FROM person ORDER BY country_of_birth ASC;
    SELECT * FROM person ORDER BY country_of_birth DESC;

#### DISTINCT
    SELECT DISTINCT country_of_birth FROM person ORDER BY country_of_birth;

#### WHERE
    SELECT * FROM person WHERE gender = 'Male';

#### AND, OR 
    SELECT * FROM person WHERE gender = 'Male' AND country_of_birth = 'Poland';
    SELECT * FROM person WHERE gender = 'Male' AND (country_of_birth = 'Poland' OR country_of_birth = 'China');
    SELECT * FROM person WHERE gender = 'Male' AND (country_of_birth = 'Poland' OR country_of_birth = 'China') AND first_name='Blake';

#### COMPARISON OPERATORS
    SELECT 1 >= 2;
    SELECT 1 <> 1;

#### LIMIT, OFFSET, FETCH
    SELECT * FROM person LIMIT 5;

#### OFFSET 5 means result will show from row 6
    SELECT * FROM person OFFSET 5 LIMIT 5;  
    SELECT * FROM person FETCH FIRST 10 ROW ONLY;

#### IN
    SELECT * FROM person WHERE country_of_birth IN ('China','Brazil','France');
    SELECT * FROM person WHERE country_of_birth IN ('China','Brazil','France') ORDER BY country_of_birth;

#### BETWEEN
    SELECT * FROM person WHERE date_of_birth BETWEEN DATE '1990-01-01' AND '1995-01-01';

#### LIKE, ILIKE
    SELECT * FROM person WHERE email LIKE '%.com';
    SELECT * FROM person WHERE email LIKE '%@gmail.com';
    SELECT * FROM person WHERE email LIKE '%@google.%';
    SELECT * FROM person WHERE email LIKE '__________@%';    # returns email with ten character
    SELECT * FROM person WHERE country_of_birth ILIKE 'p%';  # ILIKE is case insensitive

#### GROUP BY
    SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth; 
    SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth ORDER By country_of_birth; 

#### HAVING
#### GROUP BY only works with HAVING, HAVING must be used before ORDER BY
    SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth HAVING COUNT(*) > 10 ORDER By country_of_birth; 

#### MIN, MAX, AVG, ROUND, SUM

#### creating table car by executing car.sql file
    \i C:/Users/SHEHA/Downloads/Database/car.sql

    SELECT MAX(price) FROM car;
    SELECT MIN(price) FROM car;
    SELECT AVG(price) FROM car;
    SELECT make, model, MIN(price) FROM car GROUP BY make, model ORDER BY make;
    SELECT make, MAX(price) FROM car GROUP BY make ORDER BY make;
    SELECT make, SUM(price) FROM car GROUP BY make ORDER BY make;

#### Arithmetic Operators
    SELECT 10+2;
    SELECT 10-2;
    SELECT 10*2;
    SELECT 10/2;
    SELECT 10^2;                # ^ means power of
    select 12%5;                # % means Modulus
    SELECT factorial(5);  
    SELECT id, make, model, price AS old_price, ROUND(price * .10,2) AS discount FROM car;
    SELECT id, make, model, price AS old_price, ROUND(price * .10,2) AS discount, ROUND(price - price * .10,2) AS offer_price FROM car;

#### COALESCE, NULLIF
#### COALESCE used to handle NULL values 
    SELECT COALESCE(1);
    SELECT COALESCE(2) AS number;
    SELECT COALESCE(null, 5, 3) AS number;                  # returns 5 as first value is null
    SELECT COALESCE(email,'email not found') FROM person;   # shows 'email not found' in rows where email field in null

#### NULLIF used to handle error
    SELECT NULLIF(10,10);       # returns null as two values are same
    SELECT NULLIF(10,2);        # returns first argument value as they aren't same
    SELECT 10 / NULLIF(10,2);  
    SELECT 10 / NULLIF(2,10); 
    SELECT 10 / NULLIF(0,0);    # don't through a error because of NULLIF
    SELECT COALESCE(10/NULLIF( 0, 0), 0);
    SELECT COALESCE(10/NULLIF( 5, 0), 0);
    SELECT COALESCE(10/NULLIF( 0, 0), 7);

#### NOW, INTERVAL, EXTRACT
    SELECT NOW();                           # returns current timestamp
    SELECT NOW()::DATE;                     # only returns date
    SELECT NOW()::TIME;                     # only returns time
    SELECT NOW() - INTERVAL '1 YEAR';       # subtracts one year from current year
    SELECT NOW() + INTERVAL '1 YEAR';       # adds one year with current year
    SELECT NOW() + INTERVAL '2 MONTH';
    SELECT NOW() + INTERVAL '10 DAY';
    SELECT (NOW() + INTERVAL '10 DAY')::DATE;
    SELECT EXTRACT(YEAR FROM NOW());        # extracting year from current timestamp
    SELECT EXTRACT(MONTH FROM NOW());
    SELECT EXTRACT(DAY FROM NOW());
    SELECT EXTRACT(DOW FROM NOW());         # DOW means day of week, week starts from monday

#### AGE 
#### calculating age in person table using AGE function
    SELECT first_name, last_name, AGE(NOW(), date_of_birth) AS age FROM person;
    SELECT first_name, last_name, AGE(NOW()::DATE, date_of_birth) AS age FROM person;

#### DELETE
    DELETE FROM person WHERE gender='Polygender';
    DELETE FROM person;                     # deletes all records
    DELETE FROM person WHERE gender='Female' AND country_of_birth='Nigeria';

#### to delete all tables from a database, need to drop schema as tables are created in schema, by default tables are created inside public schema
    DROP SCHEMA public CASCADE;

#### after delete creating fresh schema public
    CREATE SCHEMA public;

## PRIMARY KEY, UNIQUE CONSTRAINT, CHECK

#### dropping PRIMARY KEY from person table, name of PRIMARY key can be found using command: \d person
    ALTER TABLE person DROP CONSTRAINT person_pkey;

#### adding PRIMARY KEY to column id, PRIMARY key can be composed of multiple column
    ALTER TABLE person ADD PRIMARY KEY (id);

#### UNIQUE CONSTRAINT ensures that all values in a column are different
#### adding UNIQUE CONSTRAINT named unique_email_address to column email, UNIQUE CONSTRAINT can be composed of multiple column
    ALTER TABLE person ADD CONSTRAINT unique_email_address UNIQUE (email);

#### dropping CONSTRAINT unique_email_address
    ALTER TABLE person DROP CONSTRAINT unique_email_address;

#### adding UNIQUE CONSTRAINT, this time name of UNIQUE CONSTRAINT will be given by postgres
    ALTER TABLE person ADD UNIQUE (email);

#### CHECK constraint on a column means it will allow only certain values for this column
#### adding CHECK constraint to column gender, it will not allow string other than strings defined in parenthesis
    ALTER TABLE person ADD CONSTRAINT gender_constraint CHECK(gender='Male' OR gender='Female' OR gender='Bigender' OR gender='Non-binary');

## UPDATE, SET
    UPDATE person SET email='Blake@gmail.com';                          # updates all rows
    UPDATE person SET email='Blake@gmail.com' WHERE first_name='Blake';
    UPDATE person SET email='Blake@gmail.com' WHERE id='11';
    UPDATE person SET email='Alan@gmail.com', first_name='Alan' WHERE id='14';
    SELECT * FROM person ORDER BY id LIMIT 15;          # use ORDER BY to view updated row in select command as Postgresql row gets hidden after update

#### ON CONFLICT DO NOTHING, ON CONFLICT DO UPDATE
#### ON CONFLICT DO NOTHING simply avoids inserting a row, column passed on as argument must be UNIQUE, NOT NULL, CHECK or PRIMARY KEY constraints
    INSERT INTO person (id,first_name, last_name, gender, date_of_birth,country_of_birth) VALUES
        (1,'Anne','Smith','Female',date '1988-01-09','England')
        ON CONFLICT (id) DO NOTHING;

#### ON CONFLICT DO UPDATE inserts info on a existing row, column passed on as argument must be UNIQUE, NOT NULL, CHECK or PRIMARY KEY constraints
#### here first command updated email into id 2, second command updated first_name and country_of_birth into id 1 
    INSERT INTO person (id, email, first_name, last_name, gender, date_of_birth, country_of_birth) VALUES
        (2,'jones@gmail.com','Jack','Jones','Male','1990-12-31','England') ON CONFLICT
        (id) DO UPDATE SET email = EXCLUDED.email;

    INSERT INTO person (id,first_name, last_name, gender, date_of_birth,country_of_birth) VALUES
        (1,'Alice','Smith','Female',date '1988-01-09','Wales')
        ON CONFLICT (id) DO UPDATE SET first_name = EXCLUDED.first_name, country_of_birth = EXCLUDED.country_of_birth;

## Relations

#### creating multiple table car1 and person1 by executing car1_person1.sql file, there is relationship between car1 and person1 table
    \i C:/Users/SHEHA/Downloads/Database/car1_person1.sql
    SELECT * FROM person1;
    SELECT * FROM car1;
    UPDATE person1 SET car1_id = 2 WHERE id=1;
    UPDATE person1 SET car1_id = 1 WHERE id=2;
    UPDATE person1 SET car1_id = 3 WHERE id=4;

## JOIN, LEFT JOIN, RIGHT JOIN
    SELECT * FROM person1 JOIN car1 ON person1.car1_id=car1.id;
    SELECT person1.first_name, car1.make, car1.model, car1.price FROM person1 JOIN car1 ON person1.car1_id=car1.id;
    SELECT person1.first_name, car1.make, car1.model, car1.price FROM person1 LEFT JOIN car1 ON person1.car1_id=car1.id;
    SELECT person1.first_name, car1.make, car1.model, car1.price FROM person1 LEFT JOIN car1 ON person1.car1_id=car1.id WHERE car1.* IS NULL;
    SELECT person1.first_name, car1.make, car1.model, car1.price FROM person1 RIGHT JOIN car1 ON person1.car1_id=car1.id;

## Deleting Records with Foreign Key

#### as every record of car1 is working as Foreign key in person1 table, any record of car1 can't be deleted directly
#### first have to delete record from person1 which is using record of car1 as Foreign key to that respective record, then delete record from car1
    DELETE FROM person1 WHERE id=4;
    DELETE FROM car1 WHERE id=3;

#### another way to delete record of car1 is to first make referencing column which is in this case car1_id column of table person1 to NULL
#### then can delete record of car1 without deleting record from person1
    UPDATE person1 SET car1_id = NULL WHERE id=1;
    DELETE FROM car1 WHERE id=2;

#### Generate CSV file from postgres
    \COPY (SELECT * FROM person) TO 'C:/Users/SHEHA/Downloads/Database/person.csv' DELIMITER ',' CSV HEADER;

#### Importing Data From CSV into Table using psql
#### In order to copy data, a table must be created with proper table structure same as csv file that is going to be imported 
#### means table column and csv file column should be same to get client_encoding and server_encoding 
    SHOW client_encoding;
    SHOW server_encoding;

#### server_encoding and client_encoding must be same in order to work with data, specify what encoding client should use when connecting to database
    SET client_encoding TO 'UTF8';

#### if Client Encoding is set to WIN1252, need to set HEADER ENCODING 'UTF8' parameter when Importing csv data
#### here my_authors is table in test where data will be imported from csv
    \COPY my_authors FROM 'C:\Users\SHEHA\Downloads\Database\PostgreSQL_Database\CSV\my_authors.csv' DELIMITER ',' CSV HEADER ENCODING 'UTF8';

#### Importing tar file
#### while importing tar file using restore, have to disable trigger
#### A PostgreSQL trigger is a function called automatically whenever an event such as an insert, update or deletion occurs, A PostgreSQL trigger can be defined to fire in cases: Before attempting any operation on a row (before constraints are checked and INSERT, UPDATE or DELETE is attempted) 
#### SERIAL and SEQUENCE
#### SEQUENCE name person_id_seq taken from Default column when running '\d person' command, below command returns last column id
    SELECT * FROM person_id_seq;

#### below command will increase id number without any insert to table person
    SELECT nextval('person_id_seq'::regclass);

#### restart SEQUENCE from any id, restarted SEQUENCE from id 1002 by below command
    ALTER SEQUENCE person_id_seq RESTART WITH 1002;

## postgres extensions

#### list available extensions
    SELECT * FROM pg_available_extensions;

#### UUID (Universally unique identifier) data types is a 128-bit label used for information in computer systems
#### using uuid-ossp extension from postgres extension to Generate UUID
#### first installing extension uuid-ossp
    CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

#### view list of functions
    \df

#### invoking uuid_generate_v4 function
    SELECT uuid_generate_v4();

#### UUID as PRIMARY key
#### creating multiple table car2 and person2 by executing car2_person2.sql file, there is relationship between car2 and person2 table
#### car2 and person2 both tables PRIMARY key is created with uuid
    \i C:/Users/SHEHA/Downloads/Database/car2_person2.sql
    SELECT * FROM person2;
    SELECT * FROM car2;

#### creating relations using uuid
    UPDATE person2 SET car_uid='5e886a1f-bf33-4078-9bcf-5ca8004ff79e' WHERE person_uid='b124db02-d045-420c-99b8-8918ab0dc142';
    UPDATE person2 SET car_uid='c519607e-05da-41e7-9f5e-4df752a5062b' WHERE person_uid='02bb2e77-ee9a-4cba-bbc8-84f945f5b07e';

#### as PRIMARY and Foreign key has same name, join can be done by both following way
    SELECT person2.first_name, car2.make, car2.model, car2.price FROM person2 JOIN car2 ON person2.car_uid=car2.car_uid;
    SELECT person2.first_name, car2.make, car2.model, car2.price FROM person2 JOIN car2 USING (car_uid);

#### Working on PSQL Tool also known as Postgres command line interface (CLI) in pgAdmin4 
#### restoring database from sql file using CLI
#### first go to any existing database from under Databases tab, right click on any existing database tab
#### drop down option will occur after right rig click, click on PSQL Tool and it will open a CLI in right side of pgAdmin4
#### running sql file on CLI to restore database, command syntax: \i <file_name>, file path should be added
    \i C:/Users/SHEHA/Downloads/flights_RUSSIA_small.sql

#### after running flights_RUSSIA_small.sql file, database will create based on sql file, created database name 'demo' will occur under Databases tab
#### \dt command lists all tables in current database schema
#### \dt command will show List of relations such as all schemas and their tables of 'demo' database
    \dt 

## Configure PostgreSQL Server Instance

#### A PostgreSQL server instance has a corresponding file named postgresql.conf, contains configuration parameters for server
#### modifying this file helps to enable, disable or customize settings of PostgreSQL server instance based on needs of database administrator 
#### modify can be done manually on postgresql.conf file and then restart server for changes to take effect
#### modification can be done using CLI directly to edit some configuration parameters 
#### viewing current setting of wal_level parameter, wal_level parameter dictates how much information is written to write-ahead log (WAL)
    SHOW wal_level;

#### wal_level used for continuous archiving, default value is replica which writes enough data to support WAL archiving and replication
#### ALTER SYSTEM command used to modify global defaults of a PostgreSQL instance without manually edit configuration file 
#### changing wal_level parameter replica to logical
    ALTER SYSTEM SET wal_level = 'logical';

#### some configuration parameters such as wal_level require a server restart before any changes take effect
#### go to windows services, find postgresql server and there will be option to stop and start again postgresql server
#### after restarting server, wal_level parameter will show change after running command SHOW wal_level;
#### when ALTER SYSTEM command executes, a new file named postgres.auto.conf creates on directory C:\Program Files\PostgreSQL\14\data
#### This file was automatically modified to contain new parameter set using ALTER SYSTEM command
#### For more advanced instance configuration like many parameter changes are required, using a series of ALTER SYSTEM commands may be cumbersome
#### then Instead of command, have to edit postgresql.conf file directly which is on directly C:\Program Files\PostgreSQL\14\data 

## Navigate System Catalog

#### system catalog stores schema metadata such as information about tables and columns 
#### system catalogs are regular tables for adding columns and insert and update values
#### directly modifying system catalogs is not recommended as it can cause severe problems in system
#### Instead system catalogs are updated automatically when performing other SQL commands like running a CREATE DATABASE command and a new database is created on disk and a new row is automatically inserted into pg_database system catalog table, storing metadata about that database
#### pg_tables is a system catalog containing metadata about each table in the database 
#### querying to display metadata about all tables belonging to bookings schema in demo database
    SELECT * FROM pg_tables WHERE schemaname = 'bookings';

#### When row security is enabled on a table, all normal access to table for selecting or modifying rows must be specified by a row security policy
#### To enable row security on Table boarding_passes by following command and verify change using previous command
#### in output, Table boarding_passes has t for "true" under rowsecurity column which indicates that row security enabled successfully
    ALTER TABLE boarding_passes ENABLE ROW LEVEL SECURITY; 
    SELECT * FROM pg_tables WHERE schemaname = 'bookings';

#### system catalog called pg_settings stores data about configuration parameters of PostgreSQL server
    SELECT name, setting, short_desc FROM pg_settings WHERE name = 'wal_level';

#### changing name of table aircrafts_data to aircraft_fleet by directly editing pg_tables table from system catalogs
    ALTER TABLE aircrafts_data RENAME TO aircraft_fleet;

#### verify name change of table by querying pg_tables from system catalog for schemaname 'bookings' to display tablename column
    SELECT tablename FROM pg_tables WHERE schemaname = 'bookings';

## Modify Database 

#### connect to any database in psql CLI, syntax: \connect <databasename>
    \connect demo

#### to show file location where database is saved 
    show data_directory;

#### viewing data from table aircraft_fleet
    SELECT * FROM aircraft_fleet;

#### viewing data using condition
    SELECT * FROM tickets WHERE book_ref = '0002D8';

#### To update or change an entry in a table
    UPDATE tickets SET passenger_name = 'SANYA KORELEVA' WHERE book_ref = '0002D8';

#### inserting data and verify with previous command
    INSERT INTO aircraft_fleet(aircraft_code, model, range) VALUES (380, '{"en": "Airbus A380-800"}', 15700);

#### exit long query result or view by typing \q, typing won't be visible but as soon as q input given, it will exit current query
    \q 

## Backup and Restore Database using pgAdmin

#### first quit postgresql CLI using \q command
\q  

#### then in pgAdmin GUI, Right click on demo which is under Databases tab and click Backup button, a prompt window will appear
#### in prompt window, Enter a filename for backup like 'demo_backup' on specified folder by clicking folder icon, set Format to Tar then click Backup button
#### if backup folder path isn't specified, backup folder will create on directory C:\Users\SHEHA\Documents by default 

#### Restore a Full Backup
#### first create an empty database to restore database from backup file by right clicking Databases tab then click "Create" > "Database..."
#### a restore prompt window will appear and input Name of database such as 'restored_demo' then click Save button on bottom right of prompt window
#### empty database name will appear under Databases tab and right click on empty database and then click "Restore..." button, a prompt window will appear
#### first set Format to 'Custom or tar' and then Click on button containing three dots in Filename box and filesystem window will appear
#### near bottom right of filesystem window, select "All files" from drop down box besides filename box to view file name of all file types
#### then select backup file 'demo_backup' from specified directory and click on restore button, after that restoration will complete
#### After successful restoration, open CLI and connect with restored_demo database
    \connect restored_demo

#### To set proper search path for database, enter following command, without it no data will show in any query or relations can't be done
    SELECT pg_catalog.set_config('search_path', 'bookings', false);

#### To see restored tables in database
    \dt

## Create New Roles and Grant them Relevant Privileges

#### users, groups and roles are all same entity in PostgreSQL, difference is users can log in by default
#### creating two new roles: read_only and read_write, then grant them relevant privileges
    CREATE ROLE read_only;
    CREATE ROLE read_write;

#### granting privilege to role read_only and read_write to connect to demo database itself
    GRANT CONNECT ON DATABASE demo TO read_only;
    GRANT CONNECT ON DATABASE demo TO read_write;

#### role needs to use schema used in database, granting privilege for read_only and read_write role to use schema bookings
    GRANT USAGE ON SCHEMA bookings TO read_only;
    GRANT USAGE ON SCHEMA bookings TO read_write;

#### To access info in tables in database, SELECT command is used, granting privilege to read_only role to access contents of database but not to edit or alter it, So only SELECT privilege is granted to read_only, allowing read_only role to execute SELECT command on all tables in bookings schema
    GRANT SELECT ON ALL TABLES IN SCHEMA bookings TO read_only;

#### read_write role should have privileges to not only access contents of database but also to create, delete and modify entries
#### granting privilege role read_write to SELECT, INSERT, DELETE and UPDATE
    GRANT SELECT, INSERT, DELETE, UPDATE ON ALL TABLES IN SCHEMA bookings TO read_write;

#### Add a New User and Assign them a Relevant Role
#### Assigning roles created earlier to new user, new user inherits privileges of role given to them
#### creating a new user named user_a, password will be used to access database through this user
    CREATE USER user_a WITH PASSWORD 'user_a_password';

#### Assigning role read_only to user_a  
    GRANT read_only TO user_a;

#### \du command to list all roles and users, resulting system column 'Member of' will have read_only for role user_a
    \du

#### Revoke and Deny Access
#### revoking a user's privilege to access specific tables in a database
#### revoking SELECT privilege on aircrafts_data table in demo database from user_a
    REVOKE SELECT ON aircraft_fleet FROM user_a;

#### revoking user_a to accessing demo database, removing all their SELECT privileges by simply revoking read_only role assigned to them earlier
#### then using \du to check all users and their roles to see that column 'Member of' won't have read_only for role user_a and read_only role was successfully revoked from user_a, user_a is no longer a member of read_only role
    REVOKE read_only FROM user_a;
    \du

## Database monitoring

#### Database monitoring refers to reviewing operational status of database and maintaining its health and performance
#### Tools such as pgAdmin, an open source graphical user interface (GUI) tool for PostgreSQL, come with several features helps to monitor database
#### checking Server Activity
    SELECT pid, usename, datname, state, state_change FROM pg_stat_activity;

#### query will retrieve following:
    pid	            # Process ID
    usename	        # Name of user logged in
    datname	        # Name of database
    state	        # Current state with two common values being: active (executing a query) and idle (waiting for new command)
    state_change	# Time when state was last changed

#### to view recent query, adding query column to previous command
    SELECT pid, usename, datname, state, state_change, query FROM pg_stat_activity;

#### query with pg_stat_activity
    SELECT pid, usename, datname, state, state_change, query FROM pg_stat_activity WHERE state = 'active';

#### checking Database Activity
#### in datname column in output table, template1 and template0 are default templates for databases
    SELECT datname, tup_inserted, tup_updated, tup_deleted FROM pg_stat_database;

#### query will retrieve following:
    datname	            # Name of database
    tup_inserted	    # Number of rows inserted by queries in database
    tup_updated	        # Number of rows updated by queries in database
    tup_deleted	        # Number of rows deleted by queries in database

#### query with pg_stat_database
#### tup_fetched is number of rows that were fetched by queries, tup_returned is number of rows that were returned by queries
    SELECT datname, tup_fetched, tup_returned FROM pg_stat_database;
    SELECT datname, tup_inserted, tup_updated, tup_deleted, tup_fetched, tup_returned FROM pg_stat_database WHERE datname = 'demo';

## Monitor Performance Over Time using psql CLI

#### Extensions enhances PostgreSQL experience and helpful in monitoring database, pg_stat_statements extension gives aggregated view of query statistics
#### command to enable pg_stat_statements extension, after enable pg_stat_statements will start to track statistics for database
    CREATE EXTENSION pg_stat_statements;

#### editing PostgreSQL configuration file to include this extension, then restart server for changes to take effect
    ALTER SYSTEM SET shared_preload_libraries = 'pg_stat_statements';

#### check if extension has been loaded by checking installed extensions 
    \dx

#### check if extension has been loaded by shared_preload_libraries
    show shared_preload_libraries;

#### using \x to turn on expanded table formatting as results returned by pg_stat_statements can be quite long, turn it off by repeating \x command again
    \x

#### retrieving database ID, query and total time took to execute statement (in milliseconds)
    SELECT dbid, query, total_exec_time FROM pg_stat_statements;

#### query to retrieve database name with database ID
    SELECT oid, datname FROM pg_database;

#### adding these extensions can increase server load which may affect performance, dropping extension and check current extensions with \dx
    DROP EXTENSION pg_stat_statements;

#### resetting shared_preload_libraries in configuration file, then restart server for changes to take effect
    ALTER SYSTEM RESET shared_preload_libraries;

## Monitor with pgAdmin

#### On home page, under Dashboard, user have access to server or database statistics
#### listing all displayed statistics on Dashboard that correspond with statistics accessed with psql CLI
#### Server/Database sessions Displays total sessions that are running, for servers this is similar to pg_stat_activity and for databases this is similar to pg_stat_database
#### Transactions per second Displays commits, rollbacks and transactions taking place
#### Tuples in Displays number of tuples (rows) that have been inserted, updated and deleted similar to tup_inserted, tup_updated and tup_deleted columns from pg_stat_database
#### Tuples out Displays number of tuples (rows) that have been fetched (returned as output) or returned (read or scanned), similar to tup_fetched and tup_returned from pg_stat_database
#### Server/database activity Displays sessions, locks, prepared transactions and configuration for server/database, Sessions tab offers a look at breakdown of sessions that are currently active on server similar to view provided by pg_stat_activity, select refresh button at top-right corner To check for any new processes

## Optimizing Database

#### Data optimization is maximization of speed and efficiency of retrieving data from database, Optimizing database will improve its performance for inserting or retrieving data from database, Similar to MySQL there are optimal data types and maintenance (known as "vacuuming") that can be applied to optimize databases
#### checking current data types (and additional details such as indexes and constraints) of aircrafts_data table
    \d aircraft_fleet;

#### to change a column's data type, first need to check if column is part of any view, if it is part of any view, that view needed to delete otherwise Database won't let to change column's data type, since demo database loaded from a SQL file that included commands to create views using columns
#### changing range column's data type in aircraft_fleet table after deleting VIEW aircrafts as it contained range column and check using \d aircraft_fleet;
    DROP VIEW aircrafts;
    ALTER TABLE aircraft_fleet ALTER COLUMN range TYPE smallint;
    \d aircraft_fleet;

#### In PostgreSQL, vacuuming means to clean out databases by reclaiming any storage from "dead tuples" otherwise known as rows that have been deleted but have not been cleaned out, Generally autovacuum feature is automatically enabled means PostgreSQL will automate vacuum maintenance process for user
# checking if autovacuum is enabled or not, if result is on then it is enabled 
    show autovacuum;

#### pg_stat_user_tables displays statistics about each table that is a user table (instead of a system table) in database
#### checking table name, number of dead rows, last time it was auto vacuumed and number of times table has been auto vacuumed by following query
    SELECT relname, n_dead_tup, last_autoanalyze, autovacuum_count FROM pg_stat_user_tables;

## Troubleshooting in PostgreSQL

#### to check if Server Logging is enabled or not, if result is on then it is enabled
    SHOW logging_collector;

#### inspecting server logs, logs can be a valuable tool when troubleshooting issues as a database administrator
#### To find system logs are stored, Every time starting server again creates a new .log file in log folder in path C:\Program Files\PostgreSQL\14\data\log
    SHOW log_directory;

#### Test Performance of PostgreSQL Server
#### enable timer to inspect how long each query or command takes and then run a select query to check query execution time
#### turn off timer by repeating \timing command again
    \timing
    SELECT * FROM aircraft_fleet;

#### some parameters in postgresql.conf file:
#### max_connections parameter sets maximum number of connections that can be made to server at any given time
#### shared_buffers configuration parameter sets amount of memory database server has at its disposal
#### work_mem parameter increases server performance
#### maintenance_work_mem parameter specifies maximum amount of memory used by maintenance operations such as VACUUM, CREATE INDEX and ALTER TABLE