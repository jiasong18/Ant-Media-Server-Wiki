
Ant Media Server can convert the RTMP URL or WebRTC Stream to the HLS and can record in MP4. 

## Play Live Streams with HLS
Firstly, make sure that HLS muxing is enabled in your application. You can check it on web management panel and check the `HLS Muxing` box in Settings of the app.
 
Assume that HLS Muxing is enabled and there is a stream publishing to Ant Media Server with an RTMP URL in this format:
 `rtmp://<SERVER_NAME>/LiveApp/<STREAM_ID>`

* In Community Edition, HLS URL is in this format: `http://<SERVER_NAME>:5080/LiveApp/streams/<STREAM_ID>.m3u8 ` 

* In Enterprise Edition, HLS URL is in this format: `http://<SERVER_NAME>:5080/LiveApp/streams/<STREAM_ID>_adaptive.m3u8`

## Play VoD Streams with MP4
Firstly, make sure again that MP4 muxing for live streams is enabled on your application. You can see that in the settings of the app on web management panel.
 
Assume that there is a stream is publishing to the Ant Media Server with an URL in this format `rtmp://<SERVER_NAME>/LiveApp/<STREAM_ID>` . 

After publishing is finished, MP4 file will be created.
* In community edition, MP4 URL will be available in this URL `http://<SERVER_NAME>:5080/LiveApp/streams/<STREAM_ID>.mp4` 

* In Enterprise Edition, MP4 for different bitrates is also created. Assume that you have 480p and 240p resolution enabled in adaptive bitrates settings. Then you will have two more mp4 files with following format
  * `http://<SERVER_NAME>:5080/LiveApp/streams/<STREAM_ID>_480p.mp4` 
  * `http://<SERVER_NAME>:5080/LiveApp/streams/<STREAM_ID>_240p.mp4` 


## Play Live and VoD streams with Embedded Player in Ant Media Server
Both Community and Enterprise Edition have embedded player. It can play both Live and VoD streams. 
Again assume that there is a live stream in Ant Media Server with `<STREAM_ID>` then you can watch and embed the live the stream in following URL format

`http://<SERVER_NAME>:5080/LiveApp/play.html?name=<STREAM_ID>`

After live stream is finished, the URL above plays the recorded MP4 file if it is created.

## Get Preview Live and VoD Streams (Enterprise Only)
* Preview image URL will be available in this URL 
`http://<SERVER_NAME>:5080/<APP_NAME>/previews/<STREAM_ID>.png` 
	
* Preview image is saved as 640x480.
* Preview image creation period can be set as `settings.createPreviewPeriod=<PERIOD>` in
`<ANT_MEDIA_DIR>/webapps/<APP_NAME>/WEB-INF/red5-web.properties` 