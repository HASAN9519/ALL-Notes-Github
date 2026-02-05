## Commands for WCP (windows command prompt)

#### check version on WCP (windows command prompt)
    mysql --version

#### other command on WCP
    mysql --help

#### connect mysql database using root user
    mysql -u root -p

#### exit mysql shell
    \q

#### Connecting to mysql using mysql shell
    \sql
    \connect root@localhost

## Connecting to mysql using mysql command line client

#### just need to give password, after giving password connects to mysql database

## Basic Commands on mysql

#### to get all databases
    SHOW DATABASES;

#### show all schemas
    SHOW SCHEMAS;
    SHOW DATABASES LIKE '%schema';

#### create database students
    CREATE DATABASE students;

#### to use or select students database to work
    USE students;

#### create table student in students database, primary key can be defined in two ways
    CREATE TABLE student(student_id INT PRIMARY KEY, name VARCHAR(20) NOT NULL UNIQUE, major VARCHAR(20));
    CREATE TABLE student(student_id INT, name VARCHAR(20) NOT NULL UNIQUE, major VARCHAR(20), PRIMARY KEY(student_id));

#### create table student1 using default value for column major
    CREATE TABLE student1(student_id INT, name VARCHAR(20) NOT NULL UNIQUE, major VARCHAR(20) DEFAULT 'Not given', PRIMARY KEY(student_id));
    DESCRIBE student1;
    INSERT INTO student1(student_id, name) VALUES (1, 'Ryan');
    INSERT INTO student1(student_id, name, major) VALUES (2, 'Rock', 'CSE');
    SELECT * FROM student1;

#### create table student2 using AUTO_INCREMENT to primary key
    CREATE TABLE student2(student_id INT AUTO_INCREMENT, name VARCHAR(20) NOT NULL UNIQUE, major VARCHAR(20) DEFAULT 'Not given', PRIMARY KEY(student_id));
    DESCRIBE student2;
    INSERT INTO student2(name) VALUES ('Ryan');
    INSERT INTO student2(name, major) VALUES ('Rock', 'CSE');
    SELECT * FROM student2;

#### describe table student
    DESCRIBE student;

#### to get all tables
    SHOW TABLES;

#### adding new column 
    ALTER TABLE student ADD COLUMN gpa DECIMAL(3,2);

#### insert data to student table in two ways
    INSERT INTO student(student_id, name, major, gpa) VALUES (1, 'Ryan', 'CSE', 3.88);
    INSERT INTO student VALUES (2,'Ramesh', 'EEE', 2.56);
    INSERT INTO student(student_id, name, major) VALUES (3, 'karim', 'CSE');
    INSERT INTO student(student_id,name,major,gpa) VALUES (4,'Rahim','CSE',3.50),(5,'Tazim','EEE',3.67),(6,'Wazim','BBA',3.32);

#### retrieve data from student table
    SELECT * FROM student;

#### drop table student
    DROP TABLE student;

## UPDATE and DELETE
    UPDATE student SET major = 'Biology' WHERE major='CSE';
    UPDATE student SET major = 'Pharmacy' WHERE student_id=3;
    DELETE FROM student WHERE student_id=2;

## ORDER BY, LIMIT, AND, OR, IN
    SELECT * FROM student ORDER BY name;
    SELECT student.name, student.major FROM student ORDER BY student_id DESC;
    SELECT student.name, student.major FROM student ORDER BY student_id ASC;
    SELECT student.name, student.major FROM student ORDER BY student_id ASC LIMIT 2;
    SELECT student.name, student.major FROM student WHERE student_id >= 3;
    SELECT student.name, student.major FROM student WHERE student_id >= 3 AND major <> 'CSE';
    SELECT student.name, student.major FROM student WHERE major = 'CSE' OR major = 'EEE';
    SELECT student.name, student.major FROM student WHERE major IN ('CSE','EEE','BBA');
    SELECT student.name, student.major FROM student WHERE major IN ('CSE','EEE','BBA') AND student_id > 4;

## Designing schema

#### creating company database or schema
    CREATE DATABASE employee_db;
    USE employee_db;

#### creating tables in employee_db database
#### recommended to use ON DELETE SET NULL for Foreign key if it is not used as primary key on same table
#### recommended to use ON DELETE CASCADE for Foreign key if it is used as primary key on same table
    CREATE TABLE employee (emp_id INT PRIMARY KEY, first_name VARCHAR(40), last_name VARCHAR(40), birth_day DATE, sex VARCHAR(1), salary INT, super_id INT, branch_id INT);

#### ON DELETE SET NULL means if emp_id of employee table gets deleted, referencing mgr_id of that emp_id in branch table will be set to null
    CREATE TABLE branch (
        branch_id INT PRIMARY KEY, branch_name VARCHAR(40), mgr_id INT, mgr_start_date DATE, 
        FOREIGN KEY(mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
    );

#### after creating table employee and branch altering table to set Foreign key, 2nd command to create Foreign key into same table
    ALTER TABLE employee ADD FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL;
    ALTER TABLE employee ADD FOREIGN KEY(super_id) REFERENCES employee(emp_id) ON DELETE SET NULL;

    CREATE TABLE client (
        client_id INT PRIMARY KEY, client_name VARCHAR(40), branch_id INT,
        FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL
    );

#### A composite key is a combination of two or more columns in a table that can be used to uniquely identify each row in table when columns are combined
#### A primary key that is made by combination of more than one attribute is known as a composite key
#### here (emp_id, client_id) and (branch_id, supplier_name) are composite key 

#### ON DELETE CASCADE means it will delete entire row, here delete by client_id or emp_id of works_with table will delete entire row  
    CREATE TABLE works_with (
        emp_id INT, client_id INT, total_sales INT, PRIMARY KEY(emp_id, client_id),
        FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
        FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
    );

    CREATE TABLE branch_supplier (
        branch_id INT, supplier_name VARCHAR(40), supply_type VARCHAR(40), PRIMARY KEY(branch_id, supplier_name),
        FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE
    );

#### altering table to set Foreign key, 2nd command to create Foreign key into same table
    ALTER TABLE employee ADD FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL;
    ALTER TABLE employee ADD FOREIGN KEY(super_id) REFERENCES employee(emp_id) ON DELETE SET NULL;

    SHOW TABLES;
    DESCRIBE employee;
    DESCRIBE branch;
    DESCRIBE client;
    DESCRIBE branch_supplier;
    DESCRIBE works_with;

#### inserting data into tables, first have to insert data on employee table then branch table as their are relations between them
    INSERT INTO employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL);
    INSERT INTO branch VALUES(1, 'Corporate', 100, '2006-02-09');

