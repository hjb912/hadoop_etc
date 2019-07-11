# HA hadoop directive for a clean centos 7

## java config

---
    yum -y update
    yum install java-1.8.0-openjdk
    yum -y install java-1.8.0-openjdk-devel
---

## donwload hadoop & edit ~/.bashrc

---
    export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64/jre
    export PATH="$JAVA_HOME:$PATH"
    source /usr/local/src/hadoop-3.2.0/etc/hadoop/hadoop-env.sh
    export HDFS_NAMENODE_USER="root"
    export HDFS_DATANODE_USER="root"
    export HDFS_SECONDARYNAMENODE_USER="root"
    export YARN_RESOURCEMANAGER_USER="root"
    export YARN_NODEMANAGER_USER="root"
    export HDFS_ZKFC_USER="root"
    export HDFS_JOURNALNODE_USER="root"
---

## install & generate ssh

---
    yum install -y openssh openssh-server openssh-clients openssl-libs
    ssh-keygen -t rsa -C ""
---

## install zookeeper

---
---

## watch /etc/hosts

---
    127.0.0.1 localhost
    172.17.0.2 namenode1
    172.17.0.3 slave1
    172.17.0.4 slave2
    172.17.0.5 zoo1
    172.17.0.6 namenode2
---

## config all *.xml
