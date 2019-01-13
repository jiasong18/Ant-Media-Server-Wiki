# Dashboard
Todo
# Application Pages
## Live Streams
### Live Streams Table
#### Stream Name
#### Status
#### Viewers
In this column of the Live Stream Table number of subscribers of the stream are presented. This presentation is made separately for each protocol ie. WebRTC, HLS and RTMP as shown in the image below.
![Antmedia Stream Viewers](images/antmedia-stream-viewers.png)

_**Remarks For Cluster Mode:**_
* _In cluster mode if a viewer tries to play the stream from an edge server, edge server connects to the origin server. Then HLS viewer count is also incremented because of the edge server connections._ 
* _The same situation goes for RTMP but now WebRTC. _ 

##### WebRTC Client Statistics
WebRTC Client Statistics Window opens when you click the eye icon near the WebRTC Viewer count. Here is an example window for a stream with 10 connected clients.

![Antmedia WebRTC Client Statistics](images/antmedia-webrtc-statistics.png)
There are two graphics in WebRTC Client Statistics Window.
**Bitrate vs Clients:**
This graphic shows the measured bitrate and sending bitrate of a stream for each client.

**Media Send Period vs Clients:**
This graphic shows send period of video and audio for each client in ms. The period is expected as 1/fps for video and 10ms for audio.

#### Social Share
#### Actions
# Cluster Page
Cluster page can be opened from the sidebar of the management console. In cluster page, there is a cluster table in which cluster nodes are listed as rows.

In cluster mode, each node registers itself to MongoDB as a cluster node. All registered nodes are listed in the cluster table even if it is off at the moment.

![Antmedia Cluster Page](images/antmedia-cluster.png)

In the image above there are 3 nodes in MongoDB but the node with IP 172.17.0.3 is not in the cluster at the moment. If you don't want to see it in the cluster table anymore, you can delete it by clicking the Delete icon under the Actions column.

## Columns:
**Node IP:** It shows the IP address of the node.\
**Last Update:** Each node updates its status in MongoDB with a period. This column shows the last update time.\
**CPU Usage:** It shows the CPU usage percentage of the node.\
**Memory Usage:** It shows the heap memory usage/total heap of the node.\
**Cluster Status:** It shows whether the node is in the cluster or not as Accessible or Inaccessible.\
**Actions:** It contains a Delete button to remove the node from MongoDB. It is meaningful for the Inaccessible nodes.


