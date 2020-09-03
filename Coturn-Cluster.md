In this how-to article, I will tell you how to do a load balancer using Turn Server (Mysql support) as DNS Round Robin.

![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/turn_dns_round_robin.png)

### What is Round Robin DNS?
Round Robin DNS is a fast, simple and cost-effective way to load balance or distribute traffic evenly over multiple servers or devices.

### How does Round Robin work?
By using Round Robin DNS, when one user accesses the home page, the request will be sent to the first IP address. The second user who accesses the home page will be sent to the next IP address, and the third user will be sent to the third IP address. In a nutshell, Round Robin network load balancing rotates connection requests among web servers in the order that requests are received.

### System Requirements
```
2 x Turn Server
1 x MySQL/MariaDB server
1 x DNS Access
```
```
DNS    : 192.168.1.199
MariaDB: 192.168.1.200
Coturn1: 192.168.1.201
Coturn2: 192.168.1.202
```
This "How to" guide has been tested on a real lab environment so you have to set up the configuration according to your own build.

### DNS Configuration

Assuming this is a fully-registered domain, we would add the following in the DNS settings. We add two A records for the subdomain turn.antmedia.io and point them to the turn server servers IP address.

Example DNS Record as follows.
```
turn.antmedia.io	IN		A		192.168.1.201
turn.antmedia.io	IN		A		192.168.1.202
```
In this way, when we make a request to turn.antmedia.io, it will distribute every request in the round-robin structure to the ip addresses we have stated above.

### Database Configuration

We always prefer to install the Database Server on a separate server and we choose MariaDB. We use long-term authentication in this structure and we authenticate to the turn server with the users that we created.

Update the repository and install MariaDB with the following command

`apt-get update && apt-get install mariadb-server -y`

Edit the following file with your favorite editor 
`vim /etc/mysql/mariadb.conf.d/50-server.cnf`

Please add the following lines then save and exit.
```
bind-address            = 0.0.0.0
innodb_file_format=Barracuda
innodb_file_per_table=1
innodb_large_prefix=1
```
Restart the MariaDB Server.

`systemctl restart mysqld`

Login Mariadb shell as follows.

`mysql -uroot -p `

Run the SQL command as follows on the MariaDB shell.

```
SET SESSION innodb_strict_mode=ON;
SET GLOBAL innodb_default_row_format='dynamic';

create database coturn;
CREATE USER 'coturn'@'192.168.1.201' IDENTIFIED BY 'coturn123';
CREATE USER 'coturn'@'192.168.1.202' IDENTIFIED BY 'coturn123';

GRANT ALL PRIVILEGES ON coturn.* TO 'coturn'@'192.168.1.201';
GRANT ALL PRIVILEGES ON coturn.* TO 'coturn'@'192.168.1.202';
flush privileges;
quit;
```
### Install TURN Server

In this section, we will install and configure Coturn on Coturn1 and Coturn2 server.

Update the repository and install CoTurn with the following command

`apt-get update && apt-get install coturn -y`

Enable the TURN server as follows.

`sed -i 's/#TURNSERVER_ENABLED.*/TURNSERVER_ENABLED=1/g' /etc/default/coturn`

Next, add CoTurn to start at boot time

`systemctl enable coturn`

Backup original conf file.

`mv /etc/turnserver.conf{,_bck}`

Create the following file with the editor

`vim /etc/turnserver.conf`

Add below lines then save and exit (don't forget to change database credentials)
```
fingerprint
lt-cred-mech
realm=turn.antmedia.io
mysql-userdb="host=192.168.1.200 dbname=coturn user=coturn password=coturn123 port=3306 connect_timeout=60 read_timeout=60"
syslog
```
Make sure you're doing this step on Coturn1 and Coturn2 server.

The syslog output of all servers is as follows.

![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/coturn-2.png)

Import SQL schema to the database server.

The file /usr/share/coturn/schema.sql in one of the turn servers is uploaded to the database server and scheme.sql is imported.

`scp -r /usr/share/coturn/schema.sql root@192.168.1.200:`

Run the following command to import the SQL file.

`mysql -uroot -p coturn < schema.sql`

Restart the service on both nodes

`systemctl restart coturn`

To create a username and password, run the following command on the turn1 or turn2 server.

`turnadmin -a --mysql-userdb="host=192.168.1.200 dbname=coturn user=coturn password=coturn123" -u antmedia -p 123456 -r turn.antmedia.io`

Let's check if the configurations are working correctly.

`turnutils_uclient -v -t -T -u antmedia -w 123456 -p 3478 turn.antmedia.io`

If everything is fine, your output will be as follows.

![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/coturn-output.png)


### Troubleshooting

You can use the following command to check that DNS Round-Robin is working correctly.

`nslookup turn.antmedia.io`

![](https://raw.githubusercontent.com/wiki/ant-media/Ant-Media-Server/images/coturn-nslookup.png)
