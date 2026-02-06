## Data Warehouse Overview

- A data warehouse is a system that aggregates data from one or more sources into a single consistent data store to support data analytics. Data warehouses support data mining, AI, machine learning, OLAP and front-end reporting.
- Data warehouses and BI help organizations improve data quality, speed business insights and improve decision-making all of which can result in competitive gains.

## Popular Data Warehouse Systems

- Data warehouse systems can include appliances, exist on-premises, exist in cloud or use a combination of these deployment options. Popular appliance data warehouse system solutions are Oracle Exadata, IBM Netezza. Popular cloud-based data warehouse systems providers are Amazon RedShift, Snowflake, Google BigQuery. Popular vendors that provide both on-premises and cloud-based data warehouse systems are Microsoft Azure Synapse Analytics, Teradata Vantage, IBM Db2 Warehouse, Vertica, Oracle Autonomous Data Warehouse.

## Selecting a Data Warehouse System

- Businesses evaluate data warehouse systems based on features & capabilities, compatibility & implementation, ease of use, required skills, support quality & availability and multiple cost considerations.
- An organization might need a traditional on-premises installation to adhere to data security and privacy requirements.
- Public cloud sites offer organizations benefits of economies of scale including powerful compute power and scalable storage resulting in flexible price-for-performance options, select a data warehouse system considering total cost of ownership including infrastructure, compute and storage, data migration, administration, data maintenance costs in calculations.

## IBM Db2 Warehouse

- IBM Db2 Warehouse is a cloud-ready, highly flexible data warehouse platform. Key features of IBM Db2 Warehouse include speed, scalability, automated schema generation and built-in machine learning. Use cases include data integration and rapid development of data marts and Db2 Warehouse integrates with JDBC, Apache Spark, Python and R Studio.

## Data Marts Overview

- Data Mart is an isolated part of larger enterprise data warehouse that is specifically built to serve a particular business function, purpose or community of users, data mart is designed to provide specific, timely and rapid support for making tactical decisions and typically has a star or snowflake schema, unlike an OLTP database, an OLAP data mart stores clean and validated data and accumulates historical data, can categorize data marts in terms of their dependence on enterprise data warehouse.
- A data mart can be completely dependent on data warehouse, a completely independent & standalone mini data warehouse or a hybrid of two.

## Data Lakes Overview

- A data lake is a storage repository that can store large amounts of structured, semi-structured and unstructured data in their raw or native format, classified and tagged with metadata, do not need to define structure and schema of data before loading into data lake.
- Data lakes offer several benefits such as storage for all types of data, scalable storage capacity, time savings and flexible data reuse, data lakes can be used as a kind of self-serve staging area for a variety of use cases including machine learning development and advanced analytics.

## Overview of Data Warehouse Architectures

- An architectural model for a general data warehousing platform includes data sources, ETL pipelines, optional staging and sandbox areas, an enterprise data warehouse repository, optional data marts and analytics & business intelligence tools.
- Companies can modify general enterprise data warehouse architecture to suit their analytics requirements. Vendors offer proprietary reference architecture based on general model which they test for interoperability among components. An IBM enterprise data warehouse solution combines InfoSphere with Db2 Warehouse and Cognos Analytics.

## Cubes, Roll ups, Materialized Views and Tables

- A data cube represents a star or snowflake schema’s dimensions as coordinates plus a fact from schema to populate its cells with values. Many operations can be applied to data cubes such as: drilling down into hierarchical dimensions, slicing, dicing and rolling up.
- Materialized views can be used to replicate data or to precompute expensive queries, modern enterprise data warehouse tools such as Oracle and Db2 allows to automatically keep material views up-to-date.

## Grouping Sets in SQL

- GROUPING SETS clause is used in conjunction with GROUP BY clause to easily summarize data by aggregating a fact over as many dimensions as user like.
- SQL GROUP BY clause summarizes an aggregation such as SUM or AVG over distinct members or groups of a categorical variable or dimension, can extend functionality of GROUP BY clause using SQL clauses such as CUBE and ROLLUP to select multiple dimensions and create multi-dimensional summaries. These two clauses also generate grand totals like a report that might see in a spreadsheet application or an accounting style sheet.
- Just like CUBE and ROLLUP, SQL GROUPING SETS clause aggregate data over multiple dimensions but does not generate grand totals.

## Facts and Dimensional Modeling

- Business data falls into two categories: Facts and dimensions. Facts are usually numerical measures of business processes such as sale amounts in dollars. Dimensions such as sold by and store ID categorize facts such as who sold product, when and which store it was sold from.
- Dimensions are categorical variables that provide context for facts and are used for filtering, grouping and labeling.
- Facts and dimension tables are linked together by foreign and primary keys in database.

## Data Modeling using Star and Snowflake Schemas

- Facts and dimension tables together with foreign and primary keys are used to form star and snowflake modeling schemas.
- Design considerations for data modeling with star schema include identifying a business process, its granularity and its facts & dimensions. 
- Snowflake schemas can be described as normalized star schemas where normalization involves separating dimension tables into individual tables defined by levels or hierarchies of parent dimension and reduces storage footprint.
- Star schemas are optimized for reads and are widely used for designing data marts whereas snowflake schemas are optimized for writes and are widely used for transactional data warehousing.
- A star schema is a special case of a snowflake schema in which all hierarchical dimensions have been denormalized or flattened.

## Staging Areas for Data Warehouses

- A staging area acts as a bridge between data sources & target system and are mainly used to integrate disparate data sources in data warehouses.
- Staging areas can be implemented as a set of flat files in a directory and managed with scripts or tables in a database and decouple data processing from source systems and minimize risk of data corruption. Staging areas are often transient but can be held for archiving or troubleshooting purposes.

## Verify Data Quality

- Data verification includes checking data for accuracy, completeness, consistency and currency. Data verification is about managing data quality, enhancing data reliability and maximizing data value. Determining how to resolve and prevent bad data can be a complex and iterative process. Enterprise-grade tools such as IBM InfoSphere Information Server for Data Quality can perform data verification in a unified environment.

## Populating a Data Warehouse

- Populating an enterprise data warehouse includes initial creation of fact & dimension tables and their relations & loading of clean data into tables.
- Populating enterprise data warehouse is an ongoing process that starts with an initial load followed by periodic incremental loads. Fact tables are dynamic and require frequent updating while dimension tables are more static and don’t change often, can automate incremental loading and periodic maintenance of data warehouse using scripting or built-for-purpose data pipeline tools.

## Querying Data

- CUBE and ROLLUP summaries on materialized views provide powerful capabilities for quickly querying and analyzing data in data warehouses. CUBE and ROLLUP operations generate kinds of summaries grouped by dimensions that management often requests, can denormalize star schemas using joins to bring together human-interpretable facts and dimensions in a single materialized view, can create staging tables from materialized views which incrementally refresh during off-peak hours.

## Introduction to Analytics and Business Intelligence (BI) Tools

- Analytics is building models with data to make better decisions. Business Intelligence is a technology that enables data preparation, data mining, data management and data visualization, software market offers many business intelligence tools and IBM Cognos Analytics is one of them.