### How can I reset the password for Web Manager of Ant Media Server?

Go to the installation directory(/usr/local/antmedia) of the server. 
Remove "server.db" file and restart the server. 

```
cd /usr/local/antmedia
sudo rm server.db
sudo service antmedia restart
```