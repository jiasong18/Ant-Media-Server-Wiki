## Enable DB Based Cluster Mode
To run Ant Media Server in DB Based Clustering please follow these steps.
### MongoDB Installation
* Install MongoDB into a server. You can look at [here](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/).
* Set `bind_ip` value as `0.0.0.0` in `/etc/mongod.conf` file to let all connections to the MongoDB.
* Restart Mongodb and make sure that you can connect to the MongoDB with something like  `mongo`  

  ```sudo service mongod restart```
* Make Mongodb start at boot 

  ```systemctl enable mongod.service```

### Ant Media Server Installation
* Install Ant Media Server one or more server. You can look at [here](https://github.com/ant-media/Ant-Media-Server/wiki/Getting-Started).
* Download the `change_server_mode.sh` shell script.
```
wget https://raw.githubusercontent.com/ant-media/Scripts/master/change_server_mode.sh
chmod 755 change_server_mode.sh
```
* Run the command to restart Ant Media Server in DB based cluster mode.

`sudo ./change_server_mode.sh cluster <MONGO_SERVER_IP>`

* **Additionally**, Do not forget to open 5000 TCP port for all nodes in order to nodes found each other  
***

_**Note:** run the command to exit from cluster mode and restart Ant Media Server in standalone mode._

`sudo ./change_server_mode.sh standalone`


## Autoscaling
* As new Ant Media Server instances started in DB Based Cluster mode, they are automatically added to the cluster. You can check nodes from Management Console.

`http://<ANT_MEDIA_SERVER_NODE_k_IP>:5080/#/cluster`.

## How DB Based Cluster Works?
* In this mode there are no multicast messages.
* Newly started instance register it to the MongoDB.
* When an instance starts to receive live stream, it register itself as origin of the stream.
* When the load balancer forwards a play request to any of the instances in the cluster, instance get the origin from MondoDB. It fetches live stream from the origin and send to audience.

