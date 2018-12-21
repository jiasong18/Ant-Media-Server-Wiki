Ant Media Server is a software that can stream live and vod videos. It supports adaptive streaming on the fly and 
records live videos in several formats like HLS, MP4, etc. 

## Features
* Receive live streams in RTMP, RTSP and WebRTC
* Records live streams in MP4, FLV, HLS and Dash Formats
* Transcodes live streams into lower resolutions on the fly for adaptive streaming
* Play live streams with RTMP, RTSP, WebRTC, HLS and Dash Formats


## Installation

### Linux (Ubuntu)

#### 1. Download Ant Media Server 
Download and save the Ant Media Server Community/Enterprise Edition from http://antmedia.io to your disk.
Ant Media Server is being tested on ubuntu 14.04 and 16.04 versions on CI. 

#### 2. Open Terminal and Go to Directory

Open a terminal and go to the directory where you have downloaded Ant Media Server Zip file

```
cd path/to/where/ant-media-server....zip
```

#### 3. Download Installation Script
Download the `install_ant-media-server.sh` shell script 

```
wget https://raw.githubusercontent.com/ant-media/Scripts/master/install_ant-media-server.sh
chmod 755 install_ant-media-server.sh
```

#### 4. Run the Installation Script
Call the download script file by giving ant-media-server zip file. The command below installs Ant Media Server and starts the service. Ant Media Server zip files are in format `ant-media-server-<community|enterprise>-<version>-<date>.zip`

##### IMPORTANT
You need to add "true" to the end of the command line if you want to keep your settings from previous installation.
```
sudo ./install_ant-media-server.sh ant-media-server-VERSION-DATE_TIME.zip true
```
For a clean new installation:
```
sudo ./install_ant-media-server.sh ant-media-server-VERSION-DATE_TIME.zip 
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

#### Accessing Web panel 
Open your browser and type `http://SERVER_IP_ADDRESS:5080` to go to the web panel. If you're having difficulty in accessing the web panel, there may be a **firewall** that blocks accessing the 5080 port. 


## Server Ports
In order to server run properly you need to open some network ports. 
Here are the ports server uses

* TCP:1935 (RTMP)
* TCP:5080 (HTTP)
* TCP:5443 (HTTPS)
* TCP:5554 (RTSP)
* UDP:5000-65000 (WebRTC and RTSP)

### Forward Default http(80), https(443) Ports to 5080 and 5443 

Generally port forwarding is used to forward default ports to the server's ports in order to have easy of use.
For instance let's forward 80 to 5080, just type the command below.

```
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 5080
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 5443
```

After running the command above, the request goes to 80, 443 is being forwarded to 5080, 5443 consecutively



### List and Delete Current Port Forwardings
To List port forwarding run the command below
``` 
sudo iptables -t nat --line-numbers -L
```

To delete a port forwarding run the command below
```
iptables -t nat -D PREROUTING [LINE_NUMBER_IN_PREVIOUS_COMMAND]
```

### Make Port Forwarding Persistent

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