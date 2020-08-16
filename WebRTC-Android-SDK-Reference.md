## IDataChannelObserver, IEncoderStatisticsListener, IWebRTCClient, IWebRTCListener

In this doc, you can see the method references in `IWebRTCListener` and `AntMediaClientDelegate`

### IDataChannelObserver
```java
    /** The data channel's bufferedAmount has changed. */
    void onBufferedAmountChange(long previousAmount, String dataChannelLabel);
    /** The data channel state has changed. */
    void onStateChange(DataChannel.State state, String dataChannelLabel);
    /**
     * A data buffer was successfully received.
     */
    void onMessage(DataChannel.Buffer buffer, String dataChannelLabel);

    void onMessageSent(DataChannel.Buffer buffer, boolean successful);
```

### IEncoderStatisticsListener
```java
    /**
     * It is called on other thread then UI, if this function changes something on UI
     * run view updates in UI Thread with runOnUIThread
     * @param reports
     */
    public void updateEncoderStatistics(final StatsReport[] reports);
```

### IWebRTCClient
```java
     /**
     * Publish mode
     */
    String MODE_PUBLISH = "publish";

    /**
     * Play mode
     */
    String MODE_PLAY = "play";

    /**
     * Join mode
     */
    String MODE_JOIN = "join";

    /**
     * Multi track play
     */
    String MODE_MULTI_TRACK_PLAY = "multi_track_play";


    /**
     * Camera open order
     * By default front camera is attempted to be opened at first,
     * if it is set to false, another camera that is not front will be tried to be open
     * @param openFrontCamera if it is true, front camera will tried to be opened
     *                        if it is false, another camera that is not front will be tried to be opened
     */
    void setOpenFrontCamera(boolean openFrontCamera);


    /**

     * If mode is MODE_PUBLISH, stream with streamId field will be published to the Server
     * if mode is MODE_PLAY, stream with streamId field will be played from the Server
     *
     * @param url websocket url to connect
     * @param streamId is the stream id in the server to process
     * @param mode one of the MODE_PUBLISH, MODE_PLAY, MODE_JOIN
     * @param token one time token string
     */
    void init(String url, String streamId, String mode, String token, Intent intent);


    /**
     * Starts the streaming according to mode
     */
    void startStream();

    /**
     * Stops the streaming
     */
    void stopStream();

    /**
     * Switches the cameras
     */
    void switchCamera();

    /**
     * Switches the video according to type and its aspect ratio
     * @param scalingType
     */
    void switchVideoScaling(RendererCommon.ScalingType scalingType);

    /**
     * toggle microphone
     * @return
     */
    boolean toggleMic();

    /**
     * Stops the video source
     */
    void stopVideoSource();

    /**
     * Starts or restarts the video source
     */
    void startVideoSource();

    /**
     * Swapeed the fullscreen renderer and pip renderer
     * @param b
     */
    void setSwappedFeeds(boolean b);

    /**
     * Set's the video renderers,
     * @param pipRenderer can be nullable
     * @param fullscreenRenderer cannot be nullable
     */
    void setVideoRenderers(SurfaceViewRenderer pipRenderer, SurfaceViewRenderer fullscreenRenderer);

    /**
     * Get the error
     * @return error or null if not
     */
    String getError();



    void setMediaProjectionParams(int resultCode, Intent data);

    /**
     * Return if data channel is enabled and open
     * @return true if data channel is available
     * false if it's not opened either by mobile or server side
     */
    boolean isDataChannelEnabled();
```

IWebRTCListener
```java
/**
  * It's called when websocket connection has been disconnected
  */
 void onDisconnected();

 /**
  * This method is fired when publishing(broadcasting) to the server has been finished
  */
 void onPublishFinished();

 /**
  * This method is fired when playing stream has been finished
  */
 void onPlayFinished();

 /**
  * This method is fired when publishing to the server has been started
  */
 void onPublishStarted();

 /**
  * This method is fired when playing has been started
  */
 void onPlayStarted();

 /**
  * This method is fired when client tries to play a stream that is not available in the server
  */
 void noStreamExistsToPlay();

 void onError(String description);

 void onSignalChannelClosed(WebSocket.WebSocketConnectionObserver.WebSocketCloseNotification code);

 /**
  * This method is called every time, connection is established with the remote peer.
  * It's called both p2p, play and publish modes.
  */
 void onIceConnected();

 /**
  * This method is fired when Ice connection has been disconnected
  */
 void onIceDisconnected();

 /**
  * It's called in multi track play mode and reports the tracks to the listener
  * @param tracks
  */
 void onTrackList(String[] tracks);

 /**
  * It's called when bitrate measurements received from serves.
  * targetBitrate should be greater than (videoBitrate + audioBitrate) for a good quality stream
  * @param streamId
  * @param targetBitrate
  * @param videoBitrate
  * @param audioBitrate
  */
 void onBitrateMeasurement(String streamId, int targetBitrate, int videoBitrate, int audioBitrate);
```
