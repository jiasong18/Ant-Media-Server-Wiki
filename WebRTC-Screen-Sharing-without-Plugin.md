Seamless switch between WebRTC Screen Sharing & Camera is available on both Community and Enterprise Edition. We mean by seamless is that you can switch between Screen & Camera in the same session.

<!-- Picture -->

### Try WebRTC Screen Sharing without plugin

Firstly, you should have `getDisplayMedia` supported browser. You can see `getDisplayMedia` supported browser list in [here](https://caniuse.com/#search=getDisplayMedia).

<li> Install Ant Media Server to your server in the cloud with <a href="https://github.com/ant-media/Ant-Media-Server/wiki/Getting-Started#installation">Getting Started</a>. In order to download community edition, visit <a href="https://antmedia.io">main page</a> or if you want to try Enterprise edition,<a href="https://antmedia.io/#contact"> keep in touch.</a></li>

<li> Go to WebRTC publishing web page which is `https://domainAddress:5443/WebRTCApp` (Community Edition ) or `https://domainAddress:5443/WebRTCAppEE` (Enterprise Edition). By the way,  you need to assign a domain to your server and <a href="https://github.com/ant-media/Ant-Media-Server/wiki/SSL-Setup">install SSL. </a>Otherwise, chrome does not let you access the camera or screen.
</li>

### Implementing WebRTC Screen Sharing

You can see simple functions to [js/webrtc_adaptor.js](https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/js/webrtc_adaptor.js) file to seamless switch between screen sharing without plugin and camera. You can take a look at the source code of <a href="https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/index.html">WebRTCApp/index.html</a>  to see the full implementation.





