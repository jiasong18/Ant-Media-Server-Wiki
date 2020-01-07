Ant Media Server can pull RTSP streams from IP Cameras easily and it can be configured on the management panel. In other words, you don’t need to write any commands or use the terminal to add IP Cameras. IP Camera re-streaming is available both in Community and Enterprise Edition. 

The main point is that the Ant Media Server is compatible with IP Cameras that supports ONVIF standard. In fact, almost all IP Cameras in the market supports ONVIF because it creates a common way manage IP Cameras from different manufacturers.

![Onvif](https://raw.githubusercontent.com/mekya/antmedia-doc/master/onvif.jpeg)

Let’s have a look at how to pull stream from IP Camera.

## 1. Add & Watch IP Camera

<!-- # TODO: Please explain the operations step by step. 2 or 3 steps would be ok.  -->
 
Select `LiveApp` from applications on the left side menu, then click `New Live Stream` and select “IP Camera”. Fill camera name, camera username, and camera password. You should add the ONVIF URL of the IP Camera. Generally, it is `http://IP-ADDRESS-OF-IPCAMERA:8080`. 

You can also use `Auto-Discovery` feature! If IP cameras and Ant Media Server are in the same network, Ant Media Server can automatically discovers them. The screenshot of the Auto-Discovery result is shown below.

![](https://raw.githubusercontent.com/mekya/antmedia-doc/master/addipcamera.png)

![](images

If the IP cameras are reachable and configured correctly, Ant Media Server starts the pull live streams from them. You can see its status on the management panel.

Watch the IP Camera, just click the Play button on Actions.

![](images

## 2. Record IP Camera Streams

The Ant Media Server can save IP Camera streams as MP4 format. In addition, it record streams with defined periods such one hour, ten hours interval. You can see these recorded files on the VoD tab in the management panel. You can set the period the `settings.streamFetcherRestartPeriod` in [App Configuration](App-Configuration)

![](images

## 3. PTZ(Pan-Tilt-Zoom) and REST API
Ant Media Server supports PTZ calls through REST API. So that you can move your cameras left, right, top, bottom and change the zoom level. 

![Ant Media Server PTZ](https://antmedia.io/wp-content/uploads/2020/01/antmedia-server-dashboard-onvif-camera.png)

All operations in here and more can be done programmatically via REST API. 

> Quick Links 
> * [REST API Reference](https://antmedia.io/rest)
> * [REST Guide](Rest-Guide).