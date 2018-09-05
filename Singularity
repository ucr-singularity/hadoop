Bootstrap: docker
From: centos:7

%post 
# Update
yum update -y
# Install some useful packages
yum install -y vim screen tmux wget

# HADOOP 
yum install -y java-1.8.0-openjdk

# SPARK
cd /usr/local/src
wget https://www.apache.org/dyn/closer.lua/spark/spark-2.3.1/spark-2.3.1-bin-hadoop2.7.tgz
shasum_actual=`sha512sum spark-2.3.1-bin-hadoop2.7.tgz | awk '{ print $1 }'`
shasum_should_be=dc3a97f3d99791d363e4f70a622b84d6e313bd852f6fdbc777d31eab44cbc112ceeaa20f7bf835492fb654f48ae57e9969f93d3b0e6ec92076d1c5e1b40b4696
[[ "${shasum_should_be}" == "${shasum_actual}" ]] || exit -1
tar xzvf spark-2.3.1-bin-hadoop2.7.tgz
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

