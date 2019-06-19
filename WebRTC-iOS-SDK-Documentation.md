This library simplifies connection between browsers and iOS Devices (iPhone and iPad for now) by using Ant Media server as a signaling server. 

This documentation aims to introduce how to use AntMedia WebRTC iOS SDK in your application. This SDK supports three modes: Peer to peer, play and publish.  

## Features
* Fully written in Swift 4. 
* Peer Connection: Two nodes connect to each other, 1-1 connection
* Publish: One node publishes, 1-N connection
* Play: Other nodes play broadcast from publisher,1-N connection
* Allows to enable/disable camera and micrphone for local stream
* Simple and concise codebase. You just need a few initialization and delegation.
* Clear error messages


## Requirements
In order to use this SDK, you need iOS 10+, WebRTC build, AntMedia WebRTC iOS SDK, Starscream library for signaling  and Ant Media Server Enterprise Edition. 

## Installation 
* We already built a version of WebRTC for iOS platform. Here is the link to download: https://antmedia.io/. After download, in Xcode, select your application as target and add WebRTC.framework as embedded binaries. 

* Next step is about signaling: This SDK uses  Starscream to manage WebSocket connections. So if you use Pod or Carthage, please follow the steps: [GitHub - daltoniam/Starscream](https://github.com/daltoniam/starscream#installation)

* Now add AntMedia WebRTC iOS SDK to your project as embedded binaries. 
	
Eventually, you should have two embedded binaries and Starscream via dependency management tool. Now you are ready to use SDK in your application.

Usage
First, in ViewController, import AntMediaSDK and WebRTC libraries. 

```
import AntMediaSDK
import WebRTC
```

Than we are ready to use AntMediaClient with just one line:

```
let client = AntMediaClient.init()
```

Before to start connection, we just need to set a few options: Local and remote video views as `RTCEAGLVideoView` which is provided by WebRTC and `AntMediaClientDelegate` to handle notifications that delivered by SDK. Let’s define these in ViewController:

```
class YourViewController: UIViewController {
    @IBOutlet weak var localVideoView: RTCEAGLVideoView!
    @IBOutlet weak var remoteVideoView: RTCEAGLVideoView!
}
```

And here is AntMediaClientDelegate: 

```
extension YourViewController: AntMediaClientDelegate {  
	// Add all protocol methods.
}
```

So we are ready to initialize and start AntMediaClient with using server url, stream id and mode:

```
client.delegate = self
client.setOptions(url: server, streamId: room, mode: self.getMode())
client.setVideoViews(local: localVideoView, remote: remoteVideoView)
```

If you run the Ant Media Server (Enterprise Edition) on your computer, server url should be localhost, if not ip address should be used. Please do not forget to the protocol (ws or wss).

Stream id is a kind of room name to make it easier finding each node. 

There are three modes: P2P, Play and Publish. You can use `AntMediaClientMode` to use each one. 

If you complete these steps, AntMediaClient instance is ready for connection:

`client.connect()`

After you call connect method, clientDidConnect or clientDidDisconnect methods will be called. If connection is okay, client is ready to start for streaming. You just need to call start method:

`client.start()`

Before to go, please do not forget to add Camera and Microphone usage description in Info.plist. Otherwise it will crash. 

### Delegation

There are 6 methods on AntMediaClientDelegate: 

* clientDidConnect: This method will be called if connection is okay with Ant Media Server.
* clientDidDisconnect: This method will be called if connection fails. Message is available as an argument to handle what’s wrong: Network issues or server issues etc.
* clientHasError: This method will be called if something goes wrong: Socket issues, if remote close the connection etc.
* remoteStreamStarted: This method will be called after the remote source is available. This method is generally used for setting ratio and content mode.
* remoteStreamRemoved: This method will be called if the remote source closes connection or leaves from room. This method is generally used for setting ratio and content mode.
* localStreamStarted: This method will be called when camera capturing and microphone is ready to use. 

## Support

This SDK is still on the beta version. We appreciate if you share any experience with us. Please do not hesitate to open issue.