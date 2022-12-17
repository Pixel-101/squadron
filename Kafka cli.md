# Kafka on CLI 


Initially we used cli(cmd) to run the producer as well as the consumer server on our local machine. Used Zookeeper server to run kafka services.

Kafka has 5 main APIs that do the majority of the work.
	1) Producer API:  Permits the application to publish streams of records(Broadcast) 
	2) Consumer API: Permits an application to subscribe to topics and process streams of records.
	3) Streams API: Converts the input streams to output streams either processing the input streams or  sending the same ones forward
	4) Connector API: Executes the reusable producer and consumer APIs that can link the topics to the existing applications
	5) Admin API: Used to manage Kafka topics, brokers, and other Kafka objects.
	
	
	Important terms: 
			-Broker: A broker is a Kafka server that runs in a Kafka cluster( in a localhost, its the zookeeper server)
			-Controller:  One of the brokers serves as the controller, which is responsible for managing the states of the partitions and replicas and for performing administrative tasks like re-assigning partitions.
			-Partitions: A partition is basically how Kafka breaks down a data set and keep it in different nodes of cluster. 

  
  ```
  zookeeper setup:                        .\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
  Kafka server setup:                     .\bin\windows\kafka-server-start.bat .\config\server.properties
  Create topic:                           .\bin\windows\kafka-topics.bat --bootstrap-server localhost:9092 --create --topic myTopic --partitions 1 --replication-factor 1
  Run Consumer server first:              .\bin\windows\kafka-console-consumer.bat  --bootstrap-server localhost:9092 --topic myTopic --from-beginning
  Running producer server:                .\bin\windows\kafka-console-producer.bat --bootstrap-server localhost:9092 --topic myTopic
  
  ```
  
  
