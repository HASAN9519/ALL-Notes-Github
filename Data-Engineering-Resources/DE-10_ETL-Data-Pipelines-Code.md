## ETL using shell scripts

#### Extracting data using cut command on linux CLI 
#### filter command cut helps to extract selected characters or fields from a line of text
#### extracting first four characters from word database
    echo "database" | cut -c1-4

#### extracting 5th to 8th characters
    echo "database" | cut -c5-8

#### Non-contiguous characters can be extracted using comma, extracting 1st and 5th characters
    echo "database" | cut -c1,5

#### Extracting specific column/field from a delimited text file by mentioning delimiter using -d option or field number using -f option
#### command below extracts user names (first field) from /etc/passwd; /etc/passwd is a ":" delimited file
    cut -d":" -f1 /etc/passwd

#### command below extracts multiple fields 1st, 3rd and 6th (username, userid and home directory) from /etc/passwd
    cut -d":" -f1,3,6 /etc/passwd

#### command below extracts a range of fields 3rd to 6th (userid, groupid, user description and home directory) from /etc/passwd
    cut -d":" -f3-6 /etc/passwd

#### Transforming data using tr, tr is a filter command used to translate, squeeze and delete characters
#### command below translates all lower case alphabets to upper case
    echo "Shell Scripting" | tr "[a-z]" "[A-Z]"

#### using pre-defined character sets to translates all lower case alphabets to upper case
    echo "Shell Scripting" | tr "[:lower:]" "[:upper:]"

#### command below translates all upper case alphabets to lower case
    echo "Shell Scripting" | tr "[A-Z]" "[a-z]"

#### Squeeze repeating occurrences of characters
#### -s option replaces a sequence of a repeated characters with a single occurrence of that character
#### command below replaces repeat occurrences of space in output of ps command with one space
    ps | tr -s " "

    # space character within quotes can be replaced with following : "[:space:]"
    ps | tr -s "[:space:]"

    # Delete specified characters using -d option, command below deletes number 5634
    echo "My login pin is 5634" | tr -d "[:digit:]"

## Starting PostgreSQL database using linux CLI

#### On terminal run following command to start PostgreSQL database
    start_postgres

#### Running command from shell prompt to start interactive psql client which connects to PostgreSQL server
    psql --username=postgres --host=localhost

#### using a database called template1 which is already available by default, connecting to database template1 from psql
    \c template1

#### creating table called users in PostgreSQL database, This table will hold user account information
    create table users(username varchar(50), userid int, home_directory varchar(100));

#### exiting psql client
    \q

## Loading data into a PostgreSQL table

#### creating a shell script file csv2db.sh in filesystem and write following codes and then save it
    # This script
    # Extracts data from /etc/passwd file into a CSV file
    # csv data file contains user name, user id and 
    # home directory of each user account defined in /etc/passwd
    # Transforms text delimiter from ":" to ","
    # Loads data from CSV file into a table in PostgreSQL database
    # Extract phase
    echo "Extracting data"

    # Extract columns 1 (user name), 2 (user id) and 6 (home directory path) from /etc/passwd to redirect into a file named extracted-data.txt
    cut -d":" -f1,3,6 /etc/passwd > extracted-data.txt

    # Transform phase
    echo "Transforming data"
    # read extracted data and replace colons with commas
    # then redirect into a file named transformed-data.csv
    tr ":" "," < extracted-data.txt > transformed-data.csv

    # Load phase
    echo "Loading data"
    # Load data into table users in PostgreSQL
    # Sending instructions for connect to 'template1' and copy file to table users through command pipeline
    # basic structure of Load data into table is below
    # COPY table_name FROM 'filename' DELIMITERS 'delimiter_character' FORMAT;

    echo "\c template1;\COPY users FROM '/home/project/transformed-data.csv' DELIMITERS ',' CSV;" | psql --username=postgres --host=localhost

#### run script csv2db.sh and verify file extracted-data.txt and transformed-data.csv is created or not
    bash csv2db.sh
    cat extracted-data.txt
    cat transformed-data.csv

#### command below to verify if table users is populated with data
    echo '\c template1; ####SELECT * from users;' | psql --username=postgres --host=localhost

