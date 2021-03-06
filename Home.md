
[![Build Status](https://travis-ci.com/ant-media/Ant-Media-Server.svg?branch=master)](https://travis-ci.com/ant-media/Ant-Media-Server)

# Ant Media Server Enterprise and Community Edition
Ant Media Server is a software that can stream live and VoD streams. It supports scalable, ultra low latency (0.5 seconds) adaptive streaming and records live videos in several formats like HLS, MP4, etc.

Here are the fundamental features of Ant Media Server:

* Ultra Low Latency Adaptive One to Many WebRTC Live Streaming in Enterprise Edition.
* Adaptive Bitrate for Live Streams (WebRTC, MP4, HLS, CMAF) in Enterprise Edition.
* SFU in One to Many WebRTC Streams in Enterprise Edition.
* Live Stream Publishing with RTMP and WebRTC.
* WebRTC to RTMP Adapter.
* IP Camera Support.
* Recording Live Streams (MP4 and HLS).
* Restream to Social Media Simultaneously (Facebook and Youtube in Enterprise Edition).
* One-Time Token Control in Enterprise Edition.
* Object Detection in Enterprise Edition.
* H.264,H.265 and VP8 
* WebRTC Data Channels Support.

#### _This doc includes information both for Community and Enterprise Editions. If somethings are not working according to this doc, you may be using Community Edition and you try to use a feature of Enterprise. Check the Community vs. Enterprise below_

## Community Edition & Enterprise Edition
Ant Media Server has two versions. One of them is the Community Edition(Free) and the other one is Enterprise Edition. Community Edition is available to [download on Github.](https://github.com/ant-media/Ant-Media-Server)
Enterprise Edition can be purchased [on antmedia.io](https://antmedia.io) 

|      | Community Edition  | Enterprise Edition |
| :---         |     :---:      | :---: |
| Ultra Low Latency <br>One-to-Many WebRTC Streaming    | ![false](images/false-icon.png)  |  ![true](images/true-icon.png)  |
| End-to-End Latency     | 8-12 Seconds  | 0.5 Seconds (500ms)  |
| CMAF  | ![false](images/false-icon.png)  |  ![true](images/true-icon.png)  |
| Scaling  | ![false](images/false-icon.png)  |  ![true](images/true-icon.png)  |
| RTMP(Ingesting) to WebRTC (Playing)  | ![false](images/false-icon.png)  |  ![true](images/true-icon.png)  |
| Hardware Encoding(GPU)  | ![false](images/false-icon.png)  |  ![true](images/true-icon.png)  |
| Adaptive Bitrate  | ![false](images/false-icon.png)  |  ![true](images/true-icon.png)  |
| Secure Streaming  | ![false](images/false-icon.png)  |  ![true](images/true-icon.png)  |
| iOS & Android RTMP SDK  | ![true](images/true-icon.png)  |  ![true](images/true-icon.png)  |
| iOS & Android WebRTC SDK  | ![false](images/false-icon.png)  |  ![true](images/true-icon.png)  |
| VP8 and H.265 Support  | ![false](images/false-icon.png)  |  ![true](images/true-icon.png)  |
| JavaScript SDK  | ![true](images/true-icon.png)  |  ![true](images/true-icon.png)  |
| RTMP, RTSP, MP4 and HLS Support  | ![true](images/true-icon.png)  |  ![true](images/true-icon.png)  |
| WebRTC to RTMP Adapter  | ![true](images/true-icon.png)  |  ![true](images/true-icon.png)  |
| 360 Degree Live & VoD Streams  | ![true](images/true-icon.png)  |  ![true](images/true-icon.png)  |
| Web Management Dashboard  | ![true](images/true-icon.png)  |  ![true](images/true-icon.png)  |
| IP Camera Support  | ![true](images/true-icon.png)  |  ![true](images/true-icon.png)  |
| Re-stream Remote Streams | ![true](images/true-icon.png)  |  ![true](images/true-icon.png)  |
| Open Source | ![true](images/true-icon.png)  |  ![true](images/true-icon.png)  |
| Simulcast to all Social Media via RTMP | ![true](images/true-icon.png)  |  ![true](images/true-icon.png)  |
| Support |  Community |  E-mail, On-site  |
| Price |  Free |  Paid  |

## Releases 

https://github.com/ant-media/Ant-Media-Server/releases/

## Licenses

Ant Media Server has basically two types of licenses. 
1. Ant Media Server Community Edition is free to use.  
2. Ant Media Server Enterprise Edition has a paid license per instance. Paid license options are hourly, monthly, annually and perpetual. You can get licenses from [antmedia.io](https://antmedia.io) or you can use hourly licenses from Marketplaces in [AWS Marketplace](https://aws.amazon.com/marketplace/search/results?x=0&y=0&searchTerms=Ant+Media+Server&page=1&ref_=nav_search_box), [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=Ant%20Media%20Server&page=1). 

#### Enterprise Cluster License
Enterprise Cluster License is similar features with the Enterprise License. Only difference is that Enterprise Cluster License supports many instances run simultaneously with the same license key. On the other hand, Enterprise License key only supports one instance at a time. 

If you're planning to have a large deployment for your Enterprise Cluster, please contact with Sales at contact@antmedia.io in order to  have some discounts. 

#### Free Enterprise License for Education and Tech Communities
Ant Media provides *free Enterprise Licenses** for the *students, academics and communities. To get advantage of this opportunity, just send an email (from your institution or community e-mail address) to contact@antmedia.io

## Architectural Overview

![](images/Simple_Architecture.png)

## Supported Environments
Ant Media Server runs on **Linux(Ubuntu)** and **MacOS**.  it supports only x64 architecture. 
Ubuntu 18.04 is officially supported and auxiliary scripts are provided for Ubuntu 20.04 and CentOS 8. In addition, It's known that Ant Media Server is used on SUSE, Debian, RHLE distributions as well.

## Extensions
### Object Recognition with TensorFlow
Ant Media Server can use trained deep learning model to recognize objects in the live streams.
This is a CPU intensive process so if you enable this feature, server's CPU consumption will increase. 

Meanwhile, users can use any deep models to execute for the live streams on the fly.  


## Community
There is a user community available in google group. You can join the google group, ask or answer questions:
https://groups.google.com/forum/#!forum/ant-media-server

You can also ask your questions at StackOverflow with the `ant-media-server` tag.

## Contact 

 For more information and blog posts visit [antmedia.io](https://antmedia.io)
 
 [contact@antmedia.io](mailto:contact@antmedia.io)