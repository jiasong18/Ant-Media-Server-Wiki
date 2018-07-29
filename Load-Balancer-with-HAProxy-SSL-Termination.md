Load Balancer is the sister of cluster so If you make Ant Media Server instances run in Cluster Mode. 
Then a load balancer will be required to balance the load. 

In this documentation, you will learn how to install **HAProxy Load Balancer with SSL termination**

<img src="https://antmedia.io/wp-content/uploads/2018/07/haproxy_ssl.png" alt="lets encrypt" align="center" />


The configuration below balances RTMP, HLS, HTTP/HTTPS and WebSocket(WS/WSS) connections so that it will be used for RTMP, HLS and WebRTC streaming. 

## Installation

### Install HAProxy

You can install HAProxy to both one of the instances that run on Ant Media Server or you can make it run on different instance.
For our case, we assume that HAProxy runs on a specific instance. So let's install HAProxy

```
sudo apt-get install haproxy
```
### Install SSL Certificate
If you need to support SSL in load balancer, install the `certbot` as follows. If you do not plan to support SSL, you can skip this section to Configuration HAProxy 
```
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot
```

#### Prepare the Certificate
Run the command below to get the certificate, please change example.com with your domain name
```
sudo certbot certonly --standalone -d example.com -d www.example.com
```

Combine fullchain.pem and privkey.pem and save it to /etc/haproxy/certs folder
```
sudo mkdir -p /etc/haproxy/certs
DOMAIN='example.com' sudo -E bash -c 'cat /etc/letsencrypt/live/$DOMAIN/fullchain.pem /etc/letsencrypt/live/$DOMAIN/privkey.pem > /etc/haproxy/certs/$DOMAIN.pem'
sudo chmod -R go-rwx /etc/haproxy/certs
```
Right now required pem file ready under `/etc/haproxy/certs` folder to use in HAProxy

## Configuring HAProxy

Move the default configuration file
```
mv /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.original
```

Create and edit new configuration file
```
nano /etc/haproxy/haproxy.cfg
```

### Global and Default Parameters

```yaml
global
    log 127.0.0.1 local0 notice
    maxconn 2000
    user haproxy
    group haproxy
defaults
    log global
    mode http
    option forwardfor
    option http-server-close
    option httplog
    option dontlognull
    timeout connect 5000
    timeout client  5000
    timeout server  5000
    timeout tunnel  2h  #this is for websocket connections, 2 hours inactivity timeout
    timeout client-fin 5000
    errorfile 400 /etc/haproxy/errors/400.http
    errorfile 403 /etc/haproxy/errors/403.http
    errorfile 408 /etc/haproxy/errors/408.http 
    errorfile 500 /etc/haproxy/errors/500.http
    errorfile 502 /etc/haproxy/errors/502.http
    errorfile 503 /etc/haproxy/errors/503.http
    errorfile 504 /etc/haproxy/errors/504.http
```
The configuration above makes maximum number of connections to 2000. Please change it according to your hardware and cluster size.

### Monitoring Parameters  
```yaml
listen stats # Define a listen section called "stats"
  bind :6080 
  mode http
  stats enable  # Enable stats page
  stats hide-version  # Hide HAProxy version
  stats realm Haproxy\ Statistics  # Title text for popup window
  stats uri /haproxy_stats  # Stats URI
  stats auth Username:Password  # Authentication credentials
```
With the configuration above when you go to `http://HAPROXY_LB:6080/haproxy_stats` URL, you can authenticate
with `Username` and `Password` so that specify username and password.

### RTMP Load Balancing
```yaml
frontend rtmp_lb
    bind *:1935 
    mode tcp
    default_backend backend_rtmp

backend backend_rtmp
    mode tcp
    server ams1 172.30.0.42:1935 check  # Ant Media Server instance 1
    server ams2 172.30.0.48:1935 check  # Ant Media Server instance 2
    # you can add more instances 
```    

### HTTP/HTTPS Load Balancing
```yaml
frontend http_lb
  bind *:80
  bind *:5080
  mode http
  reqadd X-Forwarded-Proto:\ http
  default_backend backend_http

# if you install SSL above add below frontend_https
frontend frontend_https
  bind *:443 ssl crt  /etc/haproxy/certs/$DOMAIN.pem
  bind *:5443 ssl crt /etc/haproxy/certs/$DOMAIN.pem
  reqadd X-Forwarded-Proto:\ https
  default_backend backend_http


backend backend_http
  # below line forwards http requests to https, if you do not have SSL termination, remove it
  redirect scheme https if ! { ssl_fc }  
  # below line provides session stickiness
  cookie JSESSIONID prefix nocache  
  server ams1 172.30.0.42:5080 check cookie ams1  #if you do not use session stickiness, remove cookie ams1
  server ams2 172.30.0.42:5080 check cookie ams2  #if you do not use session stickiness, remove cookie ams2
    # you can add more instances 
```




## Starting HAProxy

When everything is comple restart the HAProxy
```
sudo service haproxy restart
```
and you can view status of the instance throught http://HAPROXY_LB:6080/haproxy_stats URL
![HAProxy Stats Panel](https://ant-media.github.io/Ant-Media-Server/doc/images/HAProxy_Stats.png)

If you have a question, please let us know through contact@antmedia.io