#### creating a shell script file cp-access-log.sh in filesystem and write following codes and then save it
    # cp-access-log.sh
    # This script downloads file 'web-server-access-log.txt.gz' from "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/"\ "IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/ETL%20using%20shell%20scripting/"
    # Download access log file, using \ to break link into multiple line
    wget "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/"\
    "IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/ETL%20using%20shell%20scripting/web-server-access-log.txt.gz"

    # script then extract .txt file using gunzip
    # Unzipping file to extract .txt file
    # -f option of gunzip is to overwrite file if it already exists
    gunzip -f web-server-access-log.txt.gz

    # .txt file contains timestamp, latitude, longitude and visitor id apart from other data
    # Extract phase
    echo "Extracting data"
    # Extract columns 1 (timestamp), 2 (latitude), 3 (longitude) and 4 (visitorid)
    # then Redirect extracted data into a file named extracted-data.txt
    cut -d"#" -f1-4 web-server-access-log.txt > extracted-data.txt  

    # Transforms text delimiter from "#" to "," and saves to a csv file
    # Transform phase
    echo "Transforming data"
    # read extracted data and replace hash with commas and save transformed data to a .csv file
    tr "#" "," < extracted-data.txt > transformed-data.csv

    # Loads data from CSV file into table 'access_log' in PostgreSQL database
    # Load phase
    echo "Loading data"

    # Send instructions to connect database 'template1' and copy file to table 'access_log' through command pipeline
    # using HEADER option as file comes with a header

    echo "\c template1;\COPY access_log FROM '/home/project/transformed-data.csv' DELIMITERS ',' CSV HEADER;" | psql --username=postgres --host=localhost

#### run script cp-access-log.sh and verify file extracted-data.txt and transformed-data.csv is created or not
    bash cp-access-log.sh
    cat extracted-data.txt
    cat transformed-data.csv

#### command below to verify if table users is populated with data
    echo '\c template1; ####SELECT * from access_log;' | psql --username=postgres --host=localhost

## Working with Apache Airflow

#### Start Apache Airflow in linux terminal
    start_airflow

#### command to list out all existing DAGs
    airflow dags list

#### List tasks in a DAG named example_bash_operator
    airflow tasks list example_bash_operator

#### Unpause a DAG named tutorial
    airflow dags unpause tutorial

#### Pause a DAG named tutorial
    airflow dags pause tutorial

## Create a DAG for Apache Airflow

#### creating a DAG that runs daily and extracts user information from /etc/passwd file, transforms it and loads it into a file
#### This DAG has two tasks extract that extracts fields from /etc/passwd file and transform_and_load that transforms and loads data into a file creating a file named by my_first_dag.py in filesystem and write following commands and save it
    # imports block
    # import libraries
    from datetime import timedelta

    # DAG object to instantiate a DAG
    from airflow import DAG

    # Operators to write tasks
    from airflow.operators.bash_operator import BashOperator

    # to make scheduling
    from airflow.utils.dates import days_ago

    # DAG Arguments block
    # defining DAG arguments, days_age(0) means today
    # can override them on a per-task basis during operator initialization
    default_args = {
        'owner': 'hasan',
        'start_date': days_ago(0),
        'email': ['hasan@gmail.com'],
        'email_on_failure': False,
        'email_on_retry': False,
        'retries': 1,
        'retry_delay': timedelta(minutes=5),
    }

    # DAG Definition block
    # define DAG, schedule_interval shows how frequently this DAG runs, In this case every day (days=1)
    dag = DAG(
        dag_id='my-first-dag',
        default_args=default_args,
        description='My first DAG',
        schedule_interval=timedelta(days=1),
    )

    # Task Definitions block
    # define first task
    extract = BashOperator(
        task_id='extract',
        bash_command='cut -d":" -f1,3,6 /etc/passwd > /home/project/airflow/dags/extracted-data.txt',
        dag=dag,
    )

    # define second task
    transform_and_load = BashOperator(
        task_id='transform',
        bash_command='tr ":" "," < /home/project/airflow/dags/extracted-data.txt > /home/project/airflow/dags/transformed-data.csv',
        dag=dag,
    )

    # task pipeline block
    extract >> transform_and_load

#### Submit a DAG
#### Submitting a DAG by copying DAG python file into dags folder in AIRFLOW_HOME directory
    sudo cp my_first_dag.py $AIRFLOW_HOME/dags

#### Verify DAG got submitted or not by running following commands
    airflow dags list
    airflow dags list|grep "my-first-dag"

#### to list out all tasks in my-first-dag
    airflow tasks list my-first-dag

