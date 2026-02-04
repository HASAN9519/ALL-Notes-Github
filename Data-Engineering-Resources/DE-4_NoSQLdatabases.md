## Overview of NoSQL

- NoSQL stands for Not only SQL and NoSQL refers to a class of databases that are non-relational in architecture; implementations of NoSQL databases are technically different from each other but all share some common traits; historically relational databases were more prevalent but since year 2000 NoSQL databases have become more popular in database marketplace due to scale demands of Big Data.

## Characteristics of NoSQL Databases

- NoSQL databases are non-relational; there are four categories of NoSQL database: Key-Value, Document, Column-based and Graph style; NoSQL databases have their roots in open source community; Most NoSQL databases are built to scale horizontally and share their data more easily than their relational counterparts; NoSQL databases allow more agile development through their flexible schemas as compared to fixed schemas of relational databases; several benefits to adopting NoSQL databases are scalability, performance, High Availability, reducing cost, Flexible schema and intuitive data structures, specific indexing and querying.

## NoSQL Database Categories - Key-Value

- four main categories of NoSQL database are Key-Value, Document, Wide Column and Graph; Key-Value NoSQL databases have least complex architecture and data is stored with a key and corresponding value blob and is represented by a hashmap; primary use cases for Key-Value NoSQL database category are quick CRUD operations for example storing and retrieving session information, storing in-app user profiles and storing shopping cart data in online stores; popular key-value NoSQL databases are: Amazon DynamoDB, Oracle NoSQL Database, Redis, Aerospike, Riak KV, MemcacheDB, Project Voldemort.

## NoSQL Database Categories - Document

- Document-based NoSQL databases use documents to make values visible and able to be queried; Each piece of data is considered a document and typically stored in either JSON or XML format; Each document offers a flexible schema; primary use cases for document-based NoSQL databases are event logging for apps and processes, online blogging and operational datasets or metadata for web and mobile apps; some popular document NoSQL databases are: IBM Cloudant, MongoDB, Apache CouchDB, Terrastore, OrientDB, Couchbase, RavenDB.

## NoSQL Database Categories - Column

- Column-based databases spawned from architecture of Google’s Bigtable storage system; Column-based databases store data in columns or groups of columns; Column families are several rows with unique keys belonging to one or more columns; primary use cases for Column-based NoSQL databases are event logging and blogs, counters, data with expiration values; popular Column-based NoSQL databases are: Cassandra, HBASE, Hypertable, accumulo.

## NoSQL Database Categories - Graph

- Graph databases store information in entities or nodes and relationships or edges; Graph databases are impressive when data set resembles a graph-like data structure; Graph databases don't shard well but are ACID transaction compliant; primary use cases for Graph NoSQL database category are for highly connected & related data, for social networking sites, for routing, for spatial & map applications and for recommendation engines; popular Graph NoSQL databases are: Neo4j, OrientDB, ArangoDB, Amazon Neptune (part of Amazon Web Services), Apache Giraph, JanusGraph.

## ACID vs BASE

- ACID and BASE are consistency models used in relational and NoSQL databases; ACID stands for Atomicity, Consistency, Isolated, Durable; BASE stands for Basically Available, Soft state, Eventually consistent; ACID model is focused on data consistency but BASE model is focused on data availability; Both consistency models have their applicability and Usage should be selected based on a case-by-case analysis.

## Distributed Databases

- Distributed databases are physically distributed across data sites by fragmenting and replicating data; Fragmentation enables an organization to store a large piece of data across all servers of a distributed system by breaking data into smaller pieces; Replication means that all partitions of data are stored redundantly in two or more sites; With replication if one node fails that piece of data can be retrieved from another node; Distributed databases provide several advantages such as reliability, availability, performance but also have their disadvantages such as don't really support Transactions; Distributed databases follow BASE consistency model.

## CAP Theorem

- CAP Theorem is also called Brewer's Theorem; CAP theorem states that there are three essential system requirements necessary for successful design, implementation and deployment of applications in distributed systems; These are Consistency, Availability and Partition Tolerance or CAP; A distributed system can guarantee delivery of only two of these three desired characteristics; CAP theorem can be used to classify NoSQL databases; NoSQL databases chooses between availability and consistency for example MongoDB chooses consistency as primary design driver of solution but Apache Cassandra chooses availability; system continues to operate despite data loss or network failures is called Partition Tolerance.

