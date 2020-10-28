Ant Media Server can generate periodic previews(snapshots) of the incoming streams on the fly. This guide will help you learn the configuration parameters for generating and using previews   

* In order to activate preview generation, you just need to add at least 1 adaptive bitrate. You can do that in the Dashboard `Application > Your App > Settings > Add New Bitrate`

![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/preview_1.png)
* The generated preview images will be available in this URL template 
  ```
   http://<SERVER_NAME>:5080/<APP_NAME>/previews/<STREAM_ID>.png
  ````
* Absolute path of the preview image as follows  
  ```
  <ANT_MEDIA_SERVER_DIR>/webapps/<APP_NAME>/previews/<STREAM_ID>.png
  ```
* In addition, you can upload preview images to Amazon S3. Please [check out the instructions for S3 Integration](https://github.com/ant-media/Ant-Media-Server/wiki/Amazon-(AWS)-S3-Integration)

#### Configuration Parameters

You can add/change following properties to the `<ANT_MEDIA_SERVER_DIR>/webapps/<APP_NAME>/WEB-INF/red5-web.properties`

* `settings.previewHeight`: Preview image is saved as 480p default. If you want to increase the resolution, add the following parameter into red5-web.properties file.
  ```
  settings.previewHeight=360
  ```

* `settings.createPreviewPeriod`: Preview image creation period in milliseconds. Default value is 5000 ms.
For example, If you change it as below , it will create preview for every second.
  ```
  settings.createPreviewPeriod=1000
  ```

* `settings.previewOverwrite`: Default value is false. If false, when a new stream is received with the same stream id, _N (increasing number) suffix is added to preview file name. If true, new preview file is replaced with the old file when a new stream with the same stream id is received.
  ```
  settings.previewOverwrite=false
  ```

* `settings.addDateTimeToMp4FileName`: Default value is false. If true, adds date-time value to preview file names. If false, it does not add date-time values to preview file names

  ```
  settings.addDateTimeToMp4FileName=false
  ```

  You alternatively enable this feature on the web panel. By checking box on `Application > Your App > Settings > Add Date-Time to Record File names` and saving the settings

![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/preview_2.png)

* `settings.previewGenerate`: Default value is true. If false, Preview images will not be generated.

  ```
  settings.previewGenerate=true
  ```

Keep in mind that If you change the configuration files, you need to restart Ant Media Server.

```
systemctl restart antmedia
```

