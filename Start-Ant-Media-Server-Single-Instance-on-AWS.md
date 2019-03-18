Ant Media Server can be easily deployed on the Amazon Web Services Instances. Please note that; this documentation describes single instance deployment. Let's have a look at the steps.


## Step 1) Log in to AWS Console

First, you need to log in the [AWS Console](https://aws.amazon.com/console/). If you don’t have an AWS account, you need to [register](https://portal.aws.amazon.com/billing/signup#/start) first. Then you are able to use AWS services.

## Step 2) Create an Instance

After logging in to AWS Console, you will see the list of AWS Services. Click on the EC2 ([Elastic Computing](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)).

<img src="https://i0.wp.com/antmedia.io/wp-content/uploads/2019/02/Screenshot-from-2019-02-20-21-36-06-768x732.png" alt=" Start Ant Media Server Single Instance on AWS" />  



EC2 dashboard shows a summary of current resources. Click on **Launch Instance** button.  



<img src="https://i0.wp.com/antmedia.io/wp-content/uploads/2019/02/Screenshot-from-2019-02-20-21-36-21-768x385.png" alt=" Start Ant Media Server Single Instance on AWS" />

At this stage, you need to select Amazon Machine Image. Click Ubuntu Server from Quick Start tab.

<img src="https://antmedia.io/wp-content/uploads/2019/02/Screenshot-from-2019-03-18-16-42-05.png" alt=" Start Ant Media Server Single Instance on AWS" />

After your image selection, you need to define the instance type which is compatible with the image.

<img src="https://antmedia.io/wp-content/uploads/2019/02/Screenshot-from-2019-03-18-16-42-22.png" alt=" Start Ant Media Server Single Instance on AWS" />

You need configure security parameters. Please keep in mind that, Ant Media Server needs following ports to operate. Here are the ports that server uses;

* TCP:1935 (RTMP)
* TCP:5080 (HTTP)
* TCP:5443 (HTTPS)
* TCP:5554 (RTSP)
* UDP:5000-65000 (WebRTC and RTSP)

<img src="https://antmedia.io/wp-content/uploads/2019/02/Screenshot-from-2019-03-18-17-41-45.png" alt=" Start Ant Media Server Single Instance on AWS" />

At the last stage, AWS creates a key file to connect your instance. If you have created before for your other instance, you can continue to use it for your new instance also. Do not forget to download the key file you have not before.

<img src="https://i0.wp.com/antmedia.io/wp-content/uploads/2019/02/Screenshot-from-2019-02-20-21-59-32.png" alt=" Start Ant Media Server Single Instance on AWS" />



## Step 3) Monitor and Connect

Once you have successfully started instance then you can monitor it in the Instance Panel easily.

<img src="https://antmedia.io/wp-content/uploads/2019/02/Screenshot-from-2019-03-18-17-46-33.png" alt=" Start Ant Media Server Single Instance on AWS" />

To connect your instance, select your instance and click the **Connect** button to get connection instructions.

<img src="https://i0.wp.com/antmedia.io/wp-content/uploads/2019/02/Screenshot-from-2019-02-20-23-12-45.png" alt=" Start Ant Media Server Single Instance on AWS" />

You need to first configure permission of the key file correctly.

`chmod 400 ant.pem`

Then connect using ssh credentials. Such as:

`ssh -i ant.pem ubuntu@ec2-3-120-228-117.eu-central-1.compute.amazonaws.com`

## Step 4) Install Ant Media Server

Just apply 2 basic steps that described in the  [Getting Started](https://github.com/ant-media/Ant-Media-Server/wiki/Getting-Started) page in order to install Ant Media Server easily. Basically you need to;

* Download installation script and define required permissions `wget https://raw.githubusercontent.com/ant-media/Scripts/master/install_ant-media-server.sh` and `chmod 755 install_ant-media-server.sh`

* Run installation script `sudo ./install_ant-media-server.sh [ANT_MEDIA_SERVER_INSTALLATION_FILE] `
