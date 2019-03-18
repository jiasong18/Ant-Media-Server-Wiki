Ant Media Server can be easily deployed on the Amazon Web Services Instances. Please note that; this documentation describes single instance deployment. Let's have a look at the steps.


### Step 1 Log in to AWS Console

First, you need to log in the [AWS Console](https://aws.amazon.com/console/). If you don’t have an AWS account, you need to [register](https://portal.aws.amazon.com/billing/signup#/start) first. Then you are able to use AWS services.

### Step 2 Create an Instance

After logging in to AWS Console, you will see the list of AWS Services. Click on the EC2 ([Elastic Computing](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)).

<img src="https://i0.wp.com/antmedia.io/wp-content/uploads/2019/02/Screenshot-from-2019-02-20-21-36-06-768x732.png" alt=" Start Ant Media Server Single Instance on AWS" />

<br>
EC2 dashboard shows a summary of current resources. Click on **Launch Instance** button.

<br>
<br>

<img src="https://i0.wp.com/antmedia.io/wp-content/uploads/2019/02/Screenshot-from-2019-02-20-21-36-21-768x385.png" alt=" Start Ant Media Server Single Instance on AWS" />



### Step 3 

Let's have a look at how to pull stream from IP Camera. Of course, first, you need to install Ant Media Server, please see <a href="https://antmedia.io/documentation/">documentation</a>.
<h2>Add IP Camera</h2>
Select "LiveApp" from applications, then click "New Live Stream" and select "IP Camera". Fill camera name, camera username, and camera password. You should define ONVIF URL of IP Camera. Generally, it is IP-ADDRESS-OF-IPCAMERA:8080.  Another promising feature comes :) You can use "auto-discovery " feature! If cameras and server are in the same subnet, Ant Media Server automatically discovers them. The screenshot of  auto-discovery result is shown below.
<ol></ol>
<img src="https://antmedia.io/wp-content/uploads/2018/03/Screenshot-from-2018-03-21-21-00-04-1024x536.png" alt="ONVIF IP Camera Auto Discovery"  class="aligncenter wp-image-3327" title="ONVIF IP Camera Auto Discovery" />
<h2>Watch IP Cameras</h2>
If the IP cameras are reachable and configured correctly, Ant Media Server add theirs streams as live streams and starts to pull streams from them. You can see its status on the management panel.

<img src="https://antmedia.io/wp-content/uploads/2018/03/Screenshot-from-2018-03-21-21-10-12-1024x372.png" alt="ONVIF IP Camera fetching stream" class="aligncenter wp-image-3328" title="ONVIF IP Camera fetching stream" />

&nbsp;

To watch the stream, just click the Play button on Actions.

<img src="https://antmedia.io/wp-content/uploads/2018/03/Screenshot-from-2018-03-21-21-15-15-1024x502.png" alt="ONVIF IP Camera watching"  class="aligncenter wp-image-3330" title="ONVIF IP Camera watching" />
<br/><br/>
Also, you can switch to Grid view if you have more than one camera and want to see them simultaneously.
<br/><br/>
<img src="https://antmedia.io/wp-content/uploads/2018/03/Screenshot-from-2018-03-21-21-17-47-1024x494.png" alt=""  class="aligncenter wp-image-3332" title="IP Camera watching in Grid View" />
<h2>Record IP Camera Streams</h2>
The Ant Media Server can save IP Camera streams as MP4 format. In addition, it record streams with defined periods such one hour, ten hours interval . You can see these recorded files on VoD tab in the management panel.
<br/>
<img src="https://antmedia.io/wp-content/uploads/2018/03/Screenshot-from-2018-03-21-21-19-46-1024x347.png" alt=""  class="aligncenter wp-image-3333" title="ONVIF IP Camera watching" />

&nbsp;

We hope you will be happy about this feature and the good news is that it is offered in Community Edition. In other words, it is free! Please keep in touch because we are releasing new versions, of course, new features. If you have any questions or comments,  just send an email to contact at antmedia dot io or fill the <a href="https://antmedia.io/#contact">contact form.</a>

&nbsp;