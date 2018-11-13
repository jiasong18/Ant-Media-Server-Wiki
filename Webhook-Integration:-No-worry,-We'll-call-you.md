Ant Media Server provides webhooks for making your system/app know when certain events occurs on the server. Therefore, you can register your URL to Ant Media Server which makes POST request when a live stream is started, finished or recorded. Firstly,  let's look at how to register your backend URL to Ant Media Server as a webhook. After that, we'll provide reference for webhooks.

<img src="https://antmedia.io/wp-content/uploads/2018/11/webhooks-300x273.png" alt="webhook ant media server" width="300" height="273" class="aligncenter wp-image-5807 size-medium" title="webhooks ant media server" />
<h2>Register Your Webhook URL</h2>
You can add default webhook URL to your streaming app on Ant Media Server. In addition, it lets you add custom specific webhook URL's in creating broadcast.
<h3>Add default Webhook URL</h3>
In order to add default URL,  just follow the steps below
<ul>
 	<li>Open your apps <em>red5-web.properties</em>  and add <em>settings.listenerHookURL</em> property to that file. <em>red5-web.properties </em>file is under <em>webapps/&lt;app_name&gt;/WEB-INF</em>  folder
<pre>...
settings.listenerHookURL=http://www.example.com/webhook/
...</pre>
</li>
 	<li>Open your apps <em>red5-web.xml</em> file and add <em>settings.listenerHookURL</em> to the <em>app.settings</em> bean.
<pre class="p1"><span class="s1">...
&lt;</span><span class="s2">bean</span><span class="s3"> </span><span class="s4">id</span><span class="s3">=</span>"app.settings"<span class="s3"> </span><span class="s4">class</span><span class="s3">=</span>"io.antmedia.AppSettings"<span class="s1">&gt;
...</span>
<span class="s1">  &lt;</span><span class="s2">property</span><span class="s3"> </span><span class="s4">name</span><span class="s3">=</span>"listenerHookURL"<span class="s3"> </span><span class="s4">value</span><span class="s3">=</span>"${settings.listenerHookURL}"<span class="s3"> </span><span class="s1">/&gt;
</span>...
<span class="s1">&lt;/</span>bean&gt;
...</pre>
</li>
 	<li>Restart the server on command line
<pre>sudo service antmedia restart</pre>
</li>
</ul>
Right now, there is a default webhook URL for your app.
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
It's time to know when Ant Media Server calls webhooks with which parameters. Here is a simple list for that

 * <strong>liveStreamStarted: </strong>Ant Media server calls this hook when a new live stream is started. It sends POST (application/x-www-form-urlencoded) request to URL with following variables
   * <strong>id</strong>:  stream id of the broadcast
   * <strong>action</strong>: "liveStreamStarted"
   * <strong>streamName</strong>: stream name of the broadcast. It can be null.
   * <strong>category</strong>:  stream category of the broadcast. It can be null. 
 * <strong>liveStreamEnded: </strong>Ant Media Server calls this hook when a live stream is ended. It sends POST(application/x-www-form-urlencoded) request to URL with following variables.
   * <strong>id: </strong>stream id of the broadcast
   * <strong>action: </strong>"liveStreamEnded"
   * <strong>streamName: </strong>stream name of the broadcast. It can be null.
   * <strong>category: </strong>stream category of the broadcast. It can be null.
 * <strong>vodReady: </strong>Ant Media Server calls this hook when the recording of the live stream is ended. It sends POST(application/x-www-form-urlencoded) request to URL with following variables.
   * <strong>id</strong>: stream id of the broadcast
   * <strong>action: </strong>"vodReady"
   * <strong>vodName:  </strong>vod file name

That's all. As a result, you can take some custom actions according to your project by checking <em>action</em> variable in your backend URL.

We hope this post will help you to make automation in your project.  Please <a href="https://antmedia.io/#contact">keep in touch</a> if you have any question. We will be happy if we can help you.

> **Attention:** Please process the result in your backend URL as soon as possible because these callbacks is called in event loop threads that does not support long running operations.  

&nbsp;