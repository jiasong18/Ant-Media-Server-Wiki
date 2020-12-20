This guide describes how to live streaming in Social Media on Ant Media Server. This tutorial assumes that you have any Ant Media Server types.

1. How to Publish Live Stream on Facebook?
2. How to Publish Live Stream on Youtube?
3. How to Publish Live Stream on Periscope?
4. How to Add/Remove RTMP Endpoint with Ant Media Stream?

## How to Publish Live Stream on Facebook?

You can <span>publish live streams</span> on your pages/accounts. Just click the <strong>Live</strong> button in the Create Post tab.

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-facebook-fan-page-go-live.png" alt="ant-media-publish-stream-facebook-fan-page-go-live" title="ant-media-publish-stream-facebook-fan-page-go-live" />

After the click Live Button, you can see Facebook Live Dashboard as in the image shown below:

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-facebook-fan-page-go-live-panel.png" alt="ant-media-facebook-fan-page-go-live-panel" title="ant-media-facebook-fan-page-go-live-panel" />

You just need to copy the <strong>Stream URL</strong> and <strong>Stream Key.</strong>

PS: If you want to use a persistent stream key, you just need to enable <strong>Use a Persistent Stream key</strong> in Setup Option.

Your Facebook RTMP Endpoint URL that you will use in Ant Media Server should be like this: <code>&lt;StreamURL&gt;&lt;StreamKey&gt;</code>

For example: <code>rtmps://live-api-s.facebook.com:443/rtmp/677122211923308?s_bl=1&amp;s_psm=1&amp;s_sc=677124129589969&amp;s_sw=0&amp;s_vt=api-s&amp;a=AbxqZXR6X1VaKBzk</code>

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-dashboard-edit-RTMP-endpoints.png" alt="ant-media-dashboard-edit-RTMP-endpoints" title="ant-media-dashboard-edit-RTMP-endpoints" />

You just need to Add your Facebook RTMP Endpoint URL to the Ant Media Server stream RTMP Endpoint tab as the following image.

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-dashboard-add-facebook-rtmp-endpoint.png" alt="ant-media-dashboard-add-facebook-rtmp-endpoint" title="ant-media-dashboard-add-facebook-rtmp-endpoint" />

So, you can start broadcasting now!

## How to Publish Live Stream on Youtube?

You can <span>publish live streams</span> on your YouTube account. Just click the <strong>Create</strong> button and select <strong>Go Live.</strong>

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-youtube-go-live.png" alt="ant-media-publish-stream-youtube-go-live" title="ant-media-publish-stream-youtube-go-live" />

Just Click the <strong>Go</strong> button on the <strong>Streaming Software</strong> tab.

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-youtube-live-select-software.png" alt="ant-media-youtube-live-select-software" title="ant-media-youtube-live-select-software" />

Then copy the <strong>Stream URL</strong> and <strong>Stream Key.</strong>

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-youtube-live-dashboard.png" alt="ant-media-youtube-live-dashboard" title="ant-media-youtube-live-dashboard" />

Your Youtube RTMP Endpoint URL that you will use in Ant Media Server should be like this: <code>&lt;StreamURL&gt;/&lt;StreamKey&gt;</code>

For example: <code>rtmp://a.rtmp.youtube.com/live2/dq1j-waph-e322-waxd-dxzd</code>

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-dashboard-edit-RTMP-endpoints.png" alt="ant-media-dashboard-edit-RTMP-endpoints" />

You just need to add your Youtube RTMP Endpoint URL to the Ant Media Server stream RTMP Endpoint tab as the following image.

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-dashboard-add-youtube-rtmp-endpoint.png" alt="ant-media-dashboard-add-youtube-rtmp-endpoint"  title="ant-media-dashboard-add-youtube-rtmp-endpoint" />

So, you can start broadcasting now!

## How to Publish Live Stream on Youtube?

You can <span>publish live stream</span>s on your periscope account. Just click the <strong>Profile</strong> button and select <strong>Producer.</strong>

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-periscope-producer-tab.png" alt="ant-media-publish-stream-periscope-producer-tab"  title="ant-media-publish-stream-periscope-producer-tab" />

Then copy <strong>Stream URL</strong> and <strong>Stream Key.</strong>

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-periscope-producer-dashboard.png" alt="ant-media-periscope-producer-dashboard" title="ant-media-periscope-producer-dashboard" />

Your Periscope RTMP Endpoint URL that you will use in Ant Media Server should be like this: <code>&lt;StreamURL&gt;/&lt;StreamKey&gt;</code>

For example: <code>rtmp://de.pscp.tv:80/x/baps3a3x7j32</code>

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-dashboard-edit-RTMP-endpoints.png" alt="ant-media-dashboard-edit-RTMP-endpoints" />

You just need to add your Periscope RTMP Endpoint URL to the Ant Media Server stream RTMP Endpoint tab as the following image.

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-dashboard-add-periscope-rtmp-endpoint.png" alt="ant-media-dashboard-add-periscope-rtmp-endpoint" title="ant-media-dashboard-add-periscope-rtmp-endpoint" />

So, you can start broadcasting now!

## How to Add/Remove RTMP Endpoint with Ant Media Stream?
You can Add/Remove RTMP Endpoint with 2 options.

One of them is Add/Remove RTMP Endpoint with Dashboard. It's for the general users.

Another option is Add/Remove RTMP Endpoint with REST API. This option is for advanced users. You can Add/Remove RTMP Endpoint with programmatically. REST API Usage is so basic.

### 1. Add/Remove RTMP Endpoint with Dashboard
This option is for general users. You just need to click the <strong>broadcast properties</strong> tab and click <strong>Edit RTMP Endpoint</strong> as the below image.

<img src="https://antmedia.io/wp-content/uploads/2020/10/ant-media-dashboard-edit-RTMP-endpoints.png" alt="ant-media-dashboard-edit-RTMP-endpoints" title="ant-media-dashboard-edit-RTMP-endpoints" />

### 2. Add/Remove RTMP Endpoint with REST API
This option is for advanced users. You just need to request rtmp-endpoint REST API.

<strong>Here is Add RTMP Endpoint Javascript XHR example:</strong>
{% gist 1091ae93a58d8b62255c5a41803260f0 %}

<script src="https://gist.github.com/SelimEmre/1091ae93a58d8b62255c5a41803260f0.js"></script>

You can get more info in the <a href="https://antmedia.io/rest/#/BroadcastRestService/addEndpointV3" target="_blank" >REST API</a>.

<strong>Here is Remove RTMP Endpoint Javascript XHR example:</strong>
{% gist 6aa2fd90de535f3cd56bf33cbdddfb51 %}

You can get more info in <a href="https://antmedia.io/rest/#/BroadcastRestService/removeEndpointV2" target="_blank" rel="noopener noreferrer">REST API</a>.

Click for more detail about <a href="https://github.com/ant-media/Ant-Media-Server/wiki/REST-Guide" target="_blank" rel="noopener noreferrer">REST API Guide</a>.

<strong>PS:</strong> Please be sure to add your IP Address to the <strong>Use IP Filtering for RESTful API</strong> option on Application Settings.
<h5>Conclusion</h5>
After adding RTMP Endpoint, you need to publish a live stream. Here is our guide for <a href="https://github.com/ant-media/Ant-Media-Server/wiki/Publishing-Live-Streams" target="_blank" rel="noopener noreferrer">publishing live stream</a>. Finally check the social media account to see the live stream.

Please let us know if you have any question by sending an e-mail to contact@antmedia.io