## Challenges in Migrating from RDBMS to NoSQL Databases

- NoSQL systems are not a de facto replacement of RDBMS; RDBMS and NoSQL cater to different use cases; solution could use both RDBMS and NoSQL. A migration from RDBMS to NoSQL could be triggered by requirements of performance driven by data volume or flexibility in schema or system scalability. Migrating from RDBMS to NoSQL requires adoption of NoSQL concepts.

## Overview of MongoDB

- MongoDB is a document and a NoSQL database; MongoDB supports various data types; Documents provide a flexible way of storing data; MongoDB documents of a similar type are grouped into collections; MongoDB models data as read/write, brings structured or unstructured data and provides high availability; MongoDB can be used for a variety of purposes because of flexibility of storing structured or unstructured data.

## Advantages of MongoDB

- Database schema can be flexible when working with MongoDB, can change it as needed without involving complex data definition language statements; MongoDB uses a code-first approach instead of a design then code approach; MongoDB also utilizes an evolving schema; Complex data analysis can be done on server using Aggregation Pipelines; MongoDB provides native high availability.

## Use Cases for MongoDB

- MongoDB can be used in a variety of use cases such as in field of IoT, E-commerce, Gaming; Scalability makes it easier to work across globe; Can perform real-time analysis on data; with flexible schema it is easier to support ever-changing data; Complex data analysis can be done on server using Aggregation Pipelines; can ingest multiple shapes and formats of data from different sources.

## CRUD Operations

- Mongo shell is an interactive command line tool provided by MongoDB to interact with databases; To use Mongo shell, first need to make a connection to cluster via a connection string; using 'show dbs' to list databases; 'use databasename' to select a database and 'show collections' to list collections in a database; CRUD operations consist of Create, Read, Update and Delete; Useful functions include insertOne, insertMany, findOne, find, count, replace, updateOne, updateMany, deleteOne and deleteMany; Different kinds of acknowledgements are returned based on operation being run.

## Indexes

- Indexes help quickly locate data; Indexes should be created for most frequent queries; A compound index indexes more than one field; MongoDB stores data being indexed on index entry and a location of document on disk; MongoDB stores an index as a tree to make finding documents more efficient.

## Aggregation Framework

- Using an aggregation framework, can perform complex analysis on data in MongoDB; can build aggregation process in stages such as match, group, project and sort; outcome can be queried or stored into another collection using $merge.

## Replication & Sharding

- Replication is duplication of data and any changes made to data; it provides fault tolerance, redundancy and high availability for data; it will not prevent a disaster such as deletion of documents, collections or even databases; use Sharding to scale horizontally for growing data sets.

## Accessing MongoDB from Python

- MongoClient is a class that interact with MongoDB; MongoClient is imported from pymongo which is MongoDB driver for Python; can perform single or bulk inserts; can replace whole documents and perform an in-place update which is a preferred option; can delete one or more documents from collection.

## Overview of Cassandra

- Apache Cassandra is an open source, distributed, decentralized, elastically scalable, highly available, fault tolerant, tunable and consistent database; it is best used by always available types of online applications that require a database that is always available, that scales fast in situations of high traffic, that can be installed in a geographically distributed manner and that require a high write performance; Apache Cassandra is best used by online services like Netflix, Uber, Spotify, eCommerce websites and Timeseries applications.

## Architecture of Cassandra

