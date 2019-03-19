We have 5 different load test plans for different aims.  
### HLS Player Load Test
*aim:* This test is performed to determine the maximum number of HLS players that play the same stream.
*scenario:* We publish a stream with RTMP and connect increasing numbers of HLS players to the stream.
*measurements:* By using REST api we get CPU and memory usage. We get speed from ffmpeg.
*expectations:* speed > 1x

*results:* As a result, we have 3 plots: 
  - CPU usage vs time 
  - Publisher/Players count vs time
  - Memory usage vs time

### RTMP Player Load Test
*aim:* This test is performed to determine the maximum number of RTMP players that play the same stream.
*scenario:* We publish a stream with RTMP and connect increasing numbers of RTMP players to the stream.
*measurements:* By using REST api we get CPU and memory usage. We get speed from ffmpeg.
*expectations:* speed > 1x
*results:* As a result, we have 3 plots: 
  - CPU usage vs time 
  - Publisher/Players count vs time
  - Memory usage vs time

### WebRTC Player Load Test
*aim:* This test is performed to determine the maximum number of WebRTC players that play the same stream.
*scenario:* We publish a stream with WebRTC and connect increasing numbers of WebRTC players to the stream.
*measurements:* By using REST api we get CPU usage, memory usage.
*results:* As a result, we have 6 plots:
  - CPU usage vs time 
  - Publisher/Players count vs time
  - Memory usage vs time
  - Measured & Send bitrate vs time 
  - Video Send Period vs time
  - Audio Send Period vs time

### RTMP Publisher Load Test
*aim:* This test is performed to determine the maximum number of RTMP publishers for a server.
*scenario:* We create the increasing number of publishers that publish to the same server. This test can be made for different encoding settings.
*measurements:* By using REST api we get CPU usage, memory usage.
*expectations:* If there is no encoding CPU usage is to be very low compared to the existence of encoding.
*results:* As a result, we have 3 plots: 
  - CPU usage vs time 
  - Publisher/Players count vs time
  - Memory usage vs time 

### WebRTC Publisher Load Test
*aim:* This test is performed to determine the maximum number of WebRTC publishers for a server.
*scenario:* We create the increasing number of publishers that publish to the same server. This test can be made for different encoding settings.
*measurements:* By using REST api we get CPU usage, memory usage.
*expectations:* If there is no encoding CPU usage is to be very low compared to the existence of encoding.
*results:* As a result, we have 3 plots: 
  - CPU usage vs time 
  - Publisher/Players count vs time
  - Memory usage vs time 