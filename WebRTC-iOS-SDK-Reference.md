## AntMediaClient Protocol and Delegate

In this doc, you can see the method references in `AntMediaClientProtocol` and `AntMediaClientDelegate`

### AntMediaClientProtocol
```swift
    /**
     Sets the required options to for Ant Media Client to Run
     - Parameters:
        - url: Full Ant Media Server's websocket url. You can use ws or wss . It should be something
        ws://your_server_address:5080/WebRTCAppEE/websocket
        wss://your_server_address:5443/WebRTCAppEE/websocket
        - streamId: The stream id that you use in your connection. You either play or publish with this stream id.
        - token: If you active one-time token on server side, you should enter token value in here. If one-time token is not activated, just leave empty
        - mode: The Mode of the Client. It should .play, .publish or .join. If it's .play, it means your WebRTC client will play a stream with your streamId
        on the server. If it's .publish, it mean your WebRTC client will publish stream with your stream id.
        - enableDataChannel: Enable or disable data channel on the mobile side. In order to make data channel work, you also need to enable it on server side
    */
    func setOptions(url: String, streamId: String, token: String, mode: AntMediaClientMode ,enableDataChannel: Bool)
    
    /**
     Enable or disable video completely in the WebRTC Client.  It should be called before `initPeerConnection()` and `start()` method.
     It's generally used for disabling video in order to have only audio streaming. If video is disabled by this method, it's not enabled in the same session again. Video is enabled by default.
     - Parameters:
         enable: Enable or disable video in the connection.
     */
    func setVideoEnable( enable: Bool)
    
    /**
     Set the speaker on. It works if audio session is already started so calling this method may not work if it's called too early.
     The correct place to call it in AntMediaClientDelegate's `audioSessionDidStartPlayOrRecord` method.
     */
    func speakerOn();
    
    /**
    Set the speaker off. It works if audio session is already started so calling this method may not work if it's called too early.
    The correct place to call it in AntMediaClientDelegate's `audioSessionDidStartPlayOrRecord` method.
    */
    func speakerOff();
    
    /**
     Initializes the peer connection and opens the camera if it's publish mode but it does not start the streaming. It's not necessary to call this method. `start()` method calls this method if it's required. This method is generally used opening the camera and let the user tap a button to start publishing
     */
    func initPeerConnection()
    
    /**
    Starts the streaming according to the mode of the client.
    */
    func start();
    
    /**
    Sets the camera position front or back. This method is effective if it's called before `initPeerConnection()` and `start()` method.
     - Parameters:
        - position: The camera position to open
     */
    func setCameraPosition(position: AVCaptureDevice.Position);
    
    /**
    Sets the camera resolution. This method is effective if it's called before `initPeerConnection()` and `start()` method.
     - Parameters:
        - width: Resolution width
        - height:Resolution height
     */
    func setTargetResolution(width: Int, height: Int);
    
    /**
    Stops the connection and release resources
     */
    func stop();
    
    /**
    Switches camera on the fly.
     */
    func switchCamera()
    
    /**
    Sends data via WebRTC's Data Channel.
     - Parameters:
        - data: The Data to send via data channel
        - binary:  The type of data. It should be true, if it's binary
     */
    func sendData(data: Data, binary: Bool);
    
    /**
    Status of the data channel. Both server and mobile side, should enable data channel to let this method return true
    - Returns: true if data channel is active, false if it's disabled
    */
    func isDataChannelActive() -> Bool;
    
    /**
     The UIView element that local camera view will be rendered to.
     - Parameters
        - container: The UI View element
        -  mode: Scale mode of the view. 
     */
    func setLocalView( container: UIView, mode:UIView.ContentMode)
    
    /**
    The UIView element that remote stream(playing stream) will be rendered to.
    - Parameters
       - container: The UI View element
       -  mode: Scale mode of the view.
    */
    func setRemoteView(remoteContainer: UIView, mode:UIView.ContentMode)
    
    /**
     - Returns: true if websocket is connected, false if websocket is not connected
     */
    func isConnected() -> Bool;
    
    /**
     Set the debug mode. If it's true, log messages will be available.
     */
    func setDebug(_ value: Bool);
    
    /**
    Toggle audio in the current stream. If it's muted, it will be unmuted. If it's unmuted, it'll be muted.
     */
    func toggleAudio();
    
    /**
     Toggle video stream(enable, disable) in the current stream.
     */
    func toggleVideo();

```

### AntMediaClientDelegate
```swift

    /**
     Called when websocket is connected
     */
    func clientDidConnect(_ client: AntMediaClient)

    /**
     Called when websocket is disconnected
     */
    func clientDidDisconnect(_ message: String)
    
    /**
     Called when websocket connection has error
     */
    func clientHasError(_ message: String)
    
    /**
     Called when stream is added to peer to peer connection.
     This is a low level communicatin and it's good to use in P2P mode.
     Not good to use in publish and play mode
     */
    func remoteStreamStarted()
    
    /**
     Called when stream is removed from peer to peer connection
     This is a low level notification and it's good to use in P2P mode.
     Not good to use in publish and play mode
    */
    func remoteStreamRemoved()
    
    /**
     Called when local audio and video is started
     */
    func localStreamStarted()
    
    
    /**
     Called when playing is started.
     Triggered by server.
     */
    func playStarted()
    
    /**
     Called when playing is finished.
     Triggered by server.
     */
    func playFinished()
    
    /**
     Called when publish is started.
     Triggered by server.
     */
    func publishStarted()
    
    /**
     Called when publish is finished.
     Triggered by server.
     */
    func publishFinished()
    
    /**
     Called when peer to peer connection is failed, disconnected or closed
    */
    func disconnected()
    
    /**
     Called when audio session start play or record
     */
    func audioSessionDidStartPlayOrRecord()
    
    /**
     Called when data is received from webrtc data channel.
     You can convert data to String as follows
       String(decoding: data, as: UTF8.self)
     
     If you receive json data you can parse it after converting string this
       let message = msg.toJSON();
     Then you can access each field by their values like.
     
     Assume that  {"command":"message","content":"hello"} is received.
     
     Convert it to String and then parse the json
       let rawJSON =  String(decoding: data, as: UTF8.self)
       let json = rawJSON.toJSON();
     
     Access command and content as follows
     json["command"]
     json["content"]
     
     */
    func dataReceivedFromDataChannel(streamId: String, data: Data, binary: Bool)
```
