Http forwarding is when user wants to store files in other storage or wants to lower the strain on the server side.

Http forwarding is possible  with a few changes on the following file:

`AMS-DIR / webapps / {application}(LiveApp or WebRTCAppEE) / WEB-INF / red5-web.properties`. 

Add `settings.httpforwarding.extension` to the red5-web.properties file. But you need to specify what you want to forward. 

As an example, if i do like the following, `settings.httpforwarding.extension=mp4,png` it will forward mp4 and png files.

After that baseUrl needs to be set like following: `settings.httpforwarding.baseURL=https://{yourdomainhere}`. 

If your domain is antmedia.io it will be like:`settings.httpforwarding.baseURL=https://antmedia.io`.

This link can be anything you need. As an example of a s3 bucket: 

`https://{s3BucketName}.s3.{awsLocation}.amazonaws.com`.

To exemplify:
`https://vod-docs-123456789012.s3-accesspoint.us-west-2.amazonaws.com`

So, structure depends on your requirement.

Note: Don't add any leading, trailing white spaces.

After that, Ant Media Server needs to be restarted which can be done with the following in the terminal:

`sudo service antmedia restart`

After that making these changes, when user requests the following link:

`https://{serverDomain}:5443/{streamApp}/streams/{streamId}.mp4`

Ex:`https://antmedia.io:5443/WebRTCAppEE/streams/stream123`

It will be forwarded to here:

`https://vod-docs-123456789012.s3-accesspoint.us-west-2.amazonaws.com/streams/stream123.mp4`

