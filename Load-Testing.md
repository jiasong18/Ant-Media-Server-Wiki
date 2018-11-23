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
You can make load tests for Ant Media Server by following the steps.
Step 0: Preparation of Software Under Test

You must have at least one server in which Ant Media Server is installed. If you want cluster tests you must prepare cluster.  