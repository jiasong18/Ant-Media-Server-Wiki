
HTTPS and WSS(WebSocket Secure) is mandatory for Google Chrome to run WebRTC and WebSocket applications.
In addition, developers want to serve their content with secure connection as well. The script in this document
install Let's Encrypt SSL certificate


## Enabling SSL in Linux(Ubuntu)

Go to the folder where Ant-Media-Server is installed. Default directory is /usr/local/antmedia

```
cd /usr/local/antmedia
```
If apache is running, you need to disable it first.

```
sudo service apache2 stop
```

There should be a `enable_ssl.sh` file in the installation directory. 
Call the enable_ssl.sh with your domain name

```
sudo ./enable_ssl.sh example.com
```

#### v1.5+, `enable_ssl.sh` script supports external fullchain.pem and privkey.pem files. It's usage has been changed to
```
Usage:
sudo ./enable_ssl.sh -d {DOMAIN_NAME}
sudo ./enable_ssl.sh -f {FULL_CHAIN_FILE} -p {PRIVATE_KEY_FILE} -d {DOMAIN_NAME} 
sudo servive apache2 start
```	
	


Make sure that your domain points to your server public IP address in the DNS records 

If the above scripts returns successfully, SSL will be installed your server, 
you can use https through 5443. Like below

```
https://example.com:5443
```

**ATTENTION:** If port 80 is used by some other process or it's forwarded to some other port, 
`enable_ssl.sh` will not be successful. Please disable the process or delete the port forwarding temporarily in running the `enable_ssl.sh` script above  

If you are still having issues, please let us know that. [contact@antmedia.io](mailto:contact@antmedia.io) 

#### References
- [Blog Post: Enable SSL with Just One Command](https://antmedia.io/enable-ssl-on-ant-media-server/)
- [Let's Encrypt](https://letsencrypt.org/)