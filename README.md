Apache Spark on Docker
==========

[![DockerPulls](https://img.shields.io/docker/pulls/sequenceiq/spark.svg)](https://cloud.docker.com/app/rohitbarnwal7/repository/docker/rohitbarnwal7/spark/general)


This repository contains a Docker file to build a Docker image with Apache Spark. This Docker image depends on our previous [Hadoop Docker](https://github.com/sequenceiq/hadoop-docker) image, available at the SequenceIQ [GitHub](https://github.com/sequenceiq) page.
The base Hadoop Docker image is also available as an official [Docker image](https://registry.hub.docker.com/u/sequenceiq/hadoop-docker/).

##Pull the image from Docker Repository
```
docker pull rohitbarnwal7/spark:2.2.0
```

## Building the image
```
docker build --rm -t rohitbarnwal7/spark:2.2.0 .
```

## Running the image

* if using boot2docker make sure your VM has more than 2GB memory
* in your /etc/hosts file add $(boot2docker ip) as host 'sandbox' to make it easier to access your sandbox UI
* open yarn UI ports when running container
```
docker run -it -p 8088:8088 -p 8042:8042 -p 4040:4040 -p 18080:18080 -h sandbox rohitbarnwal7/spark:2.2.0 bash

## Versions

```
Hadoop 2.7.0 and Apache Spark v2.2.0 on Centos
```
## Testing

There are two deploy modes that can be used to launch Spark applications on YARN.

### YARN-cluster mode

In yarn-cluster mode, the Spark driver runs inside an application master process which is managed by YARN on the cluster, and the client can go away after initiating the application.

Estimating Pi (yarn-cluster mode):

```
# execute the the following command which should write the "Pi is roughly 3.1418" into the logs
# note you must specify --files argument in cluster mode to enable metrics
spark-submit \
--class org.apache.spark.examples.SparkPi \
--files $SPARK_HOME/conf/metrics.properties \
--master yarn \
--deploy cluster \
--driver-memory 1g \
--executor-memory 1g \
--executor-cores 1 \
usr/local/spark/examples/jars/spark-examples_2.11-2.2.0.jar
```

### Submitting from the outside of the container
To use Spark from outside of the container it is necessary to set the YARN_CONF_DIR environment variable to directory with a configuration appropriate for the docker. The repository contains such configuration in the yarn-remote-client directory.

```
export YARN_CONF_DIR="`pwd`/yarn-remote-client"
```

Docker's HDFS can be accessed only by root. When submitting Spark applications from outside of the cluster, and from a user different than root, it is necessary to configure the HADOOP_USER_NAME variable so that root user is used.

```
export HADOOP_USER_NAME=root
```
