Wirecast is a live video streaming production tool by Telestream. It allows users to create live or on-demand broadcasts for the web. Wirecast supports various sources for capturing such as webcams, ip cameras, NDIs, capture cards etc… We are going to use macbook’s webcam for this post.

We need to create live stream in Ant Media Server, because we will use this live stream id for publishing stream in Wirecast. In Ant Media Server create a live stream with name WireCast1 as in the screen:

![](images/wirecast/image6.png?raw=true)

Live stream will be added to the table, and note the stream id of WireCast1 stream:

![](images/wirecast/image3.png?raw=true)


Now we are going to create a live stream in Wirecast and publish it to an output destination which is Ant Media Server in our case.

In Wirecast click the + button in Wirecast as in the screenshot:

![](images/wirecast/image4.png?raw=true)

Chose FaceTime as video capture source which is webcam of macbook as in the screenshot:

![](images/wirecast/image7.png?raw=true)

We are going to publish stream to an RTMP url in Ant Media Server. Click Output Settings in the upper menu and choose RTMP Server  and click OK as in the screenshot:

![](images/wirecast/image8.png?raw=true)

Fill the settings using the Stream Id that you noted in previous steps as in the screen shot:

![](images/wirecast/image1.png?raw=true)

**Tune for Ultra Low Latency Streaming**
Wirecast by default is not optimized for ultra low latency streaming. If you push RTMP stream with Wirecast and play with WebRTC, please open Output Settings > Edit Encoding configuration and make **Baseline** for Profile. Secondly, you can configure the bitrate according to your quality and internet bandwidth requirements.

![](images/wirecast-encoding-settings.png?raw=true)

Click the right arrow to select the source of video stream as in the screenshot:

![](images/wirecast/image11.png?raw=true)


Start broadcasting live stream by clicking the Start/Stop Broadcasting in the upper menu as in the screenshot:

![](images/wirecast/image2.png?raw=true)


Now the live stream is published to Ant Media Server. You will see the status of live stream in Ant Media Server is changed to Broadcasting status:

![](images/wirecast/image5.png?raw=true)


Click the play button and watch the live stream:

![](images/wirecast/image9.png?raw=true)