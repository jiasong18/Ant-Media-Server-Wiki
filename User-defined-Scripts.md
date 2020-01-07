This feature running automatically defined script after MP4 Muxing process is finished in Ant Media Server. Let’s have a look at that step by step.

1. [Define run script location in App Settings](#1--define-run-script-location-in-app-settings)
2. [Script running instructions](#2--script-usage-instructions)

### 1- Define run script location in App Settings

Add script setting in `[AMS-DIR]` / `webapps` / `applications(LiveApp or etc.)` / `WEB-INF` / `red5-web.properties`

Usage:

    settings.muxerFinishScript

Example Usage:

    settings.muxerFinishScript=/Script-DIR/scriptFile.sh

Save the file and restart the server

    sudo service antmedia restart

**The script should be able to executable permission**

Mark the file as executable with below code:

    chmod +x scriptFile.sh

Setting References: [settings.muxerFinishScript Setting](Application-Configuration-Documentation)

### 2- Script usage instructions

After the muxing process is finished, the AMS runs the following code snippets.

    scriptFilePath fullPathOfMP4File

Example:

    ~/test_script.sh /usr/local/antmedia/webapps/LiveApp/streams/test_stream.mp4

When script finished successfully, AMS writes in INFO log as a below:

    running muxer finish script: ~/test_script.sh /usr/local/antmedia/webapps/LiveApp/streams/test_stream.mp4