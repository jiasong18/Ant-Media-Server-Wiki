## What is in Docker Container?
After building the dockerfile we get a docker image which contains all necessary software and artifacts for load test.
### Installed Softwares
- *Java:* Used by WebRTCTest Tool and test web application.
- *FFmpeg:* Used for RTMP publishing and, RTMP and HLS playing.
- *Ant Media Tools:* WebRTCTest (used for both publishing and playing) and Test web application (used for managing tests).

### Directory Structure  
```
/home/antmedia/test
|-- TestScriptAndTools-master   (this directory is downloaded from hithub)
|   |-- Media
|   |   '-- Test_600kbps.mp4
|   '-- Tools
|       |-- webrtctest
|       |   |-- run.sh
|       |   |-- lib
|       |   '-- webrtctest.jar
|       '-- loadtester-0.0.1-SNAPSHOT-spring-boot.jar
||
|
'-- results      (created after first test)
    '-- YYYY-MM-DD_hh:mm:ss
    
```