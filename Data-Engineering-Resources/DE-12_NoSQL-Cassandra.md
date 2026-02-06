## Using CQL Shell in Linux (cqlsh)

#### Starting cassandra server
    start_cassandra

#### command to connect to instance of cassandra
    cqlsh --username cassandra --password MjE4NS1zaGVoYWIx

#### Finding host details On cqlsh
    show host

#### Finding version of server
    show version

#### Listing all keyspaces present on server
    describe keyspaces

#### Disconnecting from cassandra server
    exit

#### Working on DataStax Astra online database using cassandra query language (CQL)
#### first created a tabular type database named fcc_tutorial and keyspace named tabular using GUI
#### creating table books in tabular keyspace
    DESCRIBE KEYSPACES;
    USE tabular;
    CREATE TABLE IF NOT EXISTS books (bookid UUID, author TEXT, title TEXT, year INT, categories SET <TEXT>, added TIMESTAMP, PRIMARY KEY (bookid));
    DESCRIBE KEYSPACE tabular;

#### Inserting data
    INSERT INTO books(bookid, author, title, year, categories, added) VALUES (UUID(), 'Bobby Brown', 'black',1999,{'program','computer'},toTIMESTAMP(now()));
    INSERT INTO books(bookid, author, title, year, categories, added) VALUES (UUID(), 'Andy agnes', 'blue',1995,{'story','nonfic'},toTIMESTAMP(now()));
    SELECT * FROM books;
    SELECT * FROM books WHERE bookid = 0f36f88f-fffc-4fb3-8070-231762b3a337;

#### clear terminal
    CLEAR;

#### creating table rest_by_country in tabular keyspace, in PRIMARY key country is a partition key and clustering keys are name and url
    CREATE TABLE IF NOT EXISTS rest_by_country(country TEXT, name TEXT, cuisine TEXT, url TEXT, 
        PRIMARY KEY((country),name,url)) WITH CLUSTERING
        ORDER BY (name DESC, url ASC);

    INSERT INTO rest_by_country(country, name, cuisine, url) VALUES ('india','korma','traditional','www.google.com');
    INSERT INTO rest_by_country(country, name, cuisine, url) VALUES ('bd','poloa','native','www.googled.com');
    INSERT INTO rest_by_country(country, name, cuisine, url) VALUES ('pk','beef','trad','www.googlpk.com');
    SELECT * FROM rest_by_country;
    SELECT * FROM rest_by_country WHERE country='bd';
    DROP TABLE books;

#### Creating document type database
#### first created a keyspace named document in database named fcc_tutorial using GUI
#### work done using GUI
#### Creating key_value type database
#### first created a keyspace named keyvalue in database named fcc_tutorial using GUI

## Cassandra Keyspace Operations in cqlsh

#### creating a keyspace called training using SimpleStrategy and a replication_factor of 3, SimpleStrategy is used when all nodes in cassandra cluster exist in a single data center, verify by Listing all keyspaces
    CREATE KEYSPACE training WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3};
    describe keyspaces;

#### Describing training keyspace
    describe training

#### Altering training keyspace by changing class to NetworkTopologyStrategy, NetworkTopologyStrategy is used when all nodes in cassandra cluster are spread across multiple data centers, verify by Describing training keyspace
    ALTER KEYSPACE training WITH replication = {'class': 'NetworkTopologyStrategy'};
    describe training

#### Using a keyspace
    use training;

#### List all tables in training keyspace
    describe tables

#### Dropping a keyspace
    drop keyspace training;

#### Verify changes using describe command
    use system;
    describe keyspaces

## Cassandra Table Operations

#### creating a keyspace called training
    CREATE KEYSPACE training WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3};

#### creating a table named movies in training keyspace, Verify that table got created or not by listing all tables
    use training;
    CREATE TABLE movies(movie_id int PRIMARY KEY, movie_name text, year_of_release int);
    describe tables;

#### Describing table movies
    describe movies

#### Altering table movies by adding a column named genre which is of type text, Verify changes using describe command
    ALTER TABLE movies ADD genre text;
    describe movies;

#### Altering table movies by Dropping column movie_name, verify changes using describe command
    ALTER TABLE movies drop movie_name; 
    describe movies;

#### Dropping table movies, verify using describe command
    drop table movies;
    describe movies;

## Cassandra CRUD Operations

#### creating a keyspace called training, then creating a table named movies
    CREATE KEYSPACE training WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3}; 
    use training; 
    CREATE TABLE movies( movie_id int PRIMARY KEY, movie_name text, year_of_release int);

#### Inserting data into movies table, verify by selecting all data from table
    INSERT into movies(movie_id, movie_name, year_of_release) VALUES (1,'Toy Story',1995);
    INSERT into movies(movie_id, movie_name, year_of_release) VALUES (2,'Jumanji',1995);
    INSERT into movies(movie_id, movie_name, year_of_release) VALUES (3,'Heat',1995);
    INSERT into movies(movie_id, movie_name, year_of_release) VALUES (4,'Scream',1995);
    INSERT into movies(movie_id, movie_name, year_of_release) VALUES (5,'Fargo',1996);
    select * from movies;

#### Query movie name with movie_id 1
    select movie_name from movies where movie_id = 1;

#### Updating data in movies table, verify with select command
    UPDATE movies SET year_of_release = 1996 WHERE movie_id = 4;
    select * from movies where movie_id = 4;

#### Creating an index on movies table on column movie_name
    create index movie_index on movies(movie_name);

#### Deleting data from movies table, verify with select command
    DELETE from movies WHERE movie_id = 5; 
    select * from movies;

## Cassandra import/export data

#### starting cassandra
    start_cassandra

#### to import a csv file, first need to create suitable table inside a keyspace
#### Creating keyspace training
    CREATE KEYSPACE training WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3};

#### In training keyspace, creating a table named diamonds with same field as in csv file that going to be imported
    use training; 
    CREATE TABLE diamonds(id int PRIMARY KEY, clarity text, cut text, price int);

#### now Importing diamonds.csv into training keyspace and diamonds table/column family
    COPY training.diamonds(id,clarity,cut,price) FROM 'mongodb_exported_data.csv' WITH DELIMITER=',' AND HEADER=TRUE;

#### Exporting diamonds table into a csv file
    COPY diamonds TO 'cassandra-diamonds.csv';
 
#### Importing partial_data.csv into cassandra server into a keyspace named entertainment and a table named movies
    CREATE KEYSPACE entertainment WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3};
    use entertainment; 
    CREATE TABLE movies(
        id text PRIMARY KEY,
        title text,
        year text, 
        rating text,
        Director text
    );
    COPY entertainment.movies(id,title,year,rating,Director) FROM 'partial_data.csv' WITH DELIMITER=',' AND HEADER=TRUE;

#### cql query to count number of rows in movies table
    select count(*) from movies;

#### Creating an index for rating column in movies table using cql
    create index ratingIndex on movies(rating);

#### cql query to count number of movies that are rated 'G', use 'G' not "G", using "G" will give error
    SELECT COUNT(*) FROM movies WHERE rating='G';