## Definition of Big Data

- Big Data is digital trace that gets generated in this digital era. Big Data is a high-volume, high-velocity and high-variety information asset that demands cost-effective and innovative tools for processing. Core features of Big Data are 4 V’s: Velocity, Volume, Variety and Veracity. Big Data creates a fifth V which is Value, when data is collected, stored and processed correctly.

## Impact of Big Data

- Big Data is everywhere and is being collected and used to drive business decisions and influence people’s lives. Virtual personal assistants like Siri on Apple devices or Alexa on Amazon devices use Big Data to devise answers to infinite number of questions end-users may ask. Google Now impacts people through using Big Data to forecast future needs and behavior. Internet of Things (IoT) devices continually generate massive volumes of data. Big Data analytics helps companies gain insights from data collected by IoT devices.

## Parallel Processing, Scaling and Data Parallelism

- Big Data requires parallel processing on account of massive volumes of data that are too large to fit on any one computer. Linear processing is sequential while Parallel processing works on multiple instructions at same time, Parallel processing has significant advantages over linear processing and it is best suited for processing Big Data.

- Parallelism in Big Data is parallelization across multiple processors in parallel computing environments. It focuses on distributing data across different nodes operating on data in parallel.

- Horizontal scaling or scaling out is strategy to increase capacity of a single node as a means of increasing capacity. Embarrassingly parallel calculations are kinds of workloads that can easily be divided and run independently of one another. If any one process fails, it has no impact on others and can simply be re-run.

- Fault tolerance is property that enables a system to continue operating properly in event of failure of some of its components.

## Big Data Tools and Ecosystem

- There are six main components of Big Data tools, namely: Data technologies, Analytics and visualization, Business Intelligence, Cloud providers, NoSQL databases and Programming tools. Each tooling category plays a very specific and critical role in Big Data life cycle. Several major commercial and open source vendors provide tools and support for Big Data processing.

## Open Source and Big Data

- Open source runs world of Big Data. Open source projects are free and completely transparent, biggest component of big data is by far Hadoop project including MapReduce, HDFS and YARN. Open source has big data tools like Apache Hive and Apache Spark.

## Big Data Use Cases

- Companies are relying on Big Data to differ themselves from competition and are several ways in which retail, insurance, telecom, manufacturing, automotive and finance industries are leveraging Big Data to reduce cost, increase customer satisfaction and make competitive business decisions.

## Introduction to Hadoop

- Hadoop is an open-source framework for Big Data. It is used for processing massive data in distributed file systems that are linked together. It allows for running applications on clusters.

- A cluster is a collection of computers working together at same to time to perform tasks.

- Hadoop is not a database but an ecosystem that can handle processes and jobs in parallel or concurrently. Hadoop is optimized to handle massive quantities of data which could be Structured, tabular data, Unstructured data such as images and videos or Semi-structured data using relatively inexpensive computers.

- Core components of Hadoop are HDFS, MapReduce, YARN(Yet Another Resource Negotiator) and Hadoop Common. Drawbacks of Hadoop outweighed benefits.

- YARN prepares RAM and CPU for Hadoop to run data in batch, stream, interactive and graph processing.

## Intro to MapReduce

- MapReduce is a framework used in Parallel Computing, it contains two major tasks: Map and Reduce.

- Map processes data into Key Value pairs, sorts and organizes data.

- Reducer aggregates and computes a set of results and produces a final output. It is flexible for all data types like structured and unstructured data and can be applied to multiple industries such as social media, entertainment and many more.

## Hadoop Ecosystem

- Apart from core components of Hadoop, Hadoop Common refers to common utilities and libraries that support other Hadoop modules.

- Hadoop Distributed File System (HDFS) stores data collected from ingestion and distributes data across multiple nodes.

- MapReduce is used for making Big Data manageable by processing them in clusters.

- Yet Another Resource Negotiator (YARN) is resource manager across clusters.

