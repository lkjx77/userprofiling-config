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

Configure Hadoop
$ cd /usr/local/hadoop/etc/hadoop
$ vi core-site.xml
#Paste following between <configuration>

<configuration>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>


$ vi yarn-site.xml
#Paste following between <configuration>


<configuration>

<!-- Site specific YARN configuration properties -->

    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
</configuration>


$ mv mapred-site.xml.template mapred-site.xml
$ vi mapred-site.xml
#Paste following between <configuration>

<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>


$ cd ~
$ mkdir -p mydata/hdfs/namenode
$ mkdir -p mydata/hdfs/datanode
$ cd /usr/local/hadoop/etc/hadoop
$ vi hdfs-site.xml
Paste following between <configuration> tag
<configuration>
<property>
    <name>dfs.replication</name>
    <value>1</value>
</property>
<property>
    <name>dfs.namenode.name.dir</name>
    <value>file:/home/kim/mydata/hdfs/namenode</value>
</property>
<property>
    <name>dfs.datanode.data.dir</name>
    <value>file:/home/hduser/mydata/hdfs/datanode</value>
</property>
</configuration>
 

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







