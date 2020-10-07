`vMix` is a software vision mixer available for the Windows operating system. The software is developed by StudioCoast PTY LTD. Like most vision mixing software, it allows users to switch inputs, mix audio, record outputs, and live stream cameras, videos files, audio, and more, in resolutions of up to 4K. It can stream up to three destination at one time.

Assuming you have installed the vMix in your personal computer.

### 1. Provide Sources
Click to the add input button and add some input for the broadcast. As an example i will be adding an display input.

You can check https://www.vmix.com/support/training-videos.aspx to get better in vMix.
![image](https://user-images.githubusercontent.com/54481799/95338115-41285180-08bb-11eb-8e61-d8a63e564cf5.png)

As you can see my input has been added successfully and its preview can be seen.
![image](https://user-images.githubusercontent.com/54481799/95338335-7df44880-08bb-11eb-839c-5f9a443ec6bf.png)

### 2. Configure vMix
* Click to the gear icon near the stream button on the bottom
* Choose `custom RTMP Server` in destination.
* In the URL box, type your RTMP URL without stream id. It's like `rtmp://your_server_domain_name/LiveApp/`
* In the Stream key, you can write any stream id because we assume that no security option is enabled.
  * If token is enabled you need to add `?token={yourtokenhere}`. So it would be like `{streamid}?token={yourtokenhere}`
![image](https://user-images.githubusercontent.com/54481799/95346595-b51b2780-08c4-11eb-8d1a-95b5b2c40bac.png)
### 3. Tuning
You can use predefined settings but if you click to the gear button next to the quality options, you can tinker the options.
* Profile should be `baseline` and `keyframe latency` should be `1`.
* You can set your `level` and your `preset` according to your configuration but `3.1` and `medium preset` is good enough for having good quality streams.
* You can enable the `hardware encoder` for using your `GPU` in the `encoding process`.
![image](https://user-images.githubusercontent.com/54481799/95342776-75ead780-08c0-11eb-99ca-45d5a06af5b8.png)

### 4. Start Streaming
After configuring according to your needs and setting the server address, you can start the streaming by clicking to stream button at the bottom of the dashboard.
![image](https://user-images.githubusercontent.com/54481799/95345483-6faa2a80-08c3-11eb-80e7-33d1aa7122c9.png)
As you can see from the following screenshot, it started to streaming.
![image](https://user-images.githubusercontent.com/54481799/95346239-476efb80-08c4-11eb-9eb9-a408cd47fd43.png)
And you are publishing with `vMix!`