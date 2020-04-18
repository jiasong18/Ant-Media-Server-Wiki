In this document, you'll learn how to monitor Ant Media Servers with Apache Kafka, Elastic Search and Grafana. So we need to install these components. Here is a step by step guide to install your monitoring system from scratch

* [Install Apache Kafka](#Install-Apache-Kafka)
* [Install Elasticsearch and Logstash](#Install-Elasticsearch-and-Logstash)
* [Install Grafan](#Install-Grafana)

# Install Apache Kafka

Kafka is useful for building real-time streaming data pipelines to get data between the systems or applications.

1. Install Java because Apache Kafka requires Java.

   `apt-get update && apt-get install openjdk-8-jdk -y`

2. Download the Apache Kafka and then extract the archive file

   `wget -qO- https://archive.apache.org/dist/kafka/2.2.0/kafka_2.12-2.2.0.tgz | tar -zxvf- -C /opt/ && mv /opt/kafka* /opt/kafka`

3. Edit **server.properties** file as below.

   `vim /opt/kafka/config/server.properties`

   `listeners=PLAINTEXT://your_server_ip:9092`

4. Start Kafka

   ```
   /opt/kafka/bin/zookeeper-server-start.sh /opt/kafka/config/zookeeper.properties &
   /opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties &
   ```

   Firstly, we've started ZooKeeper because Kafka needs ZooKeeper and then we've started Kafka

5. Check if it's working. Run the command below

   `netstat -tpln | egrep "9092|2181"`

    if you see that the ports(9092 and 2181) are in listening mode, it means it's working.

## Run Apache Kafka as a `systemd` service. 
Running Apache Kafka as a `systemd` service will let us manage Kafka services to start/stop using the `systemctl` commands. Follow the instructions below

 * Create `systemd` unit file for Apache Kafka

   `vim /lib/systemd/system/kafka.service`

 * Copy and paste the below content into the `kafka.service` you've created above.
    Make sure that you set the correct JAVA_HOME path for your system in the content below

    ```
    [Unit]
    Description=Apache Kafka Server
    Requires=network.target remote-fs.target
    After=network.target remote-fs.target kafka-zookeeper.service

    [Service]
    Type=simple
    Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
    ExecStart=/opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties
    ExecStop=/opt/kafka/bin/kafka-server-stop.sh

    [Install]
    WantedBy=multi-user.target
    ```

 * Create `systemd` unit file for Zookeeper

     `vim /lib/systemd/system/kafka-zookeeper.service`

 * Copy and paste the below content to the `kafka-zookeeper.service` file you've created above.
     ```
     [Unit]
     Description=Apache Zookeeper Server
     Requires=network.target remote-fs.target
     After=network.target remote-fs.target

     [Service]
     Type=simple
     Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
     ExecStart=/opt/kafka/bin/zookeeper-server-start.sh /opt/kafka/config/zookeeper.properties
     ExecStop=/opt/kafka/bin/zookeeper-server-stop.sh

     [Install]
     WantedBy=multi-user.target
     ```
* Enable and reload the systemd daemon to apply new changes.
  ```
  systemctl enable kafka-zookeeper.service
  systemctl enable kafka.service
  ```
  Start kafka server
  ```
  systemctl start kafka-zookeeper.service
  systemctl start kafka.service
  ```

## Kafka settings for Ant Media Server

If you want to monitor Ant Media Server, you need to set the IP address of your Apache Kafka in `AMS_INSTALLTION_DIR/conf/red5.properties` file.

* Open the following line with the editor

  `vim /usr/local/antmedia/conf/red5.properties` 

* Edit the following line

  `server.kafka_brokers=ip_address:port_number`

  Replace `ip_address:port_number` with Apache Kafka IP Address and port number

  Example:
  `server.kafka_brokers=192.168.1.230:9092`

* Restart Ant Media Server.

  `service antmedia restart`

* Check if it's working

  When you run the following command on Kafka server, if there is data flow, everything is configured properly.

  `/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 192.168.1.230:9092 --topic ams-instance-stats --from-beginning`

  Output should be something like below:
  ```
  {"instanceId":"a06e5437-40ee-49c1-8e38-273544964335","cpuUsage":
  {"processCPUTime":596700000,"systemCPULoad":0,"processCPULoad":1},"jvmMemoryUsage": 
  {"maxMemory":260046848,"totalMemory":142606336,"freeMemory":21698648,"inUseMemory":120907688},"systemInfo": 
  {"osName":"Linux","osArch":"amd64","javaVersion":"1.8","processorCount":1},"systemMemoryInfo":
  ...
  ```
### Some Useful Apache Kafka commands

* List all topics

  `/opt/kafka/bin/kafka-topics.sh --list --bootstrap-server your_kafka_server:9092`

  Example:
  ```
  /opt/kafka/bin/kafka-topics.sh --list --bootstrap-server 192.168.1.230:9092
  ams-instance-stats
  ams-webrtc-stats
  kafka-webrtc-tester-stats
  ```
* Monitor messages for a specific topic with Kafka Consumer as we've used above

   `/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 192.168.1.230:9092 --topic ams-instance-stats --from-beginning`


# Install Elastic Search

To begin, run the following commands to import the Elasticsearch public GPG key into APT, install prerequisites packages and add the Elastic source list to the sources.list.d directory.
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

update your package lists

`apt-get update`

Then install Elasticsearch with the following command

`apt-get install elasticsearch`

Enable and start elasticsearch service
```
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
```
### Install Logstash

Logstash is a server‑side data processing pipeline that ingests data from multiple sources simultaneously, transforms it, and then sends it to a “stash” like Elasticsearch

update your package lists, then install logstash with the following command
```
apt-get update && apt-get install logstash
```
enable logstash service

`systemctl enable logstash.service`


Create **/etc/logstash/conf.d/logstash.conf** file and add below content.

```
#kafka
input {
  kafka {
    bootstrap_servers => "kafka_server_ip:9092"
    client_id => "logstash"
    group_id => "logstash"
    consumer_threads => 3
    topics => ["ams-instance-stats","ams-webrtc-stats","kafka-webrtc-tester-stats"]
    codec => "json"
    tags => ["log", "kafka_source"]
    type => "log"
  }
}

#elasticsearch
output {
  elasticsearch {
       hosts => ["127.0.0.1:9200"] #elasticsearch_ip
       index => "logstash-%{[type]}-%{+YYYY.MM.dd}"
  }
  stdout { codec => rubydebug }
}
```
Save and close the file, then restart logstash service

`systemctl restart logstash`

#### How to test Elasticsearch and Logstash
You can test that Elasticsearch and Logstash are working correctly with the command below.

`curl -XGET 'localhost:9200/_cat/indices?v&pretty'`

Example output:
```
health status index                   uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   logstash-log-2020.03.23 mf-ffIHBSNO4s7_YoUr_Rw   1   1       1300            0    527.5kb        527.5kb
```
### Install Grafana

Grafana is an open source metric analytics & visualization suite.

In order to install Grafana Server, run the following commands.
```
sudo apt-get install -y software-properties-common wget apt-transport-https
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt-get update && sudo apt-get install grafana
```
enable and start grafana server 
```
systemctl enable grafana-server
systemctl start grafana-server
```
login as follow

`http://your_ip_address:3000/login`

Default username and password

`admin/admin`

**1.** Click `Add data source`
![Grafana1](images/grafana1.png)

**2.** Select Elasticsearch
![Grafana2](images/grafana2.png)

**3.** Set setting as below
```
URL : http://127.0.0.1:9200
Index name: logstash-
Time filed name: @timestamp
Version: 7.0+
```
![Grafana3_1](images/grafana3_1.png)
![Grafana3](images/grafana3.png)

**4.** Click `New dashboard`
![Grafana4](images/grafana4.png)

**5.** Click `Add Query`
![Grafana5](images/grafana5.png)

**6.** Choose whatever you want to monitor.

`Query: ElasticSearch`
![Grafana6](images/grafana6.png)

**7.** Ant Media Example Dashboard
![Grafana7](images/grafana7.png)

> If you would like to use the same dashboard, you can download it from the below link.

> [https://antmedia.io/antmedia-dashboard.json](https://antmedia.io/antmedia-dashboard.json)

### Grafana Telegram Alert

1. Create a New Bot

* Open Telegram and search for @BotFather user and send command the following.

`/newbot`

When you create a new bot, you will get as below lines.
```
Use this token to access the HTTP API:
1254341629:AAHYHhJK8TgsUXa7jqBK7wU1bJ8hzWhUFzs
Keep your token secure and store it safely, it can be used by anyone to control your bot.
```
2. Create a Channel and retrieve the channel's chat ID. 
* Create a channel in telegram
* Invite your bot to that channel as admin
* Send a test message.
* Get the Chat id

Use cURL or just place this on any Browsers Address Bar:

`https://api.telegram.org/bot1254341629:AAHYHhJK8TgsUXa7jqBK7wU1bJ8hzWhUFzs/getUpdates`

You will get the below lines.
```
{"ok":true,"result":[{"update_id":222389875,
"channel_post":{"message_id":2,"chat":{"id":-1001181377238,"title":"test","type":"channel"},"date":1587016720,"text":"test"}}]}
```
You need that number: `-1001181377238` 

We have configured the chatbot. Now let's start to configure Grafana Notification.

Login to Grafana

`http://your_grafana_server:3000`

Click Alerting / Notification Channel

![Grafana_telegram](images/grafana_telegram_1.png)

Add New Channel
![Grafana_telegram](images/grafana_telegram_2.png)

You can configure as below screenshot
![Grafana_telegram](images/grafana_telegram_3.png)

```
Name : name_of_your_notification.
Type : Telegram
Bot Api Token: your_bot_token_id
Chat ID: your_channel_id
```
If you click on the Send Test and there is a message on the telegram, everything is fine.

![Grafana_telegram](images/grafana_telegram_5.png)

Now you can set up notifications as you wish.

If you have any questions, please just drop a line to contact (at) antmedia.io
