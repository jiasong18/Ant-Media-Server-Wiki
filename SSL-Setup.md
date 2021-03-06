<!--
> TODO: It's not mandatory for all cases. It's mandatory when requesting access to mic and camera. 
> It's not  mandatory for playing streams
`HTTPS` and `WSS` (WebSocket Secure) is mandatory for Google Chrome to run WebRTC and WebSocket applications.
In addition, developers want to serve their content with a secure connection as well. The script in this document
install Let's Encrypt SSL certificate
-->

It's not mandatory for all cases. It's mandatory when requesting access to mic and camera. It's not mandatory for playing streams HTTPS and WSS (WebSocket Secure) is mandatory for Google Chrome to run WebRTC and WebSocket applications. 

In addition, developers want to serve their content with a secure connection as well. 
The script in this document install Let's Encrypt SSL certificate.

## Enabling SSL in Linux(Ubuntu)

Go to the folder where Ant-Media-Server is installed. Default directory is /usr/local/antmedia

```
cd /usr/local/antmedia
```

_If there is a service that uses 80 port, you need to disable it. If your system has Apache Web Server, you need to disable it first such a command below_
```
sudo service apache2 stop
```

There should be an `enable_ssl.sh` file in the installation directory. 
Call the enable_ssl.sh with your domain name

```
sudo ./enable_ssl.sh -d example.com
```

`enable_ssl.sh` script supports external fullchain.pem and privkey.pem files as in the following format
```
sudo ./enable_ssl.sh -f {FULL_CHAIN_FILE} -p {PRIVATE_KEY_FILE} -d {DOMAIN_NAME} 
```
Ex:	
```
sudo ./enable_ssl.sh -f yourdomain.crt -p yourdomain.key -d yourdomain.com
sudo ./enable_ssl.sh -f yourdomain.pem -p yourdomain.key -d yourdomain.com
```
	
_If you disable any service that binds to 80 port such as Apache Web Server, enable it again_
```
sudo service apache2 start
```

Make sure that your domain points to your server public IP address in the DNS records 

If the above scripts return successfully, SSL will be installed your server, 
you can use https through 5443. Like below

```
https://example.com:5443
```

**ATTENTION:** If port 80 is used by some other process or it's forwarded to some other port, 
`enable_ssl.sh` will not be successful. Please disable the process or delete the port forwarding temporarily  before running the `enable_ssl.sh` script above  


#### References
- [Blog Post: Enable SSL with Just One Command](https://antmedia.io/enable-ssl-on-ant-media-server/)
- [Let's Encrypt](https://letsencrypt.org/)