## Build from Source

### Linux (Ubuntu)
A couple of repos should be clone and build with maven so that git and maven should be installed in advance

* Go to a directory where you clone repos
* Clone and build Ant-Media-Server-Common

```
$ git clone https://github.com/ant-media/Ant-Media-Server-Common.git
$ cd Ant-Media-Server-Common
$ mvn clean install -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -Dgpg.skip=true
$ cd ..
```

* Clone and build the Ant-Media-Server-Service 

```
$ git clone https://github.com/ant-media/Ant-Media-Server-Service.git
$ cd Ant-Media-Server-Service
$ mvn clean install -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -Dgpg.skip=true
$ cd ..
```

* Clone and build the Tomcat Plugin
```
$ git clone https://github.com/ant-media/red5-plugins.git
$ cd red5-plugins/tomcat/
$ mvn clean install -Dmaven.javadoc.skip=true -Dmaven.test.skip=true
$ cd ../..
```



* Clone, build and package Ant-Media-Server
```
$ git clone https://github.com/ant-media/Ant-Media-Server.git
$ cd Ant-Media-Server
$ mvn clean install -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -Dgpg.skip=true
$ ./repackage.sh
```

If everthing goes well, a new packaged Ant Media Server(ant-media-server-x.x.x.zip) file will be created 
in Ant-Media-Server/target directory