#### creating a DAG named ETL_Server_Access_Log_Processing
#### creating a file named by ETL_Server_Access_Log_Processing.py in filesystem and write following commands and save it
    # imports block
    from datetime import timedelta
    from airflow import DAG
    from airflow.operators.bash_operator import BashOperator
    from airflow.utils.dates import days_ago

    # DAG Arguments block
    default_args = {
        'owner': 'Hasan Uz Zaman',
        'start_date': days_ago(0),
        'email': ['hasan@gmail.com'],
        'email_on_failure': False,
        'email_on_retry': False,
        'retries': 1,
        'retry_delay': timedelta(minutes=5),
    }

    # DAG Definition block
    dag = DAG(
        dag_id='ETL_Server_Access_Log_Processing',
        default_args=default_args,
        description='ETL_Server_Access_Log_Processing',
        schedule_interval=timedelta(days=1),
    )

    # Task Definitions block
    # define task Download
    lk="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Apache%20Airflow/\
        Build%20a%20DAG%20using%20Airflow/web-server-access-log.txt"
    download = BashOperator(
        task_id='download',
        bash_command='wget $lk',
        dag=dag,
    )

    # define extract task
    # server access log file contains fields in order timestamp, latitude, longitude, visitorid, accessed_from_mobile, browser_code
    # extracting fields timestamp and visitorid which are in 1st and 4th position defined in -f1,4
    extract = BashOperator(
        task_id='extract',
        bash_command='cut -f1,4 -d"#" web-server-access-log.txt > /home/project/airflow/dags/extracted.txt',
        dag=dag,
    )

    # define transform task
    transform = BashOperator(
        task_id='transform',
        bash_command='tr "[a-z]" "[A-Z]" < /home/project/airflow/dags/extracted.txt > /home/project/airflow/dags/capitalized.txt',
        dag=dag,
    )

    # define load task
    load = BashOperator(
        task_id='load',
        bash_command='zip log.zip capitalized.txt',
        dag=dag,
    )

    # task pipeline block
    download >> extract >> transform >> load

#### Submit a DAG
#### Submitting a DAG by copying DAG python file into dags folder in AIRFLOW_HOME directory
    sudo cp ETL_Server_Access_Log_Processing.py $AIRFLOW_HOME/dags

#### Verify DAG got submitted or not by running following commands
    airflow dags list
    airflow dags list|grep "ETL_Server_Access_Log_Processing"

#### to list out all tasks in ETL_Server_Access_Log_Processing
    airflow tasks list ETL_Server_Access_Log_Processing

#### file name ETL_toll_data.py
    # imports block
    from datetime import timedelta
    from airflow import DAG
    from airflow.operators.bash_operator import BashOperator
    from airflow.utils.dates import days_ago

    # DAG Arguments block
    default_args = {
        'owner': 'Hasan Uz Zaman',
        'start_date': days_ago(0),
        'email': ['hasan@gmail.com'],
        'email_on_failure': True,
        'email_on_retry': True,
        'retries': 1,
        'retry_delay': timedelta(minutes=5),
    }

    # DAG Definition block
    dag = DAG(
        dag_id='ETL_toll_data',
        default_args=default_args,
        description='Apache Airflow Final Assignment',
        schedule_interval=timedelta(days=1),
    )

    # Task Definitions block
    # define task unzip_data
    source_dic="/home/project/airflow/dags/finalassignment/staging/tolldata.tgz"
    dest_dic="/home/project/airflow/dags/finalassignment/staging"
    unzip_data = BashOperator(
        task_id='unzip_data',
        bash_command='sudo tar -zvxf $source_dic -C $dest_dic',
        dag=dag,
    )

    # define task extract_data_from_csv
    # extracting fields are from 1st to 4th position defined in -f1-4
    source_dic="/home/project/airflow/dags/finalassignment/staging/vehicle-data.csv"
    dest_dic="/home/project/airflow/dags/finalassignment/staging/csv_data.csv"
    extract_data_from_csv = BashOperator(
        task_id='extract_data_from_csv',
        bash_command='cut -d"," -f1-4 $source_dic > $dest_dic',
        dag=dag,
    )

    # define task extract_data_from_tsv
    # first read tollplaza-data.tsv file and replace \t with commas into redirected file named tsv_data.csv
    # extracting fields 5,6,7 position defined in -f5,6,7 from tsv_data.csv
    source_dic="/home/project/airflow/dags/finalassignment/staging/tollplaza-data.tsv"
    dest_dic="/home/project/airflow/dags/finalassignment/staging/tsv_data.csv"
    extract_data_from_tsv = BashOperator(
        task_id='extract_data_from_tsv',
        bash_command='tr "\t" "," < $source_dic > $dest_dic | cut -d"," -f5,6,7 $dest_dic > $dest_dic',
        dag=dag,
    )

    # define task extract_data_from_fixed_width
    # first extracting column 59-68 for every line in text file payment-data.txt and save it to fixed_width_data.csv file
    # then replace space with commas into fixed_width_data.csv file and save into same file fixed_width_data.csv
    source_dic="/home/project/airflow/dags/finalassignment/staging/payment-data.txt"
    dest_dic="/home/project/airflow/dags/finalassignment/staging/fixed_width_data.csv"
    extract_data_from_fixed_width = BashOperator(
        task_id='extract_data_from_fixed_width',
        bash_command='cut -c59-68  $source_dic > $dest_dic | tr " " "," < $dest_dic > $dest_dic',
        dag=dag,
    )

    # define task consolidate_data
    # first merge csv_data.csv with tsv_data.csv, then merge fixed_width_data.csv with them using pipeline
    source_dic1="/home/project/airflow/dags/finalassignment/staging/csv_data.csv"
    source_dic2="/home/project/airflow/dags/finalassignment/staging/tsv_data.csv"
    source_dic3="/home/project/airflow/dags/finalassignment/staging/fixed_width_data.csv"
    dest_dic="/home/project/airflow/dags/finalassignment/staging/extracted_data.csv"
    consolidate_data = BashOperator(
        task_id='consolidate_data',
        bash_command='paste -d"," $source_dic1 $source_dic2 > $dest_dic | paste -d"," $source_dic3 $dest_dic > $dest_dic',
        dag=dag,
    )

    # define task transform_data
    source_dic="/home/project/airflow/dags/finalassignment/staging/extracted_data.csv"
    dest_dic="/home/project/airflow/dags/finalassignment/staging/transformed_data.csv"
    transform_data = BashOperator(
        task_id='transform_data',
        bash_command='cut -d"," -f4 $source_dic > $dest_dic | ls -l $dest_dic | tr "[a-z]" "[A-Z]" < $dest_dic > $dest_dic',
        dag=dag,
    )

    # task pipeline block
    unzip_data >> extract_data_from_csv >> extract_data_from_tsv >> extract_data_from_fixed_width >> consolidate_data >> transform_data

