## Final Lab ETL Tasks

#### Preparing lab environment
    start_mysql
    mysql --host=127.0.0.1 --port=3306 --user=root --password=MjYwMzAtc2hlaGFi
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0321EN-SkillsNetwork/ETL/sales.sql
    mysql --host=127.0.0.1 --port=3306 --user=root --password sales < sales.sql
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0321EN-SkillsNetwork/ETL/mysqlconnect.py
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0321EN-SkillsNetwork/ETL/db2connect.py
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0321EN-SkillsNetwork/ETL/automation.py

    python3 -m pip install ibm-db
    python3 -m pip install mysql-connector-python

    python3 automation.py

#### db2
    ALTER TABLE sales_data ALTER COLUMN timestamp SET DATA TYPE TIMESTAMP;
    ALTER TABLE sales_data ALTER COLUMN timestamp SET NOT NULL;
    ALTER TABLE sales_data ALTER COLUMN timestamp SET DEFAULT CURRENT_TIMESTAMP;
    ALTER TABLE sales_data ALTER COLUMN price SET NOT NULL;
    ALTER TABLE sales_data ALTER COLUMN price SET DEFAULT 0;

#### mysql
    CREATE DATABASE sales;
    USE sales;

#### Data Pipelines Using Apache AirFlow
    cut -d" " -f1 /home/project/test/accesslog.txt > /home/project/test/extracted_data.txt
    grep -v "218.30.103.62" /home/project/test/test.txt > /home/project/test/testnew.txt

    sudo cut -d" " -f1 /home/project/airflow/dags/cap/accesslog.txt > sudo touch /home/project/airflow/dags/cap/extracted_data.txt
    sudo chmod 777 accesslog.txt
    sudo chmod 777 extracted_data.txt
    sudo cut -d" " -f1 /home/project/airflow/dags/cap/accesslog.txt > chmod 777 /home/project/airflow/dags/cap/extracted_data1.txt
    sudo touch /home/project/airflow/dags/cap/extracted_data3.txt | sudo chmod 777 /home/project/airflow/dags/cap/extracted_data3.txt | 
    sudo cut -d" " -f1 /home/project/airflow/dags/cap/accesslog.txt > /home/project/airflow/dags/cap/extracted_data3.txt


    sudo touch /home/project/airflow/dags/cap/extracted_data.txt ; sudo chmod 777 /home/project/airflow/dags/cap/extracted_data.txt ; 
    sudo cut -d" " -f1 /home/project/airflow/dags/cap/accesslog.txt > /home/project/airflow/dags/cap/extracted_data.txt

    sudo touch /home/project/airflow/dags/cap/transformed_data.txt ; sudo chmod 777 /home/project/airflow/dags/cap/transformed_data.txt ; 
    grep -v "198.46.149.143" /home/project/airflow/dags/cap/extracted_data.txt > /home/project/airflow/dags/cap/transformed_data.txt

    sudo tar -cvf /home/project/airflow/dags/cap/weblog1.tar -C /home/project/airflow/dags/cap/ transformed_data.txt

    start_airflow
    sudo mkdir capstone
    /home/project/airflow/dags/capstone/accesslog.txt
    sudo wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0321EN-SkillsNetwork/ETL/accesslog.txt
    sudo chmod 777 accesslog.txt
    chmod 777 process_web_log4.py


#### creating a file named by process_web_log.py in filesystem and write following commands and save it
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
        dag_id='process_web_log',
        default_args=default_args,
        description='process_web_log',
        schedule_interval=timedelta(days=1),
    )


    # Task block
    # define extract_data task
    # task should extract ipaddress field from web server log file and save it into a file named extracted_data.txt
    extract_data = BashOperator(
        task_id='extract_data',
        bash_command='cut -d" " -f1 /home/project/airflow/dags/capstone/accesslog.txt > /home/project/airflow/dags/capstone/extracted_data.txt',
        dag=dag,
    )

    # Task block
    # define transform_data task
    # task should filter out all occurrences of ipaddress “198.46.149.143” from extracted_data.txt and 
    # save output to a file named transformed_data.txt
    transform_data = BashOperator(
        task_id='transform_data',
        bash_command='grep -v "198.46.149.143" /home/project/airflow/dags/capstone/extracted_data.txt > 
        /home/project/airflow/dags/capstone/transformed_data.txt',
        dag=dag,
    )

    # Task block
    # define load_data task
    # task should archive file transformed_data.txt into a tar file named weblog.tar
    load_data = BashOperator(
        task_id='load_data',
        bash_command='tar -cvf /home/project/airflow/dags/capstone/weblog.tar -C /home/project/airflow/dags/capstone/ transformed_data.txt',
        dag=dag,
    )


    # task pipeline block
    extract_data >> transform_data >> load_data

#### Submitting a DAG by copying DAG python file into dags folder in AIRFLOW_HOME directory
    sudo cp process_web_log.py $AIRFLOW_HOME/dags
    airflow tasks list process_web_log4
    airflow dags list|grep "process_web_log"
    sudo cp process_web_log4.py $AIRFLOW_HOME/dags|grep "process_web_log2"

    sudo cp /home/project/airflow/dags/capstone/process_web_log3.py $AIRFLOW_HOME/dags
    airflow dags unpause process_web_log4

#### Example 
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
        dag_id='process_web_log',
        default_args=default_args,
        description='process_web_log',
        schedule_interval=timedelta(days=1),
    )


    # Task block
    # define extract_data task
    # task should extract ipaddress field from web server log file and save it into a file named extracted_data.txt
    extract_data = BashOperator(
        task_id='extract_data',
        bash_command='sudo touch /home/project/airflow/dags/capstone/extracted_data.txt ; 
        sudo chmod 777 /home/project/airflow/dags/capstone/extracted_data.txt ; 
        sudo cut -d" " -f1 /home/project/airflow/dags/capstone/accesslog.txt > /home/project/airflow/dags/capstone/extracted_data.txt',
        dag=dag,
    )

    # Task block
    # define transform_data task
    # task should filter out all occurrences of ipaddress “198.46.149.143” from extracted_data.txt and 
    # save output to a file named transformed_data.txt
    transform_data = BashOperator(
        task_id='transform_data',
        bash_command='sudo touch /home/project/airflow/dags/capstone/transformed_data.txt ; 
        sudo chmod 777 /home/project/airflow/dags/capstone/transformed_data.txt ; 
        grep -v "198.46.149.143" /home/project/airflow/dags/capstone/extracted_data.txt > /home/project/airflow/dags/capstone/transformed_data.txt',
        dag=dag,
    )

    # Task block
    # define load_data task
    # task should archive file transformed_data.txt into a tar file named weblog.tar
    load_data = BashOperator(
        task_id='load_data',
        bash_command='sudo tar -cvf /home/project/airflow/dags/capstone/weblog.tar -C /home/project/airflow/dags/capstone/ transformed_data.txt',
        dag=dag,
    )


    # task pipeline block
    extract_data >> transform_data >> load_data