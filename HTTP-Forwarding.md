Http forwarding is when user wants to store files in other storage or wants to lower the strain on the server side.

Http forwarding is possible  with a few changes on the `AMS-DIR / webapps / {application} / WEB-INF / red5-web.properties`. 

Add `settings.httpforwarding.extension` to the red5-web.properties file. But you need to specify what you want to forward. 

As an example, if i do like the following, `settings.httpforwarding.extension=mp4,png` it will forward mp4 and png files.

After that baseUrl needs to be set like following. `settings.httpforwarding.baseURL=https://{yourdomainhere}.com/{streamId.mp4}`. 

This link can be anything you need. As an example of a s3 bucket: 

`https://{s3BucketName}.s3.{awsLocation}.amazonaws.com/streams/{streamId.mp4}`.

So, structure depends on your requirement.

Note: Don't add any leading, trailing white spaces.

After that, Ant Media Server needs to be restarted which can be done with the following in the terminal:

`sudo service antmedia restart`

After that making these changes, when user requests the following link:

`https://serverDomain:5443/{streamApp}/streams/{streamId}.mp4`

It will be forwarded to here:`https://{yourdomainhere}.com/{streamId.mp4}`