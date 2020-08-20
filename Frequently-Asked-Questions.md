### How to install SSL on the AWS EC2 server instance?
1. Please use an Elastic IP address.
2. Add A record in your Elastic IP address.
3. After that please check DNS records in here -> https://dnschecker.org/
4. If everything is fine, follow this tutorial -> https://github.com/ant-media/Ant-Media-Server/wiki/SSL-Setup

### Where to download JavaScript SDK?
JavaScript SDK is available in the Ant Media Server. It can be accessed via `http://SERVER_ADDR:5080/LiveApp/js/webrtc_adaptor.js`. Its file location is `/usr/local/antmedia/webapps/LiveApp/js/webrtc_adaptor.js`.  Its source code is also available [in here](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/js/webrtc_adaptor.js)  

### May I use Docker images to deploy Ant Media Server
YES. Utilizing Docker images is a very common way of deploying Ant Media Server.  Check the [Installation](https://github.com/ant-media/Ant-Media-Server/wiki/Installation#docker-installation)

### How to reset admin password?
* Stop the server `service antmedia stop`.
* Delete the `server.db` file under `/usr/local/antmedia/`
* Start the server `service antmedia start``

If you're using `mongodb` as database, your password will be in `serverdb` database and in `User` collection. 
* Connect to your `mongodb` server with `mongo` client. 
* Type `use serverdb;`
* Type `db.User.find()` and it shows you the output like below.
    `
    { "_id" : ObjectId("5ea486690f09e71c2462385a"), "className" : "io.antmedia.rest.model.User", 
      "email" : "test@antmedia.io", "password" : "1234567", "userType" : "ADMIN" }
    `
* You can update the password with a command something like below. Change the parameters below according to the your case.
   `
   db.User.updateOne( { email:"test@antmedia.io" }, { $set: { "password" : "test123" }})
   `
* Alternatively, you can delete the user with a command something like below. Change the parameters below according to the your case.
   `
   db.User.deleteOne( { "email": "test@antmedia.io" } )
   `

### How to reduce latency in RTMP to HLS ?
In this article, we will explain how to reduce latency for RTMP to HLS. But firstly we will keep you inform some of terms Stream protocols like RTMP and HLS about the used technologies.

##### What is RTMP (Real Time Messaging Protocol) ?
* RTMP means Real Time Messaging Protocol. RTMP is mostly depreciated for use as a viewer-facing video streaming protocol. However, RTMP is the most commonly used streaming protocol.

##### What is HLS (HTTP Live Streaming) ?
* The HLS(HTTP Live Streaming) protocol was developed by Apple. The HLS streaming protocol works by chopping MPEG-TS video content into short chunks. On slow network speed, HLS allows the player to use a lower quality video, thus reducing bandwidth usage. HLS videos can be made highly available by providing multiple servers for the same video, allowing the player to swap seamlessly if one of the servers fails.

##### How to Reduce latency for RTMP -> HLS Streaming ?
To reduce the HLS latency there are some parameters and it can be reduced to 8-10 secs for now.
* One of the parameter is having HLS segment time lower value which is by default 2 sec in Ant Media Server and you can decrease this value to have lower latency but then players start to poll server more frequently and it can be waste of resource usage.
* OBS Advanced Keyframe Interval: Second critical parameter is sending key frame in every 2 seconds(This value should consistent with HLS segment time) and it is critical to split videos into 2 sec duration segments. Open Broadcaster Software(OBS) sends key frame for every 10 seconds by default and the latency will increase to 30 seconds.

![Key Frame Interval](https://i0.wp.com/antmedia.io/wp-content/uploads/2018/05/obs-keyframe-setting.png)

After you have done these adjustments, your delay will be significantly reduced.
If you are still having issues, please let us know that. contact@antmedia.io

### How to enable SSL for Ant Media Server ?
HTTPS and WSS(WebSocket Secure) is mandatory for Google Chrome to run WebRTC and WebSocket applications.
In addition, developers want to serve their content with secure connection as well. The script in this document
install Let's Encrypt SSL certificate


##### Enabling SSL in Linux(Ubuntu)

Go to the folder where Ant-Media-Server is installed. Default directory is /usr/local/antmedia

```
cd /usr/local/antmedia
```

_If there is a service that uses 80 port, you need to disable it. If your system has Apache Web Server, you need to disable it first such a command below_
```
sudo service apache2 stop
```

There should be a `enable_ssl.sh` file in the installation directory. 
Call the enable_ssl.sh with your domain name

```
sudo ./enable_ssl.sh -d example.com
```

##### v1.5+, `enable_ssl.sh` script supports external fullchain.pem and privkey.pem files. It's usage has been changed to
```
Usage:
sudo ./enable_ssl.sh -d {DOMAIN_NAME}
sudo ./enable_ssl.sh -f {FULL_CHAIN_FILE} -p {PRIVATE_KEY_FILE} -d {DOMAIN_NAME} 
```	
	
_If you disable any service that binds to 80 port such as Apache Web Server, enable it again_
```
sudo service apache2 start
```

Make sure that your domain points to your server public IP address in the DNS records 

If the above scripts returns successfully, SSL will be installed your server, 
you can use https through 5443. Like below

```
https://example.com:5443
```

**ATTENTION:** If port 80 is used by some other process or it's forwarded to some other port, 
`enable_ssl.sh` will not be successful. Please disable the process or delete the port forwarding temporarily in running the `enable_ssl.sh` script above  

If you are still having issues, please let us know that. [contact@antmedia.io](mailto:contact@antmedia.io) 

##### References
- [Blog Post: Enable SSL with Just One Command](https://antmedia.io/enable-ssl-on-ant-media-server/)
- [Let's Encrypt](https://letsencrypt.org/)

### How to remove port forwarding ?
Check that which port forwardings exist in your system with below command. 
```
sudo iptables -t nat --line-numbers -L
````

The command above should give an output live below
```
Chain PREROUTING (policy ACCEPT)
num  target     prot opt source               destination         
1    REDIRECT   tcp  --  anywhere             anywhere             tcp dpt:https redir ports 5443
2    REDIRECT   tcp  --  anywhere             anywhere             tcp dpt:http redir ports 5080

...
```

Delete the rule by line number. For instance to delete the http -> 5080 forwarding, run the command below
```
iptables -t nat -D PREROUTING 2
``` 
parameter 2 is the line number, if you want to delete https -> 5443, you should use 1 instead of 2 

If you are still having issues, please let us know that. [contact@antmedia.io](mailto:contact@antmedia.io)

### How to fix "Make sure that your domain name was entered correctly and the DNS A/AAAA record(s)" ?
* First of all make sure that A record is entered in your DNS settings and point to your server.
* If you are sure about that, check your ports whether 443 or 80 ports are not blocked or forwarded to any port. 
* If you forward 80 or 443 ports to 5080 and 5443, then please remove these port forwarding settings as described [here](https://github.com/ant-media/Ant-Media-Server/wiki/How-to-Remove-Port-Forwarding%3F).

If you are still having issues, please let us know that. [contact@antmedia.io](mailto:contact@antmedia.io)
 
### How to fix "NotSupportedError" in Publishing WebRTC ?
Problem is caused from attempting to access media source as discussed in https://stackoverflow.com/questions/34215937/getusermedia-not-supported-in-chrome.

To solve this problem you must enable SSL. You can follow instructions in this post https://antmedia.io/enable-ssl-on-ant-media-server.

If you are still having issues, please let us know that. [contact@antmedia.io](mailto:contact@antmedia.io) 

### WebRTC stream stops after a few seconds
This issue is generally caused by unopened UDP ports. 
Please make sure that UDP ports 5000 to 65535 of your server are open. 

If you are still having issues, please let us know that. [contact@antmedia.io](mailto:contact@antmedia.io) 

### In IOS, Chrome and Firefox cannot open the camera.
This is an IOS bug: [https://stackoverflow.com/questions/51501642/chrome-and-firefox-are-not-able-to-access-iphone-camera/53093348#53093348](https://stackoverflow.com/questions/51501642/chrome-and-firefox-are-not-able-to-access-iphone-camera/53093348#53093348)

### Which codecs are supported by AntMedia?
In video H264 is supported,

In audio, for WebRTC, opus is supported and for HLS, AAC is supported.

### How to Fix a 403 Forbidden Error?
You can use IP Filtering for your Ant Media Server's RESTful API gate.
If it's ON and your IP is not listed on the enabled IPs List, you cannot access to RESTful API.
If you delete 127.0.0.1, localhost web panel will no longer work. 
Write access IP Address like: 127.0.0.1,192.168.1.1/24,34.22.16.222

### How does ADAPTIVE mechanism work on Ant Media Server?
Actually, the bottleneck is the network throughput.
So, Ant Media Server is always aware of the network speed, the end-user has on his side.
Regardless of the resolution in the Adaptive settings, a bitrate selection is made either upward or downward, depending on the bit rate information and the instantaneous network speed.

### How to set up an auto-scaling cluster with Ant Media Server?
Main documentation of Ant Media Server is on
http://docs.antmedia.io/en/latest/

Scaling and Load Balancing
http://docs.antmedia.io/en/latest/Scaling-and-Load-Balancing.html

AMS Cluster On AWS
http://docs.antmedia.io/en/latest/Cloud-Deployments.html#ams-cluster-on-aws

AMS Cluster On Azure
http://docs.antmedia.io/en/latest/Cloud-Deployments.html#ams-cluster-on-azure

Documentation of AMS has improved and is still developing.

Wish you a great Live Video Streaming experience with AMS Cluster :-)

### How to improve WebRTC bit rate?
Let's remember the definition of WebRTC from its founders: 
"WebRTC is a free, open project that provides browsers and mobile applications with Real-Time Communications (RTC) capabilities via simple APIs. The WebRTC components have been optimized to best serve this purpose."

As you may know, the main purpose of WebRTC is Real-Time Communication.
Image quality is an opponent power against real-time (ultra-low latency) communication.
So, there should be a break-even point for the balance of latency and image quality.
The optimum video speed with the current processor and communication platforms is 2500 Kbps.

There are some references to this issue:
- A blog from WebRTC Expert Tashi Levent Levi:  https://bloggeek.me/webrtc-vs-zoom-video-quality/
- A paper from academia: http://wimnet.ee.columbia.edu/wp-content/uploads/2017/10/WebRTC-Performance.pdf
- Test results for the limits from webrtc-experiment.com 
        https://www.webrtc-experiment.com/webrtcpedia/
        Maximum video bitrate on chrome is about 2Mb/s (i.e. 2000kbits/s).
        Minimum video bitrate on chrome is .05Mb/s (i.e. 50kbits/s).
        Starting video bitrate on chrome is .3Mb/s (i.e. 300kbits/s).

As a result, everyone needs to measure the best performant configuration of their infrastructure by changing them step-by-step.
My suggestions are as follows:
- 20 for FPS is optimum; however, 10 and 15 should be examined.
- 720p is good enough for video quality, especially for mobile platforms.
- 1000 Kbps is optimum for 720p, 750 Kbps is also acceptable when FPS is 10.

Have a nice ultra-low latency live streaming experience with WebRTC and sub-second expert Ant Media Server.

### What are the deployment options for Ant Media Server?
There're 4 different methods to use Ant Media Server (AMS):
1. If you want to host AMS on your own server/cloud, you can buy Ant Media Server Enterprise Edition License: https://antmedia.io/#products

2. If you don't want to concern on server/cloud stuff, you can buy small/medium/large server instances which have already installed AMS: https://antmedia.io/#products

3. If you have an AWS account, you can use AMS and pay for Amazon: https://aws.amazon.com/marketplace/search/results?x=0&y=0&searchTerms=Ant+Media+Server&page=1&ref_=nav_search_box

4. If you have an Azure account, you can use AMS and pay for Microsoft: https://azuremarketplace.microsoft.com/en-us/marketplace/apps/antmedia.ant_media_server_enterprise

### What latencies can I achieve with Ant Media Server Enterprise Edition?
Ant Media Server Enterprise Edition is capable of:
* 0,5 seconds typical latency with WebRTC to WebRTC streaming path. (usually around 0,2 seconds)
* 2-3 seconds typical latency with RTSP/RTMP to WebRTC streaming path.
* 6-10 seconds typical latency with RTMP/WebRTC to HLS streaming path.

### How many different bit rates possible with Ant Media Server Enterprise Edition?
There is virtually no limit.
AMSEE typically run 4-5 different bitrates with the option to go lower.

The recommended default resolutions are:
* 240p - 500 Kbps
* 360p - 800 Kbps
* 480p - 1000 Kbps
* 720p - 1500 Kbps
* 1080p - 2000 Kbps

### Have the ultra-low latency streams adaptive bit rates as well?
Definitely YES.
Ant Media Server provides ultra-low latency and adaptive bit rate at the same time.

### Does Ant Media Server have an Embedded SDK?
Yes, Ant Media Server Enterprise has a native Embedded SDK for arm, x86 and x64.

### How to configure path for mp4 recordings?
Mp4 files are recorded to the streams folder under the WebApps. A soft link could be created for that path 
using this command: ln -s {target_folder}  {link_name}, so that they are recorded to the path is defined in the command.

### How to enable IP filter for Ant Media Servers behind load balancer in AWS?
How to enable IP filter for Ant Media Servers behind load balancer in AWS?

1. Edit the following file with a text editor.

`vim /usr/local/antmedia/conf/jee-container.xml`

2. Find the line as follow. 

`<bean id="valve.access" class="org.apache.catalina.valves.AccessLogValve">`

And add the following line before it.

`<bean id="valve.access" class="org.apache.catalina.valves.RemoteIpValve" />`

3. Find another line as follow.

`<property name="rotatable" value="true" />`

and add the following line before it.

`<property name="requestAttributesEnabled" value="true" />`

4. After adding these lines, restart Ant Media Server with the following terminal command.

`systemctl restart antmedia`

The last edited version of the file will look like the following.
```
    <bean id="valve.access" class="org.apache.catalina.valves.RemoteIpValve" />
    <bean id="valve.access" class="org.apache.catalina.valves.AccessLogValve">
            <property name="directory" value="log" />
            <property name="prefix" value="${http.host}_access" />
            <property name="suffix" value=".log" />
            <property name="pattern" value="common" />
            <property name="rotatable" value="true" />
            <property name="requestAttributesEnabled" value="true" />
```
Now, You can use the IP filter.

* ### How to tune OBS for Ultra Low Latency Streaming?
?
* ### How to improve quality in WebRTC Streams?
?
