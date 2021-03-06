# Ant Media Server
## How to fix Pixelating?
?
## How to fix Choppy Stream?
?
## I see High CPU Usage.
Please get Thread Dump by using the following REST methods:
`GET http://AMS_URL:5080/rest/threads-info`
`GET http://AMS_URL:5080/rest/thread-dump-json`
`GET http://AMS_URL:5080/rest/thread-dump-raw`
You can easily call these methods via the browser address bar since all of them are GET methods. 

Check whether there exists a **dead-locked-thread** by the `threads-info` method.

Check **blocked time** of the threads got by `thread-dump-json` method.

For more details, analyze threads by loading the raw dump file by the VisualVM tool.

## I see High Memory Usage.
Please get Memory Dump by using the following REST method:
`GET http://AMS_URL:5080/rest/heap-dump`
You can easily call this method via the browser address bar since all of it is a GET method. 

Load the created heapdump.hprof file bt the VisualVM tool and make memory analyze.

Check for any leaks. You can also use [Eclipse Memory Analyzer Tool](https://www.eclipse.org/mat/) tool to find leaks automatically.

## I get notSetRemoteDescription error.
Your device may not have the necessary h264 codec. Check your device codec compatibility from:
https://mozilla.github.io/webrtc-landing/pc_test_no_h264.html

## I send a stream to the server with RTMP but I can't play the stream with WebRTC player?

This issue( "NoStreamExist") due to your sending stream resolution is not enough for the configured Adaptive Bitrate values.

In WebRTC side, you can check your camera resolution capacity in below links:
https://webrtchacks.github.io/WebRTC-Camera-Resolution/
https://webrtc.github.io/samples/src/content/getusermedia/resolution/

In RTMP side, you need to check your Video Resolution(Output Resolution) setting values in OBS Video section as an image -> https://imgur.com/a/JlVv04h
Your Output resolution size should be high than your Adaptive Streaming setting.

If you want to stream 720HD, you need to provide the requirements at least 720p.

Be sure about video resolution in your adaptive settings is equals and less than the stream you send.

## I can't start autoplay in Google Chrome & Mozilla Firefox?
This is about browsers' policy rules. 

https://developers.google.com/web/updates/2017/09/autoplay-policy-changes

https://developer.mozilla.org/en-US/docs/Web/Media/Autoplay_guide#The_autoplay_feature_policy

## Connect JMX port via SSH Tunel

JMX port in Ant Media Server only accepts incoming requests coming from `localhost`. In order to connect JMX port to your remote server, you need to have port forwarding like below 
```
ssh  -N -L 5599:localhost:5599 username@your_server_address
```
The command above forwards your local computer port 5599 to your server's 5599 port. After running this command, you can have JMX connection to your own local computer 5599 port via VisualVM, JConsole, etc. and it's connected to your server automatically.

## Caused by: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target

The reasons for this error are that your CA certificate is not available on your server. For this, you need to download the root and intermediate certificates of the SSL provider (SHA-1, SHA-2 must be the correct version). After that, you need to import Java, which works currently active with the keytool tool.

For example:

`keytool -import -trustcacerts -alias AddTrustExternalCARoot -file comodorsaaddtrustca.crt -keystore /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/cacerts`

`keytool -import -trustcacerts -alias comodointermediate -file addtrustexternalcaroot.crt -keystore /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/cacerts`

`keytool -import -trustcacerts -alias comodointermediate2 -file comodorsadomainvalidationsecureserverca.crt -keystore /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/cacerts`