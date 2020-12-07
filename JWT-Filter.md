In this section, we're going to explain simply how to use JWT REST API Filter in Ant Media Server. By default, JWT Filter is disabled. Ant Media Server enabled only IP Filter. But you can use JWT Filter for more security operations. Here is a simple step by step guide for JWT Filter.

### Step 1. JWT Filter Instructions / Technical details
We are using [JJWT Library](https://github.com/jwtk/jjwt) for Ant Media Server REST API security. If you want to enable this filter, you just need to enable JWT Filter and type the Secret key. Secret key encrypts with `hmacShaKeyFor` in JWT Filter. 

<img src="images/jwt-filter-enable.png?raw=true" alt="">

### Step 2. JWT Filter Usages for REST APIs
If you want to use JWT Filter with REST API, you just need to add `Authorization` header with JWT Token. 
  ```
curl -X POST -H "Content-Type: application/json, Authorization: '{JWTToken}'" "https://{domain:port}/{application}/rest/v2/broadcasts/create -d '{"name":" {streamName}"}'"
  ```

***This feature is available in Ant Media Server 2.3+ versions.**

