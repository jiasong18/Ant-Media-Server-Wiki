# Preparation:
* Open `UDP ports(5000-65000)` in order to make WebRTC work.
* In case of instance instance stop/start, IP may be changed by AWS. So please use [Elastic IP](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html).
* Most of the browsers require secure connection for WebRTC publishing. So if you want to use WebRTC publishing, you need to run enable SSL script. Please look at [this](https://github.com/ant-media/Ant-Media-Server/wiki/SSL-Setup#enabling-ssl-in-linuxubuntu).
* Add STUN server configuration by running the following two commands:
```
echo 'settings.webrtc.stunServerURI=stun:stun1.l.google.com:19302' | sudo tee -a /usr/local/antmedia/webapps/WebRTCAppEE/WEB-INF/red5-web.properties
echo 'settings.webrtc.stunServerURI=stun:stun1.l.google.com:19302' | sudo tee -a /usr/local/antmedia/webapps/LiveApp/WEB-INF/red5-web.properties
```

# Usage:
* Login to Web Panel `http://INSTANCE_IP_ADDRESS:5080`
```
username: JamesBond 
password: {Your Instance Id, like this i-0fb29b677379280a5}
```
* [WebRTC Publishing](https://github.com/ant-media/Ant-Media-Server/wiki/WebRTC-Publishing)
* [RTMP Publishing](https://github.com/ant-media/Ant-Media-Server/wiki/RTMP-Publishing)
* [WebRTC Playing](https://github.com/ant-media/Ant-Media-Server/wiki/WebRTC-Playing)
* [HLS Playing](https://github.com/ant-media/Ant-Media-Server/wiki/HLS-Playing)

# Support:
By using this product, you are eligible to request professional support through e-mail: [contact@antmedia.io](contact@antmedia.io)