- Extended Hadoop Ecosystem consists of libraries or software packages that are commonly used with or installed on top of Hadoop core. Hadoop ecosystem is made up of components that support one another for Big Data processing.

- Four main stages of Hadoop Ecosystem are Ingest, Store, Process & Analyze and Access. When data is received from multiple sources, Flume and Sqoop are responsible for ingesting data and transferring them to Storage component, HDFS and HBase. Then data is distributed to a MapReduce framework like Pig and Hive to process & analyze data and processing is done by parallel computing, tools like Hue are used to access refined data.

- Ingesting is first stage of Big Data processing, Flume is a distributed service that collects, aggregates and transfers Big Data to storage system.

- Flume has a simple and flexible architecture based on streaming data flows and uses a simple extensible data model that allows for online analytic application.

- Sqoop is an open-source product designed to transfer bulk data between relational database systems and Hadoop, Sqoop looks in relational database and summarizes schema. It then generates MapReduce code to import and export data as needed.

- Sqoop quickly develop any other MapReduce applications that use records that Sqoop stored into HDFS.

- HBase is a column-oriented non-relational database system that runs on top of HDFS. It provides real time wrangling access to Hadoop file system.

- HBase uses hash tables to store data in indexes and allow for random access of data which makes lookups faster.

- Cassandra is a scalable, NoSQL database designed to have no single point of failure.

- In Analyze Data stage, Pig is used for analyzing large amounts of data. Pig is a procedural data flow language and a procedural programming language that follows an order and set of commands. Hive is used mainly for creating reports and operates on server side of a cluster.

- Hive is a declarative programming language which means it allows users to express which data they wish to receive, final stage is Access data where users have access to analyzed and refined data. At this stage tools like Impala are often used.

- Impala is a scalable system that allows non-technical users to search and access data in Hadoop, don't need to be skilled in programming to use Impala And Hue is another tool of choice at this stage.

- Hue is an acronym for Hadoop user experience and allows to upload, browse and query data, can run Pig jobs and workflow in Hue. Hue also provides a SQL editor for several query languages like Hive and MySQL.

## HDFS

- Key HDFS benefits include its cost efficiency, scalability, data storage expansion and data replication capabilities.

- A block is minimum size of a file.

- A node is a single system which is responsible for storing and processing data.

- HDFS has two types of nodes: Primary node known as name node that regulates file access to clients and maintains, manages & assigns tasks to secondary node also known as a data node. There can be hundreds of data nodes in HDFS that manage storage system. They perform read and write requests at instruction of name node.

- When performing operations like read and write, it is important that name node maximizes performance by choosing data nodes closest to themselves, This could be by choosing data nodes on same rack or in nearby racks. This is called rack awareness. A rack is collection of about forty to fifty data nodes using same network switch.

- Rack awareness helps reduce network traffic and improve cluster performance.

- Replication creates a copy of data blocks for backup purposes. HDFS enables write once, read many operations.

## HIVE

- Hive is a data warehouse software for reading, writing and managing datasets. Although Hive works very similarly to traditional RDBMS, they are slightly different, three main parts of Hive architecture are Hive Client, Hive Services & Hive Storage and Computing.

## HBASE

- HBase is a column-oriented non-relational database management system that runs on HDFS. HBase columns are defined by column families. HBase is linearly scalable, highly efficient and provides an easy-to-use Java API for client access.

- A key difference between HDFS and HBase is that HBase allows dynamic changes compared to rigid architecture of HDFS. HBase architecture consists of HMaster, Region servers, Region, Zookeeper and HDFS.

## Apache Spark

- Spark is an open-source in-memory application framework for distributed data processing and iterative analysis on massive data volumes.

- Distributed computing is a group of computers or processors working together behind scenes. Both distributed systems and Apache Spark are inherently scalable and fault-tolerant. Apache Spark keeps a large portion of data required in-memory and avoids expensive and time-consuming disk I/O. Distributed computing utilizes each processors own memory where Parallel computing utilizes shared memory.

## Functional Programming Basics

