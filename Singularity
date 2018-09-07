Bootstrap: docker
From: centos:7

%post 
# Update
yum update -y
# Install some useful packages
yum install -y vim screen tmux wget elinks
# Install/enable the EPEL repository
yum install -y epel-release

# SPARK
yum install -y java-1.8.0-openjdk python2-pip python-devel
pip install py4j
cd /usr/local/src
wget https://archive.apache.org/dist/spark/spark-2.3.1/spark-2.3.1-bin-hadoop2.7.tgz
shasum_actual=`sha512sum spark-2.3.1-bin-hadoop2.7.tgz | awk '{ print $1 }'`
shasum_should_be=dc3a97f3d99791d363e4f70a622b84d6e313bd852f6fdbc777d31eab44cbc112ceeaa20f7bf835492fb654f48ae57e9969f93d3b0e6ec92076d1c5e1b40b4696
[[ "${shasum_should_be}" == "${shasum_actual}" ]] || exit -1
tar xzf spark-2.3.1-bin-hadoop2.7.tgz
pip install pyspark

# HADOOP
yum install -y java-1.8.0-openjdk python2-pip python-devel
pip install py4j
cd /usr/local/src
wget http://www-us.apache.org/dist/hadoop/common/hadoop-2.7.7/hadoop-2.7.7.tar.gz
shasum_actual=`sha512sum hadoop-2.7.7.tar.gz | awk '{ print $1 }'`
shasum_should_be=17c8917211dd4c25f78bf60130a390f9e273b0149737094e45f4ae5c917b1174b97eb90818c5df068e607835120126281bcc07514f38bd7fd3cb8e9d3db1bdde
[[ "${shasum_should_be}" == "${shasum_actual}" ]] || exit -1
tar xzf hadoop-2.7.7.tar.gz

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

