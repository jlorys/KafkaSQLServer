# Kafka intro

https://dzone.com/articles/running-apache-kafka-on-windows-os
https://www.linkedin.com/learning/kafka-essential-training

:fireworks: [1](#1) Downloading the Required Files <br />
:fireworks: [2](#2) JRE Setup. In the order to use Kafka, you have to have JRE installed. Once you do this <br />

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

Important: Please ensure that your ZooKeeper instance is up and running before starting a Kafka server. <br />
•	Go to your Kafka installation directory: C:\kafka_2.11-0.9.0.0\ <br />
•	Open a command prompt here by pressing Shift + right click and choose the “Open command window here” option). <br />
•	Now type  <br />

```powershell
.\bin\windows\kafka-server-start.bat .\config\server.properties 
```
    
•	Now your Kafka Server is up and running, you can create topics to store messages. Also, we can produce or consume data from Java or Scala code or directly from the command prompt. <br />







