# Splunk Workshop
## Content to be covered
* What is splunk
* Why use splunk
* Using splunk Interface
  * How to use search in splunk
  * How to import data static files into splunk
  * how to visualize data and create dashboards
* Splunk Connect for Kafka – Connecting Apache Kafka with Splunk
## Contact
- Vipul chandoor (s530459@nwmissouri.edu)
- Saivarun Illendula (s530464@nwmissouri.edu)
- Krishnaveni Karri (s530471@nwmissouri.edu)
- Santhosh Bonala (s530859@nwmissouri.edu)

## What is splunk 
Splunk is the software for searching, monitoring, analyzing the big data though web interface. Splunk can capture live data and it can genarate graphs, reports, alerts, dashboards and visualizations.  

## Why use Splunk? 
- We can Analyze system performance  
- Troubleshoot any failure condition
- Monitor business metrics  
- Search & Investigate a particlar outcome  
- Create dahboards to visualize & analyze results
- Store and retrive data for later use

## Splunk-Kafka-Connector
> Clone this repo before you start working!!

### 1. Download Splunk Connector for kafka
By Following the link below download the executable jar file [Download](https://github.com/splunk/kafka-connect-splunk/releases)
![Download](/images/kafka-connect-splunk.PNG)
> Make sure that you place the jar file in `Splunk-Kafka-Connector/kafka/bin/windows/`
> we will use this path later to start our kafka connector

### 2. Start Zoopkeeper
> Please make sure that your current working directory is Splunk-Kafka-Connector

Start the Zoopkeeper using the following command `kafka/bin/windows/zookeeper-server-start.bat zookeeper.properties`
> You can also double click on the `zookeeper-server-start.bat` file

### 3. Start Kafka Server
> Please make sure that your current working directory is Splunk-Kafka-Connector

Start the kafka server using the following command `kafka/bin/windows/kafka-server-start.bat server.properties`
> You can also double click on the `kafka-server-start.bat` file

### 4. Create a topic in kafka
> Please make sure that your current working directory is Splunk-Kafka-Connector

Create a topic on the previous server using the following command `kafka/bin/windows/kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic bearcat_messages`
> You can also double click on the `kafka-topics.bat` file

### 5. Start a console Producer
> Please make sure that your current working directory is Splunk-Kafka-Connector

Start a console producer to write to the kafka topic we created earlier using the following command `kafka/bin/windows/kafka-console-producer.bat --broker-list localhost:9092 --topic bearcat_messages`
> You can also double click on the `kafka-console-producer.bat` file

### 6. Start a console Customer
> Please make sure that your current working directory is Splunk-Kafka-Connector

Start a console consumer to read from the kafka topic we created earlier using the following command `kafka/bin/windows/kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic bearcat_messages --from-beginning`
> You can also double click on the `kafka-console-consumer.bat` file

### 7. Create Connection Splunk-Kafka
> Please make sure that your current working directory is Splunk-Kafka-Connector

Start a console consumer to read from the kafka topic we created earlier using the following command `kafka\bin\windows\connect-distributed.bat kafka\config\connect-distributed.properties`
> You can also double click on the `connect-distributed.bat` file

### 8. verifying Sink Connector
#### 1. Postman
* Make a `GET` Request to the Kafka Connector API Service and verify that you have started in step 7 using the URI `http://localhost:8083/connector-plugins/` and verify that you must have an entry for `com.splunk.kafka.connect.SplunkSinkConnector`
* You can refer to the below screenshot for more details.
![verifySink](/images/verifySink.png)

#### 2. CURL
* Use the following command to make a `GET` request using CURL 
> curl http://localhost:8083/connector-plugins/ and verify that you must have an entry for `com.splunk.kafka.connect.SplunkSinkConnector`
* You can refer to the below screenshot for more details.
![verifySink](/images/curlverifySink.png)

### 9. Create OAuth Token for HEC(HTTP Event Collector)

1. On the Search and reporting Page, Click on Settings.

2. In the Settings, click on Data Inputs.
![datainputs](/images/datainputs.png)

3. On the Data Inputs page, click on the HTTP Event Collector option.
![HEC](/images/HEC.png)


4. On the HTTP Event Collector Page click on Global Settings.
![globalsettings](/images/globalsettings.png)

5. In the Edit Global Settings modal, make sure all the details are similar to the image shown below.
![globalsettingsconfig](/images/globalsettingsconfig.png)

6. Then Click on Save.
7. Then, click on New Token and make sure all of the details as per the following screenshots.

![generatetoken-1](/images/generatetoken-1.png)


![generatetoken-2](/images/generatetoken-2.PNG)


![generatetoken-3](/images/generatetoken-3.png)


8. After creating a new token, remember to make a note of it.



### 10. create Connector Tasks
- `POST` the following data to splunk HEC using the following URI and payload
> URI: http://localhost:8083/connectors  
```
{
  "name": "splunk-prod-demo12",
    "config": {
     "connector.class": "com.splunk.kafka.connect.SplunkSinkConnector",
     "tasks.max": "2",
     "topics": "bearcat_messages",
     "splunk.hec.uri":"http://localhost:8088",
     "splunk.hec.token": "<use the token generated in previous step>",
     "splunk.hec.ack.enabled" : "false",
     "splunk.hec.raw" : "false",
     "splunk.hec.json.event.enrichment" : "org=fin,bu=south-east-us",
     "splunk.hec.track.data" : "true"
    }
}
```
- Refer to the following screenshot for more details.
![POSTconfig](/images/POSTconfig.png)

## References
* https://github.com/splunk/kafka-connect-splunk/
* https://docs.confluent.io/current/connect/userguide.html#id3
* https://docs.confluent.io/current/connect/references/restapi.html
* https://kafka.apache.org/intro



## 
- [Published Page](https://santhoshbonala.github.io/Splunk-Kafka-Connector/) 
- [View Source](https://github.com/SanthoshBonala/Splunk-Kafka-Connector) 
