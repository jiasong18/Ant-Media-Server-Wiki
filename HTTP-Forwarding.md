HTTP forwarding is implemented to forward incoming HTTP requests to any other place. It's generally used for forwarding incoming request to a storage like S3. Let us tell how HTTP Forwarding works step by step

* Open the file `{AMS-DIR} / webapps / {APPLICATION} / WEB-INF / red5-web.properties`
* Add comma separated file extensions like this `settings.httpforwarding.extension=mp4,png` to the file. 
* Add the base URL with `settings.httpforwarding.baseURL=https://{YOUR_DOMAIN_HERE}` for forwarding. Please replace `{YOUR_DOMAIN_HERE}` with your own URL. Please pay attention that there is no leading or trailing white spaces.
* Save the file and restart the Ant Media Server with `sudo service antmedia restart`

If it's configured properly, your incoming MP4 requests such as `https://{SERVER_DOMAIN}:5443/{APPLICATION_NAME}/streams/vod.mp4` will be forwarded to `https://{YOUR_DOMAIN_HERE}/streams/vod.mp4`