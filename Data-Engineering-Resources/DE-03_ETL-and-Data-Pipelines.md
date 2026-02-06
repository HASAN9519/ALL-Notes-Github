## ETL

- Two different approaches used to convert raw data into analytics-ready data, One approach is Extract, Transform and Load known as (ETL) process, other approach is Extract, Load and Transform known as (ELT) process.
- ETL processes apply to data warehouses and data marts. ELT processes apply to data lakes where data is load in its raw format and transformed on demand by requesting/calling application. Both ETL and ELT extract data from source systems, move data through data pipeline and store data in destination systems.
- ETL (Extract, Transform, Load) is an acronym for an automated data pipeline engineering methodology where data is acquired and prepared for subsequent use in analytics environment such as data warehouse or data mart. Extraction process obtains data from one or more sources. Transformation process wrangles data into a format that is suitable for its destination and its intended use, final Loading process takes transformed data and loads it into its new environment to visualize, exploration, further transformation and modelling.

## ELT

- ELT processes are used for cases where flexibility, speed and scalability are important. Cloud-based analytics platforms are ideally suited for handling Big Data and ELT processes in a cost-efficient manner. ELT is an emerging trend mainly because cloud platform technologies are enabling it.

## Difference Between ETL and ELT

- Key differences between ETL and ELT are location where transformation takes place, flexibility, Big Data support and time-to-insight.
- One of factors driving evolution from ETL to ELT is demand to release raw data to a wider user base for enterprise.
- Conventional ETL still has many applications. ELT is more flexible than ETL enabling end-users to perform ad-hoc, self-serve data analytics in real-time.

## Data Extraction Techniques

- Raw data sources are archived text and images from paper documents, PDFs and web pages including text, tables, images and links. SQL, NoSQL, web scraping and APIs are important techniques for extracting data, medical imaging devices and biometric sensors used to acquire data.

## Data Transformation Techniques

- Data transformation is generally about formatting data to suit needs of intended application. Common transformation techniques include typing, structuring, normalizing, aggregating and cleaning. Schema-on-write is conventional approach used in ETL pipelines and Schema-on-read relates to modern ELT approach. Ways of losing information in transformation processes include filtering, aggregation, using edge computing devices and lossy data compression.

## Data Loading Techniques

- Some data loading techniques are scheduled, on-demand and incremental. Data can be loaded in batches or it can be streamed continuously into its destination. Servers can push data to subscribers as it becomes available and Clients can initiate pull requests for data from servers. Can employ parallel loading to boost loading efficiency of large volumes of data.

## ETL Workflows as Data Pipelines 

- An ETL workflow is a well thought out process that is carefully engineered to meet technical and end-user requirements.
- Overall accuracy of ETL workflow has been a more important requirement than speed but efficiency is an important factor in minimizing resource costs. To boost efficiency data is fed through a data pipeline in smaller packets. While one packet is being extracted, an earlier packet is being transformed and another is being loaded. In this way data can keep moving through workflow without interruption. Any remaining bottlenecks within pipeline handled by parallelizing slower tasks.
- In conventional ETL pipelines, data is processed in batches usually on a repeating schedule that ranges from hours to days apart, for example records accumulating in an Online Transaction Processing System (OLTP) can be moved as a daily batch process to one or more Online Analytics Processing (OLAP) systems where subsequent analysis of large volumes of historical data is carried out.
- Batch processing intervals need not be periodic and can be triggered by events such as when source data reaches a certain size or when an event of interest occurs and is detected by a system such as an intruder alert or on-demand with web apps such as music or video streaming services. 

## Staging Areas

- ETL pipelines are frequently used to integrate data from disparate and usually siloed systems within enterprise. These systems can be from different vendors, locations and divisions of company which can add significant operational complexity, as example a cost accounting OLAP system might retrieve data from distinct OLTP systems utilized by separate payroll, sales and purchasing departments.

## ETL Workflows as DAGs 

- ETL workflows can involve considerable complexity. By breaking down details of workflow into individual tasks and dependencies between those tasks, one can gain better control over that complexity. Workflow orchestration tools such as Apache Airflow represents workflow as a directed acyclic graph (DAG).
- Airflow tasks can be expressed using predefined templates called operators. Popular operators include Bash operators for running Bash code and Python operators for running Python code which makes them extremely versatile for deploying ETL pipelines and many other kinds of workflows into production. 

