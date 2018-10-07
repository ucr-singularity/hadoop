# hadoop

Available on Singularity Hub at https://www.singularity-hub.org/collections/1636.

A Hadoop, Spark, and Cassandra Singularity recipe. This is not completely self
contained - the configurations for each of the services are not included
in the repo. The cluster is designed to have four nodes:

* One node runs a Hadoop primary namenode and resourcemanager. There's no 
  secondary namenode.
* The other 3 nodes run hadoop nodemanager and HDFS.  They also run Cassandra.

Spark is configured to use Hadoop for running jobs.


