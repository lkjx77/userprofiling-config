userprofiling-config
====================

Exploratory project for Web based User-behavior analytics

userprofiling-config
userprofiling-config
====================

Exploratory project for Web based User-behavior analytics

Install Hadoop
In this tutorial I am going to guide you through setting up hadoop 2.2.0 environment on Ubuntu.

$ sudo apt-get install openjdk-7-jdk

$ java -version

java version "1.7.0_25"

$ sudo apt-get install openssh-server

Add Hadoop Group and User
$ sudo addgroup hadoop

$ sudo adduser --ingroup hadoop hduser

$ sudo adduser hduser sudo

After user is created, re-login into ubuntu using hduser

Setup SSH Certificate

$ ssh-keygen -t rsa -P ''

$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

$ ssh localhost

Download Hadoop 2.2.0

$ cd ~

$ wget http://www.trieuvan.com/apache/hadoop/common/hadoop-2.2.0/hadoop-2.2.0.tar.gz

$ sudo tar vxzf hadoop-2.2.0.tar.gz -C /usr/local

$ cd /usr/local

$ sudo mv hadoop-2.2.0 hadoop

$ sudo chown -R hduser:hadoop hadoop

Setup Hadoop Environment Variables

$cd ~

$vi .bashrc

paste following to the end of the file 

#Hadoop variables
export JAVA_HOME=/usr/lib/jvm/jdk/
export HADOOP_INSTALL=/usr/local/hadoop
export PATH=$PATH:$HADOOP_INSTALL/bin
export PATH=$PATH:$HADOOP_INSTALL/sbin
export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_HOME=$HADOOP_INSTALL
export HADOOP_HDFS_HOME=$HADOOP_INSTALL
export YARN_HOME=$HADOOP_INSTALL
###end of paste