- Functional programming follows a declarative programming model that emphasizes "What" instead of "how to" and uses expressions. Lambda functions or operators are anonymous functions that enable functional programming. Spark parallelizes computations using lambda calculus. All functional Spark programs are inherently parallel.

## Parallel Programming using Resilient Distributed Datasets

- Can create Resilient Distributed Datasets(RDD) using an external or local file from a Hadoop supported file, a collection or another RDD. RDDs are immutable and always recoverable. Parallel programming is simultaneous use of multiple compute resources to solve a computational task. RDDs can persist or cache datasets in memory across operations which spades iterative operations. RDDs enable Apache Spark to reconstruct transformations.

## Scale out / Data Parallelism in Apache Spark

- Apache Spark architecture consists of three main pieces: components data, compute input and management. Three components of Spark architecture are: Data storage, cluster management framework and APIs. Fault-tolerant Spark Core base engine performs large-scale parallel and distributed data processing, manages memory, schedules tasks and houses APIs that define RDDs.

- Spark driver program communicates with cluster and then distributes RDDs among worker nodes, data from a Hadoop file system flows into compute interface or API which then flows into different nodes to perform distributed or parallel tasks.

## Data Frames and SparkSQL

- SparkSQL is a Spark module for structured data processing. Spark SQL provides a programming abstraction called DataFrames, also act as a distributed SQL query engine. DataFrames are conceptually equivalent to a table in a relational database or a data frame in R/Python but with richer optimizations.

## RDDs in Parallel Programming and Spark

- RDDs are Spark's primary data abstraction partitioned across nodes of cluster, Spark uses DAGS to enable fault tolerance.
- When a node goes down, Spark replicates DAG and restores node. Transformations leave existing RDDs intact and create new RDDs based on transformation function. Transformations undergo lazy evaluation, meaning they are only evaluated when driver function calls an action.

## Data-frames and Datasets

- A dataset is a distributed collection of data that provides combined benefits of both RDDs and SparkSQL. Datasets consist of strongly typed JVM objects. Datasets use DataFrame type safe capabilities and extend object-oriented API capabilities, Datasets work with both Scala and Java.

## Catalyst and Tungsten

- Catalyst is Spark SQL built-in rule-based query optimizer. Catalyst performs analysis, logical optimization, physical planning and code generation.

- Tungsten is Spark built-in cost-based optimizer for CPU and memory usage that enables cache-friendly computation of algorithms and data structures. Tungsten manages memory explicitly and does not rely on JVM object model or garbage collection & places intermediate data in CPU registers.

## ETL with DataFrames

- Basic DataFrame operations are reading, analysis, transformation, loading and writing, can use a Pandas DataFrame in Python to load a dataset, can apply print schema, select function or show function for data analysis.

- For transform tasks, keep only relevant data and apply functions such as filters, joins, column operations, grouping & aggregations and other functions.

- Spark performs Extract, Transform and Load operations in following order Read, Analyze, Transform, Load and Write.

## Real-world usage of SparkSQL

- Spark modules for structured data processing can run SQL queries on Spark DataFrames and are usable in Java, Scala, Python and R. Spark SQL supports both temporary views and global temporary views. A global temporary view exists within general Spark application and shareable across different Spark sessions.

- A temporary view has local scope which means that view exists only within current Spark session on current node, can use DataFrame function or a SQL Query plus Table View for data aggregation. Spark SQL supports Parquet files, JSON datasets and Hive tables.

## Apache Spark Architecture

- Spark Architecture has driver and executor processes, coordinated by Spark Context in driver, driver creates jobs and splits them into tasks which can be run in parallel in executors.

- An executor is a process running multiple threads to perform work concurrently for cluster.

- Stages are a set of tasks that are separated by a data shuffle. Shuffles are costly as it require data serialization, disk and network I/O. Driver can be run in either or both Client or Cluster mode. Client Mode connects driver outside cluster while Cluster Mode runs driver in cluster.

## Overview of Apache Spark Cluster Modes

- Cluster managers acquire resources and run as an abstracted service outside application.

