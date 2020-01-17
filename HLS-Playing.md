HLS Playing is available in both Community and Enterprise Editions. Before playing a stream, make sure that stream is broadcasting in the server.

> Quick Link: [Learn How to Publish Live Streams](Publishing-Live-Streams)

1. You can use play.html under the Application. Visit `https://your_domain_name:5443/WebRTCAppEE/play.html`. If you're running Ant Media Server on your local computer, you can also visit `http://localhost:5080/WebRTCAppEE/play.html`

    You will encounter Stream ID doesn't exist popup error.

![](https://antmedia.io/wp-content/uploads/2019/12/Screenshot-from-2019-12-23-17-37-53.png)

2. Write the stream ID & HLS play order parameters in URL as below.

`https://your_domain_name:5443/WebRTCAppEE/play.html?name=stream1&playOrder=hls`

![Go to the play.html](https://antmedia.io/wp-content/uploads/2019/12/hls-streaming-play-html.png)

3. The HLS stream will start to play automatically when it becomes live.  

![Press Start Playing Button](https://antmedia.io/wp-content/uploads/2019/12/hls-playing-play-html.png)

Autoplay may not be activated for some policies by Chrome & Firefox. So you may need to click the player to get it started button. Look at below links:

https://developers.google.com/web/updates/2017/09/autoplay-policy-changes
https://hacks.mozilla.org/2019/02/firefox-66-to-block-automatically-playing-audible-video-and-audio/

Congrats. You're playing with HLS.

## More Details About HLS 
Make sure that HLS muxing is enabled in your application. You can check it on web management panel and check the `HLS Muxing` box in Settings of the app.
 
Assume that HLS Muxing is enabled and there is a stream publishing to Ant Media Server with an RTMP URL in this format:
 `rtmp://<SERVER_NAME>/LiveApp/<STREAM_ID>`

* HLS URL is in this format: `http://<SERVER_NAME>:5080/LiveApp/streams/<STREAM_ID>.m3u8` 

* If there are an adaptive streams enabled in Enterprise Edition, HLS Master URL is in this format: `http://<SERVER_NAME>:5080/LiveApp/streams/<STREAM_ID>_adaptive.m3u8`