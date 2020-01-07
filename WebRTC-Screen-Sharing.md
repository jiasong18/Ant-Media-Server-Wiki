***
**_NOTE:_** We have updated our documentation. This page is outdated. You can access updated version from the sidebar menu.
***
Seamless switch between WebRTC Screen Sharing &amp; Camera is available on both Community and Enterprise Edition. We mean by seamless is that you can switch between Screen &amp; Camera in the same session.<img src="https://antmedia.io/wp-content/uploads/2018/10/screen-sharing.png" alt="WebRTC Screen Sharing" width="912" height="550" class="aligncenter wp-image-5203 size-full" />

<h2>Try WebRTC Screen Sharing</h2>
Firstly, just let me tell how to try it. After that I will give some information about JavaScript API and chrome extension.
<ol>
 	<li> Install Ant Media Server to your server in the cloud with <a href="https://github.com/ant-media/Ant-Media-Server/wiki/Getting-Started#installation">Getting Started</a>. In order to download community edition, visit <a href="https://antmedia.io">main page</a> or if you want to try Enterprise edition,<a href="https://antmedia.io/#contact"> keep in touch.</a></li>
 	<li> Go to WebRTC publishing web page which is https://FQDN:5443/WebRTCApp (Community E. ) or https://FQDN:5443/WebRTCAppEE (Enterprise E.). By the way,  you need to assign a domain to your server and <a href="https://antmedia.io/enable-ssl-on-ant-media-server/">install SSL. </a>Otherwise, chrome does not let you access the camera or screen.
<ul></ul>
</li>
 	<li>Install Chrome Extension by clicking on the link or directly from <a href="https://chrome.google.com/webstore/detail/ant-media-server-screen-s/jaefaokkgpkkjijgddghhcncipkebpnb">Chrome WebStore</a>. Luckily, source code of the extension is also available on <a href="https://github.com/ant-media/Chrome-Screen-Capture-Extension">github</a>.

<img src="https://antmedia.io/wp-content/uploads/2018/10/Screen-Shot-2018-10-15-at-16.53.44-1024x621.png" alt="Install WebRTC Screen Sharing Extension" width="750" height="455" class="aligncenter wp-image-5205 size-large" /></li>
 	<li> Return back to WebRTC Publish page and click Screen Share Checkbox

<img src="https://antmedia.io/wp-content/uploads/2018/10/Screen-Shot-2018-10-15-at-16.46.33-1024x648.png" alt="Share screen with extension" width="750" height="475" class="aligncenter wp-image-5204 size-large" /></li>
 	<li>Screen is going to be shared on the video element

<img src="https://antmedia.io/wp-content/uploads/2018/10/Screen-Shot-2018-10-15-at-16.46.51-1024x648.png" alt="WebRTC Screen Sharing" width="750" height="475" class="aligncenter wp-image-5207 size-large" /></li>
 	<li>Click "Start Publishing" and also check/uncheck "Screen Share" box while publishing in the same session. MP4 file or HLS stream will record the screen or your camera according to your preference. You can watch the stream live on web panel via HLS.</li>
</ol>
<h2>Implementing WebRTC Screen Sharing</h2>
We just add some simple functions to js/webrtc_adaptor.js file to seamless switch between screen sharing and camera. You can take a look at the source code of <a href="https://github.com/ant-media/WebRTCApp/blob/master/src/main/webapp/index.html">WebRTCApp/index.html</a>  to see the full implementation. Meanwhile, let me give some highlights about JavaScript API.
<ul>
 	<li>Firstly, There is a new callback with "screen_share_extension_available". If callback function is called with this parameter, it means that <a href="https://chrome.google.com/webstore/detail/ant-media-server-screen-s/jaefaokkgpkkjijgddghhcncipkebpnb">Ant Media Server Screen Share extension</a> is ready to use in the Chrome.</li>
</ul>

```javascript

    var webRTCAdaptor = new WebRTCAdaptor({
	websocket_url : websocketURL,
	mediaConstraints : mediaConstraints,
	peerconnection_config : pc_config,
	sdp_constraints : sdpConstraints,
	localVideoId : "localVideo",
	debug:true,
	callback : function(info, description) {
	     if (info == "initialized") {
		console.log("initialized");
				
	     } else if (info == "publish_started") {
		//stream is being published
		console.log("publish started");		
	     } else if (info == "publish_finished") {
		//stream is being finished
		console.log("publish finished");		
	     }
	     else if (info == "screen_share_extension_available") {
		console.log("screen share extension available");	
	      }
	      else if (info == "closed") {
		  //console.log("Connection closed");
		  if (typeof description != "undefined") {
			console.log("Connecton closed: " + JSON.stringify(description));
		  }
	      }
	},
	callbackError : function(error, message) {
	    //some of the possible errors, NotFoundError, SecurityError,PermissionDeniedError
            
			console.log("error callback: " +  JSON.stringify(error));
	}
});

```

<ul>
 	<li>Secondly, if extension is available in the Chrome, you only need to call "webRTCAdaptor.switchDesktopCapture(streamId);" function to switch the Screen Sharing
<blockquote>webRTCAdaptor.switchDesktopCapture(streamId);</blockquote>
</li>
 	<li>Lastly, to switch back to camera,  just again call to "webRTCAdaptor.switchVideoCapture(streamId);" function.
<blockquote>webRTCAdaptor.switchVideoCapture(streamId);</blockquote>
</li>
</ul>
Finally, that's all. I hope this blog post will help some guys. You can use this feature on your project.

If you have any question, please <a href="https://antmedia.io/#contact">keep in touch.</a>