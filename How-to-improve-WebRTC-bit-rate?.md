***
**_NOTE:_** We have updated our documentation. This page is outdated. You can access updated version from the sidebar menu.
***
Let's remember the definition of WebRTC from its founders: 
"WebRTC is a free, open project that provides browsers and mobile applications with Real-Time Communications (RTC) capabilities via simple APIs. The WebRTC components have been optimized to best serve this purpose."

As you may know, the main purpose of WebRTC is Real-Time Communication.
Image quality is an opponent power against real-time (ultra-low latency) communication.
So, there should be a break-even point for the balance of latency and image quality.
The optimum video speed with the current processor and communication platforms is 2500 Kbps.

There are some references to this issue:
- A blog from WebRTC Expert Tashi Levent Levi:  https://bloggeek.me/webrtc-vs-zoom-video-quality/
- A paper from academia: http://wimnet.ee.columbia.edu/wp-content/uploads/2017/10/WebRTC-Performance.pdf
- Test results for the limits from webrtc-experiment.com 
        https://www.webrtc-experiment.com/webrtcpedia/
        Maximum video bitrate on chrome is about 2Mb/s (i.e. 2000kbits/s).
        Minimum video bitrate on chrome is .05Mb/s (i.e. 50kbits/s).
        Starting video bitrate on chrome is .3Mb/s (i.e. 300kbits/s).

As a result, everyone needs to measure the best performant configuration of their infrastructure by changing them step-by-step.
My suggestions are as follows:
- 20 for FPS is optimum; however, 10 and 15 should be examined.
- 720p is good enough for video quality, especially for mobile platforms.
- 1000 Kbps is optimum for 720p, 750 Kbps is also acceptable when FPS is 10.

Have a nice ultra-low latency live streaming experience with WebRTC and sub-second expert Ant Media Server.