#### first update branch_id of employee according to already existed emp_id of employee table as they are related 
    UPDATE employee SET branch_id = 1 WHERE emp_id = 100;
    INSERT INTO employee VALUES(101, 'Jan', 'Levinson', '1961-05-11', 'F', 110000, 100, 1);
    INSERT INTO employee VALUES(102, 'Michael', 'Scott', '1964-03-15', 'M', 75000, 100, NULL);

    INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-04-06');
    UPDATE employee SET branch_id = 2 WHERE emp_id = 102;

    INSERT INTO employee VALUES(103, 'Angela', 'Martin', '1971-06-25', 'F', 63000, 102, 2);
    INSERT INTO employee VALUES(104, 'Kelly', 'Kapoor', '1980-02-05', 'F', 55000, 102, 2);
    INSERT INTO employee VALUES(105, 'Stanley', 'Hudson', '1958-02-19', 'M', 69000, 102, 2);

    INSERT INTO employee VALUES(106, 'Josh', 'Porter', '1969-09-05', 'M', 78000, 100, NULL);
    INSERT INTO branch VALUES(3, 'Stamford', 106, '1998-02-13');
    UPDATE employee SET branch_id = 3 WHERE emp_id = 106;

    INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', 65000, 106, 3);
    INSERT INTO employee VALUES(108, 'Jim', 'Halpert', '1978-10-01', 'M', 71000, 106, 3);
    INSERT INTO branch VALUES(4, "Buffalo", NULL, NULL);

#### inserting into BRANCH SUPPLIER
    INSERT INTO branch_supplier VALUES(2, 'Hammer Mill', 'Paper');
    INSERT INTO branch_supplier VALUES(2, 'Uni-ball', 'Writing Utensils');
    INSERT INTO branch_supplier VALUES(3, 'Patriot Paper', 'Paper');
    INSERT INTO branch_supplier VALUES(2, 'J.T. Forms & Labels', 'Custom Forms');
    INSERT INTO branch_supplier VALUES(3, 'Uni-ball', 'Writing Utensils');
    INSERT INTO branch_supplier VALUES(3, 'Hammer Mill', 'Paper');
    INSERT INTO branch_supplier VALUES(3, 'Stamford Lables', 'Custom Forms');

#### inserting into CLIENT
    INSERT INTO client VALUES(400, 'Dunmore Highschool', 2);
    INSERT INTO client VALUES(401, 'Lackawana Country', 2);
    INSERT INTO client VALUES(402, 'FedEx', 3);
    INSERT INTO client VALUES(403, 'John Daly Law, LLC', 3);
    INSERT INTO client VALUES(404, 'Scranton Whitepages', 2);
    INSERT INTO client VALUES(405, 'Times Newspaper', 3);
    INSERT INTO client VALUES(406, 'FedEx', 2);

#### inserting into WORKS_WITH
    INSERT INTO works_with VALUES(105, 400, 55000);
    INSERT INTO works_with VALUES(102, 401, 267000);
    INSERT INTO works_with VALUES(108, 402, 22500);
    INSERT INTO works_with VALUES(107, 403, 5000);
    INSERT INTO works_with VALUES(108, 403, 12000);
    INSERT INTO works_with VALUES(105, 404, 33000);
    INSERT INTO works_with VALUES(107, 405, 26000);
    INSERT INTO works_with VALUES(102, 406, 15000);
    INSERT INTO works_with VALUES(105, 406, 130000);

    SELECT * FROM employee;
    SELECT * FROM branch;
    SELECT * FROM client;
    SELECT * FROM works_with;
    SELECT * FROM branch_supplier;
    SELECT * FROM employee ORDER BY salary DESC;
    SELECT * FROM employee ORDER BY sex, first_name;
    SELECT DISTINCT sex FROM employee;
    SELECT emp_id, first_name, last_name FROM employee WHERE birth_day >= '1970-01-01';
    SELECT * FROM employee WHERE (birth_day >= '1970-01-01' AND sex = 'F') OR salary > 80000;
    SELECT * FROM employee WHERE birth_day BETWEEN '1970-01-01' AND '1975-01-01';
    SELECT * FROM employee WHERE first_name IN ('Jim', 'Michael', 'Johnny', 'David');
    SELECT COUNT(super_id) FROM employee;
    SELECT AVG(salary) FROM employee;
    SELECT COUNT(sex), sex FROM employee GROUP BY sex;

#### Finding total sales of each salesman
    SELECT SUM(total_sales), emp_id FROM works_with GROUP BY client_id;

