# Enable DB Based Cluster Mode
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