- Cassandra is based on a distributed system architecture; it can be installed on a single machine or container; A single Cassandra instance is called a node; it supports horizontal scalability achieved by adding more than one node as a part of a Cassandra cluster; it is designed to be a peer-to-peer architecture with each node connected to all other nodes; Each Cassandra node can perform all database operations and can serve client requests without need for a primary node; Gossip is protocol used by Cassandra nodes for peer-to-peer communication; gossip protocol informs a node about state of all other nodes; A node performs gossip communications with up to three other nodes every second; gossip messages follow a specific format and use version numbers to make efficient communication thus shortly each node can build entire metadata of cluster; A Cassandra cluster can be a single data center deployment but most of time Cassandra clusters are deployed in multiple data centers; There are several components in Cassandra nodes that are involved in write and read operations; Some of them are:

    - Memtable: Memtables are in-memory structures where Cassandra buffers writes; there is one active Memtable per table; Memtables are flushed onto disk and become immutable SSTables; This can be triggered in several ways such as memory usage of Memtables exceeds a configured threshold, CommitLog approaches its maximum size and forces Memtable flushes in order to allow Commitlog segments to be freed, setting a time to flush per table.
    - CommitLog: Commitlogs are an append-only log of all mutations local to a Cassandra node; Any data written to Cassandra will first be written to a commit log before being written to a Memtable; this provides durability in case of unexpected shutdown; any mutations in commitlog will be applied to Memtables on startup.
    - SSTables: SSTables are immutable data files that Cassandra uses for persisting data on disk; As SSTables are flushed to disk from Memtables or are streamed from other nodes, Cassandra triggers compactions which combine multiple SSTables into one; Once new SSTable has been written, old SSTables can be removed; Each SSTable is comprised of multiple components stored in separate files, some of which are listed below:  
    - Data.db: actual data.
    - Index.db: An index from partition keys to positions in Data.db file.
    - Summary.db: A sampling of (by default) every 128th entry in Index.db file. 
    - Filter.db: A Bloom Filter of partition keys in SSTable. 
    - CompressionInfo.db: Metadata about offsets and lengths of compression chunks in Data.db file.

- Cassandra processes data at several stages on write path, starting with immediate logging of a write and ending with a write of data to disk such as Logging data in commit log, Writing data to Memtable, Flushing data from Memtable and Storing data on disk in SSTables.
- While writes in Cassandra are very simple and fast operations done in memory, read is a bit more complicated since it needs to consolidate data from both memory (Memtable) and disk (SSTables); Since data on disk can be fragmented in several SSTables, read process needs to identify which SSTables most likely to contain info about partitions that are going to be queried; this selection is done by Bloom Filter information which have steps like Checking Memtable, Checking Bloom filter, Checking partition key cache if enabled, If partition is not in cache, partition summary is checked and Then partition index is accessed, Locates data on disk and Fetches data from SSTable on disk; Data is consolidated from Memtable and SSTables before being sent to coordinator.

## Key Features of Cassandra

- Its distributed and decentralized architecture helps Cassandra be available, scalable and fault tolerant; Data distribution and replication takes place in one or more data center clusters; Cassandra provides high write throughput; CQL is language used to communicate with Cassandra.

## Cassandra Data Model

- Cassandra stores data in tables and tables are grouped in keyspaces; It is recommended to use one keyspace per application; can create, drop and alter tables without impacting running updates on data or running queries; two roles of a primary key are to optimize read performance for queries on table and to provide uniqueness to entries; A primary key has two components, a mandatory partition key and one or more optional clustering keys; Cassandra has two types of tables: static and dynamic; Static tables are those tables that have a primary key that contains only partition key but no clustering key; A clustering key specifies order that data is arranged inside partition (ascending or descending); A clustering key can be a single or multiple column key; A clustering key provides uniqueness to a primary key and improves read query performance; Reducing amount of data to be read from a partition is crucial for query time especially in case of large partitions; In dynamic tables partitions grow dynamically with number of entries; Building a primary key in order to answer query in optimal time is beginning of Data Modeling process; To define a Cassandra table in order to get good read performance, need to start with queries that would like to answer then build primary key based on query.

## Introduction to Cassandra Query Language Shell (cqlsh)

- CQL is primary language for communicating with Apache Cassandra clusters; CQL keywords and identifiers are case-insensitive; CQL queries can be run programmatically using a licensed Cassandra client driver or they can be run on Python-based CQL shell client provided with Cassandra; can create, alter & drop keyspaces & tables, insert, update & delete data and execute queries using SELECT in CQL shell; CQL shell has several special commands including CONSISTENCY and COPY commands; CONSISTENCY command can be used to tune consistency of data; COPY command can be used to import data into and export data out of Cassandra.

## CQL Data Types

- Cassandra supports built-in, collection and user-defined data types; Both collection and user-defined data types offer a way to group and store data together; Collection data types can emulate one-to-many relationships; There are three types of collection data types: lists, maps and sets. User-defined data types can emulate one-to-one relationships and can attach multiple data fields to a column.

## Keyspace Operations

- Keyspaces are defined before creating tables and a keyspace can contain any number of tables; Replication is specified at keyspace level; Data replication depends on a replication factor and a replication strategy both set at keyspace level; Replication Factor sets number of replicas; Replication Strategy determines on which cluster nodes replicas are going to be located; Common keyspace operations are CREATE KEYSPACE, ALTER KEYSPACE and DROP KEYSPACE.

