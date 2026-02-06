## Working with Facts and Dimension Tables

#### Creating schema on data warehouse in postgresql server using Linux CLI
#### Starting postgresql server
    start_postgres

#### Using createdb command of PostgreSQL server to create database named billingDW from terminal
#### -h specifies database server is running on localhost, -U specifies using user name postgres to log into database, -p specifies database server is running on port number 5432
    createdb -h localhost -U postgres -p 5432 billingDW

#### Downloading sql file using wget
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/\
    Working%20with%20Facts%20and%20Dimension%20Tables/star-schema.sql

#### Creating schema from sql file under billingDW database
    psql -h localhost -U postgres -p 5432 billingDW < star-schema.sql

## Setting up a Staging Area

#### Starting postgresql server
    start_postgres

#### Using createdb command of PostgreSQL server to create database named billingDW from terminal
#### -h specifies database server is running on localhost, -U specifies using user name postgres to log into database, -p specifies database server is running on port number 5432
    createdb -h localhost -U postgres -p 5432 billingDW

#### Downloading zip file using wget and extracting sql file from zip file
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/Setting%20up%20a%20staging%20area/\
    billing-datawarehouse.tgz

    tar -xvzf billing-datawarehouse.tgz
    ls *.sql

#### Creating schema from star-schema.sql file under billingDW database
    psql -h localhost -U postgres -p 5432 billingDW < star-schema.sql

#### Load data into Dimension table DimCustomer from DimCustomer.sql file under billingDW database
    psql  -h localhost -U postgres -p 5432 billingDW < DimCustomer.sql

#### Load data into Dimension table DimMonth from DimMonth.sql file under billingDW database
    psql  -h localhost -U postgres -p 5432 billingDW < DimMonth.sql

#### Load data into Fact table FactBilling from FactBilling.sql file under billingDW database
    psql  -h localhost -U postgres -p 5432 billingDW < FactBilling.sql

#### checking number of rows in all tables using verify.sql file in billingDW database
    psql  -h localhost -U postgres -p 5432 billingDW < verify.sql

## Verifying Data Quality for a Data Warehouse

#### Starting postgresql server
    start_postgres

#### Downloading staging area setup script and then running setup script
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/\
    Verifying%20Data%20Quality%20for%20a%20Data%20Warehouse/setup_staging_area.sh
    bash setup_staging_area.sh

#### Getting testing framework ready by performing data quality checks by manually running sql queries on data warehouse
#### automating these checks using custom programs or tools as Automation helps to easily create new tests, run tests and schedule tests
#### Downloading python based framework to run data quality tests
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/\
    Verifying%20Data%20Quality%20for%20a%20Data%20Warehouse/dataqualitychecks.py

    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/\
    Verifying%20Data%20Quality%20for%20a%20Data%20Warehouse/dbconnect.py

    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/\
    Verifying%20Data%20Quality%20for%20a%20Data%20Warehouse/mytests.py

    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/\
    Verifying%20Data%20Quality%20for%20a%20Data%20Warehouse/generate-data-quality-report.py

    ls

#### Installing python driver for Postgresql, use ! before pip if command not Working
    python3 -m pip install psycopg2

#### testing database connectivity by if Postgresql python driver is installed properly, if server is up and running & if micro framework connected to DB
    python3 dbconnect.py

## Creating a sample data quality report

#### running command below to install pandas & tabulate and generate a sample data quality report
    python3 -m pip install pandas
    python3 -m pip install tabulate
    python3 generate-data-quality-report.py

#### Exploring data quality tests by Opening file mytests.py in editor, mytests.py contains all data quality tests
#### Checking for null values by adding following code in mytests.py & save it and run following code after that
    test5={
        "testname":"Check for nulls",
        "test":check_for_nulls,
        "column": "year",
        "table": "DimMonth"
    }

    python3 generate-data-quality-report.py

#### Checking for min max range values by adding following code in mytests.py & save it and run following code after that
    test6={
        "testname":"Check for min and max",
        "test":check_for_min_max,
        "column": "quarter",
        "table": "DimMonth",
        "minimum":1,
        "maximum":4
    }

    python3 generate-data-quality-report.py

#### Checking for invalid entries by adding following code in mytests.py & save it and run following code after that
    test7={
        "testname":"Check for valid values",
        "test":check_for_valid_values,
        "column": "quartername",
        "table": "DimMonth",
        "valid_values":{'Q1','Q2','Q3','Q4'}
    }

    python3 generate-data-quality-report.py 

#### Checking for duplicate entries by adding following code in mytests.py & save it and run following code after that
    test8={
        "testname":"Check for duplicates",
        "test":check_for_duplicates,
        "column": "customerid",
        "table": "DimCustomer"
    }

    python3 generate-data-quality-report.py 

## Populating a Data Warehouse using PostgreSQL (Done on my pc)

#### first created a database named Production in pgAdmin, then selected star-schema.sql file from filesystem on Query tool option of Production database and ran that file, it created empty DimCustomer, DimMonth and FactBilling tables on Production database
#### selected DimCustomer.sql file from filesystem on Query tool option and ran that file, it loaded data on DimCustomer table
#### selected DimMonth.sql file from filesystem on Query tool option and ran that file, it loaded data on DimMonth table
#### selected FactBilling.sql file from filesystem on Query tool option and ran that file, it loaded data on FactBilling table
#### created a materialized query table (MQT) named avg_customer_bill with fields customerid and averagebillamount from FactBilling table
    CREATE MATERIALIZED VIEW avg_customer_bill (customerid, averagebillamount) AS (select customerid, avg(billedamount) from public.FactBilling group by customerid);

