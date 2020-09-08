Quality Filter feature is implemented in order to filter the streams that has been sent to the server.
If stream does not meet the qualifications, it won't be published. This is usually used when users wants to set some sort of quality standard or eliminate streams that may impact the server.

Let's see how quality filtering can be setup:
* Open the file `{AMS-DIR} / webapps / {APPLICATION} / WEB-INF / red5-web.properties`
* Adding all of the commands are not mandatory, just add which criteria you want to filter.
  * `settings.maxFpsAccept={value}`
  * `settings.maxResolutionAccept={value}`
  * `settings.maxBitrateAccept={value}`
* Please don't forget to set values according to your needs. Multiple criterias can be added.

If you are configured properly and assume that you set `settings.maxFpsAccept=15`, streams that have higher than 15 fps will be disregarded and won't be published.
