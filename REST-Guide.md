REST API guide is one of the missing guides that our lovely users need to get started. In designing Ant Media Server, we’re trying to make almost everything configurable and accessible through REST API. Actually, we need to do this in order to develop a web management panel. So I can say that by using REST API you can almost do anything on Ant Media Server. Here is an abstract list of available methods in REST API

* CRUD(Create/Read/Update/Delete) Operations on
     * Streams
     * Stream Sources
     * IP Camera
* CRUD Operations on VoD streams
* Add/Remove RTMP Endpoints to the Streams
* Authorize/Revoke Social Endpoints
* Change Settings(Bitrates, Recording, Enable/Disable Object Detection, VoD Folder Path) via root app

![](images

For the full REST API Reference, please visit the [https://antmedia.io/rest ](https://antmedia.io/rest )

For the rest of this guide, we try to explain how to call REST methods, give some samples and mention about Security (IP Filtering)

## How to Call REST API Methods

All REST methods are bind `rest` path of the app. Let me give a sample. Ant Media Server Community Edition has `LiveApp` and `WebRTCApp` by default. On the other hand, Enterprise Edition has `LiveApp` and `WebRTCAppEE` by default. The `LiveApp` REST methods (for instance broadcast’s get method) are available as follows

`http://SERVER_ADDRESS:5080/LiveApp/rest/broadcast/get?id={STREAM_ID}`

Let’s make an example with the so-famous `curl` tool. By the way,  Postman is another great tool for testing purposes. It also generates code snippets in several programming languages such as Java, PHP, Go, Python, etc.

Create Broadcast
According to REST Reference, we should call create the method as follows.

Important Note: Please keep in mind that the REST interface only responds to the calls that are made from 127.0.0.1 by default. If you call from any other IP Address, it does not return. For allowing more IP Address, take a look at the  Security section later in this post.
```
curl -X POST
http://localhost:5080/LiveApp/rest/broadcast/create
-H ‘content-type: application/json’
-d ‘{“name”:”test_video”}’
```
We can provide a `Broadcast` object as a parameter in JSON format. Ant Media Server returns created broadcast object in JSON format again.  The most critical field in the returned response is `streamId` field in JSON. We use streamId in getting broadcast.

## Get Broadcast

Getting a broadcast is easier. You just need to add the `streamId` as a query parameter to `streamId` variable as follows.
```
curl -X GET
‘http://localhost:5080/LiveApp/rest/broadcast/get?id=650320906975923279669775’
```
`get` method returns the Broadcast object as `create` method. This is the sample JSON response that get method returns.
```

{
  "streamId":"650320906975923279669775",
  "status":"created",
  "type":"liveStream",
  "name":"test_video",
  "description":null,
  "publish":true,
  "date":1555431732095,
  "plannedStartDate":null,
  "duration":null,
  "endPointList":null,
  "publicStream":true,
  "is360":false,
  "listenerHookURL":null,
  "category":null,
  "ipAddr":null,
  "username":null,
  "password":null,
  "quality":null,
  "speed":0.0,
  "streamUrl":null,
  "originAdress":null,
  "mp4Enabled":0,
  "expireDurationMS":0,
  "rtmpURL":"rtmp://10.2.42.53/LiveApp/650320906975923279669775",
  "zombi":false,
  "pendingPacketSize":0,
  "hlsViewerCount":0,
  "webRTCViewerCount":0,
  "rtmpViewerCount":0
}

```

## REST API Reference

The samples below show how to call the REST methods in an easy way. In order to have a look at all methods and their parameters, you can study the REST API Reference at [https://antmedia.io/rest](https://antmedia.io/rest) which has a good look and feel. Thanks to the Swagger.

![](images

## Security – IP Filtering

Ant Media Server generally runs behind an application server so that you want Ant Media Server responds to the calls that are made from specific IP ranges.  By default, Ant Media Server only responds to the calls that are made from 127.0.0.1.

In order to add IP ranges, you should go to the Settings of the app in Web Management Panel and add
IP Ranges in [CIDR notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#CIDR_notation). You can add multiple comma-separated IP Address Ranges.

![](images

## Conclusion

You can use the REST API of Ant Media Server as explained above. If you’re using community edition and having questions, you can ask the [community](https://groups.google.com/forum/#!forum/ant-media-server) or contact@antmedia.io (reply will be provided with the best effort). If you’re enterprise user, you can ask contact@antmedia.io or to the specified channel allocated for you.
