# HA hadoop directive for a clean centos 7

## java config

    yum -y update
    yum install java-1.8.0-openjdk
    yum -y install java-1.8.0-openjdk-devel


## donwload hadoop & edit ~/.bashrc


    export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.222.b10-0.el7_6.x86_64
    export PATH=$JAVA_HOME:$PATH
    source /usr/local/hadoop-3.2.0/etc/hadoop/hadoop-env.sh
    export HDFS_NAMENODE_USER="root"
    export HDFS_DATANODE_USER="root"
    export HDFS_SECONDARYNAMENODE_USER="root"
    export YARN_RESOURCEMANAGER_USER="root"
    export YARN_NODEMANAGER_USER="root"
    export HDFS_ZKFC_USER="root"
    export HDFS_JOURNALNODE_USER="root"
    export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar

	# for hive
    export HIVE_HOME=/usr/local/apache-hive-3.1.1-bin
    export PATH=$HIVE_HOME/bin:$PATH
    export HCAT_HOME=$HIVE_HOME/hcatalog
    # sqoop
    export SQOOP_HOME=/usr/local/sqoop-1.4.7.bin__hadoop-2.6.0
    export PATH=$SQOOP_HOME/bin:$PATH

    export LD_LIBRARY_PATH=$HADOOP_HOME/lib/native
    # spark ipython
    export PYSPARK_DRIVER_PYTHON=ipython


## install & generate ssh


    yum install -y openssh openssh-server openssh-clients openssl-libs
    ssh-keygen -t rsa -C ""


## install zookeeper



## vim /etc/hosts


    127.0.0.1 localhost
    172.17.0.2 namenode1
    172.17.0.3 slave1
    172.17.0.4 slave2
    172.17.0.5 zoo1
    172.17.0.6 namenode2

## hive config

  ~/conf add config hive-env.sh && hive-site.xml

## config all *.[xml,sh]

## sqoop

  sqoop-import --options-file `sqoop_import_config`
  sqoop-export --options-file `sqoop_export_config`

## flume

  flume-ng agent  --conf-file flume-env.conf --name a1

## spark

    add config:
        $SPARK_HOME/conf + hive-site.xml
        $SPARK_HOME/conf + spark-env.sh
    start servcer:
        $HIVE_HOME/bin/hiveserver2 for client query
        hive --service metastore for thrift

## kafka

    relies on Zookeeper

    add cluster config/server.properties:
        broker.id:0~x
        listeners=PLAINTEXT://:9092~909x
        log.dirs=/tmp/kafka-logs-x
    bin/kafka-server-start.sh config/server-x.properties &

    kafka as sink with flume:
        bin/flume-ng agent --conf-file flume-env.conf --name a1 -Dflume.root.logger=INFO,console
        ...
            a1.sinks.k1.type = org.apache.flume.sink.kafka.KafkaSink
            a1.sinks.k1.kafka.topic = mytopic
            a1.sinks.k1.kafka.bootstrap.servers = localhost:9092
            a1.sinks.k1.kafka.flumeBatchSize = 20
            a1.sinks.k1.kafka.producer.acks = 1
            a1.sinks.k1.kafka.producer.linger.ms = 1
            a1.sinks.k1.kafka.producer.compression.type = snappy
        ...

    kafka-* [producer, cosumer]


## spark-streaming

    kafka as source:
    demo:
        cd $SPARK_HOME
        && bin/spark-submit --packages org.apache.spark:spark-streaming-kafka-0-8_2.11:2.4.3 spark-direct-kafka.py localhost:port mytopic

