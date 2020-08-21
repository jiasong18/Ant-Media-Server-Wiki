Quality selection was one of the most requested feature and now it is implemented here.  We will see how to use it.

Ant Media Server measures the viewers internet speed and sends the best quality according to the internet speed of the viewer.

As an example:
* Assume that there are two bitrates on the server
  * First one is 360p and 800kbps
  * Second one is 480p and 1500kbps.

* if Viewer internet speed
  * is above 1500kbps, then the resolution with 480p is sent.
  * is between 800kbps and 1500kbps or less than 800kbps, then the resolution with 360p is sent.

Client was getting only what server has sent to it. Now client side can force the resolutions which are included in the adaptive resolutions.

To achieve that:
* We need to have some adaptive bitrate resolutions.
* Assuming that you created WebRTCadaptor, you need to call following method to get stream info.
`webRTCAdaptor.getStreamInfo("{your_stream_Id}");` This code needs to be added after publish_started command received. So when you receive the publish started command, this code segment should also needs to be called.
* After getting stream info, you can call the following function to force the video quality you want to watch. `webRTCAdaptor.forceStreamQuality("{your_stream_Id}",  {the_resolution_to_be_forced});`


