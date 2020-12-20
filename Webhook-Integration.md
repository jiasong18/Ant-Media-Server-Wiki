Ant Media Server provides webhooks for making your system/app know when certain events occurs on the server. Therefore, you can register your URL to Ant Media Server which makes POST request when a live stream is started, finished or recorded. Firstly,  let's look at how to register your backend URL to Ant Media Server as a webhook. After that, we'll provide reference for webhooks.

<img src="https://antmedia.io/wp-content/uploads/2018/11/webhooks-300x273.png" alt="webhook ant media server" width="300" height="273" class="aligncenter wp-image-5807 size-medium" title="webhooks ant media server" />
<h2>Register Your Webhook URL</h2>
You can add default webhook URL to your streaming app on Ant Media Server. In addition, it lets you add custom specific webhook URL's in creating broadcast.
<h3>Add default Webhook URL</h3>
In order to add default Webhook URL, you just need to add/change your app settings as below:

<img src="images/ant-media-server-webhook-configuration.png?raw=true" alt="">

Your Ant Media Server now has a default hook which is called when certain events happen (see below) 
<h3>Add Custom Webhook for Streams</h3>
Ant Media Server provides creating streams through rest service. Therefore, If you want to specify the webhook URL for each stream, you can use <em>createBroadcast</em> method in <a href="https://github.com/ant-media/Ant-Media-Server/blob/master/src/main/java/io/antmedia/rest/BroadcastRestService.java">rest service.</a>  <em>createBroadcast</em> method has <a href="https://github.com/ant-media/Ant-Media-Server-Common/blob/master/src/main/java/io/antmedia/datastore/db/types/Broadcast.java">Broadcast</a> object parameter which has <em>listenerHookURL </em>field<em> . </em>

As a result,  you can set<em> listenerHookURL </em>for creating stream at Ant Media Server.

Here is a sample JSON for using <em>createBroadcast</em> method with <a href="https://www.getpostman.com/">Postman</a>
```json
{
	"variables": [],
	"info": {
		"name": "samples",
		"_postman_id": "cbef37ab-d4ae-c349-4845-b4a91d1ab201",
		"description": "",
		"schema": "https://schema.getpostman.com/json/collection/v2.0.0/collection.json"
	},
	"item": [
		{
			"name": "http://localhost:5080/LiveApp/rest/broadcast/create",
			"request": {
				"url": "http://localhost:5080/LiveApp/rest/broadcast/create",
				"method": "POST",
				"header": [
					{
						"key": "Content-Type",
						"value": "application/json",
						"description": ""
					}
				],
				"body": {
					"mode": "raw",
					"raw": "{\"name\":\"test_video\", \"listenerHookURL\":\"http://www.example.com/webhook\"}"
				},
				"description": "ListenerHookURL sample"
			},
			"response": []
		}
	]
}
```


## Webhooks List
Ant Media Server will hook to your website using a POST request with "application/x-www-form-urlencoded" as the body. Some example responses are: 

 * <strong>liveStreamStarted: </strong>Ant Media server calls this hook when a new live stream is started. It sends **POST (application/x-www-form-urlencoded)** request to URL with following variables
   * <strong>id</strong>:  stream id of the broadcast
   * <strong>action</strong>: "liveStreamStarted"
   * <strong>streamName</strong>: stream name of the broadcast. It can be null.
   * <strong>category</strong>:  stream category of the broadcast. It can be null. 
 * <strong>liveStreamEnded: </strong>Ant Media Server calls this hook when a live stream is ended. It sends **POST(application/x-www-form-urlencoded)** request to URL with following variables.
   * <strong>id: </strong>stream id of the broadcast
   * <strong>action: </strong>"liveStreamEnded"
   * <strong>streamName: </strong>stream name of the broadcast. It can be null.
   * <strong>category: </strong>stream category of the broadcast. It can be null.
 * <strong>vodReady: </strong>Ant Media Server calls this hook when the recording of the live stream is ended. It sends **POST(application/x-www-form-urlencoded)** request to URL with following variables.
   * <strong>id</strong>: stream id of the broadcast
   * <strong>action: </strong>"vodReady"
   * <strong>vodName:  </strong>vod file name
   * <strong>vodId:  </strong>vod id in the datastore

That's all. As a result, you can now determine the type of the request by using the <em>action</em> parameter within the POST request.

We hope this post will help you to make automation in your project.  Please <a href="https://antmedia.io/#contact">keep in touch</a> if you have any question. We will be happy if we can help you.

> **Attention:** Please process the POST request within your application as quick as possible as the hooks are called within the event loop thread which will not wait for your application to complete complex tasks.  