#### Finding total amount of money spent by each client
    SELECT SUM(total_sales), client_id FROM works_with GROUP BY client_id;

    SELECT * FROM client WHERE client_name LIKE '%LLC';
    SELECT * FROM branch_supplier WHERE supplier_name LIKE '%Label%';
    SELECT * FROM employee WHERE birth_day LIKE '_____10%';
    SELECT * FROM client WHERE client_name LIKE '%Highschool%';

#### using UNION, need to have same number of columns and same data types for tables used in UNION
    SELECT employee.first_name AS Employee_Branch_Names FROM employee UNION SELECT branch.branch_name FROM branch;
    SELECT client.client_name AS Non_Employee_Entities, client.branch_id AS Branch_ID FROM client UNION
    SELECT branch_supplier.supplier_name, branch_supplier.branch_id FROM branch_supplier;

    SELECT emp.emp_id, emp.first_name, branch.branch_name FROM employee AS emp JOIN branch ON emp.emp_id = branch.mgr_id;

## Nested Queries

#### Finding names of all employees who have sold over 50,000
    SELECT emp.first_name, emp.last_name FROM employee AS emp WHERE emp.emp_id IN (SELECT works_with.emp_id FROM works_with WHERE works_with.total_sales > 50000);

#### Finding all clients who are handles by branch that Michael Scott manages, here Michael's ID is 102
    SELECT cl.client_id, cl.client_name FROM client AS cl WHERE cl.branch_id = (SELECT branch.branch_id FROM branch WHERE branch.mgr_id = 102);

#### Finding all clients who are handles by branch that Michael Scott manages
    SELECT cl.client_id, cl.client_name FROM client AS cl WHERE cl.branch_id = (SELECT branch.branch_id FROM branch WHERE branch.mgr_id = (SELECT emp.emp_id FROM employee AS emp WHERE emp.first_name = 'Michael' AND emp.last_name ='Scott' LIMIT 1));

#### Finding names of employees who work with clients handled by scranton branch
    SELECT emp.first_name, emp.last_name FROM employee AS emp WHERE emp.emp_id IN (SELECT works_with.emp_id FROM works_with) AND emp.branch_id = 2;

#### Finding names of all clients who have spent more than 100,000 dollars
#### Having clause works with a group by clause on aggregate function condition, WHERE condition don't work with group by clause on aggregate function
    SELECT cl.client_id, cl.client_name FROM client AS cl WHERE cl.client_id IN (SELECT client_id FROM works_with GROUP BY client_id HAVING SUM(total_sales) > 100000);

    SELECT client_id, SUM(total_sales) AS ts FROM works_with GROUP BY client_id HAVING ts > 100000;
    SELECT client_id, SUM(total_sales) AS ts FROM works_with WHERE client_id > 403 GROUP BY client_id ORDER BY client_id DESC;

#### delete operation, first inserting them then deleting them
    INSERT INTO employee VALUES(109, 'Tazim', 'Khan', '1971-05-15', 'F', 70000, 106, 2);
    INSERT INTO employee VALUES(110, 'Wazim', 'Shekh', '1965-07-11', 'M', 80000, 109, 4);
    INSERT INTO branch VALUES(5, 'los angeles', 109, '1996-09-06');
    DELETE FROM employee WHERE emp_id=109;

    INSERT INTO branch_supplier VALUES(4, 'Ak furniture', 'furniture');
    DELETE FROM branch_supplier WHERE branch_id=4;

## TRIGGER

- A trigger is a special type of stored procedure that automatically runs when an event occurs in database server, DML triggers run when a user tries to modify data through a data manipulation language (DML) event, DML events are INSERT, UPDATE or DELETE statements on a table or view DELIMITER used to end a command, default DELIMITER of sql is ; (semicolon), DELIMITER can be changed for certain condition.

#### syntax of trigger
    CREATE
        TRIGGER 'event_name' BEFORE/AFTER INSERT/UPDATE/DELETE
        ON 'database'.'table'
        FOR EACH ROW BEGIN
            trigger body
            this code is applied to every
            inserted/updated/deleted row
        END;

#### first create count_record table to store values generated by trigger 
    CREATE TABLE count_record (message VARCHAR(100));

#### creating trigger named test_trigger, have to change DELIMITER to $$ in order to write condition in trigger, after working on trigger DELIMITER must be back to default DELIMITER which is ; after each record inserted in employee table test_trigger will add string to count_record table
    DELIMITER $$
    CREATE
        TRIGGER test_trigger BEFORE INSERT
        ON employee_db.employee
        FOR EACH ROW BEGIN
            INSERT INTO count_record VALUES('added new employee');
        END$$
    DELIMITER ;

#### now inserting new record into employee table, then checking count_record table if any changes happened or not
    INSERT INTO employee VALUES(111, 'Oscar', 'Martinez', '1968-02-19', 'M', 69000, 107, 3);
    SELECT * FROM count_record;

#### creating trigger named test_trigger2, after each record inserted in employee table test_trigger2 will add first_name to count_record table
#### NEW.first_name means first_name of newly added record
    DELIMITER $$
    CREATE
        TRIGGER test_trigger2 BEFORE INSERT
        ON employee_db.employee
        FOR EACH ROW BEGIN
            INSERT INTO count_record VALUES(NEW.first_name);
        END$$
    DELIMITER ;

    INSERT INTO employee VALUES(112, 'Kevin', 'Malone', '1978-02-19', 'M', 69000, 107, 3);
    SELECT * FROM count_record;

