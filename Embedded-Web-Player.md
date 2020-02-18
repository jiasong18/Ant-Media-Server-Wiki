You can use `play.html` page on your website with IFrame. `play.html` page is in the Application folder on Ant Media Server. 

### `play.html` page accepts 4 arguments

* **`id`** : the stream id to play. It is mandatory.
* **`token`** : the token to play the stream. It's mandatory if token security is enabled on the server-side.
* **`autoplay`** : To start playing automatically if streams are available. Optional. The default value is true
* **`playOrder`** : the order which technologies is used in playing. Optional. Default value is `webrtc,hls`. possible values are `hls,webrtc`,`webrtc,hls`

**You can use `play.html` in IFrame as below:**

`<iframe width="560" height="315" src="https://your_domain_name:5443/LiveApp/play.html?name=125214322064017559554903" frameborder="0" allowfullscreen></iframe>`

![](https://antmedia.io/wp-content/uploads/2019/12/Screenshot-from-2019-12-23-19-50-09.png)

IFrame status changes to playing when any broadcast is streaming as below image

![](https://antmedia.io/wp-content/uploads/2019/12/Screenshot-from-2019-12-23-19-52-47.png) 

### Change width/height resolution in`play.html` page

If you embed `play.html` page in somewhere, stream resolution seems max-width = 640px, max-height = 480px. If you want to show more than height/width, you should change max-width and max-height parameters in `play.html` page.

HLS Player parameters URL: `https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/play.html#L34`

WebRTC Player parameters URL: `https://github.com/ant-media/StreamApp/blob/master/src/main/webapp/play.html#L50`