If you are using the cluster structure and want to keep track of all logs from one place, this article is for you.

I will do this installation on Ubuntu 20.04, but it is similar in other Linux based operating systems.

**System requirements:**

- Ubuntu 18.04 or Ubuntu 20.04
- Minimum 4GB RAM

### Prerequisites

In order to run Elasticsearch, you must install Java. Run the following commands to install.
```
sudo apt-get update
sudo apt-get install apt-transport-https openjdk-11-jre openjdk-11-jre-headless uuid-runtime pwgen
```

Step 1: MongoDB

MongoDB stores the configurations and meta information. 

Install MongoDB using the following commands.
```
sudo apt-get install gnupg
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu `lsb_release -cs`/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
sudo apt-get update & sudo apt-get install -y mongodb-org
```
Enable and restart MongoDB service by running the commands below.
```
sudo systemctl enable mongod.service & sudo systemctl restart mongod.service
```
Make sure the service is running.
```
sudo systemctl status mongod.service
```
Step2: Elasticsearch

Graylog can be used with Elasticsearch 6.x, please follow the below instructions to install the open-source version of Elasticsearch. Elasticsearch is software that acts as a search server, requiring Graylog to work.

Install Elasticsearch using the following commands.

```
wget -O - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add
echo "deb https://artifacts.elastic.co/packages/oss-6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
sudo apt-get update && sudo apt-get install elasticsearch-oss
```
Once the installation of Elasticsearch 6.x  is complete, set cluster name for Graylog.

Edit the following file. 
```
vim /etc/elasticsearch/elasticsearch.yml
```
and then add the 2 lines below.

```
cluster.name: graylog
action.auto_create_index: false
```
Save the file and exit.

Enable and restart Elasticsearch service by running the commands below.
```
sudo systemctl enable elasticsearch.service
sudo systemctl restart elasticsearch.service
```
Make sure the service is running. To check the status of Elasticsearch, run the commands below:
```
sudo systemctl status elasticsearch.service
```
Make sure everything is correct by running the following commands.
```
curl -X GET http://localhost:9200
```
Output:

```
root@graylog:~# curl -X GET http://localhost:9200
{
  "name" : "cdN0aJ1",
  "cluster_name" : "graylog",
  "cluster_uuid" : "hyWsngLVRqq_IWU1cr75AA",
  "version" : {
    "number" : "6.8.13",
    "build_flavor" : "oss",
    "build_type" : "deb",
    "build_hash" : "be13c69",
    "build_date" : "2020-10-16T09:09:46.555371Z",
    "build_snapshot" : false,
    "lucene_version" : "7.7.3",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```
Make sure the output status is green.
```
curl -XGET 'http://localhost:9200/_cluster/health?pretty=true'
```

```
{
  "cluster_name" : "graylog",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 12,
  "active_shards" : 12,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}
```

Step3: Graylog