## Table Operations

- Data in Cassandra is organized logically in tables; A table’s metadata specifies primary key instructing Cassandra how to distribute table data at cluster level and at node level; can add Time-To-Live parameter at table level meaning that can expire (or delete) all data that has surpassed TTL; can modify columns and column names but only for regular columns; A Primary Key once defined at table creation cannot be modified; can either drop a table or truncate its data.

## CRUD Operations

- By default Cassandra doesn't perform a read before writes therefore INSERT and UPDATE operations behave similarly, Lightweight Transactions can be used in order to enforce a read before write but should be used sparingly due to performance degradation; Cluster level writes are sent to all partition's replicas irrespective of consistency factor; For an operation to be successful, acknowledgement is expected from at least minimum number of nodes specified for consistency; Reads at cluster level are sent only to number of replicas according to consistency setting; Reads should follow Primary Key columns order for best performance; Deletes can be done at record, cell, range and partition levels. 

## Overview of Cloudant

- IBM Cloudant is a fully managed database-as-a-service that is built on Apache CouchDB and it uses a JSON document store; Cloudant is a distributed database optimized for large workloads such as web, mobile, IoT and serverless applications; As Cloudant is a Service Level Agreement-backed DBaaS, it means there are no admin overheads and Cloudant databases are securely hosted in cloud; Cloudant offers a powerful replication protocol & API and is compatible with many open source ecosystems and libraries; Cloudant provides search, geospatial querying, offline & mobile access and language specific library capabilities.

## Cloudant Architecture and Key Technologies

- IBM Cloudant provides over 55 data centers distributed around world; Cloudant is supported on several cloud platforms; Cloudant’s cloud architecture provides high availability, disaster recovery and optimal performance; Cloudant provides offline sync capabilities for web and mobile applications; Cloudant can be offered as a fully managed service, as an on-premise deployment or as a hybrid cloud deployment; Cloudant consists of several key technology components such as a well-defined HTTP API, MapReduce, Full-text Search, Geospatial capabilities and an integrated dashboard to monitor, manage and develop NoSQL databases.

## Cloudant Benefits and Solutions

- key benefits of IBM Cloudant are its scalability, availability, durability, partition tolerance, online upgrade and patching; IBM Cloudant solves several data challenges such as exponential user growth, increasing cost, time & skills required to develop, host and administer a database in-house; Cloudant uses a document database for improved security and querying; Cloudant runs on clustered servers in Cloud; Typical use cases for Cloudant include building web and mobile apps, AI solutions and analysis of IoT sensor data.

## Dashboards in Cloudant

- Cloudant dashboard is used to create & manage databases, configure replication, view active tasks and monitor Cloudant instances; Databases section of dashboard used to create databases, add documents to databases, replicate databases, configure permissions for databases, delete databases and run queries on documents in databases; Replication section of dashboard used to configure replication between source and target databases; View Active Tasks section of dashboard used to view a list of currently running tasks to assist in determining any issues with system performance; Monitoring section used to view consumption of provisioned throughput capacity and data storage currently being utilized in Cloudant instance.

## Working with Databases in Cloudant

- can create and use two types of database in IBM Cloudant: non-partitioned and partitioned; Non-partitioned databases have no partitioning scheme to define but they only provide global querying; Partitioned databases provide both partitioned & global querying and offer substantial performance & cost benefits; Partitioning type is set at database creation; Cloudant is a document database system that uses a JSON document store.

## HTTP API Basics

- Cloudant’s databases have an HTTP API so they can be accessed by using requests from standard HTTP library; HTTP is used by web browsers, mobile devices, common programming languages and command-line scripting tools such as curl; Curl is a free, open source command-line tool that can make HTTP requests to Cloudant API; Curl can get and send data including files using standard URL syntax from command-line or in a script; need to create a variable to access Cloudant service; can use an alias as a shortcut for curl; can pipe output of curl commands to Python or jq for improved JSON formatting in terminal.

## Working with Databases in Cloudant lab

#### Inserting sample documents on database

    {
        "_id":"1",
        "square_feet":1500,
        "bedrooms":3,
        "price":147890
    }

