Bootstrap: docker
From: centos:7

%post 
yum -y update
yum -y install vim screen tmux
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

