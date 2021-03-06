### Linux (Ubuntu)
A couple of commons repos should be cloned and build with maven. 

* Go to a directory where you will clone repos

* Clone and build ant-media-server-parent
  ```
  $ git clone https://github.com/ant-media/ant-media-server-parent.git
  $ cd ant-media-server-parent/
  $ mvn clean install -Dgpg.skip=true
  $ cd ..
  ```

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
### Building Community Edition

* Clone, build and package Ant-Media-Server
  ```
  $ git clone https://github.com/ant-media/Ant-Media-Server.git
  $ cd Ant-Media-Server
  $ mvn clean install -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -Dgpg.skip=true
  $ ./repackage_community.sh
  ```

### Building Enterprise Edition

* Clone, build Ant-Media-Server
  ```
  $ git clone https://github.com/ant-media/Ant-Media-Server.git
  $ cd Ant-Media-Server
  $ mvn clean install -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -Dgpg.skip=true
  ```
* Build Ant-Media-Enterprise
  Source code of Ant-Media-Enterprise is provided to the Enterprise users
  ```
  $ cd /where/you/download/enterprise/repo
  $ ./redeploy.sh
  ```

* Package Enterprise Edition
  ```
  $ cd Ant-Media-Server
  $ ./repackage_enterprise.sh
  ```

If everything goes well, a new packaged Ant Media Server(ant-media-server-x.x.x.zip) file will be created 
in Ant-Media-Server/target directory