Ant Media Server provides IP filtering for accessing the web panel. By default, the web panel is open to all IP addresses, but you can filter IP addresses by CIDR notation. Here is a simple step by step guide for changing configuration.

* Open `/usr/local/antmedi/conf/red5.properties` file
* Find the below line. Default configuration lets all IP access the web panel. 
  ```
  server.allowed_dashboard_CIDR=0.0.0.0/0
  ```
* Change the configuration according to the your CIDR notation such as
  ```
  server.allowed_dashboard_CIDR=13.197.23.11/16,87.22.34.66/8
  ```
  You can add comma separated CIDR notations. 
* Save the file and restart the server. 

Now only the IP’s that are in CIDR block can access the web panel.

***This feature is available in Ant Media Server 2.0+ versions.**


