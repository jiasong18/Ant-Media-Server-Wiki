## How to fix “NotSupportedError” in publishing WebRTC stream in Ant Media Server?
Problem is caused from attempting to access media source as discussed in https://stackoverflow.com/questions/34215937/getusermedia-not-supported-in-chrome.

To solve this problem you must enable SSL. You can follow instructions in this post https://antmedia.io/enable-ssl-on-ant-media-server.

##  How to fix "Make sure that your domain name was entered correctly and the DNS A/AAAA record(s)"?

First of all make sure that A record is entered in your DNS settings and point to your server. If you are sure about that, check your ports. 443 or 80 port are not blocked or forwarded to any port. If you forward 80 or 443 ports to 5080 and 5443, then please remove these port forwardings as describe in the FAQ

## How to remove port forwardings?
Check that which port forwardings exist in your system with below command. 
```
sudo iptables -t nat --line-numbers -L
````

The command above should give an output live below
```
Chain PREROUTING (policy ACCEPT)
num  target     prot opt source               destination         
1    REDIRECT   tcp  --  anywhere             anywhere             tcp dpt:https redir ports 5443
2    REDIRECT   tcp  --  anywhere             anywhere             tcp dpt:http redir ports 5080

...
```

Delete the rule by line number. For instance to delete the http -> 5080 forwarding, run the command below
```
iptables -t nat -D PREROUTING 2
``` 
parameter 2 is the line number, if you want to delete https -> 5443, you should use 1 instead of 2 