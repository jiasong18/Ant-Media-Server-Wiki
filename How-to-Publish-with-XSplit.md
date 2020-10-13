# Publish with a Desktop Software - XSplit 

XSplit is a free and open source software for live streaming and video recording. It is easy to use and provides a great canvas with screen share option for different purposes( pc gaming, talk shows, presentations etc.). Embedded or external camera and audio sources can be used with XSplit.

Step by step guide;	

### 1. Install the XSplit
Download Open Broadcaster Software from [xsplit.com](https://www.xsplit.com) and Install it. It is only available in Windows 7 or newer.

### 2. Provide Sources
When you open the XSplit, it will ask the canvas option for different purposes. They are easy to manipulate with drag and drop. It has some useful features for streaming, for further information you can google about [XSplit tutorial](https://www.google.com/search?q=XSplit+tutorial)
 
![image](https://user-images.githubusercontent.com/32591015/95835582-13be2680-0d47-11eb-93f9-42834de37f95.jpg)

### 3. Configure XSplit
We're assuming that your Ant Media Server accepts all streams. (There is no any security option enabled.)

* Click `Broadcast` in the XSplit Window and then click ‘Set up a new output’ 
* Choose `Custom RTMP` in the `Set up a new output` dropdown menu.
![Adsız4](https://user-images.githubusercontent.com/32591015/95836553-3ef54580-0d48-11eb-8110-b28cf5e4087c.jpg) 

* You can write any name and description you want.
* In the RTMP URL box, type your RTMP URL without stream id. It's like `rtmp://your_server_domain_name/LiveApp`
PS: Your 1935 port should be open and your server should be reachable. 
* In the Stream key, you can write any stream id because we assume that no security option is enabled. 

![Adsız5](https://user-images.githubusercontent.com/32591015/95835836-639ced80-0d47-11eb-8ad3-cbfa8645ae99.jpg)
 

**When you're using tokens** you need to generate a publish token and use it in this format inside the stream key : `streamdid?token=tokenid`
 

### 4: Start Streaming
Close `Settings` window and just click the “Stream” button in the main window of XSplit. It will start streaming.
You can play it in the server like this;
 
![Image](https://user-images.githubusercontent.com/32591015/95836048-a52d9880-0d47-11eb-85e4-4f07d5d2e529.jpeg)





> Quick Link: [Learn How to Play Live Streams](Playing-Live-Streams)
