Bootstrap: docker
From: centos:7

%post 
# Update
yum update -y
# Install some useful packages
yum install -y vim screen tmux wget
# Install/enable the EPEL repository
yum install -y epel-release

# HADOOP 
yum install -y java-1.8.0-openjdk python2-pip python-devel
cd /usr/local/src
wget http://mirrors.gigenet.com/apache/hadoop/common/hadoop-2.9.1/hadoop-2.9.1-src.tar.gz
shasum_actual=`sha512sum hadoop-2.9.1-src.tar.gz | awk '{ print $1 }'`
shasum_should_be=8b3140d6e28337f575e87663a5b76ef1027cb1d41ac06a82bf69cf47d231c71c3a2cab736809361db49687132252693659ae1f084d2382711074808a73a40c5b
[[ "${shasum_should_be}" == "${shasum_actual}" ]] || exit -1
pip install py4j

# SPARK
cd /usr/local/src
wget https://archive.apache.org/dist/spark/spark-2.3.1/spark-2.3.1-bin-without-hadoop.tgz
shasum_actual=`sha512sum spark-2.3.1-bin-without-hadoop.tgz | awk '{ print $1 }'`
shasum_should_be=9db20723c7591b1127dd549653a4a9b9480cdbc565c57d397d7ddb30ae64ab583fb5d5aee21a45d3417de1a53ed7edd026693aedd61d90c6bde921aa08391a0a
[[ "${shasum_should_be}" == "${shasum_actual}" ]] || exit -1
tar xzvf spark-2.3.1-bin-without-hadoop.tgz
pip install pyspark

# Clean up
yum clean all

%environment
export HADOOP_PLACEHOLDER=hadoop_placeholder

## Hadoop namenode

%apprun hadoop-namenode
exec echo "HADOOP NAMENODE PLACEHOLDER"

%appinstall hadoop-namenode
touch /hadoop-namenode-app-available

%appenv hadoop-namenode
export HADOOP_NAMENODE_PLACEHOLDER=hadoop_namenode_placeholder

%apphelp hadoop-namenode
This app runs a Hadoop namenode

## Hadoop data node

%apprun hadoop-data-node
exec echo "HADOOP DATANODE PLACEHOLDER"

%appinstall hadoop-datanode
touch /hadoop-datanode-app-available

%appenv hadoop-datanode
export HADOOP_DATANODE_PLACEHOLDER=hadoop_datanode_placeholder

%apphelp hadoop-datanode
This app runs a Hadoop data node

## Hadoop - all services

%apprun hadoop-all
exec echo "HADOOP ALL SERVICES PLACEHOLDER"

%appinstall hadoop-all
touch /hadoop-all-services-app-available

%appenv hadoop-all
export HADOOP_ALL_SERVICE_PLACEHOLDER=hadoop_all_service_placeholder

%apphelp hadoop-all
This app runs all Hadoop services