#### creating trigger named test_trigger3 using IF ELSE condition
    DELIMITER $$
    CREATE
        TRIGGER test_trigger3 BEFORE INSERT
        ON employee_db.employee
        FOR EACH ROW BEGIN
            IF NEW.sex = 'M' THEN
                    INSERT INTO count_record VALUES('added male employee');
            ELSEIF NEW.sex = 'F' THEN
                    INSERT INTO count_record VALUES('added female');
            ELSE
                    INSERT INTO count_record VALUES('added other employee');
            END IF;
        END$$
    DELIMITER ;

    INSERT INTO employee VALUES(113, 'Pam', 'Beesly', '1988-02-19', 'F', 69000, 106, 3);
    SELECT * FROM count_record;

#### to show triggers and their info, also dropping trigger 
    SHOW triggers;
    SELECT trigger_schema, trigger_name, action_statement FROM information_schema.triggers;
    SELECT * FROM information_schema.triggers where information_schema.triggers.trigger_schema like '%employee_db%';
    SELECT trigger_name FROM information_schema.triggers where information_schema.triggers.trigger_schema like '%employee_db%';
    DROP TRIGGER test_trigger;

## ER Diagram

- Composite attribute: An attribute that is a combination of other attributes is known as composite attribute
- A multi valued attribute may have one or more values for a particular entity, For example Location as attribute of an entity called ENTERPRISE is multi valued because each enterprise can have one or more locations
- A derived attribute is an attribute whose values are calculated from other attributes, if there are attribute named as date_of_birth and age in a student table, can derive value of age with help of date_of_birth attribute
- Attribute relationships are associations between attributes that specify how attributes are connected, Attribute relationships define how tables and columns are joined and used and which tables are related to other tables

## Logical Backup

- A logical backup creates a file containing DDL and DML commands that recreate objects and data in database
- this backup file used to recreate database on same or on another system, performing a logical backup and restore can reclaim any wasted space from original database as restoration process creates a clean version of tables, Logical backups enables to backup granular objects, Granular data is detailed data or lowest level that data can be in a target set, refers to size that data fields are divided into, how detail-oriented a single field is
- Granularity is relative size, scale, level of detail or depth of penetration that characterizes an object or activity in logical backup, backup an individual database table is possible but can't use it to backup log files or database configuration settings 
- if one or more tables of a database accidentally drop or deleted, logical backup of a database table used to restore structure and data of table
- A physical or raw backup creates a copy of all physical storage files and directories that belong to a table, database or other object including data files, configuration files and log files, Physical backups are often smaller and quicker than logical backups, useful for large or important databases that require fast recovery times

## Backup and Restore operation using system terminal (not mysql CLI)

#### backup using mysqldump utility
#### –u parameter specifies the username of root 
#### employees is name of database and employees_backup.sql is name of file in which to create backup
#### greater than sign signifies output to the .sql file or backup
    mysqldump -u root employees > employees_backup.sql

#### backup country_language table of world database using system terminal, after password first give database name then table name
    mysqldump --host=127.0.0.1 --port=3306 --user=root --password world country_language > world_country_language_mysql_backup.sql

#### To view contents of backup file within system terminal
    cat world_country_language_mysql_backup.sql

#### backup of all databases using mysqldump, have to enter password when running command
    mysqldump --all-databases --user=root --password > backup-file.sql

#### backup of all databases using mysqldump with password, don't need to enter password when running command
    mysqldump --all-databases --user=root --password=MTQxOTEtc2hlaGFi > backup-file.sql

#### restore using system terminal
#### less than sign signifies input to the database or restore
    mysql -u root restored_employees < employees_backup.sql

#### restoring structure and data of table country_language using system terminal
    mysql --host=127.0.0.1 --port=3306 --user=root --password world < world_country_language_mysql_backup.sql

#### querying world database using system terminal
    mysql --host=127.0.0.1 --port=3306 --user=root --password --execute="SELECT * FROM world.country_language WHERE country_code='CAN';"

#### Retrieving column names of city table, first query gives detail information
    mysql --host=127.0.0.1 --port=3306 --user=root --password --execute="SELECT * FROM INFORMATION_SCHEMA.COLUMNS where TABLE_NAME='city';"
    mysql --host=127.0.0.1 --port=3306 --user=root --password --execute="SHOW COLUMNS FROM world.city;"

#### list all table names from world database using system terminal
    mysql --host=127.0.0.1 --port=3306 --user=root --password --execute="SHOW TABLES FROM world;"

#### dropping table using system terminal
    mysql --host=127.0.0.1 --port=3306 --user=root --password --execute="DROP TABLE world.country_language;"

## Perform Point-in-Time Backup and Restore

- Using point-in-time backup and restore, can get each and every change that occurred since last full logical backup of whole database system, Backup keeps a record of all new transactions even after last logical backup, Point-in-time backup is set of binary log files generated subsequent to a logical backup operation of a database, binary log files contain events that describe database changes such as table creation operations or changes to table data, to Restore a database to a point-in-time, need to use binary log files containing changes of a database for a time interval along with last logical backup of the database

#### First creating a full logical backup of current state of whole world database
#### two parameters in command below, --flush-logs (starts writing to a new binlog file) and --delete-master-logs (removes old binlog files) ensures that there will be only binary log files created after a full logical backup
    mysqldump --host=127.0.0.1 --port=3306 --user=root --password --flush-logs --delete-master-logs  --databases world > world_mysql_full_backup.sql

#### for test purpose, creating a scenario where a database crash conducted intentionally resulting a significant loss of world database files
#### last command will give error as database lost significant data
    docker exec mysql-mysql-1 rm -rf /var/lib/mysql/world
    docker exec -it mysql-mysql-1 mysqladmin -p shutdown
    mysql --host=127.0.0.1 --port=3306 --user=root --password --execute="SELECT * FROM world.city;"

