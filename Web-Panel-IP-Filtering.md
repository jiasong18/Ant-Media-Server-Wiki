Ant Media Server provides IP filtering for accessing the web panel. By default, the web panel is open to all IP addresses, but you can filter IPs by changing configurations.

<h2>Add your IPs to the configuration file </h2>
In order to add default IPs,  just follow the steps below
<ul>
 <li>Open your apps <em>red5.properties</em>  and modify <em>server.allowed_dashboard_CIDR </em> property of that file. <em>red5.properties </em>file is under <em>conf</em>  folder.
<pre>...
# Default value is open everyone
server.allowed_dashboard_CIDR=0.0.0.0/0
...</pre>
By changing this line you can change the allowed IPs. You should use the CIDR notation to write your IPs and separate them with a comma.
<pre>...
# Default value is open everyone
server.allowed_dashboard_CIDR=12.167.126.111/32,21.147.156.0/24
...</pre> After making changes save the file. 
</li>
 
 <li>Restart the server on command line
<pre>sudo service antmedia restart</pre>
</li>
</ul>
Now only the IP’s that are listed here can access the web panel.


***This feature is available in Ant Media Server 2.0+ versions.**


