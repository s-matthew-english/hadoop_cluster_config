Make sure you delete all the folders in here: 

    /usr/local/hadoop_work/hdfs/namenode/
    /usr/local/hadoop_work/hdfs/datanode
    /usr/local/hadoop_work/hdfs/namesecondary

Usually just have to do something along the lines of `rm -rf current/`.

Configured accordingly: 

**yarn-site.xml**

    <configuration>
      <property>
         <name>yarn.nodemanager.aux-services</name>
         <value>mapreduce_shuffle</value>
      </property>
      <property>
         <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
         <value>org.apache.hadoop.mapred.ShuffleHandler</value>
      </property>
      <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>master</value>
      </property>
    </configuration>

Turns out that setting `yarn.resourcemanager.hostname` is pretty important, that was tripping me up for a while :/

**core-site.xml**

    <configuration>
      <property>
         <name>fs.defaultFS</name>
         <value>hdfs://master:9000</value>
      </property>
    </configuration>

**mapred-site.xml**

    <configuration>
      <property>
         <name>mapreduce.framework.name</name>
         <value>yarn</value>
      </property>
    </configuration>

**hdfs-site.xml**


    <configuration>
      <property>
         <name>dfs.replication</name>
         <value>1</value>
      </property>
      <property>
         <name>dfs.namenode.name.dir</name>
         <value>file:/usr/local/hadoop_work/hdfs/namenode</value>
      </property>
      <property>
        <name>dfs.namenode.checkpoint.dir</name>
        <value>file:/usr/local/hadoop_work/hdfs/namesecondary</value>
      </property>
      <property>
         <name>dfs.datanode.data.dir</name>
         <value>file:/usr/local/hadoop_work/hdfs/datanode</value>
      </property>
      <property>
        <name>dfs.secondary.http.address</name>
        <value>172.31.46.85:50090</value>
      </property>
    </configuration>

**/etc/hosts**

    666.13.46.70  master
    666.13.35.80  slave1
    666.13.43.131 slave2

Essentially, you wanna be looking at this: 

[![enter image description here][1]][1]

Execute the commands...

Very simple tutorial: 

    hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar wordcount /input /output

For this [example](https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html):

    $HADOOP_HOME/bin/hadoop jar wc.jar WordCount /input /output


  [1]: https://i.stack.imgur.com/UgRHS.png
