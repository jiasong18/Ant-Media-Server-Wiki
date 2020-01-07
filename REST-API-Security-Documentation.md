<!-- > TODO: Don't use "We". Because this is a formal documentation. -->

This guide explains how to Control REST API Security on Ant Media Server. You can able to secure your REST Services with IP Filter feature.

### IP Filter in REST API

<!--

> TODO: Don't give version reference. We fix this problem in another way. 
> In addition, 1.6.2 is an older version. I mean it's not required 

> Important Note: IP Filter feature is available for later versions of the 1.6.2+ version. 

> TODO: Below definition is ambiguous for me. Please use ` when specifiying a path or something similar.   
IP Filter feature usage is in Dashboard / Application(LiveApp or etc.) / Use IP Filtering for RESTful API section.

> TODO: Please give screenshot in a wider perspective

-->

<!--
> TODO: Use ` in specifying paths use `IP Filtering Settings` in `Dashboard > {Application} > Settings` in panel.  
-->

If you want some IP addresses to be able to access REST APIs, you should add IP’s or IP Ranges in `Dashboard > {Application} > Settings` > `IP Filtering Settings` in panel.

![REST API Filter](https://antmedia.io/wp-content/uploads/2019/12/antmedia-dashboard-IP-Filter.png)

<!-- > TODO: If 127.0.0.1 is deleted, requests in the server(localhost) is disabled. People in the same network but 
> in different machines still cannot access the REST API when 127.0.0.1 is on the list.
--> 

**If 127.0.0.1 is deleted, requests in the server(localhost) is disabled. People in the same network but in different machines still cannot access the REST API when 127.0.0.1 is on the list.**

<!-- > TODO: I think you would want to mean `If you want to remove` not `if you remove` ? -->
If you want to remove REST Filter in AMS, you should delete below codes in SERVER_FOLDER/webapp/Application(LiveApp or etc.)/WEB-INF/web.xml

    <filter>
    <filter-name>AuthenticationFilter</filter-name>
    <filter-class>io.antmedia.console.rest.AuthenticationFilter</filter-class>
    </filter>

    <filter-mapping>
    <filter-name>AuthenticationFilter</filter-name>`
    <url-pattern>/rest/*</url-pattern>
    </filter-mapping>

<!-- > TODO: Don't quote the sentences too much. -->

**If you delete AuthenticationFilter code block in Application, every one access your REST API.**

<!-- > TODO: Don't give contact details. Because this is not a blog post. -->