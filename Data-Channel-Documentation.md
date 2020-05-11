This guide explains Data Channel technical details in Ant Media Server. Briefly, Data Channel features are;
1. [What is Data Channel, How to use Data Channel with Ant Media Server](#1-what-is-data-channel-how-can-i-use)
2. [Data Channel Usage in Javascript SDK](#2-data-channel-usage-in-javascript-sdk)
3. [Data Channel Usage in Android SDK](#3-data-channel-usage-in-android-sdk)
4. [Data Channel Usage in iOS SDK](#4-data-channel-usage-in-ios-sdk)
5. [Data Channel Modes](#5-data-channel-modes)
6. [Data Channel Hooks](#6-data-channel-hooks)

## 1. What is Data Channel, how can I use?
A data channel is can be used when transferring data from one device to another. Data Channels can be utilized in various use cases where an arbitrary form of data communication is needed in addition to low latency video and audio communication.

### How to enable Data Channel in Management Panel

![Ant Media Server Management Panel Data Channel](https://antmedia.io/wp-content/uploads/2020/05/Data-Channel-1.png)

You can enable Data Channel basically in Ant Media Server Dashboard Panel. You need to enable data channel support in Ant Media Server Management Console `/applications/applicationName` settings tab. After enabling data channel support, the server administrator can choose if the players are allowed to send messages only to the publisher or to the publisher and to all other players or to nobody.

Note: Make sure to enable the correct application.


## 2. Data Channel Usage in Javascript SDK

![Data Channel in Web](https://antmedia.io/wp-content/uploads/2020/04/webMessageScreenshot-1024x545.png)

Sending and receiving messages via data channels can be implemented using Ant Media Server Javascript SDK with less than 10 lines of code.

When initializing WebRTCAdaptor you need to give a callback function (see Java Script SDK Guide in Wiki).

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

### Data Channel Callbacks

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


## 3. Data Channel Usage in Android SDK

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

When a data channel message is received, onMessage method will be called, where you decide how to handle the received data.

Similary, onMessageSent method is called, when a message is successfully sent or a sending attempt failed.

Before initialization of WebRTCClient you need to:

Set your Data Channel observer in the WebRTCClient object like this:

`webRTCClient.setDataChannelObserver(this);`

Enable data channel communication by putting following key-value pair to your Intent before initialization of WebRTCClient with it:

`this.getIntent().putExtra(EXTRA_DATA_CHANNEL_ENABLED, true);`

Then your Activity is ready to send and receive data.

To send data, the developer just needs to call sendMessageViaDataChannel method of WebRTCClient and pass the raw data like this:

`webRTCClient.sendMessageViaDataChannel(buf);`

Data Channel implementation in Ant Media Server opens a new set of possibilities and use cases for our customers and end-users. Furthermore, our functional and practical SDKs will make life easier for WebRTC developers.

## 4. Data Channel Usage in iOS SDK


## 5. Data Channel Modes
![Data Channel Modes](https://antmedia.io/wp-content/uploads/2020/05/Data-Channel-2.png)

There are 3 modes in Data Channel feature. These are `Publisher & All Player`, `Only Publisher` and `Nobody`

### a- Publisher & All Player
Player messages are delivered to all other players who are watching the stream and publisher. Again publisher messages are delivered to all players.

### b- Only Publisher
Player messages are only delivered to the publisher and again publisher messages are delivered to all players.

### c- Nobody
Only publisher can send messages to the players and players cannot send messages.

## 6. Data Channel Hooks
All data channel messages are delivered to these hooks as well. So that you can able to integrate into any third-party application.

* Add Data Channel Webhook URL
In order to add default URL,  just follow the steps below

Open your apps `red5-web.properties`  and add `settings.dataChannelWebHook` property to that file. `red5-web.properties` file is under `webapps/<app_name>/WEB-INF` folder.

* Restart the server on command line
sudo service antmedia restart

After the restarting, you can use the Data Channel Webhook feature.


