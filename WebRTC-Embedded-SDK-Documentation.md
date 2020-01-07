<!-- > Please provide a step by step guide. It seems confusing at first glance -->

This solution is for IP Camera manufacturers. If you just want to use Peer to Peer connection between IP Camera and Web Browser, Ant Media can provide a solution for this case as well.

1. [WebRTC IP Camera and Browser (P2P)](#1-webrtc-ip-camera-and-browser-p2p)
2. [How to Use Embedded WebRTC SDK](#2-how-to-use-embedded-webrtc-sdk)

Embedded WebRTC SDK can run both on ARM and x86 processors. So let me explain what this Embedded WebRTC SDK does.
* IP Cameras generally has a built-in RTSP URL. You can embed Native WebRTC SDK into your IP Camera and SDK lets you fetch the RTSP stream internally and can forward the RTSP stream to the other Peer via WebRTC.
* Native SDK does not transcode the RTSP stream, it just fetches the stream and forwards it to the WebRTC stack. So that it does not need much CPU resources as its normal WebRTC stack needs. In addition, it does not cause any latency.

![](https://i1.wp.com/antmedia.io/wp-content/uploads/2019/11/webrtc-ip-camera-peer-to-peer.png)

### 1. WebRTC IP Camera and Browser (P2P)

Signaling of WebRTC SDK is compatible with Ant Media Server. So that you need to use Ant Media Server as a signaling server in order to have peer to peer connection between a web browser and your IP Camera.

### 2. How to Use Embedded WebRTC SDK

We’ve deliver source code, documentation, and support for using Embedded WebRTC SDK. There is only one method you need to call in your ARM application. Here is the sample code.

```
int main(int argc, char* argv[]) {
	rtc::InitializeSSL();
	signalingThread = rtc::Thread::Current();
	startWebSocket(“ws://Your_Ant_Media_Server_Address:5080/WebRTCAppEE/websocket”,
	“rtsp://127.0.0.1:6554/stream1”, “stream1”);
	rtc::CleanupSSL();
	return 0;
}
```

As you see, the critical method is startWebSocket method which has three parameters
* The first parameter is the WebSocket URL of the Ant Media Server
* The Second parameter is the internal RTSP URL of the IP Camera
* The third parameter is the stream id that will be published
After you run this application, please visit the http://Your_Ant_Media_Server_Address:5080/WebRTCAppEE/peer.html and write stream id that you’ve used in your code(It’s stream1 in the sample code) and click the JoinButton. You will watch the IP Camera stream on your browser.