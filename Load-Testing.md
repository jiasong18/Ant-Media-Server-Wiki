In this tutorial, we explain how load tests can be made for Ant Media Server.
# Test Setup
## Testing One Instance
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
# Testing Cluster
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
|                   |                     | Load Balancer               |
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
You can make load tests for Ant Media Server by following the steps.
Step 0: Preparation of Software Under Test

You must have at least one server in which Ant Media Server is installed. If you want cluster tests you must prepare cluster.  