## Monitoring a DAG in Airflow Web-UI

- in Airflow Web-UI search any DAG by entering DAG name in Search DAGs text box; Unpause and pause DAG using Pause/Unpause button; explore grid view of DAG by Clicking on grid View button, Click on Auto Refresh button to switch on auto refresh feature in grid view; graph view shows tasks in a form of a graph, With auto refresh on each task status is also indicated with color code; calender view gives an overview of all dates when DAG was run along with its status as a color code; Task Duration view gives an overview of how much time each task took to execute over a period of time; Details view give all the details of DAG as specified in code of DAG; Code view gives view of code of DAG; To delete a DAG click on delete button

## Working with streaming data using Kafka

#### Downloading Kafka by running command below in linux CLI
    wget https://archive.apache.org/dist/kafka/2.8.0/kafka_2.12-2.8.0.tgz

#### Extract kafka from zip file
    tar -xzf kafka_2.12-2.8.0.tgz

#### ZooKeeper handles leadership election of Kafka brokers and manages service discovery as well as cluster topology; ZooKeeper is responsible for overall management of Kafka cluster as it monitors Kafka brokers and notifies Kafka if any broker or partition goes down or if a new broker or partition goes up; Starting ZooKeeper server as ZooKeeper is required for Kafka to work by following command 
    cd kafka_2.12-2.8.0
    bin/zookeeper-server-start.sh config/zookeeper.properties

#### Starting Kafka broker service by following command
    cd kafka_2.12-2.8.0
    bin/kafka-server-start.sh config/server.properties

#### creating a topic named news in a new terminal by following command
    cd kafka_2.12-2.8.0
    bin/kafka-topics.sh --create --topic news --bootstrap-server localhost:9092

#### Starting Producer as producer sends messages to Kafka
    bin/kafka-console-producer.sh --topic news --bootstrap-server localhost:9092

#### Once producer starts user will get '>' prompt, type any text like following message and press enter
    Good morning
    Good day
    Enjoy the Kafka lab

#### Starting Consumer in a new terminal as consumer reads messages from kafka, output will be messages sent from producer in above line
    cd kafka_2.12-2.8.0
    bin/kafka-console-consumer.sh --topic news --from-beginning --bootstrap-server localhost:9092

#### Exploring Kafka directories as Kafka uses directory /tmp/kakfa-logs to store messages
#### folder news-0 inside /tmp/kakfa-logs stores all messages; folder /home/project/kafka_2.12-2.8.0 has below 3 sub directories
    bin	    # shell scripts to control kafka and zookeeper
    config	# configuration files
    logs	# log files for kafka and zookeeper

#### Deleting kafka installation file
    rm kafka_2.12-2.8.0.tgz

