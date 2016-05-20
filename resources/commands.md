	1.	node1 : HDFS NameNode + Spark Master
	2.	node2 : YARN ResourceManager + JobHistoryServer + ProxyServer
	3.	node3 : HDFS DataNode + YARN NodeManager + Spark Slave
	4.	node4 : HDFS DataNode + YARN NodeManager + Spark Slave


Node 1:
	1.	$HADOOP_PREFIX/bin/hdfs namenode -format myhadoop
	2.	$HADOOP_PREFIX/sbin/hadoop-daemon.sh --config $HADOOP_CONF_DIR --script hdfs start namenode
	3.	$HADOOP_PREFIX/sbin/hadoop-daemons.sh --config $HADOOP_CONF_DIR --script hdfs start datanode

Node 2:

	1.	$HADOOP_YARN_HOME/sbin/yarn-daemon.sh --config $HADOOP_CONF_DIR start resourcemanager
	2.	$HADOOP_YARN_HOME/sbin/yarn-daemons.sh --config $HADOOP_CONF_DIR start nodemanager
	3.	$HADOOP_YARN_HOME/sbin/yarn-daemon.sh start proxyserver --config $HADOOP_CONF_DIR
	4.	$HADOOP_PREFIX/sbin/mr-jobhistory-daemon.sh start historyserver --config $HADOOP_CONF_DIR

Test Yarn:
yarn jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar pi 2 100

Start Spark in Standalone Mode
Node1: 	1.	$SPARK_HOME/sbin/start-all.sh

http://10.211.55.101:50070/dfshealth.html#tab-overview
http://10.211.55.102:8088/cluster
http://10.211.55.102:19888/jobhistory
http://10.211.55.101:8080/



echo "foo foo quux labs foo bar quux" | /vagrant/resources/scripts/mapper.py
echo "foo foo quux labs foo bar quux" | /vagrant/resources/scripts/mapper.py | sort -k1,1 | /vagrant/resources/scripts/reducer.py

hadoop fs -ls /tmp/
hadoop fs -put /vagrant/resources/books /tmp

hadoop jar /usr/local/hadoop-2.6.0/share/hadoop/tools/lib/hadoop-streaming-2.6.0.jar \
	-file /vagrant/resources/scripts/mapper.py    -mapper /vagrant/resources/scripts/mapper.py \
	-file /vagrant/resources/scripts/reducer.py   -reducer /vagrant/resources/scripts/reducer.py \
	-input /tmp/books/* -output /tmp/books-output

hadoop dfs -ls /tmp/books-output
hadoop dfs -cat /tmp/books-output/part-00000 | less
