Amazon S3 Integration lets you upload your MP4 files and previews to S3 automatically so that you do not need to worry about disk space in your server. Here is the simple manual how to enable S3 Integration to your streaming app 

## Enable AWS S3 in your Streaming App

* In order to programmatically access to S3, you should have an access token and secret keys. You can create a programmatic user to have an access token and secret key from [AWS IAM(Identity and Access Management)](https://console.aws.amazon.com/iam/home#/users) console. 

![](https://antmedia.io/wp-content/uploads/2019/12/1-aws-iam-management-console.png)

![Add User in IAM](https://antmedia.io/wp-content/uploads/2019/12/2-aws-add-user-iam-management-console.png)

Just `Add User` by checking `Programmatic Access` box and then in the next section click `Attach existing policies directly` and add `AmazonS3FullAccess` access permission to this user. Copy access token and secret key for this user. 

![Add User in IAM detail](https://antmedia.io/wp-content/uploads/2019/12/3-aws-add-user-detail-in-iam.png)

![](https://antmedia.io/wp-content/uploads/2019/12/4-aws-add-user-detail2-in-iam.png)

* Right now, you should have `access token`, `secret key`, `bucket name` in your hand.

![](https://antmedia.io/wp-content/uploads/2019/12/4-aws-add-user-detail3-in-iam.png)

You also need to know the region of your bucket. If you do not have any bucket, you can create it on [S3 Console](https://s3.console.aws.amazon.com/s3/home)  

![](https://antmedia.io/wp-content/uploads/2019/12/5-aws-add-s3-console.png)

![AWS S3 Create New Bucket](https://antmedia.io/wp-content/uploads/2020/05/aws-s3-create-new-bucket.png)

* Add a new bean to `red5-web.xml` of your app as below. `red5-web.xml` file is under `WEB-INF` folder of your app. For instance `webapps` / `LiveApp` / `WEB-INF` / `red5-web.xml`

```xml
<bean id="app.storageClient" class="io.antmedia.storage.AmazonS3StorageClient">
    <property name="accessKey" value="Enter your S3_ACCESS_KEY" />
    <property name="secretKey" value="Enter your S3_SECRET_KEY" />
    <property name="region" value="Enter your REGION_NAME e.g. eu-central-1" />
    <property name="storageName" value="Enter your BUCKET_NAME" />
</bean>
```

You should set the values accordingly to your access token, secret key, region name and bucket name. You can learn your region's programmatic name [from here](https://docs.aws.amazon.com/general/latest/gr/rande.html). 

* Save the file and restart the server
```
sudo service antmedia restart
```

Your MP4 files and Preview files will be uploaded to your S3 bucket automatically. 

###  Forward MP4 and Preview Request to AWS S3 in your Application

This section is optional. If you want to forward HTTP requests to MP4 files, Preview files or m3u8 files to AMS S3 directly, you can use this feature with few basic changes in application settings.



Open `AMS-DIR` / `webapps` / `{application}` / `WEB-INF` / `red5-web.properties` with your text editor (vim, nano)  

    settings.httpforwarding.extension

Usage Example:

`settings.httpforwarding.extension=mp4`

You can add extensions with a comma: mp4,png 

**Detail in code: [HTTP Forward Extension Settings Details](https://github.com/ant-media/Ant-Media-Server-Common/blob/master/src/main/java/io/antmedia/AppSettings.java#L542)**

    settings.httpforwarding.baseURL
    
Usage Example:

`settings.httpforwarding.baseURL=https://{s3BucketName}.s3.{awsLocation}.amazonaws.com`

**Detail in code: [HTTP Forward BaseURL Settings Details](https://github.com/ant-media/Ant-Media-Server-Common/blob/master/src/main/java/io/antmedia/AppSettings.java#L548)**

When you added above code snippets in Application Settings, your Request URI will be as below.

`https://{s3BucketName}.s3.{awsLocation}.amazonaws.com/streams/{streamId.mp4}`

**Detail in code: [HTTP Forward Filter](https://github.com/ant-media/Ant-Media-Server/blob/master/src/main/java/io/antmedia/filter/HttpForwardFilter.java#L28)**

**Note:** Don't add any leading, trailing white spaces.

Restart Ant Media Server

    sudo service antmedia restart

After server restarted, MP4 and Preview files will be forwarded to S3 directly. 

For example; when requested defined MP4 or any file:

`https://serverDomain:5443/{streamApp}/streams/{streamId}.mp4` 

HTTP request will be redirected as below

`https://{s3BucketName}.s3.{awsLocation}.amazonaws.com/streams/{streamId}.mp4`