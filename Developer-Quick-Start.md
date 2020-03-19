## Requirements
* Ubuntu 16.04 or 18.04
* Latest update of Java JDK 8. In the future Java 11 will be supported.
* Apache Maven 3
## Recommended folder structure and projects:
* Clone Ant-Media-Server, Ant-Media-ServerCommon, ant-media-server-parent,  Ant-Media-Server-Service, ManagementConsole_WebApp, red5-plugins, StreamApp projects from [GitHub](https://github.com/ant-media) into **_$home_directory_path/AntMedia_** where **_$home_directory_path_** is path of your home directory.
* If you are a Enterprise developer: clone the Ant-Media-Enterprise from Gitlab into **_$home_directory_path/AntMedia_**.
* Create folder **_$home_directory_path/softwares/ant-media-server_** for the deployed Ant Media Server. Some deployment scripts like _redeploy.sh_ will copy the compiled Ant Media Server into this directory.

## Recommended IDEs for Development
* Latest version of _Eclipse IDE for Java EE Developers_
* It is also possible to use other Java compatible IDEs like _IntelliJ_ or _NetBeans_ but core development team currently uses Eclipse for Java EE.
### Eclipse Plugins
* _SonarLint_ : for static code analysis
* _EclEmma_ : for test coverage
### Eclipse and Maven setup
* Change the default Maven used in Eclipse by giving path of the Maven home directory (_/usr/share/$maven_version_ for default installation) in the Preferences/Maven/Installations section of Eclipse.
* Import the projects to the Eclipse using import _existing Maven projects_ wizard.
## Deployment
* _redeploy.sh_ scripts in in Ant-Media-Server or Ant-Media-Enterprise can be used to build and deploy Ant Media Server into **_$home_directory_path/softwares/ant-media-server_**
## Debugging the Server
* Start the server in debug mode using _start-debug.sh_ script in **_$home_directory_path/softwares/ant-media-server_** which is generated after deployment.
* Create a Remote Java Application Debug Configuration in Eclipse in Debug Configurations settings (Host: localhost Port:8787)
* add all the AntMedia Server projects in Eclipse to the Source Lookup Path in the Debug Configuration.
