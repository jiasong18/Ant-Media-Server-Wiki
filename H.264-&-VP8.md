In this section, we're going to explain simply how to use H.264 & VP8 in Ant Media Server. VP8 and H.264 are mandatory codecs in WebRTC according to RFC 7742. On the other hand, not all browsers support VP8 and H.264 codecs at the same time. 

![H264-VP8 General Settings](https://antmedia.io/wp-content/uploads/2020/05/H264-VP8-general.png)

Ant Media Server supports H.264 for 1.x versions and it has started to support VP8 for 2.x versions including transcoding, clustering. So that all of the devices can play WebRTC streams within their browsers.
If a browser does not play streams(notSetRemoteDescription error), you can think adding H.264/VP8 support. Here are some information about H.264 and/or VP8 support in Ant Media Server. 

# H.264 & VP8 Enabled

![H.264 & VP8 Enabled](https://antmedia.io/wp-content/uploads/2020/05/H264VP8.png)
* SFU Mode(No adaptive bitrate): Ant Media Server can ingest WebRTC stream in H.264 and VP8. If both of the codecs are supported by browser, Ant Media Server chooses H.264 to ingest. Because there is not adaptive bitrate, incoming video stream is not transcoded to other codec. In other words, server can ingest H.264 or VP8 and it forwards the incoming video stream to the players. 

* Adaptive Bitrate Mode(if you have at least one adaptive bitrate): Ant Media Server ingest stream and transcode it into H.264 and VP8. So that devices that only support H264 or VP8 can play the streams.

# H.264 Enabled & VP8 Disabled

![Only H.264 Enabled](https://antmedia.io/wp-content/uploads/2020/05/Only-H.264-Enabled.png)

* SFU Mode: Ant Media Server can only ingest H.264 streams and forwards it into the players.
* Adaptive Bitrate Mode: Ant Media Server can only ingest H.264 and transcode the stream into different H264 bitrates and send to the players.

You can check if your device supports H264 [in this link](https://mozilla.github.io/webrtc-landing/pc_test_no_h264.html)

# H.264 Disabled & VP8 Enabled

![Only VP8 Enabled](https://antmedia.io/wp-content/uploads/2020/05/Only-VP8-Enabled.png)

* SFU Mode: Ant Media Server can only ingest VP8 streams and forwards it into the players.
* Adaptive Bitrate Mode: Ant Media Server can only ingest VP8 and transcode the stream into different VP8 bitrates and send to the players.

_If you only have VP8 enabled, HLS Streaming & MP4 Recording will not work. _
