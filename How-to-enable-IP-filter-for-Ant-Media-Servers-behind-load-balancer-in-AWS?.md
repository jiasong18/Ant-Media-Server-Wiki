How to enable IP filter for Ant Media Servers behind load balancer in AWS?

1. Edit the following file with a text editor.

`vim /usr/local/antmedia/conf/jee-container.xml`

2. Find the below line. 

`<bean id="valve.access" class="org.apache.catalina.valves.AccessLogValve">`

And add the following line before it.

`<bean id="valve.access" class="org.apache.catalina.valves.RemoteIpValve" />`

3. Find another line as follow.

`<property name="rotatable" value="true" />`

and add the following line before it.

`<property name="requestAttributesEnabled" value="true" />`

4. After adding these lines, restart Ant Media Server with the following terminal command.

`systemctl restart antmedia`

The last edited version of the file will look like the following.
```
    <bean id="valve.access" class="org.apache.catalina.valves.RemoteIpValve" />
    <bean id="valve.access" class="org.apache.catalina.valves.AccessLogValve">
            <property name="directory" value="log" />
            <property name="prefix" value="${http.host}_access" />
            <property name="suffix" value=".log" />
            <property name="pattern" value="common" />
            <property name="rotatable" value="true" />
            <property name="requestAttributesEnabled" value="true" />
```
Now, You can use the IP filter.