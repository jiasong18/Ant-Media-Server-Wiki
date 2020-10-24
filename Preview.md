* First, at least 1 adaptive bitrate must be created to create a preview. You can add in the Dashboard `Application > Your App > Settings > Add New Bitrate`

![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/preview_1.png)
* Preview image URL will be available in this URL template `http://<SERVER_NAME>:5080/<APP_NAME>/previews/<STREAM_ID>.png`
* Preview image stores in this folder `<ANT_MEDIA_SERVER_DIR>/webapps/<APP_NAME>/previews/<STREAM_ID>.png`
* Also, you can upload preview images to Amazon S3.
> [Amazon (AWS) S3 Integration](https://github.com/ant-media/Ant-Media-Server/wiki/Amazon-(AWS)-S3-Integration)

## Settings

Preview settings on `<ANT_MEDIA_SERVER_DIR>/webapps/<APP_NAME>/WEB-INF/red5-web.properties` file

* `settings.previewHeight`: Preview image is saved as 480p default. If you want to increase the resolution, add the following parameter into red5-web.properties file.

    `settings.previewHeight=1080`

* `settings.createPreviewPeriod`: Preview image creation period in milliseconds. Default value is 5000 ms.
For example, If you change it as below it will store every second.

    `settings.createPreviewPeriod=1000`

* `settings.previewOverwrite`: Default value is false. If false, when a new stream is received with the same stream id, _N (increasing number) suffix is added to preview file name. If true, new preview file is replaced with the old file with .png when a new stream with the same stream id is received.

    `settings.previewOverwrite=false`

* `settings.addDateTimeToMp4FileName`: Default value is false. If true, adds date-time value to preview file names. If false, it does not add date-time values to preview file names

    `settings.addDateTimeToMp4FileName=false`

or you can enable in the Dashboard `Application > Your App > Settings > Add Date-Time to Record File names`
![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/preview_2.png)

* `settings.previewGenerate`: Default value is true. If false, Preview images will not generate.

    `settings.previewGenerate=true`

**P.S:** If you change the configuration files, you must restart Ant Media Server.

`systemctl restart antmedia`

