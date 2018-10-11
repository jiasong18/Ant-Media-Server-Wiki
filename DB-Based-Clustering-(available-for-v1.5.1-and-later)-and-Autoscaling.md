## Enable DB Based Cluster Mode
To run Ant Media Server in DB Based Clustering please follow these steps.
* Install MongoDB into a server. You can look [here](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/).
* Install Ant Media Server one or more server. You can look [here](https://github.com/ant-media/Ant-Media-Server/wiki/Getting-Started).
* Download the `change_server_mode.sh` shell script.
```
wget https://raw.githubusercontent.com/ant-media/Scripts/master/change_server_mode.sh
chmod 755 change_server_mode.sh
```
* Run the command to restart Ant Media Server in DB based cluster mode.

`sudo ./change_server_mode.sh cluster <MONGO_SERVER_IP>`

## Autoscaling
* As new Ant Media Server instances started in DB Based Cluster mode, they are automatically added to the cluster. You can check nodes from `http://<ANT_MEDIA_SERVER_NODE_k_IP>:5080/#/cluster`.

## How DB Based Cluster Works
* In this mode there are no multicast messages.
* Newly started instance register it to the MongoDB.
* When an instance starts to receive live stream, it register itself as origin of the stream.
* When the load balancer forwards a play request to any of the instances in the cluster, instance get the origin from MondoDB. It fetches live stream from the origin and send to audience.

