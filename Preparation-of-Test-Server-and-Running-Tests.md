### Step 1. Install Docker to your host
You can look [here](https://docs.docker.com/install/) for docker installation.

### Step 2. Download DockerFile for Ant Media Server
Download Dockerfile prepared for Ant Media Test environment from [here](https://github.com/ant-media/Scripts/blob/master/Dockerfile_AntMediaTest). You can download with the following command:

` $ sudo wget https://raw.githubusercontent.com/ant-media/Scripts/master/Dockerfile_AntMediaTest `

### Step 3. Build Dockerfile
Build Dockerfile with the following command:

` $ sudo docker build -f Dockerfile_AntMediaTest -t antmedia/test .`

### Step 4. Run Dockerfile
Run your container with the following command:

` $ sudo docker run -w "/home/antmedia/test/TestScriptAndTools-master/Tools/"  antmedia/test java -jar loadtester-0.0.1-SNAPSHOT-spring-boot.jar`

### Step 5. Connect to Ant Media Server within Docker
Open web browser and connect to `<dockercontainer ip>:8090`

### Step 6. Complete configuration
Fill configuration parameters according to your [test setup](https://github.com/ant-media/Ant-Media-Server/wiki/Test-Environment). 
 - For one instance setup, both Origin Server and Edge Access Point are set with IP of the running server instance. 
 - For cluster setup, Origin Server is set with IP of the origin server and Edge Access Point is set with IP of the load-balancer.
 
### Step 7. Run test
Run the test and wait for the result. The result will be available in Results panel after test finishes.
