# Kafka intro for Windows

https://dzone.com/articles/running-apache-kafka-on-windows-os <br />
https://www.linkedin.com/learning/kafka-essential-training <br />

:fireworks: [1](#1) Downloading the Required Files <br />
:fireworks: [2](#2) JRE Setup. In the order to use Kafka, you have to have JRE installed. Once you do this <br />
:fireworks: [3](#3) ZooKeeper Installation <br />
:fireworks: [4](#4) Kafka setting up <br />
:fireworks: [5](#5) Running a Kafka Server <br />
:fireworks: [6](#6) Creating Topics <br />
:fireworks: [7](#7) Creating a Producer and Consumer to Test Server <br />
:fireworks: [8](#8) Some Other Useful Commands <br />
:fireworks: [9](#9) Set up a multibroker cluster <br />
:fireworks: [10](#10) Test fault-tolerance <br />

## 1
Downloading the Required Files <br />

•	Download Server JRE according to your OS and CPU architecture from http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html <br />
•	Download and install 7-zip from http://www.7-zip.org/download.html <br />
•	Download and extract ZooKeeper using 7-zip from http://zookeeper.apache.org/releases.html <br />
•	Download and extract Kafka using 7-zip from http://kafka.apache.org/downloads.html <br />

## 2
JRE Setup. In the order to use Kafka, you have to have JRE installed. Once you do this <br />

•	Open the system environment variables dialogue by opening Control Panel -> System -> Advanced system settings -> Environment Variables. <br />
•	Hit the New button in the User variables section, then type JAVA_HOME in Variable name and give your jre path in the Variable value (e.g. " C:\Program Files\Java\jdk1.8.0_144") <br />
•	Search for a Path variable in the “System Variable” section in the “Environment Variables” dialogue box you just opened. <br />
•	Edit the path and type “;%JAVA_HOME%\bin” at the end of the text already written there <br />
•	To confirm the Java installation, just open cmd and type “java -version.”  <br />

## 3
ZooKeeper Installation <br />

•	Go to your ZooKeeper config directory. For me its C:\zookeeper-3.4.7\conf <br />
•	Rename file “zoo_sample.cfg” to “zoo.cfg” <br />
•	Open zoo.cfg in any text editor, like Notepad; I prefer Notepad++. <br />
•	Find and edit dataDir=/tmp/zookeeper to :\zookeeper-3.4.7\data   <br />
•	Add an entry in the System Environment Variables as we did for Java. <br />
o	Add ZOOKEEPER_HOME = C:\zookeeper-3.4.7 to the System Variables. <br />
o	Edit the System Variable named “Path” and add ;%ZOOKEEPER_HOME%\bin;  <br />
•	You can change the default Zookeeper port in zoo.cfg file (Default port 2181). <br />
•	Run ZooKeeper by opening a new cmd and type zkserver. <br />
•	You will see the command prompt with some details <br />
Congratulations, your ZooKeeper is up and running on port 2181! <br />

## 4
Kafka setting up <br />

•	Go to your Kafka config directory. For me it's C:\kafka_2.11-0.9.0.0\config <br />
•	Edit the file “server.properties.” <br />
•	Find and edit the line log.dirs=/tmp/kafka-logs to log.dir= C:\kafka_2.11-0.9.0.0\kafka-logs. <br />
•	If your ZooKeeper is running on some other machine or cluster you can edit “zookeeper.connect:2181” to your custom IP and port. For this demo, we are using the same machine so there's no need to change. Also the Kafka port and broker.id are configurable in this file. Leave other settings as is. <br />
•	Your Kafka will run on default port 9092 and connect to ZooKeeper’s default port, 2181. <br />

## 5
Running a Kafka Server <br />

Important: Please ensure that your ZooKeeper instance is up and running before starting a Kafka server. <br />
•	Go to your Kafka installation directory: C:\kafka_2.11-0.9.0.0\ <br />
•	Open a command prompt here by pressing Shift + right click and choose the “Open command window here” option). <br />
•	Now type  <br />

```powershell
.\bin\windows\kafka-server-start.bat .\config\server.properties 
```

•	Now your Kafka Server is up and running, you can create topics to store messages. Also, we can produce or consume data from Java or Scala code or directly from the command prompt. <br />

## 6
Creating Topics <br />
•	Now create a topic with the name “test” and a replication factor of 1, as we have only one Kafka server running. If you have a cluster with more than one Kafka server running, you can increase the replication-factor accordingly, which will increase the data availability and act like a fault-tolerant system. <br />
•	Open a new command prompt in the location C:\kafka_2.11-0.9.0.0\bin\windows. <br />
•	Type the following command and hit Enter: <br />

```powershell
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```

## 7
Creating a Producer and Consumer to Test Server <br />

•	Open a new command prompt in the location C:\kafka_2.11-0.9.0.0\bin\windows <br />
•	To start a producer type the following command: <br />

```powershell
kafka-console-producer.bat --broker-list localhost:9092 --topic test
```

•	Again open a new command prompt in the same location as C:\kafka_2.11-0.9.0.0\bin\windows <br />
•	Now start a consumer by typing the following command: <br />
•	After kafka version 2.0 (>= 2.0): <br />

```powershell
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test
```

•	Now you will have two command prompts <br />
•	Now type anything in the producer command prompt and press Enter, and you should be able to see the message in the other consumer command prompt. <br />
•	If you are able to push and see your messages on the consumer side, you are done with Kafka setup. <br />

## 8 

Some Other Useful Commands <br />

•	List Topics: <br />

```powershell
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test
```

•	Describe Topic: <br />

```powershell
kafka-topics.bat --describe --zookeeper localhost:2181 --topic [Topic Name]
```

•	Read messages from the beginning, after version > 2.0: <br />

```powershell
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic [Topic Name] --from-beginning
```

•	Delete Topic: <br />

```powershell
kafka-run-class.bat kafka.admin.TopicCommand --delete --topic [topic_to_delete] --zookeeper localhost:2181
```

•	Ctrl + C to stop all things in every open window <br />

## 9
Set up a multibroker cluster <br />

In the main kafka folder: <br />

```powershell
copy .\config\server.properties .\config\server-1.properties
copy .\config\server.properties .\config\server-2.properties
```

Update the following properties in the new files <br />

```powershell
config/server-1.properties:
    broker.id=1
    listeners=PLAINTEXT://:9093
    log.dir=C:\kafka\kafka_2.13-2.6.0\kafka-logs1
config/server-2.properties:
    broker.id=2
    listeners=PLAINTEXT://:9094
    log.dir=C:\kafka\kafka_2.13-2.6.0\kafka-logs2
```

Then run below commands <br />

```powershell
.\bin\windows\kafka-server-start.bat .\config\server-1.properties
.\bin\windows\kafka-server-start.bat .\config\server-2.properties
```

Create a topic which will be replicated to all 3 nodes <br />

```powershell
.\bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 3 --partitions 1 --topic my-replicated-topic
```

See stats about our new topic <br />
```powershell
.\bin\windows\kafka-topics.bat --describe --zookeeper localhost:2181 --topic my-replicated-topic
```

See stats about our old test topic <br />
```powershell
.\bin\windows\kafka-topics.bat --describe --zookeeper localhost:2181 --topic test
```

## 10

Test fault-tolerance <br />

Send some messages to our replicated topic, then kill the producer <br />
```powershell
.\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic my-replicated-topic
```

Read messages from topic <br />
```powershell
.\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --from-beginning --topic my-replicated-topic
```

Now kill Broker 1 <br />
```powershell
wmic process where "caption = 'java.exe' and commandline like '%server-1.properties%'" get processid

taskkill /pid 6644 /f
```

Check which node is the leader for our topic now <br />
```powershell
.\bin\windows\kafka-topics.bat --describe --zookeeper localhost:2181 --topic my-replicated-topic
```







