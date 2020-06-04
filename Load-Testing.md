Test environment has two parts: test server and SUT (system under test). We have two different setups for two different SUT. 
# Preparation of SUT
## One Instance Setup
Here we have only one Ant Media Server instance as SUT.
```
+-------------------+                  +----------------------+
|                   |   streaming      |                      |
|                   |   playing        |                      |
|                   | <--------------> |                      |
|    Test Server    |                  |   Ant Media Server   |
|                   |                  |                      |
|                   | <--------------> |                      |
|                   |    rest          |                      |
+-------------------+                  +----------------------+
```
* To prepare Ant Media Server, please look at [here](Installation).

## Cluster Setup
Here we have a cluster structure as SUT which contains one origin and N edge servers.
```
                                       +--------------------+
                                       |                    |
                                       |                    |
                                       |  Ant Media Server  |
                            +--------->+                    |
                            |          |     (Origin)       |
+-----------+               |          |                    |
|           |    streaming  |          |                    |
|           +---------------+          +--------------------+
|           |
|Test Server| playing +------------------------------------------------+
|           +<--------+                                                |
|           |         |             Load Balancer                      |
|           +---------+                                                |
+-----------+   rest  +--+------------+---------------------+----------+
                         |            |                     |
                         |            |                     |
                         |            |                     |
                         |            |                     |
                         |            |                     |
           +-------------+--+  +------+---------+     +-----+----------+
           |                |  |                |     |                |
           |                |  |                |     |                |
           |Ant Media Server|  |Ant Media Server| ... |Ant Media Server|
           |                |  |                |     |                |
           |   (Edge-1)     |  |   (Edge-2)     |     |   (Edge-N)     |
           |                |  |                |     |                |
           |                |  |                |     |                |
           +----------------+  +----------------+     +----------------+

```
* To prepare Cluster, please look at [here](Scaling-and-Load-Balancing).

***

# Ant Media WebRTC Test Tool
You can download test tool from [here](https://drive.google.com/file/d/16YLb5zAbSJ8rgzmoP6P01SmOl-C0WmVe/view?usp=sharing).

Ant Media WebRTC Test Tool is a java project for testing Ant Media Server WebRTC capabilities.

* Ant Media WebRTC Test is compatible with Ant Media Server signaling protocol. 
* Ant Media WebRTC Test has two modes: publisher and player. (-m flag determines the mode)
* Ant Media WebRTC Test has two options with UI or without UI. (-u flag determines the UI on/off)
* You can save received(in player mode) video.
* You can create load with -n flag as many as your machine CPU lets you.

# Running Ant Media WebRTC Test Tool

It can be run from terminal with the following options.
```
./run.sh -f output.mp4 -m publisher -n 1  #publishes output.mp4 to the server with default name myStream
```

```
./run.sh -m player -n 100 -i stream1 -s 10.10.175.53 -u false #plays 100 viewers for default stream myStream
```

### Parameters
```
Flag 	 Name      	 Default   	 Description                 
---- 	 ----      	 -------   	 -----------   
f    	 File Name 	 test.mp4     	 Source file* for publisher output file for player        
s    	 Server IP 	 localhost 	 server ip                   
q    	 Security  	 false     	 true(wss) or false(ws)      
l        Log Level       3               0:VERBOSE,1:INFO,2:WARNING,3:ERROR,4:NONE
i    	 Stream Id 	 myStream  	 id for stream               
m    	 Mode      	 player    	 publisher or player         
u    	 Show GUI  	 true      	 true or false               
p    	 Port      	 5080      	 websocket port number 
v    	 Verbose   	 false     	 true or false 
n    	 Count     	 1         	 Number of player/publisher connctions 
k        Kafka Broker    null            Kafka broker address with port
r    	 Publish Loop 	 false           true or false
c    	 Codec           h264            h264 or VP8 
d    	 Data Channel    false           true or false 
```

*File in mp4 format should have h264 encoded video and opus encoded audio
