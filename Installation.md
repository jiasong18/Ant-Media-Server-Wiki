
## Linux (Ubuntu)

### 1. Download Ant Media Server 
Download and save the Ant Media Server Community/Enterprise Edition from http://antmedia.io to your disk.
Ant Media Server is being tested on ubuntu 14.04 and 16.04 versions on CI. 

### 2. Open Terminal and Go to Directory

Open a terminal and go to the directory where you have downloaded Ant Media Server Zip file.

```
cd path/to/where/ant-media-server....zip
```

### 3. Download Installation Script
Download the `install_ant-media-server.sh` shell script 

```
wget https://raw.githubusercontent.com/ant-media/Scripts/master/install_ant-media-server.sh
chmod 755 install_ant-media-server.sh
```

### 4. Run the Installation Script

  #### Update over Older Version
 
  You need to add "true" to the end of the command line if you want to keep your settings from previous installation.
  ```
  sudo ./install_ant-media-server.sh [ANT_MEDIA_SERVER_INSTALLATION_FILE] true
  ```

  #### Fresh Installation

  For a clean new installation:
  ```
  sudo ./install_ant-media-server.sh [ANT_MEDIA_SERVER_INSTALLATION_FILE] 
  ```

### 5. Control the Service

You can check the service if it is running.
```
sudo service antmedia status
```

You can stop/start the service anytime you want.
```
sudo service antmedia stop
sudo service antmedia start
```

### 6. Accessing Web panel 
Open your browser and type `http://SERVER_IP_ADDRESS:5080` to go to the web panel. If you're having difficulty in accessing the web panel, there may be a **firewall** that blocks accessing the 5080 port. 



## Docker Installation

### 1. Create `Dockerfile`
Create a text file with name of `Dockerfile` as follows
```
FROM ubuntu:16.04

ARG AntMediaServer

RUN apt-get update
RUN apt-get install -y libx11-dev
RUN apt-get install -y wget

ADD ./${AntMediaServer} /home

RUN cd home \
    && pwd \
    && wget https://raw.githubusercontent.com/ant-media/Scripts/master/install_ant-media-server.sh \
    && chmod 755 install_ant-media-server.sh

RUN cd home \
    && pwd \
    && ./install_ant-media-server.sh ${AntMediaServer}

ENTRYPOINT service antmedia restart && bash
```


### 2. Build Docker Image 
Download and save Ant Media Server ZIP file in the same directory with `Dockerfile`. Then run the docker build command from command line
```
docker build --network=host -t <image_tag> --build-arg AntMediaServer=<Replace_With_Ant_Media_Server_Zip_File> .
```

### 3. Run the Docker Container
Now we have a docker container with Ant Media Server. Run the image.
```
docker run --network=host --privileged=true -it <image_tag>
```

## Cluster Installation
Cluster installation is an advance topic. Please visit [Clustering & Scaling](Clustering-&-Scaling).

## Server Ports
In order to server run properly you need to open some network ports. Here are the ports server uses.

* TCP:1935 (RTMP)
* TCP:5080 (HTTP)
* TCP:5443 (HTTPS)
* UDP:5000-65000 (WebRTC)
* TCP:5000-65000 (You need to open this range in only cluster mode for internal network. It should not be open to public.)

### Forward Default http(80), https(443) Ports to 5080 and 5443
Generally, port forwarding is used to forward default ports to the server's ports in order to have ease of use. For instance let's forward 80 to 5080, just type the command below.
```
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 5080
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 5443
```
After running the command above, the http requests going to 80 is forwarded to 5080. The http requests going to 443 is being forwarded to 5443.

> Please pay attention that while you're enabling SSL, 80 port should not be used by any process or should not be forwarded to any other port either.  

### List and Delete Current Port Forwardings
To List port forwarding run the command below.
```
sudo iptables -t nat --line-numbers -L
```
To delete a port forwarding run the command below.
```
iptables -t nat -D PREROUTING [LINE_NUMBER_IN_PREVIOUS_COMMAND]
```
### Make Port Forwarding Persistent
If you want the server to reload port forwarding after reboot, we need to install iptables-persistent package and save rules like below.

```
sudo apt-get install iptables-persistent
```

Above command will install iptables-persistent package, after that just run the command below every time you make a change and want it to be persistent.

```
sudo sh -c "iptables-save > /etc/iptables/rules.v4"
```