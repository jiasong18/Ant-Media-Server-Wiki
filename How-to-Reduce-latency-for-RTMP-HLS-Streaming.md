***
**_NOTE:_** We have updated our documentation. This page is outdated. You can access updated version from the sidebar menu.
***
### What is RTMP (Real Time Messaging Protocol) ?
RTMP means Real Time Messaging Protocol. RTMP is mostly depreciated for use as a viewer-facing video streaming protocol. However, RTMP is the most commonly used streaming protocol.

### What is HLS (HTTP Live Streaming) ?
The HLS(HTTP Live Streaming) protocol was developed by Apple. The HLS streaming protocol works by chopping MP4 video content into short, 10 second chunks. On an unreliable wireless network, HLS allows the player to use a lower quality video, thus reducing bandwidth usage. HLS videos can be made highly available by providing multiple servers for the same video, allowing the player to swap seamlessly if one of the servers fails.

## How to Reduce latency for RTMP -> HLS Streaming ?
To reduce the HLS latency there are some parameters and it can be reduced to 8-10 secs for now.

One of the parameter is having HLS segment time lower value which is by default 2 sec and you can decrease this value to have lower latency but then players start to poll server more frequently and it can be waste of resource usage.

Second critical parameter is sending key frame in every 2 seconds(This value should consistent with HLS segment time) and it is critical to split videos into 2 sec duration segments. Open Broadcaster Software(OBS) generally sends key frame for every 10 seconds and the latency will increase to 30 sec because player needs 3 segments to start to play.

![OBS (Open Broadcaster Software) Keyframe Setting](https://i0.wp.com/antmedia.io/wp-content/uploads/2018/05/obs-keyframe-setting.png)