## Popular ETL tools 

- There are many ETL tools available today, Modern enterprise grade ETL tools will typically include following features: 
    - Automation: Fully automated pipelines 
    - Ease of use: ETL rule recommendations 
    - Drag-and-drop interface: “o-code” rules and data flows 
    - Transformation support: Assistance with complex calculations 
    - Security and Compliance: Data encryption and HIPAA, GDPR compliance

- Some well-known ETL tools are listed below along with some of their key features, Both commercial and open-source tools are included in list:
    - Talend Open Studio Supports big data, data warehousing and profiling Includes collaboration, monitoring and scheduling, Drag-and-drop GUI for ETL pipeline creation, Automatically generates Java code, Integrates with many data warehouses and Open-source.
    - AWS Glue has ETL service that simplifies data prep for analytics, Suggests schemas for storing data, Create ETL jobs from AWS Console.
    - IBM InfoSphere DataStage has a data integration tool for designing, developing and running ETL and ELT jobs, data integration component of IBM InfoSphere Information Server, Drag-and-drop graphical interface, Uses parallel processing and enterprise connectivity in a highly scalable platform.
    - Alteryx has Self-service data analytics platform, Drag-and-drop accessibility to ETL tools, No SQL or coding required to create pipelines.
    - Apache Airflow and Python has Versatile “configuration” as code data pipeline platform, Programmatically author, schedule and monitor workflows, Scales to Big Data, Integrates with cloud platforms.
    - Pandas Python library has Versatile and popular open-source programming tool, Based on data frames having table-like structures, Great for ETL, data exploration and prototyping, Doesn’t readily scale to Big Data.

## Data Pipelines

- Purpose of a data pipeline is to move data from one place or from to another, can visualize data flowing through a pipeline as a series of data packets flowing in and out one by one. Latency and throughput are key design considerations for data pipelines. Latency is total time it takes for a single packet of data to pass through pipeline, latency is sum of individual times spent during each processing stage within pipeline thus overall latency is limited by slowest process in pipeline. Throughput refers to how much data can be fed through pipeline per unit of time. Processing larger packets per unit of time increases throughput. Use cases for data pipelines are many and range from simple copy-and-paste-like data backups to online video meetings.

## Key Data Pipeline Processes

- In addition to extraction, transformation and loading, data pipeline processes include scheduling, triggering, monitoring, maintenance and optimization. Pipeline monitoring considerations include tracking latency, throughput, resource utilization and failures, unbalanced or varying loads can be mitigated by introducing parallelization and I/O buffers at bottlenecks.

## Batch Versus Streaming Data Pipeline Use Cases

- Batch pipelines extract and operate on batches of data, Batch processing is used when accuracy is critical or there is no need for most recent data.
- Use cases for batch data pipelines include: Periodic data backups, transaction history loading, Processing of customer orders and billing, modelling of Data on slowly varying data, mid to long range sales forecasting, weather forecasting, Analysis of historical data, Diagnostic image processing.
- Streaming data pipelines ingest data packets one-by-one in rapid succession, Streaming pipelines are used when most current data is needed.
- Use cases for streaming data pipelines include: Watching movies and listening to music, Social media feeds and sentiment analysis, Fraud detection, User behavior analysis, targeted advertising, Stock market trading, Real time product pricing, Recommender systems.
- Micro-batch processing can be used to simulate real-time data streaming.
- Lambda architecture is a hybrid architecture designed for handling Big Data, combines batch and streaming data pipeline methods. Historical data is delivered in batches to batch layer and real-time data is streamed to a speed layer. These two layers are then integrated in serving layer, data stream is used to fill in latency gap caused by processing in batch layer.
- Lambda can be used in cases where access to earlier data is required but speed is also important. A downside to this approach is complexity involved in design.

## Data Pipeline Tools and Technologies

- Modern enterprise grade data pipeline tools include technologies such as transformation support, drag-and-drop GUIs and security and compliance features. Pandas, Vaex and Dask are useful open-source Python libraries for prototyping and building data pipelines. Apache Airflow and Talend Open Studio allows to programmatically author, schedule and monitor Big Data workflows. Panoply is specific to ELT pipelines. Alteryx and IBM InfoSphere DataStage can handle both ETL and ELT workflows. Stream-processing technologies include Apache Kafka, IBM Streams, SQLstream and Apache Spark.

