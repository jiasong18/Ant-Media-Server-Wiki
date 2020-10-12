This document is to ease your understanding of how rest api curl  calls can be made. 

This guide assumes that you included your IP in the ip-filter in the `Ant Media Server dashboard`. So that you can do rest api calls. Otherwise it won't work.

Note: You need to fill curly braces according to your api calls. Like if your stream id is `stream123` then you need to fill `/{streamid}` as `stream123`. If the `{application}` is `LiveApp`, you need to set, `/LiveApp/`. 

Also you need to use http or https according to your setup.


## Methods

### GET
Let's start with some of the GET Methods. Get methods are for getting data from the server.

* Following method gets the broadcast information:
```
curl -X GET "https://{domain:port}/{application}/rest/v2/broadcasts/{streamid}"
```

* If you want to get current viewer count, you can use the following method:
```
curl -X GET "https://{domain:port}/{application}/rest/v2/broadcasts/{streamid}/broadcast-statistics"
```

* Following method gets a token from the server for its type {play or publish} and expireDate for a specific stream.
```
curl -X GET "https://{domain:port}/{application}/rest/v2/broadcasts/{streamid}/token?expireDate={unix timestamp}&type={play or publish}"
```
* As a last example of GET calls, it would be good to see a call with query parameters. So this time it will be getting vod list from database:
```
curl -X GET "https://{domain:port}/{application}/rest/v2/vods/list/{number}/{number}?streamId={streamId}"
```
### PUT
Let's continue with the PUT Methods. Put methods aims to change data in the database.
* Following method starts the recording of mentioned stream.
```
curl -X PUT "https://{domain:port}/{application}/rest/v2/broadcasts/{streamid}/recording/{true or false}"
```

* Following method changes the proporties of broadcast. As an example of changing the name of the broadcast.

```
curl -X PUT -H "Content-Type: application/json" "https://{domain:port}/{application}/rest/v2/broadcasts/{streamid}" -d '{"name":"
{streamname}"}'

```

On  Windows Command Prompt, body part would be like the following : `-d "{""name"":""{streamname}""}"`. So, two double quotation for the body variables.
### POST
Post Methods are to used to create an request on database like creating conference room or used for validation because post methods are not cached nor remained in the history in the browser.

* Following method creates a broadcast.
```
curl -X POST -H "Content-Type: application/json" "https://{domain:port}/{application}/rest/v2/broadcasts/create"
```
You can set the stream id if you don't want it to be random. So,  body part would be like the following: `-d '{"streamId":"{stream id}"}'`

* This method is to send messages through data-channel.
```
curl -X POST -H "Content-Type: application/json" "https://{domain:port}/{application}/rest/v2/broadcasts/{streamId}/data" -d "{yourmessage}"
```
* Following method is start external sources (IP Cameras and Stream Sources) again if it is added and stopped before.
```
curl -X POST -H "Content-Type: application/json" "https://{domain:port}/{application}/rest/v2/broadcasts/{streamId}/start"
```
* And this one is stops the stream.
```
curl -X POST -H "Content-Type: application/json" "https://{domain:port}/{application}/rest/v2/broadcasts/{streamId}/stop"
```
### DELETE
Delete requests are straight-forward which means these methods are aim to delete from database.

* Following method deletes the mentioned conference-room.
```
curl -X DELETE https://{domain:port}/{application}/rest/v2/broadcasts/conference-rooms/{room_id}
```
* This method deletes that broadcast.
```
curl -X DELETE https://{domain:port}/{application}/rest/v2/broadcasts/{streamId}
```

You can find all the rest methods in the [https://antmedia.io/rest/](https://antmedia.io/rest/)