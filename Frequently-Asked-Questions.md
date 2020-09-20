## How to install SSL on the AWS EC2 server instance?
1. Please use an Elastic IP address.
2. Add A record in your Elastic IP address.
3. After that please check DNS records in here -> https://dnschecker.org/
4. If everything is fine, follow the [SSL Setup Tutorial](https://github.com/ant-media/Ant-Media-Server/wiki/SSL-Setup)

## Where to download JavaScript SDK?
JavaScript SDK is available in the Ant Media Server. It can be accessed via `http://SERVER_ADDR:5080/LiveApp/js/webrtc_adaptor.js`. Its file location is `/usr/local/antmedia/webapps/LiveApp/js/webrtc_adaptor.js`.  Its source code is also available [in here](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/js/webrtc_adaptor.js)  

## May I use Docker images to deploy Ant Media Server
YES. Utilizing Docker images is a very common way of deploying Ant Media Server.  Check the [Installation](https://github.com/ant-media/Ant-Media-Server/wiki/Installation#docker-installation)

## How to reset admin password?
* Stop the server `service antmedia stop`.
* Delete the `server.db` file under `/usr/local/antmedia/`
* Start the server `service antmedia start`

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

## What is RTMP (Real Time Messaging Protocol) ?
RTMP means Real Time Messaging Protocol. RTMP is mostly depreciated for use as a viewer-facing video streaming protocol. However, RTMP is the most commonly used streaming protocol.

## What is HLS (HTTP Live Streaming) ?
The HLS(HTTP Live Streaming) protocol was developed by Apple. The HLS streaming protocol works by chopping MPEG-TS video content into short chunks. On slow network speed, HLS allows the player to use a lower quality video, thus reducing bandwidth usage. HLS videos can be made highly available by providing multiple servers for the same video, allowing the player to swap seamlessly if one of the servers fails.

## How to Reduce latency for RTMP -> HLS Streaming ?
To reduce the HLS latency there are some parameters and it can be reduced to 8-10 secs for now.
* Make HLS segment time 2 seconds. You can decrease this value to have lower latency but then players start to poll server more frequently and it is waste of resources.
* Keyframe Interval: Make key frame interval to 2 seconds(This value should be consistent with HLS segment time). Open Broadcaster Software(OBS) sends key frame for every 10 seconds by default.

![Key Frame Interval](https://i0.wp.com/antmedia.io/wp-content/uploads/2018/05/obs-keyframe-setting.png)

After you have done these adjustments, your latency will be significantly reduced.

## How to enable SSL for Ant Media Server ?
Follow the [SSL Setup Tutorial](https://github.com/ant-media/Ant-Media-Server/wiki/SSL-Setup)

## How to remove port forwarding ?
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

## How to fix "Make sure that your domain name was entered correctly and the DNS A/AAAA record(s)" ?
* First of all make sure that A record is entered in your DNS settings and point to your server.
* If you are sure about that, check the ports(443 and 80) are not blocked or are not forwarded to any other port. 
* If you forward 80 or 443 ports to 5080 and 5443, then please remove these port forwarding settings as described above.
 
## How to fix "NotSupportedError" in Publishing WebRTC ?
To solve this problem you must enable SSL. Follow the [SSL Setup Tutorial](https://github.com/ant-media/Ant-Media-Server/wiki/SSL-Setup)

## WebRTC stream stops after a few seconds
This issue is generally caused by unopened UDP ports. 
Please make sure that UDP ports 5000 to 65535 of your server are open. 

### On IOS, Chrome and Firefox cannot open the camera.
`getUserMedia` is only available in Safari, not other browsers on iOS. [Issue is here](https://bugs.chromium.org/p/chromium/issues/detail?id=752458) 

## Which codecs are supported by Ant Media Server?
Video Codecs: H264, H265 and VP8, Audio Codecs: Opus and AAC are supported by Ant Media Server. 

## How to Fix a 403 Forbidden Error?
[Check this out](https://github.com/ant-media/Ant-Media-Server/wiki/REST-Guide#security--ip-filtering)

## How does ADAPTIVE mechanism work on Ant Media Server?
Ant Media Server measures the client's bandwidth and chooses the best quality in the adaptive bitrates according to the bandwidth of the client. For instance if there are three bitrates, 2000Kbps, 1500Kbps, 1000Kbps and Client's bandwidth is 1700Kbps then the video having 1500Kbps will be sent to the client automatically. If client's bandwidth decreases to 1200Kbps or less than 1000Kbps, then the video having 1000Kbps will be sent to the client automatically on the fly.  

## How to set up an auto-scaling cluster with Ant Media Server?
[Check this out](https://github.com/ant-media/Ant-Media-Server/wiki/Clustering-&-Scaling)

## How to improve WebRTC bit rate?
You can set the `bandwidth` property to any value you want to use in `WebRTCAdaptor` in JS SDK. This is the maximum bitrate value that WebRTC can use. Its default value is 900 and its unit is kbps

## What latencies can I achieve with Ant Media Server Enterprise Edition?
* ~0.5 seconds latency with WebRTC to WebRTC streaming path.
* 0.5-1 seconds latency with RTSP/RTMP to WebRTC streaming path.
* 8-12 seconds latency with RTMP/WebRTC to HLS streaming path.

## How many different bit rates possible with Ant Media Server Enterprise Edition?
There is no soft limit. Generally it's recommend to use 2 or 3 bitrates for most of the cases.

The recommended resolutions and bitrates are:
* 240p - 500 Kbps
* 360p - 800 Kbps
* 480p - 1000 Kbps
* 720p - 1500 Kbps
* 1080p - 2000 Kbps

## Does ultra-low latency streaming supports adaptive bit rates?
Definitely YES. Ant Media Server provides ultra-low latency and adaptive bitrate at the same time.

## Does Ant Media Server have an Embedded SDK?
Yes, Ant Media Server Enterprise has a native Embedded SDK for arm, x86 and x64.

## How to configure the location for MP4 recordings?
MP4 files are recorded to the streams folder under the web apps. A soft link could be created for that path using this command: `ln -s {target_folder}  {link_name}`.

## How to enable IP filter for Ant Media Servers behind load balancer in AWS?

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

## How to use Self-Signed Certificate on Ant Media Server?
**1.** Install openssl package. 

`apt-get update && apt-get install openssl -y`

**2.** Create self-signed certificate as follows.
domain.crt = your certificate file 
domain.key = your key file

`openssl req -newkey rsa:4096 -x509 -sha256 -days 3650 -nodes -out domain.crt -keyout domain.key`

**3.** Submit the requested information and press the Enter button.
```
Country Name (2 letter code) [AU]:UK
State or Province Name (full name) [Some-State]:London
Locality Name (eg, city) []:London
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Ant Media
Organizational Unit Name (eg, section) []:Support
Common Name (e.g. server FQDN or YOUR name) []:domain.com
Email Address []: contact@antmedia.io
```
**4.** The certificate and private key will be created at the specified location. Then run the enable_ssl.sh script as below.

`/usr/local/antmedia/enable_ssl.sh -f domain.crt -p domain.key -d domain.com`
