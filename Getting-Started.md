Ant Media Server is a software that can stream live and vod videos. It supports adaptive streaming on the fly and 
records live videos in several formats like HLS, MP4, etc. 

## Features
* Receive live streams in RTMP, RTSP and WebRTC
* Records live streams in MP4, FLV, HLS and Dash Formats
* Transcodes live streams into lower resolutions on the fly for adaptive streaming
* Play live streams with RTMP, RTSP, WebRTC, HLS and Dash Formats


## Installation

### Linux (Ubuntu)


Download and save the Ant Media Server Community/Enterprise Edition from http://antmedia.io to your disk.


Now, there are two methods to run the server,
* **Running as a Service** is for production
* **Manual Start** is for testing and debugging issues.

#### Running as a Service

Open a terminal and go to the directory where you have downloaded Ant Media Server Zip file

```
cd path/to/where/ant-media-server....zip/exists
```


Download the `install_ant-media-server.sh` shell script 

```
wget https://raw.githubusercontent.com/ant-media/Scripts/master/install_ant-media-server.sh
chmod 755 install_ant-media-server.sh
```

Call the download script file by giving ant-media-server zip file. The command below installs Ant Media Server and starts the service
```
sudo ./install_ant-media-server.sh ant-media-server-.....zip 
```


You can check the service if it is running
```
sudo service antmedia status
```

You can stop/start the service anytime you want 
```
sudo service antmedia stop
sudo service antmedia start
```

#### Manual Start

If you are developing actively or you need to start and stop manually, you can stop service (`sudo service antmedia stop`) and start server manually by  running `start.sh` command 

- Unzip the downloaded Ant Media Server zip file
- Open a terminal, go to the folder where you extracted the Ant Media Server and run start.sh
type 

```
cd path/to/ant-media-server
./start.sh
```
The server should start a few seconds later.

## Server Ports
In order to server run properly you need to open some network ports. 
Here are the ports server uses

* TCP:1935 (RTMP)
* TCP:5080 (HTTP)
* TCP:5443 (HTTPS)
* TCP:5554 (RTSP)
* UDP:5000-65000 (RTP in RTSP)
* TCP: 8081 (WebSocket)
* TCP: 8082 (WebSocket Secure)

### Forward Default 80 Port to 5080 

Generally port forwarding is used to forward default ports to the server's ports in order to have easy of use.
For instance let's forward 80 to 5080, just type the command below.

```
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 5080
```

After running the command above, the request goes to 80 is being forwarded to 5080

**Make Port Forwarding Persistent**

If you want the server to reload port forwarding after reboot, we need to install iptables-persistent package and 
save rules like below

```
sudo apt-get install iptables-persistent
```

Above command will install iptables-persistent package, after that just run the command below everytime 
you make a change and want it to be persistent

```
sudo sh -c "iptables-save > /etc/iptables/rules.v4"
```
