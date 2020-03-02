Kafka, Logstash, Elasticsearch ve Grafana Kurulumu

# Kafka Kurulumu

apt-get update && apt-get install openjdk-8-jdk -y
wget -qO- https://archive.apache.org/dist/kafka/2.2.0/kafka_2.12-2.2.0.tgz | tar -zxvf- -C /opt/ && mv /opt/kafka* /opt/kafka

#server.properties dosyası düzenlenir.
vim /opt/kafka/config/server.properties

listeners=PLAINTEXT://your_server_ip:9092

#Kafka start edilir.

/opt/kafka/bin/zookeeper-server-start.sh /opt/kafka/config/zookeeper.properties &
/opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties &

Asagidaki ciktida 9092 ve 2181 portunun listening modda oldugunu goruyorsaniz hersey normaldir.
netstat -tpln | egrep "9092|2181"

#Kafka systemd servisi olarak çalıştırmak

kafka.service dosyası editörle açılır ve aşağıdaki konfigürasyon girilir.

vim /lib/systemd/system/kafka.service 

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

#Kafka-Zookeeper

kafka-zookeeper dosyası editörle açılır ve aşağıdaki konfigürasyon girilir.


vim /lib/systemd/system/kafka-zookeeper.service 

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

# Eklemiş olduğumuz systemd servisleri aktif edilip başlatılır.

systemctl enable kafka-zookeeper.service
systemctl enable kafka.service

systemctl start kafka-zookeeper.service
systemctl start kafka.service

#Bazı kafka yardımcı komutları

#Kafka'ya gelen topicleri görmek için

/opt/kafka/bin/kafka-topics.sh --list --bootstrap-server your_kafka_server:9092

/opt/kafka# bin/kafka-topics.sh --list --bootstrap-server 192.168.1.230:9092
ams-instance-stats
ams-webrtc-stats
kafka-webrtc-tester-stats

#Kafka'dan Topic silmek için

/opt/kafka/bin/kafka-topics.sh --zookeeper localhost:2181 --delete --topic ams-instance-stats

#Topic 'e test mesaji gondermek

/opt/kafka/bin/kafka-console-producer.sh --broker-list 192.168.1.230:9092 --topic test

#Kafka'ya gelen mesajları görmek için

/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 192.168.1.230:9092 --topic test --from-beginning

#Ant Media Server Kafka Ayarı

Mönitör etmek istediğiniz tüm Ant Media sunucularında red5.properties dosyasında kafka sunucusunun ip adresini ve portunu belirtmeniz gerekiyor.

Bunun için red5.properties dosyası editörle aşağıdaki gibi açılır.

vim /usr/local/antmedia/conf/red5.properties 

Aşağıdaki satır bulunup kafka sunucumuzun ip adresine göre düzenlenir.

server.kafka_brokers=ip_adres:port_numarasi

Örnek:
server.kafka_brokers=192.168.1.230:9092

Son olarak Ant Media Server yeniden başlatılır.

service antmedia restart

Kafka sunucu üstünde aşağıdaki komutu çalıştırdığınızda veri akışı varsa herşey doğru yapılandırılmıştır.

/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 192.168.1.230:9092 --topic ams-instance-stats --from-beginning

Örnek çıktı:

{"instanceId":"a06e5437-40ee-49c1-8e38-273544964335","cpuUsage":{"processCPUTime":596700000,"systemCPULoad":0,"processCPULoad":1},"jvmMemoryUsage":{"maxMemory":260046848,"totalMemory":142606336,"freeMemory":21698648,"inUseMemory":120907688},"systemInfo":{"osName":"Linux","osArch":"amd64","javaVersion":"1.8","processorCount":1},"systemMemoryInfo":{"virtualMemory":2288324608,"totalMemory":1033015296,"freeMemory":95338496,"inUseMemory":937676800,"totalSwapSpace":2065690624,"freeSwapSpace":2065416192,"inUseSwapSpace":274432},"fileSystemInfo":{"usableSpace":3274645504,"totalSpace":10498625536,"freeSpace":3828133888,"inUseSpace":6670491648},"jvmNativeMemoryUsage":{"inUseMemory":321179648,"maxMemory":520093696},"gpuUsageInfo":[],"localWebRTCLiveStreams":0,"localWebRTCViewers":0,"localHLSViewers":0,"encoders-blocked":0,"encoders-not-opened":0,"publish-timeout-errors":0,"server-timing":{"up-time":22446386,"start-time":1583128594707},"time":"2020-03-02T12:10:18.257Z"}

#Elastic Search, Logstash ve Grafana Kurulumu

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

systemctl enable elasticsearch.service
systemctl start elasticsearch.service

#Logstash Kurulumu ve Konfigürasyonu

sudo apt-get update && sudo apt-get install logstash
systemctl enable logstash.service

/etc/logstash/conf.d/logstash.conf dosyası oluşturulur ve aşağıdaki ayarlar kendimize göre düzenlenir.

vim /etc/logstash/conf.d/logstash.conf 

bootstrap_servers => kafka_sunucuları
topics => kafka_topicleri

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

Dosya kaydedildikten sonra logstash servisi yeniden başlatılır.

systemctl restart logstash

#Grafana 

sudo apt-get install -y software-properties-common wget apt-transport-https
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt-get update && sudo apt-get install elasticsearch logstash grafana

#Grafana servisi aktif edilip, başlatılır

systemctl enable grafana-server
systemctl start grafana-server

http://192.168.1.230:3000/login

admin/admin