#### Query documents
#### Select all fields in all documents

    {  
        "selector": {} 
    }

#### Select all fields in all documents with _id greater than 4

    {
    "selector": {
        "_id": {
            "$gt": "4"
        }
    }
    }

#### Select all fields in all documents with _id less than 4

    {
        "selector": {
            "_id": {
                "$lt": "4"
            }
        }
    }

#### Select fields _id, square_feet and price in all documents

    {
    "selector": {},
    "fields": [
        "_id",
        "price",
        "square_feet"
    ]
    }

#### Select fields _id, square_feet and price in documents with _id less than 4

    {
    "selector": {
    "_id": {
            "$lt": "4"
        }
    },
    "fields": [
        "_id",
        "price",
        "square_feet"
    ]
    }

#### Select fields _id, bedrooms and price in documents with _id greater than 2 and sort by _id ascending

    {
    "selector": {
        "_id": {
            "$gt": "2"
        }
    },
    "fields": [
        "_id",
        "price",
        "square_feet"
    ],
    "sort": [
        {
            "_id": "asc"
        }
    ]
    }

#### Select fields _id, bedrooms and price in documents with _id greater than 2 and sort by _id descending

    {
    "selector": {
        "_id": {
            "$gt": "2"
        }
    },
    "fields": [
        "_id",
        "price",
        "square_feet"
    ],
    "sort": [
        {
            "_id": "desc"
        }
    ]
    }

## Using HTTP API to create and query Cloudant databases

#### command to Getting environment ready
#### url contains Cloudant username and password

    export CLOUDANTURL="https://apikey-v2-regv2h1x1kh8i2gbxtopk25e6p57pm1gxi2hylix9ep:1297f4ec25c01324505d4ef7bbf56963@\
    ccc71206-2052-403f-9553-589b0a08ac4e-bluemix.cloudantnosqldb.appdomain.cloud"

#### Testing credentials

    curl $CLOUDANTURL
    curl $CLOUDANTURL/_all_dbs

#### creating a database named animals using terminal, Verify by listing all databases

    curl -X PUT $CLOUDANTURL/animals
    curl $CLOUDANTURL/_all_dbs

#### Dropping database animals, Verify by listing all databases

    curl -X DELETE $CLOUDANTURL/animals
    curl $CLOUDANTURL/_all_dbs

#### creating a database named planets

    curl -X PUT $CLOUDANTURL/planets

#### inserting a document in planets database with _id of 1, Verify by listing document with _id 1

    curl -X PUT $CLOUDANTURL/planets/"1" -d '{ 
        "name" : "Mercury" ,
        "position_from_sum" :1 
        }'
    curl -X GET $CLOUDANTURL/planets/1

#### Updating a document using its revision id, Verify by listing document with _id 1, revision id changes after each update

    curl -X PUT $CLOUDANTURL/planets/1 -d '{ 
        "name" : "Mercury" ,
        "position_from_sum" :1,
        "revolution_time":"88 days",
        "_rev":"1-473b9806b957558dd32d93615e94ec31"
        }'
    curl -X GET $CLOUDANTURL/planets/1

#### Deleting a document using its revision id, Verify by listing document with _id 1, Verify by listing document with _id 1

    curl -X DELETE $CLOUDANTURL/planets/1?rev=2-c82d0b7f587b2a90f7cb6f6d963b5486
    curl -X GET $CLOUDANTURL/planets/1

#### creating a database named diamonds and populating it with following code

    curl -X PUT $CLOUDANTURL/diamonds
    curl -X PUT $CLOUDANTURL/diamonds/1 -d '{
        "carat":0.31, "cut":"Ideal", "color": "J", "clarity": "SI2", "depth": 62.2, "table": 54, "price": 339
    }'
    curl -X PUT $CLOUDANTURL/diamonds/2 -d '{
        "carat":0.2, "cut":"Premium", "color": "E", "clarity": "SI2", "depth": 60.2, "table": 62, "price": 351
    }'
    curl -X PUT $CLOUDANTURL/diamonds/3 -d '{
        "carat": 0.32, "cut": "Premium", "color": "E", "clarity": "I1", "depth": 60.9, "table": 58, "price": 342
    }'
    curl -X PUT $CLOUDANTURL/diamonds/4 -d '{
        "carat": 0.3, "cut": "Good", "color": "J", "clarity": "SI1", "depth": 63.4, "table": 54, "price": 349
    }'
    curl -X PUT $CLOUDANTURL/diamonds/5 -d '{
        "carat": 0.3, "cut": "Good", "color": "J", "clarity": "SI1", "depth": 63.8, "table": 56, "price": 347
    }'

