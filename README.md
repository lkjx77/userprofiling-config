userprofiling-config
====================

Exploratory project for Web based User-behavior analytics

Install Hadoop
In this tutorial I am going to guide you through setting up hadoop 2.2.0 environment on Ubuntu.
Prerequistive
$ sudo apt-get install openjdk-7-jdk
$ java -version
java version "1.7.0_25"
OpenJDK Runtime Environment (IcedTea 2.3.12) (7u25-2.3.12-4ubuntu3)
OpenJDK 64-Bit Server VM (build 23.7-b01, mixed mode)
$ cd /usr/lib/jvm
$ ln -s java-7-openjdk-amd64 jdk

$ sudo apt-get install openssh-server

Add Hadoop Group and User
$ sudo addgroup hadoop
$ sudo adduser --ingroup hadoop hduser
$ sudo adduser hduser sudo
After user is created, re-login into ubuntu using hduser
Setup SSH Certificate
$ ssh-keygen -t rsa -P ''
...
Your identification has been saved in /home/hduser/.ssh/id_rsa.
Your public key has been saved in /home/hduser/.ssh/id_rsa.pub.
...
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

$ cd /usr/local/hadoop/etc/hadoop
$ vi hadoop-env.sh

#modify JAVA_HOME
export JAVA_HOME=/usr/lib/jvm/jdk/
Re-login into Ubuntu using hdser and check hadoop version
$ hadoop version
Hadoop 2.2.0
Subversion https://svn.apache.org/repos/asf/hadoop/common -r 1529768
Compiled by hortonmu on 2013-10-07T06:28Z
Compiled with protoc 2.5.0
From source with checksum 79e53ce7994d1628b240f09af91e1af4
This command was run using /usr/local/hadoop-2.2.0/share/hadoop/common/hadoop-common-2.2.0.jar
At this point, hadoop is installed.
Configure Hadoop
$ cd /usr/local/hadoop/etc/hadoop
$ vi core-site.xml
#Paste following between <configuration>


   fs.default.name
   hdfs://localhost:9000



$ vi yarn-site.xml
#Paste following between <configuration>


   yarn.nodemanager.aux-services
   mapreduce_shuffle


   yarn.nodemanager.aux-services.mapreduce.shuffle.class
   org.apache.hadoop.mapred.ShuffleHandler



$ mv mapred-site.xml.template mapred-site.xml
$ vi mapred-site.xml
#Paste following between <configuration>


   mapreduce.framework.name
   yarn



$ cd ~
$ mkdir -p mydata/hdfs/namenode
$ mkdir -p mydata/hdfs/datanode
$ cd /usr/local/hadoop/etc/hadoop
$ vi hdfs-site.xml
Paste following between <configuration> tag


   dfs.replication
   1
 
 
   dfs.namenode.name.dir
   file:/home/hduser/mydata/hdfs/namenode
 
 
   dfs.datanode.data.dir
   file:/home/hduser/mydata/hdfs/datanode
 

Format Namenode
hduser@ubuntu40:~$ hdfs namenode -format
Start Hadoop Service
$ start-dfs.sh
....
$ start-yarn.sh
....

hduser@ubuntu40:~$ jps
If everything is sucessful, you should see following services running
2583 DataNode
2970 ResourceManager
3461 Jps
3177 NodeManager
2361 NameNode
2840 SecondaryNameNode
Run Hadoop Example
hduser@ubuntu: cd /usr/local/hadoop
hduser@ubuntu:/usr/local/hadoop$ hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.2.0.jar pi 2 5

Number of Maps  = 2
Samples per Map = 5
13/10/21 18:41:03 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Wrote input for Map #0
Wrote input for Map #1
Starting Job
13/10/21 18:41:04 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
13/10/21 18:41:04 INFO input.FileInputFormat: Total input paths to process : 2
13/10/21 18:41:04 INFO mapreduce.JobSubmitter: number of splits:2
13/10/21 18:41:04 INFO Configuration.deprecation: user.name is deprecated. Instead, use mapreduce.job.user.name
