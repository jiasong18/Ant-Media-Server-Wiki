Quality selection was one of the most requested features and it is implemented and ready.  Let's see how to use it. 

## How Adaptive Bitrate Works?
Ant Media Server measures the viewers internet speed and sends the best quality according to the internet speed of the viewer.

As an example:
* Assume that there are two bitrates on the server
  * First one is 360p and 800kbps.
  * Second one is 480p and 1500kbps.

* if Viewer internet speed
  * is above 1500kbps, then the resolution with 480p is sent.
  * is between 800kbps and 1500kbps or less than 800kbps, then the resolution with 360p is sent.

Client was getting only what server has sent to. 

## How to Force Specific Quality? 
Client side can force the resolutions which are in the adaptive bitrates. Please keep in mind that if you request a quality whose bitrate is higher than the client's bitrate. You may see some packet drops, pixelations, async etc. Anyway, let us tell how to use that.


* While you play the stream, after you receive `play_started` notification in `WebRTCAdaptor`. Call `getStreamInfo` with `webRTCAdaptor.getStreamInfo({your_stream_Id});`  

  ```javascript
  else if (info == "play_started") 
  {
    console.log("play started");
    webRTCAdaptor.getStreamInfo(streamId);
  } 
  else if (info == "play_finished") 
  {
  ...
  ```


* After getting stream info, you can call the following function to force the video quality you want to watch:
### HOW TO GET THE {the_resolution_to_be_forced} from the getStreamINFO
  ```
  webRTCAdaptor.forceStreamQuality("{your_stream_Id}",  {the_resolution_to_be_forced});
  ```

There is a working sample in `player.html` as shown below. When you choose a different resolution, it'll force the quality.
As you can see from the screenshots below, you can select the resolution.
![240p is selected](https://user-images.githubusercontent.com/54481799/91039162-b514e000-e614-11ea-80ce-f009e6006a19.png)

Currently `240p` is selected and as you can see bitrate is `500000`. Now the following screenshot is `1080p`.

![1080p is selected](https://user-images.githubusercontent.com/54481799/91039211-cf4ebe00-e614-11ea-89f7-b750ee9d37af.png)

After selecting the `1080p`, bitrate went up from `500000` to `2000000`.

If you are still having issues, please let us know that. contact@antmedia.io