## Kafka Message key and offset

#### first start kafka and zookeeper
#### creating a topic and producer for processing bank ATM transactions
#### creating a bank_branch topic in new terminal to process messages that come from ATM machines of bank branches
#### --topic argument with name bank_branch and specify --partitions 2 argument to create two partitions for this topic In order to simplify topic 
#### configuration and better explain how message key and consumer offset work 
    cd kafka_2.12-2.8.0
    bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic bank_branch  --partitions 2

#### list all topics to see if bank_branch has been created successfully or not
    bin/kafka-topics.sh --bootstrap-server localhost:9092 --list

#### --describe command to check details of topic bank_branch, output will show bank_branch has two partitions: Partition 0 and Partition 1
    bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic bank_branch

#### creating a producer for topic bank_branch to publish some ATM transaction messages
    bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic bank_branch

#### Once producer starts user will get '>' prompt, type following messages and press enter
    {"atmid": 1, "transid": 100}
    {"atmid": 1, "transid": 101}
    {"atmid": 2, "transid": 200}
    {"atmid": 1, "transid": 102}
    {"atmid": 2, "transid": 201}

#### starting a new consumer to subscribe to bank_branch topic in new terminal, output will be messages sent from producer in sorted order 
    cd kafka_2.12-2.8.0
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic bank_branch --from-beginning

## Produce and consume with message keys

- using message keys to ensure that messages with same key will be consumed in same order as they were published; In backend messages with same key will be published into same partition and will always be consumed by same consumer as original publication order is kept in consumer side
- first Zookeeper terminal, Kafka Server terminal, Producer terminal and Consumer terminal should be open
- then go to consumer and Producer terminal and stop both using Ctrl + C (Windows) in respective terminal

#### starting a new producer and consumer using message keys with following message key commands:
    --property parse.key=true   # to make producer parse message keys
    --property key.separator=:  # define key separator to be : character

#### so message with key now looks like following key-value pair example "- 1:{"atmid": 1, "transid": 102}", Here message key is 1 which also corresponds to ATM id and value is transaction JSON object {"atmid": 1, "transid": 102}
#### Starting a new producer for topic bank_branch with message key enabled
    bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic bank_branch --property parse.key=true --property key.separator=:

#### Once producer starts user will get '>' prompt, type following messages to define each key to match ATM id for each message and press enter
    1:{"atmid": 1, "transid": 102}
    1:{"atmid": 1, "transid": 103}
    2:{"atmid": 2, "transid": 202}
    2:{"atmid": 2, "transid": 203}
    1:{"atmid": 1, "transid": 104}

#### switch to consumer terminal again and start a new consumer with --property print.key=true and --property key.separator=: arguments to print keys
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic bankbranch --from-beginning --property print.key=true --property key.separator=:

#### output will show messages that have same key are being consumed in following order transid102, transid103, transid104 as they were published
#### This is because each topic partition maintains its own message queue and new messages are enqueued (appended to end of queue) as they get published to partition; Once consumed, earliest messages will be dequeued and no longer be available for consumption
#### Messages with same key will always be published to same partition as their published order will be preserved within message queue of each partition
#### can keep states or orders of transactions for each ATM
#### with message key specified as atmid value, messages from two ATMs will look like following:
    Partition 0: [{"atmid": 1, "transid": 102}, {"atmid": 1, "transid": 103}, {"atmid": 1, "transid": 104}]
    Partition 1: [{"atmid": 2, "transid": 202}, {"atmid": 2, "transid": 203}]

## Consumer offset

#### Topic partitions keeps published messages in a sequence like a list; Message offset indicates a message's position in sequence for example offset of an empty Partition 0 of bank_branch is 0 and by publishing first message to partition its offset will be 1; By using offsets in consumer can specify starting position for message consumption such as from beginning to retrieve all messages or from some later point to retrieve only latest messages

#### Consumer Group
#### normally group related consumers together as a consumer group for example to create a consumer for each ATM in bank and manage all ATM related consumers together in a group; stop previous consumer by using Ctrl + C if it's still running
#### command to create a new consumer within a consumer group called atm-app using --group argument
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic bank_branch --group atm-app

#### After consumer within atm-app consumer group is started, should not expect any messages to be consumed as offsets for both partitions have already reached to end; In other words all messages have already been consumed and therefore dequeued by previous consumers; verify that by checking consumer group details; first stop consumer by Ctrl + C and run following command to show details of consumer group atm-app
    bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group atm-app

#### in output LOG-END-OFFSET column indicates last offset or end of sequence; LAG column represents count of unconsumed messages for each partition

