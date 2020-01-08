# Ant Media Server
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