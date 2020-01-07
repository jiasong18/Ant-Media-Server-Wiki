# What is Nginx ?

Nginx started out as a open source web server designed for maximum performance and stability. Today, however, it also serves as a reverse proxy, HTTP load balancer, and email proxy for IMAP, POP3, and SMTP.

> This document compatible all debian based os (Debian, Ubuntu etc.)
## Installation Steps
1. [Install Nginx](#Nginx-Installation)
2. [Install LetsEncrypt](#Let's-Encrypt-for-Nginx-SSL-Termination)
3. [Nginx Load balancer with ssl termination](#Configure-NGINX-as-a-Load-Balancer)

## Nginx Installation
> We prefer to install latest nginx version

Install the prerequisites

`sudo apt install curl ca-certificates lsb-release -y`

To set up the apt repository for stable nginx packages, run the following command:
```
echo "deb http://nginx.org/packages/`lsb_release -d | awk '{print $2}' | tr '[:upper:]' '[:lower:]'` `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```
import an official nginx signing key

`curl -fsSL https://nginx.org/keys/nginx_signing.key | sudo apt-key add -`

run the following commands to install nginx
```
apt update 
apt install nginx -y
```
### Let's Encrypt for Nginx SSL Termination
run the following commands to install certbot
```
apt install certbot python-certbot-nginx
```
run the following commands to create certificate
```
certbot --nginx -d yourdomain.com -d www.yourdomain.com
```
### edit crontab file
crontab -e

### add below line to renew certificate each 80 days.
`0 0 */80 * * root certbot -q renew --nginx`

## Configure NGINX as a Load Balancer

### Backup default nginx configuration
`mv /etc/nginx/nginx.conf{,_bck}`

create new a nginx.conf file with your favorite editor

`vim /etc/nginx/nginx.conf`

In that file, copy the following contents
```
# rtmp stream configuration
stream {
    upstream stream_backend {
	hash least_conn;
        server AMS1_SERVER:1935;
        server AMS2_SERVER:1935;
    }
    
    
    server {
        listen        1935;
        proxy_pass    stream_backend;
        proxy_timeout 3s;
        proxy_connect_timeout 1s;
    }
    
}


user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
    multi_accept        on;
    use                 epoll;
}

worker_rlimit_nofile 4096;

http {
    #backend conf.
    upstream ams_backend {
	hash least_conn;
        server AMS1_SERVER:5080;
        server AMS2_SERVER:5080;
    }

#redirect all http requests to https
server {
    listen 80 default_server;
    server_name _;
    return 301 https://$host$request_uri;
}   
 

#https configuration
    server {
	listen 443 ssl;
	ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;
        #ssl security
        ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers         HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        ssl_session_cache    shared:SSL:10m;
        ssl_session_timeout  24h;
	server_name yourdomain.com;

        location / {
            proxy_pass http://ams_backend;
	    proxy_http_version 1.1;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $host;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "Upgrade";
                }
    }
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    #added upstream info
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"'
		      '"$hostname" "upstream: $upstream_addr"';

    access_log  /var/log/nginx/access.log  main;
    sendfile        on;
    tcp_nopush     on;
    server_tokens off;
    keepalive_timeout  65;

    #gzip  on;


    include /etc/nginx/conf.d/*.conf;
}
```
Save and close that file.

On our server, we have to remove the symbolic link to default, in the /etc/nginx/sites-enabled folder.

`sudo rm /etc/nginx/sites-enabled/default`

### Check your configuration for any Error using the following command.

`nginx -t`

### And Finally Enable and Restart nginx service
```
systemctl enable nginx
systemctl restart nginx
```