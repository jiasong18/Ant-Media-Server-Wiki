# What is a Turn Server ?

A TURN server is a network entity in charge of relaying media in VoIP related protocols. This includes SIP, H.323, WebRTC and other protocols.

When you try reaching out directly from one browser to another with voice or video data (sometimes other arbitrary kind of data), you end up going through different network devices. Some of these devices include Firewalls and NATs (Network Address Translators) which may decide due to internal policies not to pass your data.

When there are some network securities like firewall, then data packet does not transfer and we do not get proper streaming of another user.

So we use turn server for this solution.

## Install turn server

`apt-get update && apt-get install coturn`

## Enable turn server

Edit the following file.

`vim /etc/default/coturn`

add the below line

`TURNSERVER_ENABLED=1`

## Configuration turn server
Edit the following file.

`vim /etc/turnserver.conf`

just add it to the 2 lines below.
```
user=username:password
realm=your_public_ip_address
```
and restart turn server

`systemctl restart coturn`

* If you use AWS EC2 instance, you need to add extra the below lines 
```
#EC2 private ip address
relay-ip=your_private_ip
#EC2 Public/Private ip address
external-ip=your_public_ip/your_private_ip
```
```
#Open the following ports on AWS console
TCP 443 #TLS listening port
TCP 3478-3479 #coturn listening port
TCP 32355-65535 #relay ports range
UDP 3478-3479 #coturn listening port
UDP 32355-65535 #relay ports range
```
That 's it. 

You can check your turn server the below link.

https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/

![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/turn1.png)
![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/turn2.png)
![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/turn3.png)
