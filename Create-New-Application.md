Ant Media Server has 2 built-in applications. Our customers want to able to use different settings for each application and able to specify the name of the application.

Because of that, customers want to add/create new applications. So, we have prepared a script that creates new applications in Ant Media Server easily. You just need to type a few simple commands.

Let’s have a look at the steps:

### Step 1

Go to the folder where Ant-Media-Server is installed. Default directory is `/usr/local/antmedia`

    cd /usr/local/antmedia

### Step 2

create_app.sh usage in below.

    sudo ./create_app.sh applicationName AMS-Installation-Directory

For example:

`sudo ./create_app.sh streamHive /usr/local/antmedia`

![](https://i1.wp.com/antmedia.io/wp-content/uploads/2019/11/ant-media-server-new-application.png)

### Step 3

Restart Ant Media Service

    sudo service antmedia restart

*This feature is available in Ant Media Server 1.9.0+ versions.