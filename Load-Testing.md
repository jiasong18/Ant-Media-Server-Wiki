Test environment has two parts: test server and SUT (system under test). We have two different setups for two different SUT. 
# Preparation of SUT
## One Instance Setup
Here we have only one Ant Media Server instance as SUT.
```
+-------------------+                  +----------------------+
|                   |   streaming      |                      |
|                   |   playing        |                      |
|                   | <--------------> |                      |
|    Test Server    |                  |   Ant Media Server   |
|                   |                  |                      |
|                   | <--------------> |                      |
|                   |    rest          |                      |
+-------------------+                  +----------------------+
```
* To prepare Ant Media Server, please look at [here](Installation).

## Cluster Setup
Here we have a cluster structure as SUT which contains one origin and N edge servers.
```
                                       +--------------------+
                                       |                    |
                                       |                    |
                                       |  Ant Media Server  |
                            +--------->+                    |
                            |          |     (Origin)       |
+-----------+               |          |                    |
|           |    streaming  |          |                    |
|           +---------------+          +--------------------+
|           |
|Test Server| playing +------------------------------------------------+
|           +<--------+                                                |
|           |         |             Load Balancer                      |
|           +---------+                                                |
+-----------+   rest  +--+------------+---------------------+----------+
                         |            |                     |
                         |            |                     |
                         |            |                     |
                         |            |                     |
                         |            |                     |
           +-------------+--+  +------+---------+     +-----+----------+
           |                |  |                |     |                |
           |                |  |                |     |                |
           |Ant Media Server|  |Ant Media Server| ... |Ant Media Server|
           |                |  |                |     |                |
           |   (Edge-1)     |  |   (Edge-2)     |     |   (Edge-N)     |
           |                |  |                |     |                |
           |                |  |                |     |                |
           +----------------+  +----------------+     +----------------+

```
* To prepare Cluster, please look at [here](Scaling-and-Load-Balancing).

***

# Preparation of Test Server
## Step 1. Install Docker to your host
You can look [here](https://docs.docker.com/install/) for docker installation.

## Step 2. Download DockerFile for Ant Media Server
Download Dockerfile prepared for Ant Media Test environment from [here](https://github.com/ant-media/Scripts/blob/master/Dockerfile_AntMediaTest). You can download with the following command:

`$ sudo wget https://raw.githubusercontent.com/ant-media/Scripts/master/Dockerfile_AntMediaTest`

## Step 3. Build Dockerfile
Build Dockerfile with the following command:

`$ sudo docker build -f Dockerfile_AntMediaTest -t antmedia/test .`

## Step 4. Run Dockerfile
Run your container with the following command:

` $ sudo docker run -w "/home/antmedia/test" --name amstest -p 8090:8090 antmedia/test java -jar loadtester.jar`

## Step 5. Connect to Ant Media Load Test Server within Docker
Open web browser and connect to `<dockercontainer ip>:8090`

## Step 6. Complete configuration
Fill configuration parameters according to your test setup. 
 - For one instance setup, both Origin Server and Edge Access Point are set with IP of the running server instance. 
 - For cluster setup, Origin Server is set with IP of the origin server and Edge Access Point is set with IP of the load-balancer.
 
## Step 7. Run test
Run the test and wait for the result. The result will be available in Results panel after test finishes.
