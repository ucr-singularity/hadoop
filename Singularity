Bootstrap: docker
From: centos:7

%post 
# Update
yum update -y
# Install some useful packages
yum install -y vim screen tmux wget elinks less net-tools bind-utils nmap-ncat git unzip
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

# HADOOP
yum install -y java-1.8.0-openjdk python2-pip python-devel java-1.8.0-openjdk-devel
pip install py4j
cd /usr/local/src
wget http://www-us.apache.org/dist/hadoop/common/hadoop-2.7.7/hadoop-2.7.7.tar.gz
shasum_actual=`sha512sum hadoop-2.7.7.tar.gz | awk '{ print $1 }'`
shasum_should_be=17c8917211dd4c25f78bf60130a390f9e273b0149737094e45f4ae5c917b1174b97eb90818c5df068e607835120126281bcc07514f38bd7fd3cb8e9d3db1bdde
[[ "${shasum_should_be}" == "${shasum_actual}" ]] || exit -1
tar xzf hadoop-2.7.7.tar.gz

# CASSANDRA
cd /usr/local/src
wget https://archive.apache.org/dist/cassandra/3.11.3/apache-cassandra-3.11.3-bin.tar.gz
shasum_actual=`sha1sum apache-cassandra-3.11.3-bin.tar.gz | awk '{ print $1 }'`
shasum_should_be=dbc6ddbd074d74da97eff66db9699b5ce28ec6f0
[[ "${shasum_should_be}" == "${shasum_actual}" ]] || exit -1
tar xzf apache-cassandra-3.11.3-bin.tar.gz
sed -i 's/$CASSANDRA_HOME\/logs/\/$CASSANDRA_LOG_DIR/' /usr/local/src/apache-cassandra-3.11.3/bin/cassandra

cat > /usr/local/bin/cqlsh << \ENDOFFILE
#!/bin/bash
ip=`ifconfig | head -2 | grep inet | awk '{print $2 }'`
/usr/local/src/apache-cassandra-3.11.3/bin/cqlsh "$@" $ip
ENDOFFILE
chmod 0755 /usr/local/bin/cqlsh

# GRADLE
cd /usr/local/src
wget https://services.gradle.org/distributions/gradle-4.10.2-all.zip
shasum_actual=`sha256sum gradle-4.10.2-all.zip | awk '{ print $1 }'`
shasum_should_be=b7aedd369a26b177147bcb715f8b1fc4fe32b0a6ade0d7fd8ee5ed0c6f731f2c
unzip -d /usr/local/src/gradle-4.10.2 gradle-4.10.2-all.zip

# Clean up
yum clean all

%environment
export HADOOP_HOME=/usr/local/src/hadoop-2.7.7
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
export CASSANDRA_HOME=/usr/local/src/apache-cassandra-3.11.3
export PATH=${PATH}:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin:/usr/local/src/spark-2.3.1-bin-hadoop2.7/bin:/usr/local/src/apache-cassandra-3.11.3/bin:/usr/local/src/gradle-4.10.2/bin
export HADOOP_CLASSPATH=$(hadoop classpath)
export HADOOP_CLASSPATH=$HADOOP_CLASSPATH:$CASSANDRA_HOME/lib
export LD_LIBRARY_PATH=$HADOOP_HOME/lib/native:/lib64:/usr/lib64
export SPARK_LOCAL_IP=`ifconfig | grep 'inet 10.0.' | awk '{ print $2 }'`
export SPARK_MASTER_OPTS=“-Dspark.master=yarn”

TEMP=`ps x --no-headers | grep -o 'singularity-instance:.*]' | head -1`
if [[ "$TEMP" == *"_1"* ]]; then
    export CASSANDRA_LOG_DIR=~/cassandra/logs/cassandra_main
    export CASSANDRA_CONF=/opt/hadoop/home/$USER/cassandra/conf/cassandra_main
    export HADOOP_LOG_DIR=~/hadoop/logs/hadoop.d/namenode
    export YARN_LOG_DIR=~/hadoop/logs/yarn.d/namenode
    export HADOOP_PID_DIR=~/hadoop/pids/namenode
    export YARN_PID_DIR=~/hadoop/pids/namenode
    export HADOOP_CONF_DIR=~/hadoop/namenode-conf.d
    export YARN_CONF_DIR=~/hadoop/namenode-conf.d
elif [[ "$TEMP" == *"_2"* ]]; then
    export CASSANDRA_LOG_DIR=~/cassandra/logs/cassandra_node_1
    export CASSANDRA_CONF=/opt/hadoop/home/$USER/cassandra/conf/cassandra_node_1
    export HADOOP_LOG_DIR=~/hadoop/logs/hadoop.d/datanode-1
    export YARN_LOG_DIR=~/hadoop/logs/yarn.d/datanode-1
    export HADOOP_PID_DIR=~/hadoop/pids/datanode-1
    export YARN_PID_DIR=~/hadoop/pids/datanode-1
    export HADOOP_CONF_DIR=~/hadoop/datanode-1-conf.d
    export YARN_CONF_DIR=~/hadoop/datanode-1-conf.d