#### restoring world database along with updates made earlier using point-in-time restore
#### Displaying binary logs
    mysql --host=127.0.0.1 --port=3306 --user=root --password --execute="SHOW BINARY LOGS;"

#### contents of all binary log files from previous command listed to a single file
    docker exec mysql-mysql-1 mysqlbinlog /var/lib/mysql/binlog.000003 /var/lib/mysql/binlog.000004 > log_file.sql

#### First restoring full logical backup of whole world database
    mysql --host=127.0.0.1 --port=3306 --user=root --password < world_mysql_full_backup.sql

#### Retrieving all Canada (country_code='CAN') related records from city table which will show no record
    mysql --host=127.0.0.1 --port=3306 --user=root --password --execute="SELECT * FROM world.city WHERE country_code='CAN';"

#### running log_file and run previous command again, this time record will show as world database in same state before intentional crash scenario
    mysql --host=127.0.0.1 --port=3306 --user=root --password < log_file.sql

#### Perform Physical Backup and Restore
#### To perform physical backup, need to take a storage snapshot of MySQL server data directory within docker container
#### backup file will be created on specified directory
    docker cp mysql-mysql-1:/var/lib/mysql /home/project/mysql_backup

#### instead of taking snapshot of whole MySQL server data directory containing several databases, takeing snapshot of world database for physical backup
    docker cp mysql-mysql-1:/var/lib/mysql/world /home/project/mysql_world_backup

#### for test purpose, Remove world database directory from mysql server docker container
    docker exec mysql-mysql-1 rm -rf /var/lib/mysql/world

#### restarting mysql server necessary after making changes to mysql server data directory
    docker exec -it mysql-mysql-1 mysqladmin -p shutdown

#### restoring physical backup and again restarting mysql server to make changes effect
    docker cp /home/project/mysql_backup/. mysql-mysql-1:/var/lib/mysql/world

## working with mysql CLI

#### Execute sql script to restore database using mysql CLI
    source employees_backup.sql

#### importing using load data infile statement
#### importing contents of a CSV file into an existing MySQL table
    load data infile 'employees_data.csv' into table employees_details

#### importing using mysqlimport utility
#### file name should match table name exactly as table name is inferred from name of CSV file 
#### here employees is the database name where residing table name should be employees_details 
    mysqlimport employees employees_details.csv

#### Storage engines available in MySQL: InnoDB, MyISAM, MEMORY, MERGE, EXAMPLE, ARCHIVE, CSV, BLACKHOLE, FEDERATED
#### SHOW ENGINES
    SHOW ENGINES;

#### CREATE TABLE using storage engine specified in ENGINE clause
#### If ENGINE clause not specified, CREATE TABLE statement creates table with default storage engine InnoDB
    CREATE TABLE Products (i INT) ENGINE = INNODB;
    CREATE TABLE Product_Codes (i INT) ENGINE = CSV;
    CREATE TABLE History (i INT) ENGINE = MEMORY;

#### For non-standard storage needs, specify a different default storage engine using set command
#### Set default storage engine for current session by setting default_storage_engine variable for creating a database to store archived data
#### To set default storage engine for all sessions, set default-storage-engine option in my.cnf configuration file
    SET default_storage_engine=ARCHIVE;

#### ALTER TABLE convert a table from one storage engine to another
#### following statement set storage engine for Products table to Memory
    ALTER TABLE Products ENGINE = MEMORY;

#### show table status of table bill_data
    SHOW TABLE STATUS WHERE Name = 'bill_data';

#### getting database info from database billing
    SELECT TABLE_NAME, ENGINE FROM information_schema.TABLES WHERE TABLE_SCHEMA = 'billing';

#### create database world
    CREATE DATABASE world;

#### use newly created world database
    USE world;

#### Execute world_mysql.sql script to complete world database creation process
    SOURCE world_mysql_script.sql;

#### showing tables
    SHOW TABLES;

#### list all table names from current session database
    SHOW FULL TABLES WHERE table_type = 'BASE TABLE';

#### To show Percentage column completely illegible means not clear to read list of Storage Engines supported on MySQL server
    SHOW ENGINES;

#### To create a new table with a storage engine other than default InnoDB database and verify using cmd SHOW TABLES;
    CREATE TABLE csv_test (i INT NOT NULL, c CHAR(10) NOT NULL) ENGINE = CSV;

#### insert data to table
    INSERT INTO csv_test VALUES(1,'data one'),(2,'data two'),(2,'data three');

#### Retrieving data from table
    SELECT * FROM csv_test;

#### drop or delete database
    DROP DATABASE world;

#### quitting mysql CLI
    \q

#### MySQL server contains a system database called mysql contains info required for server to run such as meta data on all other tables in database 
#### mysql uses MyISAM storage engine instead of default InnoDB storage engine
    SHOW DATABASES;
    USE mysql;
    SHOW TABLES;

#### user table contains user accounts, global privileges and other non privilege columns
    SELECT User from user;

#### add a new user to database and verify using SELECT User from user;
    CREATE USER test_user;

#### INFORMATION_SCHEMA is a database found inside every MySQL server contains meta data about MySQL server such as name of a database or table, data type of a column or access privileges, this database contains read-only tables so can't directly use any INSERT, UPDATE or DELETE commands 
    USE information_schema;

#### in information_schema database, COLUMNS table contains meta data about columns for all tables and views in server
#### One of columns in this table contains names of all other columns in every table, getting names of columns in country table in world database
    SELECT COLUMN_NAME FROM COLUMNS WHERE TABLE_NAME = 'country';

