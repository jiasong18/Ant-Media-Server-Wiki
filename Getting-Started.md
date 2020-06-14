
[![Build Status](https://travis-ci.com/ant-media/Ant-Media-Server.svg?branch=master)](https://travis-ci.com/ant-media/Ant-Media-Server)

# Ant Media Server
Ant Media Server is a software that can stream live and VoD streams. It supports scalable, ultra low latency (0.5 seconds) adaptive streaming and records live videos in several formats like HLS, MP4, etc.

Here are the fundamental features of Ant Media Server:

* Ultra Low Latency Adaptive One to Many WebRTC Live Streaming in Enterprise Edition.
* Adaptive Bitrate for Live Streams (WebRTC, MP4, HLS) in Enterprise Edition.
* SFU in One to Many WebRTC Streams in Enterprise Edition.
* Live Stream Publishing with RTMP and WebRTC.
* WebRTC to RTMP Adapter.
* IP Camera Support.
* Recording Live Streams (MP4 and HLS).
* Restream to Social Media Simultaneously(Facebook and Youtube in Enterprise Edition).
* One-Time Token Control in Enterprise Edition.
* Object Detection in Enterprise Edition.

If you want to see all features and comparison of Community vs Enterprise editions, please visit [website](https://antmedia.io/pricing/#comparison_table).

## Installation

### Linux (Ubuntu)

#### 1. Download Ant Media Server 
Download and save the Ant Media Server Community/Enterprise Edition from http://antmedia.io to your disk.
Ant Media Server is being tested on Ubuntu 18.04 versions on CI.

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

  ##### 4.1 Update over Older Version 
  You need to add "true" to the end of the command line if you want to keep your settings from previous installation.
  ```
  sudo ./install_ant-media-server.sh [ANT_MEDIA_SERVER_INSTALLATION_FILE] true
  ```

  ##### 4.2. Fresh Installation
  For a clean new installation:
  ```
  sudo ./install_ant-media-server.sh [ANT_MEDIA_SERVER_INSTALLATION_FILE] 
  ```

#### 5. Control the Service

You can check the service if it is running
```
sudo service antmedia status
```

You can stop/start the service anytime you want 
```
sudo service antmedia stop
sudo service antmedia start
```

#### 6. Accessing Web panel 
Open your browser and type `http://SERVER_IP_ADDRESS:5080` to go to the web panel. If you're having difficulty in accessing the web panel, there may be a **firewall** that blocks accessing the 5080 port. 


## Server Ports
In order to server run properly you need to open some network ports. 
Here are the ports server uses

* TCP:1935 (RTMP)
* TCP:5080 (HTTP)
* TCP:5443 (HTTPS)
* UDP:5000-65000 (WebRTC)
* TCP:5000-65000 (You need to open this range in only cluster mode for internal network. It should not be open to public.)

### Forward Default http(80), https(443) Ports to 5080 and 5443 

Generally, port forwarding is used to forward default ports to the server's ports in order to have ease of use.
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

Above command will install iptables-persistent package, after that just run the command below every time 
you make a change and want it to be persistent

```
sudo sh -c "iptables-save > /etc/iptables/rules.v4"
```