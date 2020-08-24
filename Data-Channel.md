This guide explains Data Channel technical details in Ant Media Server. Briefly, Data Channel features are;
* [What is Data Channel?](#what-is-data-channel)
* [Data Channel Usage](#data-channel-usage)
* [Data Channel Hooks](#data-channel-web-hooks)

## What is Data Channel?
Data channel is another channel in WebRTC other than video and audio. In data channel, you can send any kind of information to the other clients. Data Channels can be utilized in various use cases including chatting, control messages, file sharing, etc. Ant Media Server provides a generic data channel infrastructure that can be used in all use cases.

### How to enable Data Channel in Management Panel

![Ant Media Server Management Panel Data Channel](https://antmedia.io/wp-content/uploads/2020/05/Data-Channel-1.png)

Just enable data channel support in Ant Media Server Management Console `/applications/applicationName` settings tab. After enabling data channel support, you can have some options for data delivery. Here are the options you can choose

* **Publisher & All Player**:
Players' and Publisher's messages are delivered to publisher and all other players who are watching the stream and publisher.

* **Only Publisher**:
Player messages are only delivered to the publisher. Publisher messages are delivered to all players.

* **Nobody**:
Only publisher can send messages to the players and players cannot send messages.

## Data Channel Usage
### Usage in JavaScript
![Data Channel in Web](https://antmedia.io/wp-content/uploads/2020/04/webMessageScreenshot-1024x545.png)

Sending and receiving messages via data channels can be implemented using Ant Media Server Javascript SDK with less than 10 lines of code.
When initializing WebRTCAdaptor you need to give a callback function.

`sendData = function(streamId, message)` function in webrtc_adaptor.js is used to send messages like the following code:

```javascript
webRTCAdaptor.sendData("stream1", "Hi!");
```

For communication between different clients, it is always better to send the text messages in a structured data format like XML or JSON. Here is a simple example JSON TextMessage object representing our text messages:

```javascript
{ 
messageId:"uniqueId1", // unique id for each message
messageDate: 23414123235, // time and date as long unix time stamp
messageBody: "Hi" // actual text message typed by the user
} 
``` 
#### JavaScript Callbacks

```javascript
callback : function(info, description) {

 if (info == "data_channel_opened") {

     console.log("data channel is open");

 }
 else if (info == "data_received") {

     console.log("Message received ", description.data );

     handleData(description);
 }

 else if (info == "data_channel_error") {

     handleError(description);

 } else if (info == "data_channel_closed") {

     console.log("Data channel closed " );
 }
```


### Usage in Android SDK

![Data Channel in Android](https://antmedia.io/wp-content/uploads/2020/04/androidMessageScreenshot-600x577.png)

Exchanging data through WebRTC Data Channels is equally straightforward with Ant Media Server Android WebRTC SDK. Your Activity should implement IDataChannelObserver interface as shown below:

```
public interface IDataChannelObserver {
    void onBufferedAmountChange(long previousAmount, String dataChannelLabel);
    void onStateChange(DataChannel.State state, String dataChannelLabel);
    void onMessage(DataChannel.Buffer buffer, String dataChannelLabel);
    void onMessageSent(DataChannel.Buffer buffer, boolean successful);
}
```

When a data channel message is received, `onMessage` method will be called, where you decide how to handle the received data.
Similarly, `onMessageSent` method is called when a message is successfully sent or a sending attempt failed.

Before initialization of WebRTCClient you need to:
* Set your Data Channel observer in the WebRTCClient object like this:

  ```webRTCClient.setDataChannelObserver(this);```

* Enable data channel communication by putting following key-value pair to your Intent before initialization of WebRTCClient with it:

  ```this.getIntent().putExtra(EXTRA_DATA_CHANNEL_ENABLED, true);```

  Then your Activity is ready to send and receive data.

* To send data, call `sendMessageViaDataChannel` method of WebRTCClient and pass the raw data like this:

  ```webRTCClient.sendMessageViaDataChannel(buf);```


## Data Channel Web Hooks
In order to integrate Data Channel to your application, you can use web hooks. All data channel messages are delivered to these hooks as well. 

* Add Data Channel Webhook URL
In order to add default URL,  just follow the steps below

* Open your apps `red5-web.properties` which is under `webapps/<app_name>/WEB-INF` 
* Add `settings.dataChannelWebHook` property and assign your web hook url
* Save the file and sestart the server
```
sudo service antmedia restart
```
After restarting, your url is called with data channel messages.


### Webhooks List

Ant Media Server will hook to your website using a POST request with "multipart/form-data" as the body. Some example responses are:
   
* <strong>data:  </strong>message in the broadcast

### Data Channel REST Service

You can also send Data Channel message with REST API. Here is the usage:

```
curl -X POST
http://localhost:5080/WebRTCAppEE/rest/v2/broadcasts/{STREAM_ID}/data
-H 'content-type: application/json'
-d '{message: ""}'
```

> Important Note: Please keep in mind that the REST interface only responds to the calls that are made from 127.0.0.1 by default. If you call from any other IP Address, it does not return. For allowing more IP Address, take a look at the  Security section later in this post.

For the more detail, please visit our [REST API Guide](REST-Guide) 