#### in information_schema database, TABLES table contains meta data about all tables in server
#### One of columns in this table contains information about a table's storage engine type
#### view storage engine type for 'country','city','country_language' and 'csv_test' table
    SELECT table_name, engine FROM INFORMATION_SCHEMA.TABLES WHERE table_name = 'country' OR table_name = 'city' 
        OR table_name = 'country_language' OR table_name = 'csv_test';

#### TABLES table contains info on size of a given table in bytes, info is stored in two columns: data_length and index_length 
#### data_length stores size of data in table and index_length stores size of index file for that table
#### total size of table is sum of values in these two columns, sum can be converted to kB by dividing by 1024
    SELECT table_name, (data_length + index_length)/1024 FROM INFORMATION_SCHEMA.TABLES WHERE table_name = 'country' OR table_name = 'city' OR table_name = 'country_language' OR table_name = 'csv_test';

## Secure data using encryption using mysql CLI

#### implement encryption and decryption of a column in world database using official AES (Advanced Encryption Standard) algorithm 
#### AES is a symmetric encryption where same key is used to encrypt and decrypt data, AES standard permits various key lengths
#### By default key length of 128-bits is used, Key lengths of 196 or 256 bits can be used, key length is a trade off between performance and security
#### need to hash passphrase (consider passphrase is My secret passphrase) with a specific hash length (consider hash length is 512) using a hash function 
#### using hash function from SHA-2 family, It is good practice to hash passphrase, since storing passphrase in plaintext is a significant security vulnerability, using SHA2 algorithm to hash passphrase and assign it to variable key_str
    SET @key_str = SHA2('My secret passphrase', 512);

#### To encrypt Percentage column in table country_language, first convert data in column into binary byte strings of length 255 
    ALTER TABLE country_language MODIFY COLUMN Percentage VARBINARY(255);

#### using AES encryption standard and hashed passphrase for encryption, syntax: UPDATE table_name SET COLUMN_NAME = AES_ENCRYPT(COLUMN_NAME, @key_str);
    UPDATE country_language SET Percentage = AES_ENCRYPT(Percentage, @key_str);

#### result of this query will show Percentage column is encrypted and completely illegible means not clear to read properly
    SELECT * FROM country_language LIMIT 5;

#### accessing sensitive data by decryption, using AES_DECRYPT command and same key which was a passphrase and was hashed, stored in variable key_str 
    SELECT cast(AES_DECRYPT(Percentage, @key_str) as char(255)) FROM country_language;



## Improving Performance of Slow Queries in MySQL

#### EXPLAIN statement provides information about how MySQL executes statement, checking if your query is pulling more info than it needs to, resulting in a slower performance due to handling large amounts of data, EXPLAIN statement works with SELECT, DELETE, INSERT, REPLACE and UPDATE
#### EXPLAIN shows what type of select performed, table that select is being performed on, number of rows examined and any additional information
    EXPLAIN SELECT * FROM employees;

#### Indexing a Column
- indexes is like bookmarks, Indexes point to specific rows, helping query determine which rows match its conditions and quickly retrieves those results, query avoids searching through entire table and improves performance of query, particularly when using SELECT and WHERE clauses
- many types of indexes can be added to databases, popular ones are regular, primary, unique, full-text and prefix indexes
- Regular Index is where values don't have to be unique and can be NULL
- Primary Index are automatically created for primary keys, All column values are unique and NULL values are not allowed
- Unique Index is where all column values are unique, Unlike primary index, unique indexes can contain a NULL value
- Full-Text Index used for searching through large amounts of text and can only be created for char, varchar and text datatype columns
- Prefix Index uses only first N characters of a text value which can improve performance as only those characters would need to be searched
- best practice to avoid adding indexes to all columns, only adding them to ones that it may be helpful for such as column that is frequently accessed
- While indexing can improve performance of some queries, it can also slow down inserts, updates and deletes because each index will need to be updated every time, important to find balance between number of indexes and speed of queries, indexes are less helpful for querying small or large tables where almost all rows need to be examined, it would be faster to read all those rows rather than using an index

#### showing index of table employees
    SHOW INDEX FROM employees;

#### query to find execution time
    SELECT * FROM employees WHERE hire_date >= '2000-01-01';
    EXPLAIN SELECT * FROM employees WHERE hire_date >= '2000-01-01';

#### adding an index and then run previous command, results took less time than before as new index is added
    CREATE INDEX hire_date_index ON employees(hire_date);
    SHOW INDEX FROM employees;
    SELECT * FROM employees WHERE hire_date >= '2000-01-01';
    EXPLAIN SELECT * FROM employees WHERE hire_date >= '2000-01-01';

#### removing index and then verify using command SHOW INDEX FROM employees;
    DROP INDEX hire_date_index ON employees;

#### Avoid Leading Wildcards which are wildcards ("%abc") that find values ending with specific characters, results in full table scans
#### If query uses a leading wildcard and performs poorly, using a full-text index will improve speed of query by avoiding need to search through every row
#### query to find execution time
    SELECT * FROM employees WHERE first_name LIKE 'C%' OR last_name LIKE 'C%';
    EXPLAIN SELECT * FROM employees WHERE first_name LIKE 'C%' OR last_name LIKE 'C%';

#### adding index to both first_name and last_name columns and verify using SHOW INDEX command
    CREATE INDEX first_name_index ON employees(first_name);
    CREATE INDEX last_name_index ON employees(last_name);

#### re-run following query and check execution time, results took less time than before as new index is added
    SELECT * FROM employees WHERE first_name LIKE 'C%' OR last_name LIKE 'C%';
    EXPLAIN SELECT * FROM employees WHERE first_name LIKE 'C%' OR last_name LIKE 'C%';