#### Reset offset
#### reset index with --reset-offsets argument
#### Stop previous consume, command to resetting offset to earliest position (beginning) using --reset-offsets --to-earliest
    bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092  --topic bank_branch --group atm-app --reset-offsets --to-earliest --execute

#### command to Start consumer again, output will show all messages are consumed and all offsets have reached partition ends again
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic bank_branch --group atm-app

#### reset offset so that only consume last two messages
#### stop previous consume, command to Shift offset to left by 2 using --reset-offsets --shift-by -2:
    bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092  --topic bank_branch --group atm-app --reset-offsets --shift-by -2 --execute

#### running consumer again will show 4 messages consumed 2 for each partition
    bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic bank_branch --group atm-app

## Kafka Python Client

- Kafka has a distributed client-server architecture; For server side Kafka is a cluster with many associated servers called broker acting as event broker to receive, store and distribute events; All those brokers are managed by another distributed system called ZooKeeper to ensure all brokers work in an efficient and collaborative way; Kafka uses a TCP based network communication protocol to exchange data between clients and servers
- For client side Kafka provides different types of clients such as Kafka CLI which is a collection of shell scripts to communicate with a Kafka server
- Many high-level programming APIs such as Python, Java and Scala; REST APIs; Specific 3rd party clients made by Kafka community 
- kafka-python is a Python client for Apache Kafka distributed stream processing system which aims to provide similar functionalities as main Kafka Java client; can easily interact with Kafka server such as managing topics, publish and consume messages in Python With kafka-python

#### Installing kafka-python is similar to other regular Python packages in terminal, put ! before pip to install on .py file or .ipynb file
    pip install kafka-python

#### KafkaAdminClient class used to enable fundamental administrative management operations on kafka server such as creating/deleting topic, retrieving and updating topic configurations and so on; To use KafkaAdminClient, first need to define and create a KafkaAdminClient object in a .py file
#### bootstrap_servers="localhost:9092" argument specifies host/IP and port that consumer should contact to bootstrap initial cluster metadata
#### client_id specifies an id of current admin client
    admin_client = KafkaAdminClient(bootstrap_servers="localhost:9092", client_id='test')

#### most common usage of admin_client is managing topics such as creating and deleting topics
#### To create new topics, first need to define an empty topic list
    topic_list = []

#### using NewTopic class to create a topic with name equals bank_branch, partition nums equals to 2 and replication factor equals to 1
    new_topic = NewTopic(name="bank_branch", num_partitions= 2, replication_factor=1)
    topic_list.append(new_topic)

#### now can use create_topics(...) method to create new topics
    admin_client.create_topics(new_topics=topic_list)

#### Above create topic operation is equivalent to using kafka-topics.sh --topic in Kafka CLI client: 
    kafka-topics.sh --bootstrap-server localhost:9092 --create --topic bank_branch  --partitions 2 --replication_factor 1

#### Describe a topic by checking its configuration details using describe_configs() method
    configs = admin_client.describe_configs(
        config_resources=[ConfigResource(ConfigResourceType.TOPIC, "bank_branch")])

#### Above describe topic operation is equivalent to using kafka-topics.sh --describe in Kafka CLI client:
    kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic bank_branch

#### KafkaProducer class to produce messages; Since many real-world message values are in JSON, publishing JSON messages as an example creating a KafkaProducer; Since Kafka produces and consumes messages in raw bytes, need to encode JSON messages and serialize them into bytes
#### For value_serializer argument, defining a lambda function to take a Python dict/list object and serialize it into bytes
    producer = KafkaProducer(value_serializer=lambda v: json.dumps(v).encode('utf-8'))

#### After KafkaProducer created, can use it to produce two ATM transaction messages in JSON format as follows
    producer.send("bank_branch", {'atmid':1, 'transid':100})
    producer.send("bank_branch", {'atmid':2, 'transid':101})

#### 1st argument specifies topic bank_branch to be sent and 2nd argument represents message value in a Python dict format and will be serialized into bytes
#### above producing message operation is equivalent to using kafka-console-producer.sh --topic in Kafka CLI client:
    kafka-console-producer.sh --bootstrap-server localhost:9092 --topic bank_branch

#### creating a KafkaConsumer subscribing to topic bank_branch
    consumer = KafkaConsumer('bank_branch')

#### after consumer is created, it will receive all available messages from topic bank_branch; can iterate and print them with following code
    for msg in consumer:
        print(msg.value.decode("utf-8"))

#### above consuming message operation is equivalent to using kafka-console-consumer.sh --topic in Kafka CLI client:
    kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic bank_branch

## kafka final project

