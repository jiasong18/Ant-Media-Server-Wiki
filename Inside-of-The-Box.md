## What is in Docker Container?
After building the dockerfile we get a docker image which contains all necessary software and artifacts for load test.
### Installed Softwares
- *Java:* Used by JMeter and test web application.
- *JMeter:* Used as test execution engine for load tests.
- *FFmpeg:* Used for RTMP publishing and, RTMP and HLS playing.
- *QT Libraries:* Necessary for WebRTCTest tool.
- *Ant Media Tools:* WebRTCTest (used for both publishing and playing), RTMPClient (used for playing and measuring latency), Test web application (used for managing tests).
- *Other:* gnuplot (to plot result graphs), jq (to read json from bash), unzip  

### Directory Structure  
```
/home/antmedia/test
|-- TestScriptAndTools-master   (this directory is downloaded from hithub)
|   |-- 1_HLSLoadTest
|   |   |-- RTMP_HLS.jmx
|   |   '-- report.sh
|   |-- 2_RTMPLoadTest
|   |   |-- RTMP_RTMP.jmx
|   |   '-- report.sh
|   |-- 3_WebRTCLoadTest
|   |   |-- WebRTC_WebRTC.jmx
|   |   '-- report.sh
|   |-- 4_RTMPPublisherLoadTest
|   |   |-- RTMP_Publisher.jmx
|   |   '-- report.sh
|   |-- 5_WebRTCPublisherLoadTest
|   |   |-- WebRTC_Publisher.jmx
|   |   '-- report.sh
|   |-- Common
|   |   |-- config.json
|   |   |-- config.sh
|   |   '-- runtest.sh
|   |-- Media
|   |   '-- Test_600kbps.mp4
|   '-- Tools
|       |-- RTMPClient
|       |-- WebRTCTest
|       '-- loadtester-0.0.1-SNAPSHOT-spring-boot.jar
|
|-- apache-jmeter-5.0 (jmeter installation directory. subdirectories are cropped)
|
|
'-- results      (created after first test)
    |-- history
    |-- hls_play
    |   |-- 2018-11-26_18:24:49      (for each test run a directory is created like this)
    |   |   |-- getonets.csv       (created by JMeter contains result data)
    |   |   |-- jmeter.log         (created by JMeter)
    |   |   |-- m3u8.xml           (created by JMeter)
    |   |   |-- measures.csv       (created by JMeter contains result data)
    |   |   |-- measures.png       (created after test finished by report.sh)
    |   |   |-- report.sh          (copied from TestScriptAndTools-master directory)
    |   |   |-- RTMPClient         (copied from TestScriptAndTools-master directory)
    |   |   |-- RTMP_HLS.jmx       (copied from TestScriptAndTools-master directory)
    |   |   |-- rtmp_hls.jtl       (copied from TestScriptAndTools-master directory)
    |   |   |-- Test_600kbps.mp4   (copied from TestScriptAndTools-master directory)
    |   |   '-- WebRTCTest         (copied from TestScriptAndTools-master directory)
    |   '-- YYYY-MM-DD_hh:mm:ss
    |-- rtmp_play
    |   '- YYYY-MM-DD_hh:mm:ss
    |-- rtmp_publish
    |   '-- YYYY-MM-DD_hh:mm:ss
    |-- webrtc_play
    |   '-- YYYY-MM-DD_hh:mm:ss
    '-- webrtc_publish
        '-- YYYY-MM-DD_hh:mm:ss
```
## What happens?
```
             X                 .===========.                                                                            X       .================.
             X                 |TEST SERVER|                                                                            X       |ANT MEDIA SERVER|
             X                 *===========*                                                                            X       *================*
             X                                                                                                          X
             X                                                                                                          X
  +------+   X   +-------------+                   +------------+                                        +--------+     X       +-------------------+
  |      |   X   |             |                   |            |                                        |        |     X       |      SUT          |
  | User |   X   | Test Web    |                   | runtest.sh |                                        | JMeter |     X       |                   |
  |      |   X   | Application |                   |  confgi.sh |                                        |        |     X       | one instance or   |
  |      |   X   |             |                   |            |                                        |        |     X       | cluster setup     |
  +--+---+   X   +------+------+                   +-----+------+                                        +----+---+     X       +--------+----------+
     |       X          |                                |                                                    |         X                |
     |   save config    |                                |                                                    |         X                |
     | +--------------> |         +-------------+        |                                                    |         X                |
     |       X          |  save   |             |        |                                                    |         X                |
     |       X          | +-----> | config.json |        |                                                    |         X                |
     |       X          |         |             |        |                                                    |         X                |
     |       X          |         +------+------+        |                                                    |         X                |
     |       X          |                |               |                                                    |         X                |
     |       X          |                |               |                                                    |         X                |
 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     |       X          |                |               |                                                    |         X                |
     |  run test        |                |               |                                                    |         X                |
     | +--------------> |                |               |                                                    |         X                |
     |       X          |  run test      |               |                                                    |         X                |
     |       X          | +----------------------------> |                                                    |         X                |
     |  is test finished|                |               |                      +----------------+            |         X                |
     | +--------------> |                |   read        |                      | Test directory |            |         X                |
     |       X          +----+           | <-----------+ | create test dir      | named with time|            |         X                |
     |       X          |    |           |               | copy artifacts from  |                |            |         X                |
     |       X          |    |           |               | TestTools&Scrtips    | testplan.jmx   |            |         X                |
     |       X          |    |           |               | +------------------> | report.sh      |            |         X                |
     |       X          |    |           |               |                      | Test.mp4       |            |         X                |
     |       X          |    |           |               |                      | WebRTCTest     |            |         X                |
     |       X          |    |           |               |                      | RTMPClient     |            |         X                |
     |       X          |    |           |               |                      |                |            |         X                |
     |       X          |    |           |               |                      +-------+--------+            |         X                |
     |       X          |    |           |               |                              |                     |         X                |
     |       X          |    |           |               |                              |                     |         X                |
     |       X          |    |           |               |   start(config, testplan)    |                     |         X                |
     |       X          |    |           |               | +------------------------------------------------> |         X                |
     |       X          |    |           |               |                              |                     +----+  webrtc             |
     |       X          |    |           |               |                              |                     |    |  rtmp               |
     |       X          |    |           |               |                              |                     |    |  http (rest or hls) |
     |       X          |    |           |               |                              |                     |    |<<=================>>|
     |       X          |    |           |               |                              |  write measurments  |    |    X                |
     |       X          |    |           |               |                              |  to csv files       |    |    X                |
     |       X          |    |           |               |                              | <-----------------+ |    |    X                |
     |       X          |    |           |               |                              |                     |    |    X                |
     |       X          |    |           |               |     return                   |                     |<---+    X                |
     |       X          |    |           |               | <-------------------------------------------------+|         X                |
     |       X          |    |           |               |                              |                     |         X                |
     |       X          |    |           |               |    run report.sh to plot png |                     |         X                |
     |       X          |    |           |               | +--------------------------> |                     |         X                |
     |       X          |    |           |               |                              |                     |         X                |
     |       X          |    |       test|finished       |                              |                     |         X                |
     |   test finished  |<---+--------------------------------------------------------+ |                     |         X                |
     | <--------------+ |                |               |                              |                     |         X                |
     |       X          |                |               |                              |                     |         X                |
     |       X          |                |               |                              |                     |         X                |
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

```