#### Use of UNION ALL Clause
#### using OR operator with LIKE statements, a UNION ALL clause can improve speed of query, if columns on both sides of operator are indexed, improvement is due to OR operator sometimes scanning entire table and overlooking indexes where UNION ALL operator will apply them to separate SELECT statements
#### execution runs faster than when used OR operator
    SELECT * FROM employees WHERE first_name LIKE 'C%' UNION ALL SELECT * FROM employees WHERE last_name LIKE 'C%';

#### if a leading wildcard search with an index performed, entire table will still be scanned
#### query to find execution speed
#### checking with EXPLAIN and SHOW INDEX statements, although having an index on first_name, index isn't used and results in a search of entire table
#### Under EXPLAIN statement's possible_keys column, index hasn't been used as entry is NULL
    SELECT * FROM employees WHERE first_name LIKE '%C';

#### indexes work with trailing wildcards, query to find all employees whose first names begin with "C"
#### execution runs faster than when previous query used to find all employees whose first names ends with "C"
    SELECT * FROM employees WHERE first_name LIKE 'C%';

#### avoid selecting all columns from table, for larger datasets, selecting all columns can take much longer than selecting columns that needed
#### query to find execution speed
    SELECT * FROM employees;

#### query to find execution speed, execution runs faster than when previous query used to select all TABLES
    SELECT first_name, last_name, hire_date FROM employees;

## Monitoring and Optimizing Databases in MySQL

- Database monitoring is reviewing operational status of database, Monitoring is critical in maintaining health and performance of database because it helps to identify issues in a timely manner and able to avoid problems such as database outages that affect users
- In phpMyAdmin homepage, there is a left sidebar and right panel, information about servers and databases on right panel
- on right panel, There are several sub pages on Status page: Server, Processes, Query Statistics, All Status Variables, Monitor and Advisor
- These sub pages will give an inside look at current processes on server
- In Monitor page, charts used to see real-time information on how databases are doing, If there are unexpected irregularities such as a sudden spike in queries or a high volume of incoming traffic then that may point to a problem that needs to be attended to
- By default, there are three charts: Questions, Connections / Processes and Traffic
- Questions is number of queries that are being run at a given time including any background queries by phpMyAdmin, Connections / Processes and Traffic
- provide a visual of current connections, processes and incoming and outgoing bytes received and sent
- in Server page, gives more information about server status including how long server is running, there are two table in Server page
- Traffic table shows incoming and outgoing traffic where Received is number of bytes received by server (incoming) and Sent is number of bytes sent out by server (outgoing), Connections table gives information about maximum number of connections can have at same time, number of failed connection attempts, number of aborted connection attempts and total number of connections (successful or not)
- in Processes page, views operations currently being performed within server, will always be at least two users: event_scheduler and root
- in Processes page table, shows ID of process, User associated with it, Host to which it's connected to, default Database or None, Command type if it's being executed or if session is idle (Sleep), Time shows how long its in for Status and Status gives a description of what it's doing right now

#### command to show process list in mysql CLI
    SHOW PROCESSLIST;

- in Query Statistic page, shows types of queries run and how often they've been run, including queries that have been run, in addition to queries run by phpMyAdmin in background, this page also offers a visualization of breakdown of usage in All Status Variables page, shows listed variables, their value and description, If value of variable is red then that means it may need to be investigated and adjusted
- in Advisor page, offers an Advisor system that provides recommendations based on analysis of server variables to help monitor and optimize database
- Database optimization refers to maximizing speed and efficiency of retrieving data from database, Optimizing database improves its performance which can also improve performance of slow queries, best to keep data types simple, using smallest or a specialized data type when possible, Smaller data types tend to be faster because they require less space, specialized data types such as date and time (DATETIME) optimized to save disk space and memory, avoid NULL values in columns, Non-null values make operations faster as use indexes better and eliminate need of checking null values
- With Integer Types, MySQL supports TINYINT, SMALLINT, MEDIUMINT, INT and BIGINT, Depending on use should choose smallest possible data type
- types of strings are CHAR and VARCHAR, can optimize these by limiting number of characters in a column
- MySQL has special data types for date and time, these types include DATE, TIME, DATETIME, TIMESTAMP and YEAR

## Automating MySQL Database Tasks Using Shell Scripts

#### Starting MySQL service session in system terminal
    start_mysql

#### initiate mysql CLI session within MySQL service session in system terminal, if denied for password then follow command after this command 
    mysql

#### Opening .my.cnf as root user with nano editor to configure mysql password, enter line 'password = <MySQL Password>' after every user in ~/.my.cnf 
#### file and replace <MySQL Password> with password from start_mysql command, Press Ctrl+O followed by Enter key to save file, Press Ctrl+X to quit editor
    sudo nano ~/.my.cnf

#### initiate mysql CLI session within MySQL service session in system terminal after configuring password
    mysql

## Process Involved in Creating MySQL Database Backups in Linux OS

#### first create a new shell script named sql_backup.sh in filesystem and write following code in that file & save it
    #!/bin/sh
    # above line tells interpreter this code needs to be run as a shell script
    # Set database name to a variable
    DATABASE='sakila'

    # This will be printed on to screen, In case of cron job it will be printed to logs
    echo "Pulling Database: This may take a few minutes"

    # Set folder where database backup will be stored
    backupfolder=/home/theia/backups

    # Number of days to store backup 
    keep_day=30

    # set name of SQL file to dump database as "all-database-" suffixed with current timestamp and .sql extension
    sqlfile=$backupfolder/all-database-$(date +%d-%m-%Y_%H-%M-%S).sql
    # zip file to compress SQL file as "all-database-" suffixed with current timestamp and .gz extension
    zipfile=$backupfolder/all-database-$(date +%d-%m-%Y_%H-%M-%S).gz

    # Creating a backup using mysqldump command, if operation is successful, compress .sql file into .gz and delete .sql file
    if mysqldump  $DATABASE > $sqlfile ; then
    echo 'Sql dump created'
        # Compress backup 
        if gzip -c $sqlfile > $zipfile; then
            echo 'The backup was successfully compressed'
        else
            echo 'Error compressing backupBackup was not created!' 
            exit
        fi
        rm $sqlfile 
    else
    echo 'pg_dump return non-zero code No backup was created!' 
    exit
    fi

    # Delete old backups that are in system for longer than time user decided to retain backup
    find $backupfolder -mtime +$keep_day -delete

