## Hadoop Map-Reduce

#### Setting up Single-Node Hadoop
#### Hadoop is most useful when deployed in a fully distributed mode on a large cluster of networked servers sharing a large volume of data
#### configuring Hadoop on a single node for basic understanding
#### Downloading hadoop-3.2.3.tar.gz file in linux terminal
    curl https://dlcdn.apache.org/hadoop/common/hadoop-3.2.3/hadoop-3.2.3.tar.gz --output hadoop-3.2.3.tar.gz

#### Extracting tar file in currently directory
    tar -xvf hadoop-3.2.3.tar.gz

#### Navigating to hadoop-3.2.3 directory
    cd hadoop-3.2.3

#### Checking hadoop command to see if it is setup, output of command will display usage documentation for hadoop script
    bin/hadoop

#### command to download data.txt to current directory
    curl https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-BD0225EN-SkillsNetwork/labs/data/data.txt --output data.txt

#### Running Map reduce application for word count on data.txt and store output in /user/root/output directory
    bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.3.jar wordcount data.txt output

#### command to see output file it has generated, output will show part-r-00000 with _SUCCESS indicating that wordcount has been done 
    ls output

#### following command to see word count output
    cat output/part-r-00000

#### Deleting data.txt file and output folder
    rm data.txt
    rm -rf output

## Hadoop Cluster

- A Hadoop cluster is a collection of computers known as nodes that are networked together to perform parallel computations on big data sets; Name node is master node of Hadoop Distributed File System (HDFS); It maintains meta data of files in RAM for quick access; An actual Hadoop Cluster setup involves extensive resources; using dockerize hadoop to create a Hadoop Cluster which will have Namenode, Datanode, Node Manager, Resource manager and Hadoop history server

#### Cloning github repository on current directory
    git clone https://github.com/ibm-developer-skills-network/ooxwv-docker_hadoop.git

#### Navigating to docker-hadoop directory to build it
    cd ooxwv-docker_hadoop

#### Composing docker application, Compose is a tool for defining and running multi-container Docker applications, It uses YAML file to configure services and enables us to create and start all services from just one configuration file
    docker-compose up -d

#### Running namenode as a mounted drive on bash, it will change to a new prompt
    docker exec -it namenode /bin/bash

