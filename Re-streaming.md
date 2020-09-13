## IP Cameras
Ant Media Server Users can pull IP Camera streams easily on management panel. In other words, you don’t need to write any commands or use terminal.

The main critical point is that the IP Cameras should support ONVIF standard. ONVIF make it easy to manage IP Cameras. All CRUD and PTZ operations are based on well-defined SOAP messages.

![](images/onvif_conformance.gif)

Let’s have a look at how to pull stream from IP Camera.

### Add IP Camera

* Select “LiveApp” from applications, then click “New Live Stream” and select “IP Camera”. 
* Fill camera name, camera username, and camera password. You should add ONVIF URL of IP Camera. Generally, it is in the following format `IP-ADDRESS-OF-IPCAMERA:8080`. If you don't know the ONVIF URL, tou can use “auto-discovery” feature. If IP camera and server are in the same subnet, Ant Media Server automatically can discover them. The screenshot of  auto-discovery result is shown below.

### Watch IP Cameras

If the IP cameras are reachable and configured correctly, Ant Media Server add theirs streams as live streams and starts to pull streams from them. You can see its status on the management panel. To watch the stream, just click the Play button on Actions.

### Record IP Camera Streams

The Ant Media Server can save IP Camera streams as MP4 format. In addition, it record streams with defined periods such one hour, ten hours interval. You can see these recorded files on VoD tab in the management panel.

## Restreaming External Sources

Ant Media Server can operate with different streaming flows. As well as accepting and creating streaming media, it also has the capability to pull live streams from external sources. Such as; live TV streams, IP camera streams or other forms of live streams(RTSP, HLS, TS, FLV etc.). We will show how to pull live streams with Web Interface.

### Pull Live Streams with Web Interface

* First, log in to the management panel. Click New Live Stream -> Stream Source. Define stream name and URL.
* Ant Media Server starts to pull streams.
* Now, you can watch it.


