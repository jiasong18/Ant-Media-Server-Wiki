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
### How To Get The {the_resolution_to_be_forced} From The getStreamINFO?

Calling getStreamInfo methods makes server to send streamInformation callback which returns stream information such as adaptive resolutions, audio bitrate, video bitrate etc. 
* When server sends the `streamInformation`, you can get stream details like following:

  ```
  else if (info == "streamInformation") {

       var streamResolutions = new Array();

       obj["streamInfo"].forEach(function(entry) {
	      //It's needs to both of VP8 and H264. So it can be duplicate
	      if(!streamResolutions.includes(entry["streamHeight"])){
	         streamResolutions.push(entry["streamHeight"]);	

	      }// Got resolutions from server response and added to an array.

        });
        
  }// After getting stream information, forceStreamQuality can be used with the information we got.
  else if (info == "ice_connection_state_changed"){
  ...
  ```
* After getting stream info, you can call the following function to force the video quality you want to watch:
  ```
  webRTCAdaptor.forceStreamQuality("{your_stream_Id}",  {the_resolution_to_be_forced});
  ```

* There is a working sample in `player.html` as shown below. When you choose resolution, it'll force the quality.
As you can see from the screenshot below, you can select the resolution.
![240p](https://user-images.githubusercontent.com/54481799/92497488-14bcdf00-f202-11ea-9790-b9afcbe0f456.png)

Currently `240p` is selected and as you can see bitrate is `500000`.