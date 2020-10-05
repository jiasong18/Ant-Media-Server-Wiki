This document is to ease your understanding of how rest api curl  calls can be made. 

This guide assumes that you included your IP in the ip-filter in the Ant Media Server dashboard. So that you can do rest api calls. Otherwise it wouldn't work.

Note: You need to fill curly braces according to your api calls. Like if your stream id is stream123 then you need to fill /{streamid} as stream123. If the {application} is LiveApp, you need to set, /LiveApp/.

Let's start with some of the GET Methods. Get methods are for getting data from the server.
* Following method gets the broadcast information:
  * curl -X GET "https://{domain:port}/{application}/rest/v2/broadcasts/{streamid}"
* If you want to get current viewer count, you can use the following method:
  * curl -X GET "https://{domain:port}/{application}/rest/v2/broadcasts/{streamid}/broadcast-statistics"
* As a last example of GET calls, it would be good to see a call with query parameters. So this time it will be getting vod list from database:
  * curl -X GET "https://{domain:port}/{application}/rest/v2/vods/list/{number}/{number}?streamId={streamId}"

Let's continue with the PUT Methods. Put methods aims to change data in the database.
* Following method starts the recording of mentioned stream.
  * curl -X PUT "https://{domain:port}/{application}/rest/v2/broadcasts/{streamid}/recording/{true or false}"
* Following method changes the proporties of broadcast. As an example of changing the name of the broadcast.
  * curl -X PUT -H "Content-Type: application/json" "https://{domain:port}/{application}/rest/v2/broadcasts/{streamid}" -d '{"name":"{streamname}"}'
    * On  Windows Command Prompt, body part would be like the following : -d "{""name"":""{streamname}""}". So, two double quotation for the body variables.

Post Methods are to used to create an request on database like creating conference room or used for validation because post methods are not cached nor remained in the history in the browser.