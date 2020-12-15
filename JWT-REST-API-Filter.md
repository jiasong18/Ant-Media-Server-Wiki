In this section, we're going to explain simply how to use JWT REST API Filter in Ant Media Server. By default, JWT REST API Filter is disabled and REST API IP Filter is enabled. 
You can use JWT Filter when you're consuming REST API from different endpoints. Before starting, you can get more information about JWT on [jwt.io](https://jwt.io). Here is a simple step by step guide for using JWT REST API Filter.

### Enable JWT Filter
We are using [JJWT Library](https://github.com/jwtk/jjwt) for Ant Media Server REST API security. If you want to enable this filter, you just need to enable JWT REST API Filter and type the Secret key on web panel. Secret key encrypts with `HMAC-SHA256` in JWT REST API Filter. 

<img src="images/jwt-filter-enable.png?raw=true" alt="">

### JWT Filter Usages for REST APIs
If you want to use JWT Filter with REST API, you just need to add `Authorization` header with JWT Token. 
  ```
curl -X POST -H "Content-Type: application/json, Authorization: '{JWTToken}'" "https://{domain:port}/{application}/rest/v2/broadcasts/create -d '{"name":" {streamName}"}'"
  ```

***This feature is available in Ant Media Server 2.3+ versions.**

