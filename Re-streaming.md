# Restreaming

## IP Cameras
In addition to developing promising features in Ant Media Server, we have added IP Camera support to it with the 1.3.0+ release. With this feature,  users can pull IP Camera streams easily on management panel. In other words, you don’t need to write any commands or use terminal.

The main critical point is that the IP Cameras should support ONVIF standard. ONVIF make it easy to manage IP Cameras. All CRUD and PTZ operations are based on well-defined SOAP messages.

![](images/onvif_conformance.gif)

Let’s have a look at how to pull stream from IP Camera. Of course, first, you need to install Ant Media Server, please see documentation.

### Add IP Camera

Select “LiveApp” from applications, then click “New Live Stream” and select “IP Camera”. Fill camera name, camera username, and camera password. You should define ONVIF URL of IP Camera. Generally, it is IP-ADDRESS-OF-IPCAMERA:8080.  Another promising feature comes :) You can use “auto-discovery” feature! If cameras and server are in the same subnet, Ant Media Server automatically discovers them. The screenshot of  auto-discovery result is shown below.

### Watch IP Cameras

If the IP cameras are reachable and configured correctly, Ant Media Server add theirs streams as live streams and starts to pull streams from them. You can see its status on the management panel.

* To watch the stream, just click the Play button on Actions.
* Also, you can switch to Grid view if you have more than one camera and want to see them simultaneously.

### Record IP Camera Streams

The Ant Media Server can save IP Camera streams as MP4 format. In addition, it record streams with defined periods such one hour, ten hours interval . You can see these recorded files on VoD tab in the management panel.

We hope you will be happy about this feature and the good news is that it is offered in Community Edition. In other words, it is free! Please keep in touch because we are releasing new versions, of course, new features. If you have any questions or comments,  just send an email to contact at antmedia dot io or fill the contact form.

## Restreaming External Sources

Ant Media Server operates with different streaming flows. As well as accepting and creating streaming media, it also has the capability of the pull live streams from external sources. Such as; live TV streams, IP camera streams or other forms of live streams(RTSP, HLS, TS, FLV etc.). We will first show how to pull live streams with Web Interface and then we will give technical details for developers.

### Pull Live Streams with Web Interface

* First, log in to the management panel. Click New Live Stream -> Stream Source. Define stream name and URL.
* Then, Ant Media Server starts to pull streams.
* Now, you can watch it.

## Technical Details about Pulling Live Streams for Developers

There are 2 classes to operate and regulate pulling processes:

1. StreamFetcher.java: Includes operating methods, startStream, stopStream, prepare etc
2. StremFetcherManager.java: Includes regulating methods such as checkStreamFetchersStatus, restartStreamFetchers, setRestartStreamAutomatically, scheduleStreamFetcherJob etc.

### 1.  Initialize StreamFetcherManager

/* Constructor */
public StreamFetcherManager(ISchedulingService schedulingService, IDataStore datastore,IScope scope) {
    this.schedulingService = schedulingService;
    this.datastore = datastore;
    this.scope=scope;
}

streamFetcherManager = new StreamFetcherManager(AntMediaApplicationAdapter.this, dataStore,app);

### 2. Define Settings

streamFetcherManager.setRestartStreamFetcherPeriod(appSettings.getRestartStreamFetcherPeriod());
streamFetcherManager.setStreamCheckerInterval("seconds"));

A developer can define restart parameter. This is stored in the settings file as ” settings.streamFetcherRestartPeriod” (in seconds) by using setRestartStreamFetcherPeriod() method. If this parameter set as 0, stream fetcher will never restart in other words never save the fetched stream. For example, define this parameter to “1800” to save fetched stream as recorded video in every 30 minutes. Also, make sure that “Enable MP4 Recording” option enabled in the management panel or “settings.mp4MuxingEnabled ” parameter in the setting file set to true to save the video.

Another parameter is StreamCheckerInterval. This defines the time interval for StreamFetcherManager should check all fetching processes whether they continue to fetch properly or not. The default parameter is 10 seconds. The checkStreamFetchersStatus() method performs these controls.

### 3. Start Streaming

streamFetcherManager.startStreams(streams);
The parameter “streams” is a list (List<Broadcast> streams) of broadcast objects to be fetched.

### 4. Customize

You can also customize the required parameters in StreamFetcher class according to your project’s needs and requirements.

connection timeout: (defined as “timeout” in ms) Connection timeout interval during connection establishment to the stream source in the first phase.

packet receiving timeout: (defined as “PACKET_RECEIVED_INTERVAL_TIMEOUT” in ms) Checks incoming packets timestamps and defines stream is alive or not.

restart after disconnection: (defined as “restartStream” as boolean) If defined as true, tries to reconnect stream source immediately after disconnected.

buffer time: (defined as “bufferTime” in ms) Stream Fetcher first buffers received packets then restreams for smooth streaming but of course it creates a small delay.

RTMP Sources
ShoutCast