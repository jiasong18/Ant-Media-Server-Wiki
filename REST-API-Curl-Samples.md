This document is to ease your understanding of how rest api curl  calls can be made.

Let's start with some of the GET Methods:
* Following method gets the broadcast information.
  * curl -X GET "https://{domain:port}/LiveApp/rest/v2/broadcasts/{streamid}"
* If you want to get current viewer count, you can use the following method:
  * curl -X GET "https://{domain:port}/LiveApp/rest/v2/broadcasts/{streamid}/broadcast-statistics"