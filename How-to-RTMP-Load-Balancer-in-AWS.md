Follow the instructions below to configure RTMP Load Balancer in Ant Media Server Auto Scaling structure.

**1.** Click on Create Load Balancer and create a new Load Balancer. (EC2 > Load Balancers > Create Load Balancer )
![](images/aws-rtmp-2.png)


**2.** Create a Classic Load Balancer.
![](images/aws-rtmp-3.png)
**3.** Adjust the settings as in the red rectangles.
![](images/aws-rtmp-4.png)
**4.** In this section, select Create a New Security Group and adjust the settings as in the Red rectangle.
![](images/aws-rtmp-5.png)
**5.** Proceed by clicking “Next: Configure Health Check” button.
![](images/aws-rtmp-6.png)
**6.** Proceed by clicking “Next: Add EC2 Instances” button.
![](images/aws-rtmp-7.png)
**7.** Press the Next button without doing anything here.
![](images/aws-rtmp-8.png)
**8.** Click the Review and Create button here.
![](images/aws-rtmp-9.png)
**9.** Please proceed by clicking "Create"
![](images/aws-rtmp-10.png)
**10.** Finish the Load Balancer setup.
![](images/aws-rtmp-11.png)
**11.** Auto Scaling > Go to Auto Scaling > Auto Scaling Groups and select your Origin Group.
![](images/aws-rtmp-13.png)
**12.** Edit the Load Balancing setting from this section.
![](images/aws-rtmp-14.png)
**13.** In this window, add the Classic Load Balancer you have added to the Choose A Load Balancer section and click the Update button.
![](images/aws-rtmp-15.png)
**14.** If everything is fine, you should see the origin instance when you click the Instance tab from Load Balancing> Load Balancers> Antmedia-RTMP-LB.
![](images/aws-rtmp-16-1.png)

Since a new load balancer has been installed, your RTMP URL will be the Load Balancer URL.

**Example:**
```
rtmp://Antmedia-RTMP-LB-962025612.eu-west-2.elb.amazonaws.com/WebRTCAppEE/
```
![](images/aws-rtmp-url.png)