#### Exploring hadoop environment in prompt created by previous command
#### a Hadoop environment is configured by editing a set of configuration files:
#### hadoop-env.sh Serves as a master file to configure YARN, HDFS, MapReduce and Hadoop-related project settings
#### core-site.xml Defines HDFS and Hadoop core properties
#### hdfs-site.xml Governs location for storing node metadata, fsimage file and log file
#### mapred-site-xml Lists parameters for MapReduce configuration
#### yarn-site.xml Defines settings relevant to YARN, It contains configurations for Node Manager, Resource Manager, Containers and Application Master
#### viewing xml files in directory /opt/hadoop-3.2.1/etc/hadoop/
    ls /opt/hadoop-3.2.1/etc/hadoop/*.xml

#### creating a directory structure named user/root/input in HDFS
    hdfs dfs -mkdir -p /user/root/input

#### Copy all hadoop configuration xml files into input directory
    hdfs dfs -put $HADOOP_HOME/etc/hadoop/*.xml /user/root/input

#### creating a data.txt file in current directory
    curl https://raw.githubusercontent.com/ibm-developer-skills-network/ooxwv-docker_hadoop/master/SampleMapReduce.txt --output data.txt

#### Copy data.txt file into /user/root
    hdfs dfs -put data.txt /user/root/

#### Checking if file has been copied into HDFS by viewing its content
    hdfs dfs -cat /user/root/data.txt

## Submit Apache Spark Applications

#### Installing a Apache Spark cluster using Docker Compose
#### first Installing pyspark on linux terminal
    python3 -m pip install pyspark

#### getting code from git, then Change directory to downloaded code, then starting cluster
    git clone https://github.com/big-data-europe/docker-spark.git
    cd docker-spark
    docker-compose up

#### creating a file named submit.py in filesystem and write following codes on that file
    from pyspark import SparkContext, SparkConf
    from pyspark.sql import SparkSession
    from pyspark.sql.types import StructField, StructType, IntegerType, StringType
    sc = SparkContext.getOrCreate(SparkConf().setMaster('spark://localhost:7077'))
    sc.setLogLevel("INFO")
    spark = SparkSession.builder.getOrCreate()
    spark = SparkSession.builder.getOrCreate()
    df = spark.createDataFrame(
        [
            (1, "foo"),
            (2, "bar"),
        ],
        StructType(
            [
                StructField("id", IntegerType(), False),
                StructField("txt", StringType(), False),
            ]
        ),
    )
    print(df.dtypes)
    df.show()

#### executing python file
    python3 submit.py

## Apache Spark on Kubernetes

- Kubernetes is a container orchestrator which allows to schedule millions of docker containers on huge compute clusters containing thousands of compute nodes; Originally invented and open-sourced by Google, Kubernetes became de-facto standard for cloud-native application development and deployment inside and outside IBM; With RedHat OpenShift, IBM is leader in hybrid cloud Kubernetes and within top three companies contributing to Kubernetes'open source code base

#### getting code from git
    git clone https://github.com/ibm-developer-skills-network/fgskh-new_horizons.git

#### Changing directory to downloaded code, Adding an alias for less typing
    cd fgskh-new_horizons
    alias k='kubectl'

#### Saving current namespace in an environment variable
    my_namespace=$(kubectl config view --minify -o jsonpath='{..namespace}')

#### Deploying Apache Spark Kubernetes Pod
#### Installing Apache Spark POD, then checking status of Pod
    k apply -f spark/pod_spark.yaml
    k get po

#### Just in case if pod is needed to delete
    k delete po spark

## Submitting Apache Spark jobs to Kubernetes

#### running a command inside spark container of Pod, command exec will provide access to container called spark (-c), With double minus sign -- executing command to echo a message
    k exec spark -c spark  -- echo "Hello from inside the container"

#### above command ran in spark container residing in spark pod inside Kubernetes, using this container to submit Spark applications to Kubernetes cluster, This container is based on an image with Apache Spark distribution and kubectl command pre-installed Inside container can use spark-submit command which makes use of new native Kubernetes scheduler that has been added to Spark recently, following command submits SparkPi sample application to cluster; SparkPi computes Pi and more iterations it runs more precise it gets
    k exec spark -c spark -- ./bin/spark-submit \
    --master k8s://http://127.0.0.1:8001 \
    --deploy-mode cluster \
    --name spark-pi \
    --class org.apache.spark.examples.SparkPi \
    --conf spark.executor.instances=1 \
    --conf spark.kubernetes.container.image=romeokienzler/spark-py:3.1.2 \
    --conf spark.kubernetes.executor.request.cores=0.2 \
    --conf spark.kubernetes.executor.limit.cores=0.3 \
    --conf spark.kubernetes.driver.request.cores=0.2 \
    --conf spark.kubernetes.driver.limit.cores=0.3 \
    --conf spark.driver.memory=512m \
    --conf spark.kubernetes.namespace=${my_namespace} \
    local:///opt/spark/examples/jars/spark-examples_2.12-3.1.2.jar \
    10

#### Understanding spark-submit command
#### ./bin/spark-submit is command to submit applications to a Apache Spark cluster
#### --master k8s://http://127.0.0.1:8001 is address of Kubernetes API server, way kubectl and Apache Spark native Kubernetes scheduler interacts with Kubernetes cluster
#### --name spark-pi provides a name for job and subsequent Pods created by Apache Spark native Kubernetes scheduler are prefixed with that name
#### --class org.apache.spark.examples.SparkPi provides canonical name for Spark application to run (Java package and class name)
#### --conf spark.executor.instances=1 tells Apache Spark native Kubernetes scheduler how many Pods it has to create to parallelize application, Note that on this single node development Kubernetes cluster increasing this number doesn't make any sense (besides adding overhead for parallelization)
#### --conf spark.kubernetes.container.image=romeokienzler/spark-py:3.1.2 tells Apache Spark native Kubernetes scheduler which container image it should use for creating driver and executor Pods, This image can be custom build using provided Dockerfiles in kubernetes/dockerfiles/spark/ and bin/
#### docker-image-tool.sh in Apache Spark distribution
#### --conf spark.kubernetes.executor.limit.cores=0.3 tells Spark native Kubernetes scheduler to set CPU core limit to only use 0.3 core per executor Pod
#### --conf spark.kubernetes.driver.limit.cores=0.3 tells Spark native Kubernetes scheduler to set CPU core limit to only use 0.3 core for driver Pod
#### --conf spark.driver.memory=512m tells Spark native Kubernetes scheduler to set memory limit to only use 512MBs for driver Pod
#### --conf spark.kubernetes.namespace=${my_namespace} tells Spark native Kubernetes scheduler to set namespace to my_namespace environment variable
#### local:///opt/spark/examples/jars/spark-examples_2.12-3.1.2.jar indicates jar file where application is contained in, Note that local:// prefix addresses a path within container images provided by spark.kubernetes.container.image option, Since using a jar provided by Apache Spark distribution
#### this is not a problem otherwise spark.kubernetes.file.upload.path option has to be set and an appropriate storage subsystem has to be configured as described in documentation
#### 10 tells application to run for 10 iterations, then output computed value of Pi
#### documentation link for a full list of available parameters https://spark.apache.org/docs/latest/running-on-kubernetes.html

## Monitor Spark application in a parallel terminal

#### To see at least one executor, run below-mentioned command while other terminal is still executing above spark-submit command 
    kubectl get po

#### above command will give will show additional Pods being created by Apache Spark native Kubernetes scheduler - one driver and at least one executor
#### above command will show a Spark-pi-ID-driver which is spark-pi-d2786d83ebd6d678-driver in this case created while running spark-submit command
#### To check job's elapsed time, executing following code in another terminal window which allows to execute commands within Spark driver running in a POD
    kubectl logs spark-pi-d2786d83ebd6d678-driver |grep "Job 0 finished:"

#### knowing what value for Pi application came up with by following command, it will show pi value that application calculated while running on pod
    kubectl logs spark-pi-d2786d83ebd6d678-driver |grep "Pi is roughly "