#### Query for diamond with _id 1, warning will show to create index

    curl -X POST $CLOUDANTURL/diamonds/_find -H"Content-Type: application/json" \
    -d'{ 
        "selector":
            {
                "_id":"1"
            }
        }'

#### Query for diamonds with carat size of 0.3, warning will show to create index

    curl -X POST $CLOUDANTURL/diamonds/_find -H"Content-Type: application/json" \
    -d'{ "selector":
            {
                "carat":0.3
            }
        }'

#### Query for diamonds with price more than 345, warning will show to create index

    curl -X POST $CLOUDANTURL/diamonds/_find -H"Content-Type: application/json" \
    -d'{ "selector":
            {
                "price":
                    {
                        "$gt":345
                    }
            }
        }'

#### creating an index on key price, after creating index running previous command, now no warning will come as index is created

    curl -X POST $CLOUDANTURL/diamonds/_index -H"Content-Type: application/json" \
    -d'{
        "index": {
            "fields": ["price"]
        }
    }'

#### Updating price of diamond with id 2 to 352, _rev will change after each update

    curl -X PUT $CLOUDANTURL/diamonds/2 -d '{ 
    "_id": "2",
        "_rev": "1-bb99032cde872d889f41bb4069e23675",
        "carat": 0.2,
        "cut": "Premium",
        "color": "E",
        "clarity": "SI2",
        "depth": 60.2,
        "table": 62,
        "price": 352
        }'

#### installing couchimport as 'couchimport' and 'couchexport' tools are used to move data in and out of Cloudant database in terminal

    npm install -g couchimport

#### Verify by following command

    couchimport --version

#### setting environment varible CLOUDANTURL to cloudant url from service credentials of cloudant database

    CLOUDANTURL="https:/apikey-v2-regv2h1x1kh8i2gbxtopk25e6p57pm1gxi2hylix9ep:1297f4ec25c01324505d4ef7bbf56963@\
    ccc71206-2052-403f-9553-589b0a08ac4e-bluemix.cloudantnosqldb.appdomain.cloud"

#### Exporting data from diamonds database into csv format

    couchexport --url $CLOUDANTURL --db diamonds --delimiter ","

#### Exporting data from diamonds database into json format (one document per line)

    couchexport --url $CLOUDANTURL --db diamonds --type jsonl

#### Exporting data from diamonds database into json format and save to a file named diamonds.json

    couchexport --url $CLOUDANTURL --db diamonds --type jsonl > diamonds.json

#### Exporting data from diamonds database into csv format and save to a file named diamonds.csv

    couchexport --url $CLOUDANTURL --db diamonds --delimiter "," > diamonds.csv

#### final assignment in cloudant NoSQL databases
#### first creating Non-partitioned database named movies in cloudant database
#### then exporting data of movie.json file into Cloudant database movies using terminal

    curl -XPOST $CLOUDANTURL/movies/_bulk_docs -Hcontent-type:application/json -d @movie.json

#### Creating an index for director key on movies database using HTTP API

    curl -X POST $CLOUDANTURL/movies/_index -H"Content-Type: application/json" \
    -d'{
        "index": {
            "fields": ["director"]
        }
    }'

#### query to find all movies directed by Richard Gage using HTTP API

    curl -X POST $CLOUDANTURL/movies/_find -H"Content-Type: application/json" \
    -d'{ "selector":
            {
                "Director":"Richard Gage"
            }
        }'

#### Creating an index for title key on movies database using HTTP API

    curl -X POST $CLOUDANTURL/movies/_index -H"Content-Type: application/json" \
    -d'{
        "index": {
            "fields": ["title"]
        }
    }'

#### query to list only year and director keys for movie named Top Dog using HTTP API

    curl -X POST $CLOUDANTURL/movies/_find -H"Content-Type: application/json" \
    -d'{ "selector":
            {
                "title":"Top Dog"
            },
        "fields": ["year","Director"]
        }'

#### Exporting data from movies database into a file named movies.json

    couchexport --url $CLOUDANTURL --db movies --type jsonl > movies.json