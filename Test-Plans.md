We have 5 different load test plans(prepared with JMeter) for different aims.  
### HLS Player Load Test
*aim:* This test is performed to determine the maximum number of HLS players that play the same stream.
*scenario:*We publish a stream with RTMP and connect increasing numbers of HLS players to the stream.
*measurements:* By using REST api we get CPU and memory usage. With JMeter, we measure the time to get one ts file.
*expectations:* one ts download time is to be the duration of ts. (for Test_600kbps.mp4 ts durations are 2 sec)
*results:* As a result, we have 3 plots:
  - CPU usage vs Players count
  - Memory usage vs Players count
  - Time to get one ts vs Players count
### RTMP Player Load Test
*aim:* This test is performed to determine the maximum number of RTMP players that play the same stream.
*scenario:* We publish a stream with RTMP and connect increasing numbers of RTMP players to the stream.
*measurements:* By using REST api we get CPU and memory usage. With RTMPClient tool, we measure latency which is the time difference between the total time to collect 25 frames and DTS/display time) of the 25th frame.
*expectations:* latency is to be zero.
*results:* As a result, we have 3 plots:
  - CPU usage vs Players count
  - Memory usage vs Players count
  - Latency vs Players count
### WebRTC Player Load Test
*aim:* This test is performed to determine the maximum number of WebRTC players that play the same stream.
*scenario:*We publish a stream with WebRTC and connect increasing numbers of WebRTC players to the stream.
*measurements:* By using REST api we get CPU usage, memory usage, video send period and audio send period.
*expectations:* video and audio send period are to be about 40ms and 10ms respectivly.
*results:* As a result, we have 4 plots:
  - CPU usage vs Players count
  - Memory usage vs Players count
  - Video send period vs Players count
  - Audio send period vs Players count
### RTMP Publisher Load Test
*aim:* This test is performed to determine the maximum number of RTMP publishers for a server.
*scenario:* We create the increasing number of publishers that publish to the same server. This test can be made for different encoding settings.
*measurements:* By using REST api we get CPU usage, memory usage.
*expectations:* If there is no encoding CPU usage is to be very low compared to the existence of encoding.
*results:* As a result, we have 2 plots:
  - CPU usage vs Publisher count
  - Memory usage vs Publisher count
### WebRTC Publisher Load Test
*aim:* This test is performed to determine the maximum number of WebRTC publishers for a server.
*scenario:* We create the increasing number of publishers that publish to the same server. This test can be made for different encoding settings.
*measurements:* By using REST api we get CPU usage, memory usage.
*expectations:* If there is no encoding CPU usage is to be very low compared to the existence of encoding.
*results:* As a result, we have 3 plots:
  - CPU usage vs Publisher count
  - Memory usage vs Publisher count