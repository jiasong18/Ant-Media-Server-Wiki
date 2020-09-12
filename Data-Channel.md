This guide explains Data Channel technical details in Ant Media Server. Content of this guide is as follows;
* [What is Data Channel?](#what-is-data-channel)
* [Data Channel Usage](#data-channel-usage)
  * [Usage in Javascript](#usage-in-javascript)
  * [Usage in Android SDK](#usage-in-android-sdk)
  * [Usage in iOS SDK](#usage-in-ios-sdk)
  * [Usage in REST API Service](#usage-in-rest-api-service)
* [Data Channel Hooks](#data-channel-web-hooks)

## What is Data Channel?
Data channel is another channel in WebRTC other than video and audio. In data channel, you can send any kind of information to the other clients. Data Channels can be utilized in various use cases including chatting, control messages, file sharing, etc. Ant Media Server provides a generic data channel infrastructure that can be used in all use cases.

### How to Enable Data Channel in Management Panel

![Ant Media Server Management Panel Data Channel](https://antmedia.io/wp-content/uploads/2020/05/Data-Channel-1.png)

Just enable data channel support in Ant Media Server Management Console `/applications/applicationName` settings tab. After enabling data channel support, you can have some options for data delivery. Here are the options you can choose:

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

**Send Text Message with Data Channel**

In Android, we send text messages using sendTextMessage method. It first gets the text from the user input as below.

```
public void sendTextMessage() { 
    String messageToSend = messageInput.getText().toString(); String messageToSendJson = Message.createJsonTextMessage(computeMessageId(), 
    new Date(), messageToSend); final ByteBuffer buffer = ByteBuffer.wrap(messageToSendJson.getBytes(Charset.defaultCharset())); 
    DataChannel.Buffer buf= new DataChannel.Buffer(buffer,false); webRTCClient.sendMessageViaDataChannel(buf); 
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

**Receiving and Displaying Images with Data Channel**

BinaryDataReceiver receives the data chunk by chunk, parses the header information from the chunks and merges them.

In Android, onMessage is called for each chunk but this time received data is binary. We use BinaryDataReceiver object to merge the chunks like this:

```java
if (buffer.binary) {
  binaryDataReceiver.receiveDataChunk(buffer.data);
  if(binaryDataReceiver.isAllDataReceived()) {
    Bitmap bmp=BitmapFactory.decodeByteArray(binaryDataReceiver .receivedData.array(),0,binaryDataReceiver.receivedData.capacity());
    final ImageMessage message = new ImageMessage();
    message.parseJson(binaryDataReceiver.header.text);
    message.setImageBitmap(bmp);
    messageAdapter.add(message);
    // scroll the ListView to the last added element
    messagesView.setSelection(messagesView.getCount() - 1);
    binaryDataReceiver.clear();
  }
}
```

When all the data is received, we decode the image using a BitmapFactory and create a ImageMessage which will be added to our ListView in the MessageAdapter mentioned in the section above:

```java
ImageMessage imageMessage = (ImageMessage) message;
ImageView imageBody;
if (message.isBelongsToCurrentUser()) {
  convertView = messageInflater.inflate(R.layout.my_image_message, null);
  // this message was sent by us
  imageBody = convertView.findViewById(R.id.image_body_my);
  messageDate = convertView.findViewById(R.id.message_date);
  messageDate.setText(imageMessage.getMessageDate());
  imageBody.setImageBitmap(imageMessage.getImageBitmap());
} else {
  // this message was sent by someone else
  convertView = messageInflater.inflate(R.layout.their_image_message, null);
  ...
}
```

We create an ImageView for each bitmap and dependent on if the image sent by us or received, we display the image differently using a different layout for each case.

In the Web side at present, we have again some browser differences. The default expected type for binary data is Blob in the WebRTC standard. Firefox supports it currently but Chrome does not support it and sets the type of received data internally to ArrayBuffer for binary messages. To overcome these differences we handled both cases in our code and converted Blobs to more generic type ArrayBuffer whenever possible.

In the Javascript using our BinaryDataReceiver object we handle received image data chunks like this:

```java
function handleImageData(data) {
  binaryDataReceiver.receiveDataChunk(data);
  if (binaryDataReceiver.isAllDataReceived()) {
    var jsonHeader = JSON.parse(binaryDataReceiver.headerText);
    var date = new Date(jsonHeader.messageDate);
    var bytes = new Uint8Array(binaryDataReceiver.receivedData);
    var blob = new Blob([bytes.buffer]);
    // create image URL
    var urlCreator = window.URL || window.webkitURL;
    var imageUrl = urlCreator.createObjectURL(blob);

    createImageMessage(imageUrl, false);
    imageReceiver.clear();
  }
}
```

When all of the image data chunks are received, we parse metadata about the image from the header and then create a Object URL from it. Finally, we display it using <img/> HTML tag in our chat window.

![](https://antmedia.io/wp-content/uploads/2020/04/webImageScreenshot2.png)

There is also data channel usage example exist in the Sample project.

For more detail, please visit our [Android SDK Guide](WebRTC-Android-SDK-Documentation) 

### Usage in iOS SDK

Ant Media Server and WebRTC iOS SDK can use data channels in WebRTC. In order to use Data Channel, make sure that it’s enabled both server-side and mobile. In order to enable it for server-side, you can just set the `enableDataChannel` parameter to true in `setOptions` method.

```
webRTCClient.setOptions(url: "ws://your_server_url:5080/WebRTCAppEE/websocket", streamId: "stream123", token: "", mode: .play, enableDataChannel: true)
```

After that, you can send data with the following method of `AntMediaClient`

```
func sendData(data: Data, binary: Bool = false)
```

When a new message is received, the delegate’s following method is called:

```
func dataReceivedFromDataChannel(streamId: String, data: Data, binary: Bool)
```

There is also data channel usage example in the Sample project.

For more detail, please visit our [iOS SDK Guide](WebRTC-iOS-SDK-Guide) 

### Usage in REST API Service

You can also send a Data Channel message with REST API. Here is the usage:

```
curl -X POST
http://localhost:5080/WebRTCAppEE/rest/v2/broadcasts/{STREAM_ID}/data
-H 'content-type: application/json'
-d '{message: "test"}'
```

This REST Service is generic. So, you can add your parameters for your requirements.

> Important Note: Please keep in mind that the REST interface only responds to the calls that are made from 127.0.0.1 by default. If you call from any other IP Address, it does not return. For allowing more IP Address, take a look at the  Security section later in this post.

For more detail, please visit our [REST API Guide](REST-Guide) 

## Data Channel Web Hooks
In order to integrate Data Channel to your application, you can use web hooks. All data channel messages are delivered to these hooks as well. 

* Add Data Channel Webhook URL
In order to add default URL,  just follow the steps below

* Open your apps `red5-web.properties` which is under `webapps/<app_name>/WEB-INF` 
* Add `settings.dataChannelWebHook` property and assign your web hook url
* Save the file and restart the server
```
sudo service antmedia restart
```
After restarting, your url is called with data channel messages.


### Webhooks List

Ant Media Server will hook to your website using a POST request with "multipart/form-data" as the body. Some example responses are:
   
* <strong>data:  </strong>message in the broadcast