# sumana_kafka_installation

## Download kafka
- https://kafka.apache.org/quickstart

## Step-By-Step procedure
- Windows + Edit the System Environment Variables / Environment Variables / System variables (bottom half). Note:  Use the version you installed in the following. 
- JAVA_HOME = C:\Program Files\OpenJDK\jdk-version folder
- KAFKA_HOME =  C:\kafka-version folder
- M2_HOME = C:\ProgramData\chocolatey\lib\maven\apache-maven-version
- Path - must include (make sure you have only one JDK location in your path!)
       - %JAVA_HOME%\bin OR C:\Program Files\OpenJDK\jdk-version\bin (or similar, NOT both!)
       - %M2_HOME%\bin
       - %KAFKA_HOME%\bin
       - %KAFKA_HOME%\bin\windows

Window 1 - Run Zookeeper Service  (keep window open)
- .\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties


Window 2 - Run Kafka Service (keep window open)
- .\bin\windows\kafka-server-start.bat .\config\server.properties


Window 3 (temporary) - Execute One-Time Commands - create, list, delete topics 
- .\bin\windows\kafka-topics.bat --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --create --topic bearcat-messages
- .\bin\windows\kafka-topics.bat --zookeeper localhost:2181 --list

Window 4 - Run Kafka Producer (will provide a > prompt for writing messages)
- .\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic bearcat-messages

Window 5 - Run Kafka Consumer (to show messages from the beginning)
- .\bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic bearcat-messages --from-beginning

## If encountered following errors

1. If you get "'wmic is not recognized as an internal or external command"

- Add this to your Windows System Environment "Path" Variable:  C:\WINDOWS\system32

2. Error Running Kafka: ERROR Shutdown broker because all log dirs in C:\tmp\kafka-logs have failed (kafka.log.LogManager) This location doesn't exist on Windows. Open your server.properties file and change the location, for example change from this to this:

     - #log.dirs=/tmp/kafka-logs 
     - log.dirs=C:\tmp\kafka-logs

3. Error Creating Topic: ERROR org.apache.kafka.common.errors.InvalidReplicationFactorException: Replication factor: 1 larger than available brokers: 0.

- If you get this error, you don't have any Kafka broker services running. Check your Kafka services window and try the log.dirs fix abovve.
