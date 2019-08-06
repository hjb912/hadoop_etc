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

## flume

  	lume-ng agent  --conf-file flume-env.conf --name a1