- Spark can run on Spark Standalone, Apache Hadoop YARN, Apache Mesos or Kubernetes cluster managers with specific set-up requirements. Choosing a cluster manager depends on data ecosystem and factors such as ease of configuration, portability, deployment or data partitioning needs. Spark can run in local mode, useful for testing or debugging an application.

## Running an Apache Spark Application

- Spark-submit is a unified interface to submit Spark Application, no matter of what cluster manager or application language. Mandatory options include telling Spark which cluster manager to connect to, other options set driver deploy mode or executor resourcing.

- To manage dependencies, application projects or libraries must be accessible for driver and executor processes for example by creating a Java or Scala uber-JAR.

- Spark Shell simplifies working with data by automatically initializing SparkContext and SparkSession variables and providing Spark API access.

## Using Apache Spark on IBM Cloud

- Running Spark on IBM Cloud provides enterprise security and easily ties in IBM big data solutions for AIOps, IBM Watson and IBM Analytics Engine.

- AIOps tools such as IBM Watson use AI to automate or enhance IT operations. Spark’s big data processing capabilities work well with AIOps tools using machine learning to identify events or patterns and help report or fix issues. IBM Spectrum Conductor manages and deploys Spark resources dynamically on a single cluster and provides enterprise security.

- IBM Watson focuses on Spark’s machine learning capabilities by creating production-ready environments for AI.

- IBM Analytics Engine separates storage and compute to create a scalable analytics solution alongside Spark’s data processing capabilities.

## Setting Apache Spark Configuration

- Can set Spark configuration using properties (to control application behavior), environment variables (to adjust settings on a per-machine basis) or logging properties (to control logging outputs).

- Spark property configuration follows a precedence order with highest being configuration set programmatically, then spark-submit configuration and lastly configuration set in spark-defaults.conf file. Static configuration should be used for values that don’t change from run to run or properties related to application such as application name.

- Dynamic configuration should be used for values that change or need tuning when deployed such as master location, executor memory or core settings.

## Running Spark on Kubernetes

- Kubernetes runs containerized applications on a cluster, managing distributed systems such as Spark in a more flexible, resilient way. Kubernetes can run locally as a deployment environment, useful for trying out changes before deploying to clusters in cloud.

- Kubernetes can be hosted on private or hybrid clouds, set up using existing tools to bootstrap clusters or turnkey options from certified providers. While Spark can be launched in client or cluster mode, in Client mode executors must be able to connect with driver and pod cleanup settings are required.

- Precedence order to perform before running application in spark is programmatic configuration, set spark-submit configuration and set configurations in the spark-defaults.conf file.

## Apache Spark User Interface

- Jobs tab displays application’s jobs including job status and Stages tab reports state of tasks within a stage.

- Storage tab shows size of RDDs or DataFrames that persisted to memory or disk. Environment tab information includes any environment variables and system properties for Spark or JVM.

- Executors tab displays a summary that shows memory and disk usage for any executors in use. If application runs SQL queries, select SQL tab and Description hyperlink to display query’s details.

## Monitoring Application Progress

- Spark application workflow includes jobs created by SparkContext in driver program, jobs in progress running as tasks in executors and completed jobs transferring results back to driver or writing to disk.

- Spark application UI centralizes critical information including status information, can quickly identify failures then drill down to lowest levels of application to discover their root causes.

## Debugging Apache Spark Application Issues

- Common reasons for application failure on a cluster include user code, system & application configurations, missing dependencies, improper resource allocation and network communications.

- Application log files will often show complete details of a failure which are located in Spark installation directory.

## Understanding Memory Resources

- Spark has configurable memory for executor and driver processes. Executor memory and Storage memory share a region that can be tuned as needed. Caching data can help improve application performance.

## Understanding Processor Resources

- Spark assigns CPU cores to driver and executor processes during application processing. Executors process tasks in parallel according to number of cores available or assigned by application, can specify total memory and CPU cores that workers can use If Spark Standalone cluster manager used.