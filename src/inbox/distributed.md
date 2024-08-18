# Big Data

## Hadoop

### What is Hadoop

Apache Hadoop is an open-source framework that is used to efficiently store and process large datasets ranging in size from gigabytes to petabytes of data.

Hadoop allows clustering multiple computers to analyze massive datasets in parallel more quickly.

Hadoop consists of four main modules: HDFS, YARN and MapReduce

* Master node (NameNode): If NameNode is down, HDFS is down
* Slave node (DataNode)
* Heartbeats: DataNode sends **Heartbeats** (“I am alive”) to NameNode. The default heartbeat interval is **3 seconds**. Block report ("I have block A & C") is sent every 6 hours

### Function of Hadoop

* **Store** big data.
* provide programming framework and platform for **data analysis**.

**Main Component**

* **HDFS**: store data distributedly (is the file system)
* MapReduce programming framework (can be replaced by **storm** or **spark**)
* **Yarn**(Yet Another Resource Negotiator): manage recourses for the cluster (like OS)

**MapReduce**

* **Map**: perform a function on individual values in a data set to create a new list of values (run in parallel)
    * \# of Mappers = # of splits \~= file size/block size.
* **Reduce**: Combine values in a data set to create a new value (run in parallel)
    * \# of Reducers is set by`job.setNumReduceTasks()`
