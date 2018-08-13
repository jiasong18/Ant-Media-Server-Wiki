Amazon S3 Integration lets you upload your MP4 files and previews to S3 automatically so that you do not need to worry about disk space in your server. Here is the simple manual how to enable S3 Integration to your streaming app 

##  Enable AWS S3 in your Streaming App

* In order to programmatically access to S3, you should have access token and secret keys. You can create a programmatic user to have access token and secret key from [AWS IAM(Identity and Access Management)](https://console.aws.amazon.com/iam/home#/users) console. Just `Add User` by checking `Programmatic Access` box and then in the next section click `Attach existing policies directly` and add `AmazonS3FullAccess` access permission to this user. Copy access token and secret key for this user. 

* Right now, you should have `access token`, `secret key`, `bucket name` in your hand. You also need to know the region of your bucket. If you do not have any bucket, you can create it on [S3 Console](https://s3.console.aws.amazon.com/s3/home)  

* Add a new bean to `red5-web.xml` of your app as below. `red5-web.xml` file is under `WEB-INF` folder of your app. For instance `webapps/LiveApp/WEB-INF/red5-web.xml`

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

##  Forward MP4 and Preview Request to S3 in your Streaming App
This section is optional. If you want to forward http requests to MP4 files and Preview files to S3 directly, you can use a HTTP filter. 
* Clone the sample app from here [https://gitlab.com/Ant-Media/SampleApp.git](https://gitlab.com/Ant-Media/SampleApp.git)

* Open the `HttpForwardFilter` in `io.antmedia.serverapp.pscp.filter` package and uncomment the lines with changing your bucket url name
```java
public class HttpForwardFilter implements javax.servlet.Filter {

	@Override
	public void destroy() {

	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response,
			FilterChain chain) throws IOException, ServletException {
		String requestURI = ((HttpServletRequest)request).getRequestURI();

		
		File f = new File("webapps/"+ requestURI);
		if (!f.exists()) 
		{
			String redirectUri = "WRITE YOUR BUCKET URL like https://s3.eu-central-1.amazonaws.com/" + requestURI;
			HttpServletResponse httpResponse = (HttpServletResponse) response;
			httpResponse.sendRedirect(redirectUri);
			return;
		}
		
		
		chain.doFilter(request, response);
	}

	@Override
	public void init(FilterConfig arg0) throws ServletException {

	}

}
```

* You also need to add S3 bean `red5-web.xml` in `WEB-INF` folder 
* Package the app with Maven 
`
mvn clean package -Dgpg.skip=true
`
After the command is done, you will have `WAR` file under `target` directory

* Copy the created WAR file under `webapps` directory by removing old LiveApp directory and restart the server
```
sudo rm -rf /usr/local/antmedia/webapps/LiveApp
sudo cp target/LiveApp*.war /usr/local/antmedia/webapps/
sudo service antmedia restart
```

After server restarted, MP4 and Preview files will be forwarded to S3 directly. 


I hope this manual help you in S3 integration to your app, if you have any problem or question with that just let us know at contact@antmedia.io 