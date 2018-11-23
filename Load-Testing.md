In this tutorial, we explain how load tests can be made for Ant Media Server.
# Test Setup
Test setups has two parts: test server and SUT (system under test). We have two different setups for two different SUT. 
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
### Preparation and Running:
* To prepare Ant Media Server, please look at here.
* To prepare Test Server, please look at here.
* To run tests and collect results, please look at here.
## Cluster Setup
Here we have a cluster structure as SUT which contains one origin and two edge servers.
```
                                               +--------------------+
                                               |                    |
                                               |                    |
                                               |  Ant Media Server  |
                                    +--------->+                    |
                                    |          |     (Origin)       |
+-------------------+               |          |                    |
|                   |    streaming  |          |                    |
|                   +---------------+          +--------------------+
|                   |
|    Test Server    | playing             +-----------------------------+
|                   +<--------------------+                             |
|                   |                     |       Load Balancer         |
|                   +<------------------->+                             |
+-------------------+   rest              +------+-------------+--------+
                                                 |             |
                                                 |             |
                                                 |             |
                                                 |             |
                                                 |             |
                                 +---------------+----+   +----+---------------+
                                 |                    |   |                    |
                                 |                    |   |                    |
                                 |  Ant Media Server  |   |  Ant Media Server  |
                                 |                    |   |                    |
                                 |     (Edge-1)       |   |     (Edge-2)       |
                                 |                    |   |                    |
                                 |                    |   |                    |
                                 +--------------------+   +--------------------+

```
### Preparation and Running:
* To prepare Cluster, please look at here.
* If you want to use HAProxy as load balancer, please look at here.
* To prepare Test Server, please look at here.
* To run tests and collect results, please look at here.

# Preparation of Test Server
# Test Plans and Running Tests