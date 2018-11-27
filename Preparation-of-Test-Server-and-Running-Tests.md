0. Install docker to your server. You can look [here](https://docs.docker.com/install/) for docker installation.
1. Download Dockerfile prepared for Ant Media Test environment from [here](https://github.com/ant-media/Scripts/blob/master/Dockerfile_AntMediaTest). You can download with the following command

`wget https://raw.githubusercontent.com/ant-media/Scripts/master/Dockerfile_AntMediaTest`

2. Build Dockerfile with the following command

`docker build -f Dockerfile_AntMediaTest -t antmedia/test .`

3. Run your container with the following command

`docker run -w "/home/antmedia/test/TestScriptAndTools-master/Tools/"  antmedia/test java -jar loadtester-0.0.1-SNAPSHOT-spring-boot.jar`

4. Open web browser and connect to `<dockercontainer ip>:8090`

5. Fill configuration parameters according to your [test setup](https://github.com/ant-media/Ant-Media-Server/wiki/Test-Environment). 
 - For one instance setup, both Origin Server and Edge Access Point are set with IP of the running server instance. 
 - For cluster setup, Origin Server is set with IP of the origin server and Edge Access Point is set with IP of the load balancer.
 
6. Run the test and wait for the result. The result will be available in Results panel after test finishes.

