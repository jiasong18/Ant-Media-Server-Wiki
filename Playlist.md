This guide describes how to use Playlist feature on Ant Media Server. You can use this feature in both Community Edition and Enterprise Edition.

1. [What is Linear Live Streaming](#what-is-linear-live-streaming)
2. [How to create a Playlist?](#how-to-create-a-playlist)
3. [How to Build your online TV channel in AMS?](#how-to-build-your-online-tv-channel-in-ams)
4. [How to play Linear Live Streaming?](#how-to-play-linear-live-streaming)


![Linear Live Streaming 101](https://antmedia.io/wp-content/uploads/2020/07/ant-media-server-linear-live-stream-101.jpg)

## What is Linear Live Streaming?
Linear Live Streaming is basically about scheduling your streams for a 7/24 live streaming and can be delivered with different methods. Live Streams and VoD streams can be used in your scheduled live streams which means Linear as well.

So, Linear Live Streams have some programs which have a start and end date streams in the program. Furthermore, Linear Live Streaming is a live event in which all viewers are watching the same live event at the same time. This means you don’t get any spoiler before viewing.

Live linear streaming is a “passive” video viewing experience, meaning viewers don’t “search and click” (except to change the program). The experience of linear streaming is that video content comes to you and while you can change the channel, you don’t have to select an entire collection of videos to watch like you do with a playlist.

## How to create a Playlist?
You can create a playlist in Ant Media Server Dashboard. Your playlist is ready in 2 steps. Here are the steps 🙂

Click “New Live Stream > Playlist” as shown above.

![Create Playlist](https://antmedia.io/wp-content/uploads/2020/06/ant-media-server-new-playlist.jpg)

Just type the Playlist name and Playlist URL into the fields and click “Create” button.

![Create Playlist with URL](https://antmedia.io/wp-content/uploads/2020/06/ant-media-server-create-playlist.png)

> Note: Please make sure your streams can be accessible with AMS.

## How to Build your online TV channel in AMS?

You can build your online TV channel with Ant Media Server. You just need mp4 files for the streams. Furthermore, there is no need to store those mp4 files on your server. Ant Media Server can pull the mp4 files from any place that is stored.

## How can I use Playlist API?

You can call playlist with REST API. We have [REST APIs](https://antmedia.io/rest/#/Playlist%20Rest%20Service) with CRUD processes.



 Here are the sample code snippets:
```json
{
"playlistId":"testPlaylistId",
"currentPlayIndex":0,
"playlistName":"playlistName",
"broadcastItemList":[{
	"streamId":"testStreamID",
	"hlsViewerCount":0,
	"webRTCViewerCount":0,
	"rtmpViewerCount":0,
	"mp4Enabled":0,
	"name":"testPlaylistName",
	"streamUrl":"http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/Sintel.mp4",
	"type":"VoD"},
	{
	"streamId":"testStreamID",
	"hlsViewerCount":0,
	"webRTCViewerCount":0,
	"rtmpViewerCount":0,
	"mp4Enabled":0,
	"name":"testPlaylistName",
	"streamUrl":"http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4",
	"type":"VoD"},
	{
	"streamId":"testStreamID",
	"hlsViewerCount":0,
	"webRTCViewerCount":0,
	"rtmpViewerCount":0,
	"mp4Enabled":0,
	"name":"testPlaylistName",
	"streamUrl":"http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/SubaruOutbackOnStreetAndDirt.mp4",
	"type":"VoD"}
	],
"creationDate":111,
"duration":1111
}
```

## How to play Linear Live Streaming?

You can play the playlist in HLS and WebRTC.

Here is HLS player documentation -> https://github.com/ant-media/Ant-Media-Server/wiki/HLS-Playing

Here is WebRTC play documentation -> https://github.com/ant-media/Ant-Media-Server/wiki/WebRTC-Playing