#### Refreshed newly created MQT
    REFRESH MATERIALIZED VIEW avg_customer_bill;

#### querying with MQT
    select * from avg_customer_bill where averagebillamount > 11000;

#### Querying Data Warehouse using Cubes, Rollups, Grouping Sets and Materialized Views
#### GROUPING SETS, CUBE and ROLLUP easily create subtotals and grand totals in a variety of ways; these operators are used along with GROUP BY operator
#### GROUPING SETS operator group data in a number of different ways in a single SELECT statement; ROLLUP operator is used to create subtotals and grand totals for a set of columns; summarized totals are created based on columns passed to ROLLUP operator; CUBE operator produces subtotals and grand totals; it produces subtotals and grand totals for every permutation of columns provided to CUBE operator querying on Production database from Query tool option using grouping sets 
    select year, quartername, sum(billedamount) as totalbilledamount from FactBilling left join DimCustomer on 
        FactBilling.customerid = DimCustomer.customerid left join DimMonth on FactBilling.monthid = DimMonth.monthid
        group by grouping sets(year,quartername);

#### query using rollup
    select year, category, sum(billedamount) as totalbilledamount from FactBilling left join DimCustomer on 
        FactBilling.customerid = DimCustomer.customerid left join DimMonth on FactBilling.monthid = DimMonth.monthid
        group by rollup(year,category) order by year, category;

#### query using cube
    select year, category, sum(billedamount) as totalbilledamount from FactBilling left join DimCustomer on 
        FactBilling.customerid = DimCustomer.customerid left join "DimMonth" on FactBilling.monthid = DimMonth.monthid
        group by cube(year,category) order by year, category;

#### Creating Materialized Query Table(MQT)
    CREATE MATERIALIZED VIEW countrystats (country, year, totalbilledamount) AS (select country, year, sum(billedamount) 
        from FactBilling
        left join DimCustomer on FactBilling.customerid = DimCustomer.customerid left join 
        DimMonth on FactBilling.monthid = DimMonth.monthid
        group by country,year);

#### Refreshed newly created MQT
    REFRESH MATERIALIZED VIEW countrystats;

#### querying with MQT
    select * from countrystats;

## Final Assignment Data Warehouse (Done on my pc)

#### Creating tables DimDate, DimTruck, DimStation, FactTrips into database sample_warehouse, then load data into tables from csv files using Query tool
    CREATE TABLE DimDate(
        dateid integer NOT NULL PRIMARY KEY, date date NOT NULL, Year integer NOT NULL, Quarter integer NOT NULL, QuarterName varchar(5) NOT NULL, Month integer NOT NULL, Monthname varchar(10) NOT NULL, Day integer NOT NULL, Weekday integer NOT NULL, Weekdayname varchar(10) NOT NULL);
    CREATE TABLE DimTruck(Truckid integer NOT NULL PRIMARY KEY, TruckType varchar(10) NOT NULL);
    CREATE TABLE DimStation(Stationid integer NOT NULL PRIMARY KEY, City varchar(50) NOT NULL);
    CREATE TABLE FactTrips(Tripid integer NOT NULL PRIMARY KEY, Dateid integer NOT NULL, Stationid integer NOT NULL, Truckid integer NOT NULL, Wastecollected float NOT NULL);

#### query using grouping sets 
    select Stationid, Trucktype, sum(Wastecollected) as totalwastecollected from FactTrips as f left join DimTruck as dt on 
    f.Truckid = dt.Truckid group by grouping sets(Stationid, Trucktype);

#### query using rollup
    select dd.Year, ds.City, f.Stationid, sum(f.Wastecollected) as totalwastecollected from FactTrips as f
        left join DimDate as dd on f.dateid = dd.dateid left join DimStation as ds on 
        f.Stationid=ds.Stationid group by rollup(dd.Year, ds.City, f.Stationid) order by dd.Year, ds.City, f.Stationid;

#### query using cube
    select dd.Year, ds.City, f.Stationid, avg(f.Wastecollected) as averagewastecollected from FactTrips as f
        left join DimDate as dd on f.dateid = dd.dateid left join DimStation as ds on 
        f.Stationid=ds.Stationid group by cube(dd.Year, ds.City, f.Stationid) order by dd.Year, ds.City, f.Stationid;

#### Creating Materialized Query Table(MQT)
    CREATE MATERIALIZED VIEW max_waste_stats (City, StationID, TruckType, MaxWasteCollected) AS (select ds.City, f.Stationid, 
        dt.Trucktype, max(f.Wastecollected) from FactTrips as f left join DimStation as ds on f.Stationid = ds.Stationid 
        left join DimTruck as dt on f.Truckid=dt.Truckid group by ds.City, f.Stationid, dt.Trucktype);

#### Refreshed newly created MQT
    REFRESH MATERIALIZED VIEW max_waste_stats;

#### querying with MQT
    select * from max_waste_stats;