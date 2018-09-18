Bootstrap: docker
From: centos:7

%post 
# Update
yum update -y
# Install some useful packages
yum install -y vim screen tmux wget elinks less net-tools bind-utils nmap-ncat
# Install/enable the EPEL repository
yum install -y epel-release

# SPARK
yum install -y java-1.8.0-openjdk python2-pip python-devel openssh-clients
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
export HADOOP_HOME=/usr/local/src/hadoop-2.7.7
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
export HADOOP_LOG_DIR=~/hadoop/logs/hadoop.d
export YARN_LOG_DIR=~/hadoop/logs/yarn.d
export PATH=${PATH}:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin

## Hadoop Single-machine cluster app

%apprun hadoop-single
if [ "$1" == "format" ]; then
    echo "Formatting the namenode with a default name..."
    HADOOP_HOME/bin/hdfs namenode -format hadoop_cluster

elif [ "$1" == "start" ]; then
    echo "Starting the namenode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh start namenode

    echo "Starting the datanode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh start datanode

    echo "Starting the resourcemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh start resourcemanager

    echo "Startring the nodemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh start nodemanager

elif [ "$1" == "stop" ]; then
    echo "Stopping the namenode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh stop namenode

    echo "Stopping the datanode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh stop datanode

    echo "Stopping the resourcemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh stop resourcemanager

    echo "Stopping the nodemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh stop nodemanager

else
  echo "Incorrect argument for application."
  echo "Correct usage is: singularity run --app hadoop-single <singularity image> [ ARGUMENT ]"
  echo "Arguments: format, start, stop"
fi

%apphelp hadoop-single
This app runs a single-machine hadoop cluster.
Run the app using "singularity run --app hadoop-single <singularity image> [ ARGUMENT ]"
ARGUMENTS:
- format  :  Formats the cluster with a default name of "hadoop_cluster"
- start   :  Starts the namenode, datanode, resource manager and node manager daemons.
- stop    :  Stops the namenode, datanode, resource manager and node manager daemons.

## Hadoop namenode

%apprun hadoop-namenode
if [ "$1" == "format" ]; then
    echo "Formatting the namenode with a default name..."
    $HADOOP_HOME/bin/hdfs namenode -format hadoop_cluster

elif [ "$1" == "start" ]; then
    echo "Starting the namenode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh start namenode

    echo "Starting the resourcemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh start resourcemanager

    echo "Startring the nodemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh start nodemanager

elif [ "$1" == "stop" ]; then
    echo "Stopping the namenode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh stop namenode

    echo "Stopping the resourcemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh stop resourcemanager

    echo "Stopping the nodemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh stop nodemanager

else
  echo "Incorrect argument for application."
  echo "Correct usage is: singularity run --app hadoop-namenode <singularity image> [ ARGUMENT ]"
  echo "Arguments: format, start, stop"
fi

%apphelp hadoop-namenode
This app runs a Hadoop namenode

## Hadoop data node

%apprun hadoop-data-node
if [ "$1" == "start" ]; then
    echo "Starting the datanode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh start datanode

    echo "Startring the nodemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh start nodemanager

elif [ "$1" == "stop" ]; then
    echo "Stopping the datanode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh stop datanode

    echo "Stopping the nodemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh stop nodemanager

else
  echo "Incorrect argument for application."
  echo "Correct usage is: singularity run --app hadoop-namenode <singularity image> [ ARGUMENT ]"
  echo "Arguments: start, stop"
fi

%apphelp hadoop-data-node
This app runs a Hadoop data node