#### Creating a shell script which takes database name and back up directory as parameters and backup database as <dbname>_timestamp.sql in backup
#### directory by following code, displays appropriate message if database doesn't exist, create backup dir if it doesn't exist
    dbname=$(mysql -e "SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA WHERE SCHEMA_NAME = '$1'" | grep $1)

    if [ ! -d $2 ]; then 
        mkdir $2
    fi

    if [ $1 == $dbname ]; then
        sqlfile=$2/$1-$(date +%d-%m-%Y).sql
        if mysqldump  $1 > $sqlfile ; then
        echo 'Sql dump created'
        else
            echo 'Error creating backup!'
        fi
    else
        echo "Database doesn't exist"
    fi

#### creating a shell script which takes database name and script file as parameters and restores database from sql file by following code
    if [ -f $2 ]; then 
        dbname=$(mysql -e "SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA WHERE SCHEMA_NAME = '$1'" | grep $1)
        if [ $1 != $dbname ]; then
            echo "Created DB as it didn't exist"
            mysql -e "Create database $1"
        fi
        mysql -e "use $1"
        mysql $1 < $2
    else
        echo "File doesn't exist"
    fi

#### give executable permission for shell script file in system terminal, u stands for user, x stands for execute and r stands for read permission
    sudo chmod u+x+r sql_backup.sh

## Setting Up a Cron Job

#### Cron is a system helping Linux users schedule task, can be shell script or simple bash command, cron job helps to automate routine tasks and can be hourly, daily or monthly, crontab (cron table) is text file specifies schedule of cron jobs, Each line in crontab file contains six fields separated by space followed by command to be run, first five fields contain one or more values separated by comma or range of values separated by hyphen 
#### * asterisk operator means any value or always, if asterisk symbol in Hour field, it means task will be performed each hour
#### , comma operator specify a list of values for repetition for example if 1,3,5 in Hour field, task will run at 1 a.m., 3 a.m. and 5 a.m
#### - hyphen operator specify a range of values, if 1-5 in Day of week field, task will run every weekday (from Monday to Friday)
#### / slash operator specify values that will be repeated over a certain interval between them for example if */4 in Hour field, it means action will be performed every four hours, it is same as specifying 0,4,8,12,16,20 instead of asterisk before slash operator, can also use a range of values for example 1-30/10 means same as 1,11,21
#### setting up a cron job that happens every 2 minutes, starting crontab will open a crontab editor, Scroll to bottom of editor page using down arrow key and paste following code after crontab -e command, Press Ctrl+O followed by Enter key to save file and Press Ctrl+X to quit cron editor
    crontab -e
    */2 * * * * /home/project/sql_backup.sh > /home/project/backup.log

#### cron service needs to start explicitly by following command, After 2 minutes executing 2nd command to check whether backup file are created or not
    sudo service cron start
    ls -l /home/theia/backups

#### stopping cron job
    sudo service cron stop

## Truncate Tables in Database

#### For scenario where data is corrupted or lost and removing all data in database and restore data from backup
#### first create a new shell script named truncate.sh in filesystem and write following code in that file & save it
    #!/bin/sh
    # Connecting to mysql RDBMS using credentials, Lists tables using show tables and feeds output using pipe(|) operator to next command
    # iterating through each table using a while loop and truncates table
    DATABASE=sakila

    mysql -Nse 'show tables' sakila | \
        while read table; do mysql --host=127.0.0.1 --port=3306 \
        -e "use sakila;SET FOREIGN_KEY_CHECKS=0;truncate table $table;SET FOREIGN_KEY_CHECKS=1;" ;done

#### give executable permission for shell script file in system terminal, u stands for user, x stands for execute and r stands for read permission
    sudo chmod u+x+r truncate.sh

#### executing script to truncate tables and later check whether tables in database are truncated using mysql command
    sudo ./truncate.sh
    mysql

## Bash script performing a backup of all databases

#### first creating file my_backup.sh on filesystem and write following code and save it
    #!/bin/sh
    # above line tells interpreter this code needs to be run as a shell script
    mysqldump --all-databases --user=root --password > all-databases-backup.sql

#### give executable permission for shell script file in system terminal
    sudo chmod u+x+r my_backup.sh

#### executing script, after execution all-databases-backup.sql file will be created in filesystem containing backup of all databases
    sudo ./my_backup.sh

## Restore Database

#### finding list of backup files
    ls -l /home/theia/backups

#### Unzip file and extract SQL file from backup file all-database-26-09-2022_03-32-01.gz
    gunzip /home/theia/backups/all-database-26-09-2022_03-32-01.gz

#### populate and restore database with sql file that results from unzip operation
    mysql sakila < /home/theia/backups/all-database-26-09-2022_03-32-01

#### checking restored database by running following command
    mysql
    use sakila;
    select * from staff;

#### cleaning up backups folder and checking if it is cleaned or not
    sudo rm -rfv /home/theia/backups
    ls -l /home/theia/backups