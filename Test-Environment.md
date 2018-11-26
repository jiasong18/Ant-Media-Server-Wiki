Test environment has two parts: test server and SUT (system under test). We have two different setups for two different SUT. 
# One Instance Setup
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
### Preparation of Setup:
* To prepare Ant Media Server, please look at [here](https://github.com/ant-media/Ant-Media-Server/wiki/Getting-Started).
* To prepare Test Server, please look at [here](https://github.com/ant-media/Ant-Media-Server/wiki/Preparation-of-Test-Server).

# Cluster Setup
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
### Preparation of Setup:
* To prepare Cluster, please look at [here](https://github.com/ant-media/Ant-Media-Server/wiki/DB-Based-Clustering-(available-for-v1.5.1-and-later)-and-Autoscaling).
* If you want to use HAProxy as load balancer, please look at [here](https://github.com/ant-media/Ant-Media-Server/wiki/Load-Balancer-with-HAProxy-SSL-Termination).
* To prepare Test Server, please look at [here](https://github.com/ant-media/Ant-Media-Server/wiki/Preparation-of-Test-Server).