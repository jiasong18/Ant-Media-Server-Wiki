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

If you are still having issues, please let us know that. [contact@antmedia.io](mailto:contact@antmedia.io) 
