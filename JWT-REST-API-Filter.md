In this section, we're going to explain simply how to use JWT REST API Filter in Ant Media Server. By default, JWT REST API Filter is disabled and REST API IP Filter is enabled. 
You can use JWT Filter when you're consuming REST API from different endpoints. Before starting, you can get more information about JWT on [jwt.io](https://jwt.io). Here is a simple step by step guide for using JWT REST API Filter.

## Enable JWT Filter
We are using [JJWT Library](https://github.com/jwtk/jjwt) for Ant Media Server REST API security. If you want to enable this filter, you just need to enable JWT REST API Filter and type the Secret key on web panel. Secret key encrypts with `HMAC-SHA256` in JWT REST API Filter. 

<img src="images/jwt-filter-enable.png?raw=true" alt="">


## Generate JWT Token
Let's assume that our secret key is `zautXStXM9iW3aD3FuyPH0TdK4GHPmHq` so that we just need to create JWT token. Luckily, there are plenty of libraries available at [Libraries for JWT](https://jwt.io/#libraries-io) for your development. For our case, we will just use [Debugger at JWT](https://jwt.io#debugger-io)

<img src="images/generate_jwt_token.png?raw=true" alt="">

As shown above, we use `HS256` as algorithm and use our secret key `zautXStXM9iW3aD3FuyPH0TdK4GHPmHq` to generate the token. So that our JWT token to access the REST API is 
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ0b2tlbiIsImlhdCI6MTUxNjIzOTAyMn0.OESIxgNsnD_JwByKTXcrw9Ov4GaOUZw66QxMfmudhKQ
```

### Generate JWT Token with Expiration Time
Even if it's not necessary to have the payload, there are really useful options that can be used. For instance you can use **exp**(expiration time) for JWT token. In order to get more information for the structure, please visit to [Introduction to JWT](https://jwt.io/introduction).  Anyway, let me give an example about JWT Token with Expiration Time.

<img src="images/generate-jwt-expire-time.png?raw=true" alt="">

As shown above, the expiration time of the token is Mar 20, 2021 14:17:02 GMT+3. It means that you can use the generated token until the expiration time. The unit of expiration time is [unix timestamp](https://www.unixtimestamp.com/).  When it expires, the JWT token becomes invalid. 


## Use Token for Accessing REST Filter API
Using JWT token is so simple just add `Authorization` header with JWT Token as shown below.  
  ```
  curl -X POST -H "Content-Type: application/json, Authorization: '{JWTToken}'" "https://{domain:port}/{application}/rest/v2/broadcasts/create -d '{"name":" {streamName}"}'"
  ```

You can also use Postman as in the image below
<img src="images/use_jwt_token_with_postman.png?raw=true" alt="">

***This feature is available in Ant Media Server 2.3+ versions.**

