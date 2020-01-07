***
**_NOTE:_** We have updated our documentation. This page is outdated. You can access updated version from the sidebar menu.
***
Ant Media Server comes with two apps LiveApp and WebRTCApp (WebRTCAppEE in the enterprise edition). You can customize/rename existing apps or add new ones. You have two options to add new apps. Lets have a look at them;


## Option 1) Recreate from Existing Apps

**Step 1:** Copy LiveApp or WebRTCApp folder located under /webapps folder and paste with your application name.

**Step 3:** Customize app according to your needs.

**Step 4:** Configure your app according to described in the Configuration section of this document.

**Step 5:** Restart Ant Media Server.


## Option 2) Build Your App

**Step 1:** Clone app according to your needs from Github. The links of apps:

* **LiveApp:** https://github.com/ant-media/SampleApp 

* **WebRTCApp:** https://github.com/ant-media/WebRTCApp 

* **WebRTCAppEE:** https://github.com/ant-media/WebRTCApp4Enterprise (runs only with Enterprise Edition) 

**Step 2:** Customize app according to your needs.

**Step 3:** Configure your app according to described in the Configuration section of this document.

**Step 4:** Build your app with Maven.

**Step 5:** Put your built war file to under /webapps folder.

**Step 6:** Restart Ant Media Server.



## Configuration

**Step 1:** Change the app names in the webapps/<AppName>/WEB-INF/web.xml (if you build, find it in /src/main/webapp)

* `<display-name><AppName></display-name>`
* `<context-param>
	<param-name>webAppRootKey</param-name>
	<param-value>/<AppName></param-value>
   </context-param>`

**Step 2:** Change the app names in the webapps/<AppName>/WEB-INF/red5-web.properties (if you build, find it in /src/main/webapp)

* `webapp.contextPath=/<AppName>`
* `db.app.name=<AppName>`
* `db.name=<AppName>`

**Step 3:** Edit related HTML files: index.html, play.html (for WebRTCApps)

* Change websocket URL with your app name. Example: 	
`var path =  location.hostname + ":" + location.port + "/<AppName>/websocket";`