elif [[ "$TEMP" == *"_3"* ]]; then
    export CASSANDRA_LOG_DIR=~/cassandra/logs/cassandra_node_2
    export CASSANDRA_CONF=/opt/hadoop/home/$USER/cassandra/conf/cassandra_node_2
    export HADOOP_LOG_DIR=~/hadoop/logs/hadoop.d/datanode-2
    export YARN_LOG_DIR=~/hadoop/logs/yarn.d/datanode-2
    export HADOOP_PID_DIR=~/hadoop/pids/datanode-2
    export YARN_PID_DIR=~/hadoop/pids/datanode-2
    export HADOOP_CONF_DIR=~/hadoop/datanode-2-conf.d
    export YARN_CONF_DIR=~/hadoop/datanode-2-conf.d
else
    export CASSANDRA_LOG_DIR=~/cassandra/logs/cassandra_node_3
    export CASSANDRA_CONF=/opt/hadoop/home/$USER/cassandra/conf/cassandra_node_3
    export HADOOP_LOG_DIR=~/hadoop/logs/hadoop.d/datanode-3
    export YARN_LOG_DIR=~/hadoop/logs/yarn.d/datanode-3
    export HADOOP_PID_DIR=~/hadoop/pids/datanode-3
    export YARN_PID_DIR=~/hadoop/pids/datanode-3
    export HADOOP_CONF_DIR=~/hadoop/datanode-3-conf.d
    export YARN_CONF_DIR=~/hadoop/datanode-3-conf.d
fi


## Hadoop namenode

%appenv hadoop-namenode
export HADOOP_LOG_DIR=~/hadoop/logs/hadoop.d/namenode
export YARN_LOG_DIR=~/hadoop/logs/yarn.d/namenode
export HADOOP_PID_DIR=~/hadoop/pids/namenode
export YARN_PID_DIR=~/hadoop/pids/namenode
export HADOOP_CONF_DIR=~/hadoop/namenode-conf.d

%apprun hadoop-namenode
if [ "$1" == "format" ]; then
    echo "Formatting the namenode with a default name..."
    $HADOOP_HOME/bin/hdfs namenode -format hadoop_cluster

elif [ "$1" == "start" ]; then
    echo "Starting the namenode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh start namenode

    echo "Starting the resourcemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh start resourcemanager
    

elif [ "$1" == "stop" ]; then
    echo "Stopping the namenode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh stop namenode

    echo "Stopping the resourcemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh stop resourcemanager

else
  echo "Incorrect argument for application."
  echo "Correct usage is: singularity run --app hadoop-namenode <singularity image> [ ARGUMENT ]"
  echo "Arguments: format, start, stop"
fi

%apphelp hadoop-namenode
This app runs a Hadoop namenode

## Hadoop data node 1
%appenv hadoop-data-node-1
export HADOOP_LOG_DIR=~/hadoop/logs/hadoop.d/datanode-1
export YARN_LOG_DIR=~/hadoop/logs/yarn.d/datanode-1
export HADOOP_PID_DIR=~/hadoop/pids/datanode-1
export YARN_PID_DIR=~/hadoop/pids/datanode-1
export HADOOP_CONF_DIR=~/hadoop/datanode-1-conf.d

%apprun hadoop-data-node-1
if [ "$1" == "start" ]; then
    echo "Starting the datanode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh start datanode

    echo "Startring the nodemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh start nodemanager
    
    echo "Starting cassandra..."
    cassandra 2>&1 > /dev/null

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

%apphelp hadoop-data-node-1
This app runs a Hadoop data node

## Hadoop data node 2
%appenv hadoop-data-node-2
export HADOOP_LOG_DIR=~/hadoop/logs/hadoop.d/datanode-2
export YARN_LOG_DIR=~/hadoop/logs/yarn.d/datanode-2
export HADOOP_PID_DIR=~/hadoop/pids/datanode-2
export YARN_PID_DIR=~/hadoop/pids/datanode-2
export HADOOP_CONF_DIR=~/hadoop/datanode-2-conf.d

%apprun hadoop-data-node-2
if [ "$1" == "start" ]; then
    echo "Starting the datanode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh start datanode

    echo "Startring the nodemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh start nodemanager
    
    echo "Starting cassandra..."
    cassandra 2>&1 > /dev/null

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

%apphelp hadoop-data-node-2
This app runs a Hadoop data node

## Hadoop data node 3
%appenv hadoop-data-node-3
export HADOOP_LOG_DIR=~/hadoop/logs/hadoop.d/datanode-3
export YARN_LOG_DIR=~/hadoop/logs/yarn.d/datanode-3
export HADOOP_PID_DIR=~/hadoop/pids/datanode-3
export YARN_PID_DIR=~/hadoop/pids/datanode-3
export HADOOP_CONF_DIR=~/hadoop/datanode-3-conf.d

%apprun hadoop-data-node-3
if [ "$1" == "start" ]; then
    echo "Starting the datanode..."
    $HADOOP_HOME/sbin/hadoop-daemon.sh start datanode

    echo "Startring the nodemanager..."
    $HADOOP_HOME/sbin/yarn-daemon.sh start nodemanager
    
    echo "Starting cassandra..."
    cassandra 2>&1 > /dev/null

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

%apphelp hadoop-data-node-3
This app runs a Hadoop data node
