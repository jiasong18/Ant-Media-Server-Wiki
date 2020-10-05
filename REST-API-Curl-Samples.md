This document is to ease your understanding of how rest api curl  calls can be made. 

This guide assumes that you included your IP in the ip-filter in the Ant Media Server dashboard. So that you can do rest api calls. Otherwise it wouldn't work.

Note: You need to fill curly braces according to your api calls. Like if your stream id is stream123 then you need to fill /{streamid} as stream123.

Let's start with some of the GET Methods:
* Following method gets the broadcast information:
  * curl -X GET "https://{domain:port}/LiveApp/rest/v2/broadcasts/{streamid}"
* If you want to get current viewer count, you can use the following method:
  * curl -X GET "https://{domain:port}/LiveApp/rest/v2/broadcasts/{streamid}/broadcast-statistics"
* As a last example of GET calls, it would be good to see a call with query parameters. So this time it will be getting vod list from database:
  * curl -X GET "https://domain:port/LiveApp/rest/v2/vods/list/{number}/{number}?streamId={streamId}"
