This guide explains Data Channel technical details in Ant Media Server. Briefly, Data Channel features are;
1. [What is Data Channel, how can I use?](#1-what-is-data-channel-how-can-i-use)
2. [Data Channel Usage in Android SDK](#2-data-channel-usage-in-android-sdk)
3. [Data Channel Usage in Javascript SDK](#3-data-channel-usage-in-javascript-sdk)
4. [Data Channel Callbacks](#4-data-channel-callbacks)
5. [Demo Data Channel](#5-data-channel-demos)

## 1. What is Data Channel, how can I use?
A data channel is can be used when transferring data from one device to another. Data Channels can be utilized in various use cases where an arbitrary form of data communication is needed in addition to low latency video and audio communication.

### How to enable Data Channel in Management Panel

You can enable Data Channel basically in Ant Media Server Dashboard Panel. You need to enable data channel support in Ant Media Server Management Console `/applications/applicationName` settings tab. After enabling data channel support, the server administrator can choose if the players are allowed to send messages only to the publisher or to the publisher and to all other players or to nobody.

Note: Make sure the enable correct application.

AMS Dashboard photo

## 2. Data Channel Usage in Android SDK
asdasd
Farklı

## 3. Data Channel Usage in Javascript SDK
asdasd
Google Chrome ve Mozilla tipleri farklı 

## 4. Data Channel Callbacks

`callback : function(info, description) {`

 `if (info == "data_channel_opened") {`

     `console.log("data channel is open");`

 `}`
 `else if (info == "data_received") {`

     `console.log("Message received ", description.data );`

     `handleData(description);`
 `}`

 `else if (info == "data_channel_error") {`

     `handleError(description);`

 `} else if (info == "data_channel_closed") {`

     `console.log("Data channel closed " );`
 `}`

## 5. Data Channel Demos

5 - a - String
5 - b - Byte