#### Downloading Kafka
    wget https://archive.apache.org/dist/kafka/2.8.0/kafka_2.12-2.8.0.tgz

#### Extracting Kafka
    tar -xzf kafka_2.12-2.8.0.tgz

#### Starting MySQL server
    start_mysql

#### Connecting to mysql server using command below
    mysql --host=127.0.0.1 --port=3306 --user=root --password=Mjk0NDQtcnNhbm5h

#### creating a database named toll_data at mysql prompt
    create database toll_data;

#### creating a table named live_toll_data with schema to store data generated by traffic simulator, table store all streamed data that comes from kafka
    use toll_data;
    create table live_toll_data(timestamp datetime,vehicle_id int,vehicle_type char(15),toll_plaza_id smallint);

#### Disconnecting from MySQL server
    exit

#### Installing python module kafka-python using pip3 command, python module will help to interact with mysql server
    python3 -m pip install kafka-python

#### starting kafka, first start zookeeper server then kafka server, both in separate terminal
    cd kafka_2.12-2.8.0
    bin/zookeeper-server-start.sh config/zookeeper.properties
    cd kafka_2.12-2.8.0
    bin/kafka-server-start.sh config/server.properties

#### creating a topic named toll
    bin/kafka-topics.sh --create --topic toll --bootstrap-server localhost:9092

#### Downloading Toll Traffic Simulator
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Final%20Assignment/toll_traffic_generator.py

#### code of toll_traffic_generator.py file
    # Top Traffic Simulator
    from time import sleep, time, ctime
    from random import random, randint, choice
    from kafka import KafkaProducer
    producer = KafkaProducer(bootstrap_servers='localhost:9092')
    TOPIC = 'toll'
    VEHICLE_TYPES = ("car", "car", "car", "car", "car", "car", "car", "car",
                    "car", "car", "car", "truck", "truck", "truck",
                    "truck", "van", "van")

    for _ in range(100000):
        vehicle_id = randint(10000, 10000000)
        vehicle_type = choice(VEHICLE_TYPES)
        now = ctime(time())
        plaza_id = randint(4000, 4010)
        message = f"{now},{vehicle_id},{vehicle_type},{plaza_id}"
        message = bytearray(message.encode("utf-8"))
        print(f"A {vehicle_type} has passed by the toll plaza {plaza_id} at {now}.")
        producer.send(TOPIC, message)
        sleep(random() * 2)

#### Running Toll Traffic Simulator on CLI
    python3 toll_traffic_generator.py

#### Downloading streaming_data_reader.py
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Final%20Assignment/streaming_data_reader.py

#### Configuring streaming_data_reader.py
    # Streaming data consumer
    from datetime import datetime
    from kafka import KafkaConsumer
    import mysql.connector
    TOPIC='toll'
    DATABASE = 'toll_data'
    USERNAME = 'root'
    PASSWORD = 'OTU4NS1zaGVoYWIx'
    print("Connecting to the database")
    try:
        connection = mysql.connector.connect(host='localhost', database=DATABASE, user=USERNAME, password=PASSWORD)
    except Exception:
        print("Could not connect to database. Please check credentials")
    else:
        print("Connected to database")
    cursor = connection.cursor()

    print("Connecting to Kafka")
    consumer = KafkaConsumer(TOPIC)
    print("Connected to Kafka")
    print(f"Reading messages from the topic {TOPIC}")
    for msg in consumer:
        # Extract information from kafka
        message = msg.value.decode("utf-8")
        # Transform the date format to suit the database schema
        (timestamp, vehcile_id, vehicle_type, plaza_id) = message.split(",")
        dateobj = datetime.strptime(timestamp, '%a %b %d %H:%M:%S %Y')
        timestamp = dateobj.strftime("%Y-%m-%d %H:%M:%S")
        # Loading data into the database table
        sql = "insert into livetolldata values(%s,%s,%s,%s)"
        result = cursor.execute(sql, (timestamp, vehcile_id, vehicle_type, plaza_id))
        print(f"A {vehicle_type} was inserted into the database")
        connection.commit()
    connection.close()

#### Running streaming_data_reader.py on CLI
    python3 streaming_data_reader.py

#### checking if streaming toll data stored in table live_toll_data in mysql
    start mysql
    mysql --host=127.0.0.1 --port=3306 --user=root --password=OTU4NS1zaGVoYWIx
    use toll_data
    select * from live_toll_data limit 10;