* **Map phase**: master assigns each map task to a worker
    * **Partitioner**: # of partitions == # of Reduce tasks
    * **Split**: The logical division of data. By default, the split size is approximately equal to the HDFS Block size. Users change the number of mappers by changing the split size. (for more examples: [Split size vs Block size in Hadoop](https://stackoverflow.com/questions/30549261/split-size-vs-block-size-in-hadoop))
    * **Locality**: The“proximity” of the data with respect to the Mapper tasks working on the data. (Data-local: run local data; Rack-local: remote access)
* **Reduce phase**: master assigns each reduce task to a worker
    * **Shuffle/Copy**: moving map outputs to the reducers ==> It will cause network congestion
    * **Local Sort (on reducer side)**: Perform a merge sort by keys before presenting to the Reducer
    * **Reduce**: Actual reduce (By default each reducer will generate a separate output file (like part-000?), which will be stored in HDFS for reliability)

**YARN**

* **(Central) Resource Manager (RM)**： The ultimate authority that arbitrates resources among all the applications (MapReduce, Spark) in the cluster.
* **(Per-node) Node Manager (NM):** “worker” daemon running at each node; Launch the containers, monitor resource usage and report to Resource Manager.
* **(Per-application) Application Master (AM):** Negotiating resources from the RM and working with the Node Manager(s) to execute and monitor the tasks. Appear as “MRAppMaster"

**JVM memory**

* **Heap Memory** : the storage for Java objects.`-Xmx<size>` to set the maximum Java heap size. When the heap memory is **low**, JVM triggers **Garbage Collection (GC)** to reclaim the memory occupied by dead objects. (GC is triggered when your heap size is almost full (70-80%))
* **Non-Heap Memory**: used by JVM to store loaded Java classes (code) and other meta-data
* **Other**:used by JVM itself (JVM code, JVM internal structures), loaded profiler agent code, and data, etc.

**HDFS Basic Command**

```shell
# list
hdfs dfs -ls
​
# list certain dirctory
hdfs dfs -ls dir
​
# create a directory in HDFS
hdfs dfs -mkdir dir
​
# view a file content
hdfs dfs -cat myfile
​
# check disk space usage
hdfs dfs -du
​
# change file modes or Access Control Lists
hdfs dfs -chmod -R 777 file_path
​
# change the owner of files
hdfs dfs -chown owner:group file_path
​
# rm files
hdfs dfs -rm [-r] file_path
​
# Create blocksize=32MB data
hdfs dfs -Ddfs.blocksize=33554432 -cp local_path hadoop_path
```

**Application Properties**

e.g.

```shell
yarn jar /opt/hadoop-2.7.5/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.5.jar terasort\
-Dmapreduce.tasktracker.map.tasks.maximum=4\
-Dmapreduce.tasktracker.reduce.tasks.maximum=4\
-Dmapreduce.job.reduces=33\
-Dmapreduce.map.memory.mb=1500\
-Dmapreduce.reduce.memory.mb=1500\
-Dmapreduce.map.java.opts=-Xmx1350m\
-Dmapreduce.reduce.java.opts=-Xmx1350m\
-Dmapreduce.task.io.sort.mb=100\
-Dmapreduce.job.reduce.slowstart.completedmaps=0.05\
-Dmapreduce.map.speculative=false\
-Dmapreduce.reduce.speculative=false\
/user/syk/terainput10g-256m /user/syk/teraoutput10-256m
```

**HDFS block size**

* usage:
```shell
# set block size while inputing file into HDFS
# hdfs dfs -Ddfs.blocksize=33554432 -cp local_path hadoop_path
# choose the specific file
/user/syk/terainput10g-256m /user/syk/teraoutput10-256m
```

**mapreduce.job.reduces**
* Usage :

```shell
-Dmapreduce.job.reduces=33
```

**mapreduce.map.memory.mb**

* Usage :

```shell
-Dmapreduce.map.memory.mb=1500
```

**mapreduce.reduce.memory.mb**

* Usage :

```shell
-Dmapreduce.reduce.memory.mb=1500
```

**mapreduce.map.java.opts**

* Meaning: Java heap size for each map/reduce task (Default is 200 MB). If the mapper process runs out of heap memory , the mapper throws a java out of memory exceptions:
* Usage :

```shell
-Dmapreduce.map.java.opts=-Xmx1350m
```

**mapreduce.reduce.java.opts**

* Usage :

```shell
-Dmapreduce.reduce.java.opts=-Xmx1350m
```

**mapreduce.task.io.sort.mb**

* Meaning: Minimum number of streams to be merged at once during sorting.
* Usage :

```shell
-Dmapreduce.task.io.sort.mb=100
```

**reduce.slowstart.completedmaps**

* Usage :

```shell
-Dmapreduce.job.reduce.slowstart.completedmaps=0.05
```

**mapreduce.map.speculative**

* Usage :

```shell
-Dmapreduce.map.speculative=false
```

**mapreduce.reduce.speculative**

* Usage :

```shell
-Dmapreduce.reduce.speculative=false
```

**Tips**

**Runtime characteristics of TeraSort**

In general, the **map\***\* phase\*\* and **shuffle phase** are most time-consuming

* **map phase**:
    * block size should be in the proper range. **Make # of mapper <= sum parallelism of total containers.**
    * the memory of map jobs(the higher memory, the shorter **Average Map Time**)
* **shuffle phase**:
    * **mapreduce.task.io.sort.mb**: increase to reduce shuffle time
    * **reduce.slowstart.completedmaps**: If there is network congestion during shuffle, **increase** reduce.slowstart.completedmaps. If reduce.slowstart.completedmaps is too small, reduces will be stuck in the network shuffle phase for a long time, reducers cannot complete until all mappers are done.
    * increase the **number reduce job**
* **reduce phase**: mapreduce.job.reduces

**Resource constraints**

* Core number on physical machine
* Total memory of physical machine
* Docker containers' limites

**How to reduce GC time**

In general, increase the memory of JVM heap will reduce GC time.

In hadoop, mapreduce.task.io.sort.mb infects GC time. The smaller mapreduce.task.io.sort.mb, the short GC time （Recommended value: Use 1/4 to 1/2 of the map/reduce task Java heap size setting (in java.opts).）

**Should we enable mapreduce.map/reduce.speculative?**

Hadoop will predict the resouces of each node. If hadoop find certern node has limited resources to operate, other node which has more available resources will launch the same task. These two tasks will run simultaneously. If one of the task finished, the other task will be killed.

* If the file is small and the computing resources is large enough, we should disable it
* Otherwise, we should enable it

**How to improve task locality**

Make sure the configuration of Hadoop is proper and disable mapreduce.map/reduce.speculative.

**Spark Interview questions**

[https://cloud.tencent.com/developer/article/1842995](https://cloud.tencent.com/developer/article/1842995)

#### Spark Configuration Optimization

**Why Spark?**

There are many weaknesses of MapReduce:

* Support for Batch Processing only
* Many tasks do not have two-step process
* **Iterative applications**(e.g. machine learning) need to run the same Mapper and Reducer multiple times
* **Interactive applications** : Web applications for interactive queries (SQL), interactive data analysis (integrating with machine learning).
* **Streaming applications** : infinitive data stream, need to maintain aggregate state over time

**Basic Spark Knowledge**

* **Node**: Spark has **one** master node and **multiple** worker nodes (similar with master node and slave nodes in Hadoop )
    * Master node
    * Worker nodes
* **Application**: one Spark submit. Every Spark application is managed by a Driver.
* **Job**: when invoking an action (e.g., count(), or take(), collect()) on an RDD, a "job" is created.
* **Stage**: A job will then be decomposed into single or multiple stages based on the shuffle boundary (done internally by **Spark**, stages may be executed **parallelly**) .
* **Task**: represents a unit of work (may it is the atomic unit in Spark). In each stage, number \_of\_tasks == number\_of\_partitions( change repartition parameter can modify the number tasks in every stage).
* **Spark Driver**: the Spark application that launches the main method. Driver created first in task
* **Executors**: Executors are worker nodes’ JVM processes in charge of running individual tasks in a given Spark job.
* **RDD**(Resilient Distributed Datasets): a distributed data structure(Faster than MapReduce). Spark RDD is **logically partitioned** across many nodes so that they can be computed on different nodes of the cluster in parallel.
    * RDDs are divided into smaller chunks called **partitions**. (similiar the partitions in Hadoop), The maximum size of a partition = 2GB
    * RDDs are “immutable” (i.e., can’t be modified once created)
    * **Transformations**: Each transformation takes (one or more) RDDs, and outputs the transformed RDD **(another RDD**) by applying some function to the contents (e.g., map, filter, flatMap, union, groupBy, reduceByKey, sortByKey, join)
    * **Actions**: Return a value to the driver program after running a computation on the dataset (reduce, collect, count, take, countByKey) or, saveAsTextFile, saveAsObjectFile, ….
    * RDDs are stored Java Heap memory of the Executor
* **Cluster Mode**: the Driver runs inside an **ApplicationMaster** (use one Yarn container)
    * For applications in production, the best practice is to run the application in cluster mode (Spark Driver and Spark Executor are under the supervision of Yarn).
    * Cluster mode **is not supported for the interactive shell** (spark-shell & PySpark).
* **Client Mode**(Default): Spark driver runs on the **host** where the job is submitted (in this mode, ApplicationMaster is responsible only for requesting executor containers from YARN. After the containers start, the client communicates with the containers to schedule work.)
    * client mode is advantageous when you are debugging and wish to quickly see the output of your application. (Suffer higher latency if client is not in the cluster).
    * ApplicationMaster is still **needed** in client mode. However, \*\*the driver code is not running there.

**Application Properties**

Spark can load properties dynamically

i.e., if we launch with

```shell
spark-shell --master yarn
```

It will use the default properties from `spark-2.4.0-bin-hadoop2.7/conf/spark-defaults.conf`

We can load specific properties with

```shell
spark-shell --master yarn --executor-cores 4 --executor-memory 2G --conf spark.default.parallelism=66 --driver-cores 4
```

We can also use the neater method:

```shell
spark-shell --master yarn \
--executor-cores 4 \
--executor-memory 2G \
--conf spark.default.parallelism=66 \
--driver-cores 4
```

Tips:

`--conf`: arbitrary Spark configuration property in key=value format. (e.g., --conf spark.executor.instances=1 --conf spark.task.cpus=4)

**spark.executor.memory**

* Default: 1g
* Meaning: Amount of memory to use per **executor process**, in the same format as JVM memory strings with a size unit suffix ("k", "m", "g" or "t") (e.g. `512m`, `2g`).
* Tips: the more the better, but be careful of the total memory used.
*   Usage:

    ```shell
    --executor-memory 2G
    ```

**spark.executor.cores**

* Default: 1 in YARN mode, all the available cores on the worker in standalone and Mesos coarse-grained modes.
* Meaning: The number of cores to use on each executor. In standalone and Mesos coarse-grained modes, for more detail, see [this description](http://spark.apache.org/docs/latest/spark-standalone.html#Executors%20Scheduling).
* Tips: the more the better, but be careful of the total number of cores used.
* Usage:

```shell
--executor-cores 4
```

**spark.executor.instances**

* Default: spark.dynamicAllocation.minExecutors (**in COMP7305, the default value is 2**)
* Meaning: Initial number of executors to run if dynamic allocation is enabled. If `--num-executors` (or `spark.executor.instances`) is set and larger than this value, it will be used as the initial number of executors.
* Tips: combined with `spark.executor.cores` and `spark.executor.memory`. If the number of instance is too large, utility is limited and you will meet some errors
* Usage:

```shell
--num-executors 16
```

**spark.driver.memory**

* Default: 1g
* Meaning: Amount of memory to use for the driver process, i.e. where SparkContext is initialized, in the same format as JVM memory strings with a size unit suffix ("k", "m", "g" or "t") (e.g. `512m`, `2g`). _Note:_ In client mode, this config must not be set through the `SparkConf` directly in your application, because the driver JVM has already started at that point. Instead, please set this through the `--driver-memory` command line option or in your default properties file.
* Tips: TODO
*   Usage:

    ```shell
    --driver-memory 2g
    ```

**spark.driver.cores**

* Default: 1
* Meaning: Number of cores to use for the driver process, **only in cluster mode.**
* Tips:
* Usage:

```shell
--driver-cores 4
```

**HDFS blocksize**

* Default: 64mb
* Meaning: the block size of input file in HDFS
* Tips: It may not effect the performance of Spark. When data block is read from HDFS to Spark, the data is converted to Spark's internal RDD format (stored in JVM heap)
* Usage:

```shell
scala> sc.hadoopConfiguration.set("dfs.block.size","32m")
# another way: set the block size while reading the file in HDFS
# 32MB = 33554432B
hdfs dfs -Ddfs.blocksize=33554432 -copyFromLocal large_stories_32m /user/syk/
```

**spark.locality.wait**

* Default: 3s
* Meaning: How long to wait to launch a data-local task before giving up and launching it on a less-local node. The same wait will be used to step through multiple locality levels (process-local, node-local, rack-local and then any). It is also possible to customize the waiting time for each level by setting `spark.locality.wait.node`, etc. You should increase this setting if your tasks are long and see poor locality, but the default usually works well.
* Tips: if the file is small and the time of job is less than 2s, turn down it. If the time of jobs is much longer, turn up it.
* Usage: (not sure if this format is correct)

```shell
--conf spark.locality.wait=3s
```

**spark.speculation**

* Default: false
* Meaning: If set to "true", performs speculative execution of tasks. This means if one or more tasks are running slowly in a stage, they will be re-launched.
* Tips: the same with meaning
* Usage:

```shell
--conf spark.speculation=true
```

**spark.default.parallelism**

* Default: For distributed shuffle operations like `reduceByKey` and `join`, the largest number of partitions in a parent RDD. For operations like `parallelize` with no parent RDDs, it depends on the cluster manager:
    * Local mode: number of cores on the local machine
    * Mesos fine grained mode: 8
    * Others: total number of cores on all executor nodes or 2, whichever is larger
* Meaning: Default number of partitions in RDDs returned by transformations like `join`, `reduceByKey`, and `parallelize` when not set by user.
* Tips: 2 or 3 time of numbers of total cores
* Usage:

```shell
--conf spark.default.parallelism=200
```

#### Kafka

[https://www.youtube.com/watch?v=aj9CDZm0Glc](https://www.youtube.com/watch?v=aj9CDZm0Glc)

[https://www.youtube.com/watch?v=y9a3fldlvnI](https://www.youtube.com/watch?v=y9a3fldlvnI)

[https://www.youtube.com/watch?v=XFqm\_ILuhs0\&list=PLt1SIbA8guusxiHz9bveV-UHs\_biWFegU\&index=1](https://www.youtube.com/watch?v=XFqm\_ILuhs0\&list=PLt1SIbA8guusxiHz9bveV-UHs\_biWFegU\&index=1)

[https://kafka.apache.org/intro](https://kafka.apache.org/intro)

#### etcd

**raft algorithm**

官网上有不错的动画演示

一共有三个角色：

* leader： master node，也就是主节点
* follower： slave node，也就是从节点
* candidate：当主节点宕机的时候，candidate会出现

具体过程：

[https://blog.51cto.com/u\_15301988/3085390#Proxy\_\_Standby\_\_250](https://blog.51cto.com/u\_15301988/3085390#Proxy\_\_Standby\_\_250)

[http://jason5.work/etcd](http://jason5.work/etcd)面试常见问题/

**zookeeper**

ZAB algorithm

[https://www.youtube.com/watch?v=BhosKsE8up8](https://www.youtube.com/watch?v=BhosKsE8up8)
