Bootstrap: docker
From: centos:7

%post 
# Update
yum -y update
# Install some useful packages
yum -y install vim screen tmux wget
# Download the cloudera repo
wget https://archive.cloudera.com/cdh5/redhat/7/x86_64/cdh/cloudera-cdh5.repo -O /etc/yum.repos.d/cloudera-cdh5.repo
# Install all the Cloudera packages relevant to courses

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

