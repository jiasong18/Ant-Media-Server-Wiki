## How to fix “NotSupportedError” in publishing WebRTC stream in Ant Media Server?
Problem is caused from attempting to access media source as discussed in https://stackoverflow.com/questions/34215937/getusermedia-not-supported-in-chrome.

To solve this problem you must enable SSL. You can follow instructions in this post https://antmedia.io/enable-ssl-on-ant-media-server.

##  How to fix "Make sure that your domain name was entered correctly and the DNS A/AAAA record(s)"?

First of all make sure that A record is entered in your DNS settings and point to your server. If you are sure about that, check your ports. 443 or 80 port are not blocked or forwarded to any port. If you forward 80 or 443 ports to 5080 and 5443, then please remove these port forwardings as describe in the FAQ