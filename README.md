# Zookeeper Concepts:
ZooKeeper is an open-source distributed coordination service designed to manage and coordinate large, distributed applications. 
It provides essential services like configuration management, synchronization, and distributed naming, enabling distributed applications 
to function smoothly by handling critical coordination tasks. Zookeeper was originally developed by **Yahoo!**

## Key Features of Apache ZooKeeper:
<ol>
  <li><b>Centralized Coordination:</b> Acts as a centralized repository for managing distributed applications.</li>
  <li><b>High Availability:</b> Supports a replicated, fault-tolerant architecture for reliability.</li>
  <li><b>Atomicity:</b> Ensures that updates to data are atomic.</li>
  <li><b>Sequential Consistency:</b> Guarantees order consistency for client updates.</li>
  <li><b>Eventual Consistency:</b> Allows all nodes to eventually synchronize.</li>
  <li><b>Watch Mechanism:</b> Provides a notification system for clients to subscribe to changes in the data.</li>
</ol>

## Typical Use Cases:
<ol>
<li><b>Distributed Locking:</b> Implements locks for resource coordination among distributed systems.</li>
<li><b>Leader Election:</b> Helps select a leader in distributed systems.</li>
<li><b>Configuration Management:</b> Stores configuration data for applications in a centralized way.</li>
<li><b>Service Discovery:</b> Maintains the location and availability of services.</li>
<li><b>Queue Management:</b> Manages distributed task queues.</li>
</ol>

## ZooKeeper Data Model:
ZooKeeper uses a hierarchical namespace similar to a file system:

<ul>
  <li> ZNodes: The fundamental entities in ZooKeeper, akin to directories or files in a file system.</li>
  <ul>
    <li type="disc"> Ephemeral ZNodes: Exist only as long as the session with the client is active.</li>
    <li type="disc">Persistent ZNodes: Remain until explicitly deleted.</li>
    <li type="disc">Sequential ZNodes: Have unique, sequential identifiers appended to their names. (Can be Persistent or Ephemeral)</li>
  </ul>  
</ul>

## Architecture:
ZooKeeper operates in a replicated architecture with the following roles:<br>
<ul>
  <li><b>Leader:</b> Handles all write requests and synchronizes updates with followers.</li>
  <li><b>Followers:</b> Handle read requests and participate in consensus (e.g., Paxos or Zab protocol).</li>
  <li><b>Clients:</b> Applications or systems interacting with ZooKeeper for coordination.</li>
</ul>

## Benefits:
<ul>
  <li>Simplifies the development of distributed applications.</li>
  <li>Ensures reliable and consistent coordination.</li>
  <li>Handles challenges like partial failures and synchronization in distributed environments.</li>
</ul>
  
## Integration:
ZooKeeper is widely used in technologies like:
<ul>
  <li>Apache Kafka (to maintain broker metadata and consumer group offsets).</li>
  <li>For coordination between brokers in the cluster of ActiveMQ Artemis</li>
  <li>Apache Hadoop and HBase (for resource coordination).</li>
  <li>Apache Storm and other distributed systems.</li>
</ul>

## Key Points:
<ul>
  <li type="square">Zookeeper is designed in a way that u can model your problem in terms of zookeeper.</li>
  <li type="square">Zookeeper server can run in <b>standalone</b> mode or <b>multi-node-cluster</b> mode.
    <ul>
      <li type = "circle"><b>Standalone</b> mode means when only one server is running.</li>
      <li type = "circle"><b>Multi-node-cluster</b> mode means when multiple servers are running.</li>
    </ul></li>
  <li type="square"><b>Installation and running zookeeper (Standalone mode):</b>
    <ol>
      <li>After downloading zookeeper and extracting it, we need to rename conf/zoo_sample.cfg file to conf/zoo.cfg because Zookeeper treats this as default config file.</li> 
      <li>In zoo.cfg file, we have to change the value of <b>dataDir</b> (E.g., dataDir=C:/Softwares/zookeeper/apache-zookeeper-3.8.4/data)</li>
      <li>Go to bin folder and run command <b>zkServer.cmd</b> to tun the zookeeper server.</li>
      <li>If we run <b>start zkServer.cmd</b> then this will launch ZooKeeper in a new command prompt window.</li>
      <li>Now if we want to specify configuration file in the command then the command is <b>bin\zkServer.cmd conf\zoo.cfg</b> or <b>zkServer.cmd "C:\Softwares\zookeeper\apache-zookeeper-3.8.4\conf\zoo.cfg"</b>. Specifying configuration file is optional but it will be required while starting multiple servers with different config files to form multi-node-cluster (called Zookeeper Ensemble).</li>
      <li>If this error is encountered while executing command which contains zoo.cfg => <b>java.lang.NumberFormatException: For input string: "C:zookeeper\zookeeper3.8.4\bin\..\conf\zoo.cfg"</b> then open the zkServer.cmd and add this line : <b>if not "%1"=="" (set ZOOCFG=%1)</b> in zkServer.cmd. Then find this line : <br><b><p>call %JAVA% "-Dzookeeper.log.dir=%ZOO_LOG_DIR%" "-Dzookeeper.log.file=%ZOO_LOG_FILE%" "-XX:+HeapDumpOnOutOfMemoryError" "-XX:OnOutOfMemoryError=cmd /c taskkill /pid %%%%p /t /f" -cp "%CLASSPATH%" %ZOOMAIN% "%ZOOCFG%" %*</p></b> Delete <b>%*</b> from the end. For explanation see : https://stackoverflow.com/questions/11765015/zookeeper-not-starting</li>
      <li>Zookeeper client can be run using the command <b>zkCli.cmd -server localhost:2181</b></li>
    </ol>
  </li>
  <li type="square"><b>Running zookeeper in multi-node-cluster mode:</b></li>
  <ol>
    <li>Minimum recommended node is 3. Most common production setup is 5 nodes.</li>
    <li>The number of nodes should be odd.</li>
    <li>The replicated group od servers is called as <b>quorum</b>.</li>
    <li>One of the server acts as leader and other servers act as followers. As soon as leader fails new leader is elected.</li>
    <li>Zookeeper client can be run using the command <b>zkCli.cmd -server localhost:2181,localhost:2182,localhost:2183</b></li>
    <li>Zookeeper client will not give error untill majority of the servers are running.</li>
  </ol>
</ul>