#### sample etl code etl.py file using mysql and Db2 database
    # Import libraries required for connecting to mysql
    # Import libraries required for connecting to DB2
    # Connect to MySQL
    # Connect to DB2
    # Find out last rowid from DB2 data warehouse
    # function get_last_rowid must return last rowid of table sales_data on IBM DB2 database

    def get_last_rowid():

        import ibm_db

        # connection details
        dsn_hostname = "55fbc997-9266-4331-afd3-888b05e734c0.bs2io90l08kqb1od8lcg.databases.appdomain.cloud" 
        dsn_uid = "qkr40701"        
        dsn_pwd = "9MBBtlvhbqeO6Lyj"      
        dsn_port = "31929"                
        dsn_database = "BLUDB"            
        dsn_driver = "{IBM DB2 ODBC DRIVER}"     
        dsn_protocol = "TCPIP"           
        dsn_security = "SSL"           

        #Create the dsn connection string
        dsn = (
                "DRIVER={0};"
                "DATABASE={1};"
                "HOSTNAME={2};"
                "PORT={3};"
                "PROTOCOL={4};"
                "UID={5};"
                "PWD={6};"
                "SECURITY={7};").format(dsn_driver, dsn_database, dsn_hostname, dsn_port, dsn_protocol, dsn_uid, dsn_pwd, dsn_security)

        # create connection
        conn = ibm_db.connect(dsn, "", "")
        print ("Connected to database: ", dsn_database, "as user: ", dsn_uid, "on host: ", dsn_hostname)

        SQL="SELECT ROWID FROM sales_data ORDER BY ROWID DESC LIMIT 1"
        stmt = ibm_db.exec_immediate(conn, SQL)
        tuple = ibm_db.fetch_tuple(stmt)
        #while tuple != False:
        # print(tuple)
            #tuple = ibm_db.fetch_tuple(stmt)
        rownum = tuple[0]
        return rownum
        # close connection
        ibm_db.close(conn)

    last_row_id = get_last_rowid()
    print("Last row id on production datawarehouse = ", last_row_id)

    # List out all records in MySQL database with rowid greater than one on Data warehouse
    # function get_latest_records must return a list of all records that have a rowid greater than last_row_id in sales_data table in sales database on
    # MySQL staging data warehouse

    def get_latest_records(row_id):
        import mysql.connector
        # connect to database
        connection = mysql.connector.connect(user='root', password='MjcxNDktc2hlaGFi',host='127.0.0.1',database='sales')
        #num = row_id
        # create cursor
        cursor = connection.cursor()
        SQL = "SELECT * FROM sales_data WHERE rowid > %s"
        q=(row_id,)
        cursor.execute(SQL,q)

        reclist= []
        for row in cursor.fetchall():
            reclist.append(row)

        # close connection
        connection.close()
        return reclist

    new_records = get_latest_records(last_row_id)
    print("New rows on staging datawarehouse = ", len(new_records))

    # Insert additional records from MySQL into DB2 data warehouse
    # function insert_records must insert all records passed to it into sales_data table in IBM DB2 database

    def insert_records(records):
        
        import ibm_db

        # connectction details
        dsn_hostname = "55fbc997-9266-4331-afd3-888b05e734c0.bs2io90l08kqb1od8lcg.databases.appdomain.cloud"
        dsn_uid = "qkr40701"
        dsn_pwd = "9MBBtlvhbqeO6Lyj"      
        dsn_port = "31929"                
        dsn_database = "BLUDB"            
        dsn_driver = "{IBM DB2 ODBC DRIVER}"     
        dsn_protocol = "TCPIP"           
        dsn_security = "SSL"           

        #Create the dsn connection string
        dsn = (
                "DRIVER={0};"
                "DATABASE={1};"
                "HOSTNAME={2};"
                "PORT={3};"
                "PROTOCOL={4};"
                "UID={5};"
                "PWD={6};"
                "SECURITY={7};").format(dsn_driver, dsn_database, dsn_hostname, dsn_port, dsn_protocol, dsn_uid, dsn_pwd, dsn_security)

        # create connection
        conn = ibm_db.connect(dsn, "", "")
        print ("Connected to database: ", dsn_database, "as user: ", dsn_uid, "on host: ", dsn_hostname)

        # insert data
        SQL = "INSERT INTO sales_data(rowid, product_id, customer_id, quantity) VALUES(?,?,?,?)"

        for record in records:
            stmt = ibm_db.prepare(conn, SQL)
            ibm_db.execute(stmt, record)
        
        print("all data inserted")
        return records

        # close connection
        ibm_db.close(conn)

    insert_records(new_records)
    print("New rows inserted into production datawarehouse = ", len(new_records))

    # disconnect from mysql warehouse
    # disconnect from DB2 data warehouse
    # End of program