## Apache Airflow Overview

- Apache Airflow is a platform to programmatically author, schedule and monitor workflows, five main features of Airflow are its use of Python, its intuitive and useful user interface, extensive plug-and-play integrations, ease of use and it is open source. Apache Airflow is scalable, dynamic, extensible and lean. Defining and organizing machine learning pipeline dependencies with Apache Airflow is one of its common use cases.

## Advantages of Using Data Pipelines as DAGs in Apache Airflow

- DAGs are workflows defined as Python code in Apache Airflow. Tasks which are nodes in DAG are created by implementing Airflow's built-in operators.
- Pipelines are specified as dependencies between tasks which are directed edges between nodes in DAG. Airflow Scheduler schedules and deploys DAGs.
- Key advantage of Apache Airflow's approach to representing data pipelines as DAGs is they are expressed as code making data pipelines more maintainable, Version able as Code revisions can be done by a version control system such as Git, testable and collaborative.

## Apache Airflow UI

- Apache Airflow has a rich UI that simplifies working with data pipelines, can visualize DAG in several informative ways including both graph and tree mode, review Python code that originally defined DAG, analyze duration of each task in DAG over multiple runs, select metadata for any task instance.

## Build DAG Using Airflow

- An Airflow pipeline is a Python script that instantiates an Airflow DAG object. Key components of a DAG definition file are library imports, DAG arguments, DAG and task definitions and task pipeline specification, can specify a schedule interval in DAG definition for running repeatedly by setting schedule interval parameter; tasks are instantiated operators, imported from Apache Airflow operators module.

## Airflow Monitoring and Logging

- Airflow logs can be saved into local file systems and send them to cloud storage, search engines and log analyzers; Airflow recommends sending production deployment logs to be analyzed by Elasticsearch or Splunk; can view DAGs and task events easily With Airflow’s UI; three types of Airflow metrics are counters, gauges and timers; counters which are metrics that will always be increasing such as total counts of successful or failed tasks, Gauges are metrics that may fluctuate for example number of currently running tasks or DAG bag sizes, timers are metrics related to time duration for instance time to finish a task or time for a task to reach a success or failed state; Airflow recommends to send production deployment metrics for analysis by Prometheus via StatsD.

## Distributed Event Streaming Platform Components

- An event stream represents entities status updates over time; Common event formats include primitive data types, key-value and key-value with a timestamp; ESP is needed especially when there are multiple event sources and destinations; main components of ESP are Event broker, Event storage, Analytic and Query Engine; Event broker is designed to receive and consume events as it is core component of ESP, Event Storage is used for storing events being received from event sources as event destinations don't need to synchronize with event sources and stored events can be retrieved at will, Analytic and Query Engine is used for querying and analyzing stored events; Apache Kafka is most popular open source ESP; popular ESPs include Apache Kafka, Amazon Kinesis, Apache Flink, Apache Spark, Apache Storm and more.

## Apache Kafka Overview

- Apache Kafka is one of most popular open source ESPs; Common Kafka use cases include user-activity tracking, metrics and logs integrations, financial transaction processing; Apache Kafka is a highly scalable and reliable platform that stores events permanently; popular Kafka-based ESP service providers include Confluent Cloud, IBM Event Streams and Amazon Managed Streaming.

## Building Event Streaming Pipelines using Kafka

- Core components of Kafka are Brokers which is dedicated server to receive, store, process and distribute events; Topics are containers or databases of events; Partitions Divide topics into different brokers; Replications Duplicate partitions into different brokers; Producers used by Kafka client applications to publish events into topics; consumers are Kafka client applications subscribed to topics and read events from them; Kafka-topics CLI manages topics, Kafka-console-producer CLI manages producers, Kafka-console-consumer manages consumers.

## Kafka Streaming Process

- Kafka Streams API is a simple client library to help data engineers with data processing in event streaming pipelines; A stream processor receives, transforms and forwards streams; Kafka Streams API is based on a computational graph called a stream processing topology And in there each node is a stream processor while edges are I/O streams; there are two special types of processors in topology: source processor and sink processor.