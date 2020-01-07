`LiveApp`


<p>Ant Media Server can pull IP Camera streams easily and it can configured on management panel.
In other words, you don’t&nbsp;need to write any commands or use terminal to add IP Cameras.</p>
<p>The main critical point is that the IP Cameras should support ONVIF
standard. In fact, the majority of manufacturers already support this
standard because ONVIF make it easy to manage IP Cameras. All CRUD(Create-Read-Update-Delete) and
PTZ(Pan-Tilt-Zoom) operations are based on well-defined SOAP messages.</p>

<a target="_blank" rel="noopener noreferrer" href="https://camo.githubusercontent.com/fbd02816f863a8f20073644d7558ac93433bba38/68747470733a2f2f616e746d656469612e696f2f77702d636f6e74656e742f75706c6f6164732f323031382f30332f636374762d6f6e7669662d3536302e6a7067"><img alt="onvif" src="https://camo.githubusercontent.com/fbd02816f863a8f20073644d7558ac93433bba38/68747470733a2f2f616e746d656469612e696f2f77702d636f6e74656e742f75706c6f6164732f323031382f30332f636374762d6f6e7669662d3536302e6a7067" data-canonical-src="https://antmedia.io/wp-content/uploads/2018/03/cctv-onvif-560.jpg" style="max-width:100%;"></a>

<p>Let’s have a look at how to pull stream from IP Camera.</p>
<a name="user-content-add-ip-camera"></a>
<h2><a id="user-content-add-ip-camera" class="anchor" aria-hidden="true" href="#add-ip-camera"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Add IP Camera</h2>
Select `LiveApp` from applications on the left side menu, then click `New Live Stream` and
select “IP Camera”. Fill camera name, camera username, and camera
password. You should define ONVIF URL of IP Camera. Generally, it is
IP-ADDRESS-OF-IPCAMERA:8080.&nbsp; Another promising feature comes :) You can
use “auto-discovery” feature! If cameras and server are in the same
subnet, Ant Media Server automatically discovers them. The screenshot
of&nbsp; auto-discovery result is shown below.

# TODO: Add auto-discovery screenshot

<a name="user-content-watch-ip-cameras"></a>
<h2><a id="user-content-watch-ip-cameras" class="anchor" aria-hidden="true" href="#watch-ip-cameras"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Watch IP Cameras</h2>
<p>If the IP cameras are reachable and configured correctly, Ant Media
Server add theirs streams as live streams and starts to pull streams
from them. You can see its status on the management panel.</p>
<p>To watch the stream, just click the Play button on Actions.</p>
<div>
<a target="_blank" rel="noopener noreferrer" href="https://camo.githubusercontent.com/c07d01af7af10c2dab9a7cce69383f432a922673/68747470733a2f2f616e746d656469612e696f2f77702d636f6e74656e742f75706c6f6164732f323031382f30332f53637265656e73686f742d66726f6d2d323031382d30332d32312d32312d31352d31352d31303234783530322e706e67"><img alt="ONVIF IP Camera watching" src="https://camo.githubusercontent.com/c07d01af7af10c2dab9a7cce69383f432a922673/68747470733a2f2f616e746d656469612e696f2f77702d636f6e74656e742f75706c6f6164732f323031382f30332f53637265656e73686f742d66726f6d2d323031382d30332d32312d32312d31352d31352d31303234783530322e706e67" data-canonical-src="https://antmedia.io/wp-content/uploads/2018/03/Screenshot-from-2018-03-21-21-15-15-1024x502.png" style="max-width:100%;"></a>
</div>
<p>Also, you can switch to Grid view if you have more than one camera and
want to see them simultaneously.</p>
<div>
<a target="_blank" rel="noopener noreferrer" href="https://camo.githubusercontent.com/db35748b4403970d69648e083d925315f8a153f9/68747470733a2f2f616e746d656469612e696f2f77702d636f6e74656e742f75706c6f6164732f323031382f30332f53637265656e73686f742d66726f6d2d323031382d30332d32312d32312d31372d34372d31303234783439342e706e67"><img alt="" src="https://camo.githubusercontent.com/db35748b4403970d69648e083d925315f8a153f9/68747470733a2f2f616e746d656469612e696f2f77702d636f6e74656e742f75706c6f6164732f323031382f30332f53637265656e73686f742d66726f6d2d323031382d30332d32312d32312d31372d34372d31303234783439342e706e67" data-canonical-src="https://antmedia.io/wp-content/uploads/2018/03/Screenshot-from-2018-03-21-21-17-47-1024x494.png" style="max-width:100%;"></a>
</div>
<a name="user-content-record-ip-camera-streams"></a>
<h2><a id="user-content-record-ip-camera-streams" class="anchor" aria-hidden="true" href="#record-ip-camera-streams"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Record IP Camera Streams</h2>
<p>The Ant Media Server can save IP Camera streams as MP4 format. In
addition, it record streams with defined periods such one hour, ten
hours interval . You can see these recorded files on VoD tab in the
management panel.</p>
<div>
<a target="_blank" rel="noopener noreferrer" href="https://camo.githubusercontent.com/168696ec12e5860d89576276d1c0ef5a309e0671/68747470733a2f2f616e746d656469612e696f2f77702d636f6e74656e742f75706c6f6164732f323031382f30332f53637265656e73686f742d66726f6d2d323031382d30332d32312d32312d31392d34362d31303234783334372e706e67"><img alt="" src="https://camo.githubusercontent.com/168696ec12e5860d89576276d1c0ef5a309e0671/68747470733a2f2f616e746d656469612e696f2f77702d636f6e74656e742f75706c6f6164732f323031382f30332f53637265656e73686f742d66726f6d2d323031382d30332d32312d32312d31392d34362d31303234783334372e706e67" data-canonical-src="https://antmedia.io/wp-content/uploads/2018/03/Screenshot-from-2018-03-21-21-19-46-1024x347.png" style="max-width:100%;"></a>
</div>
<p>We hope you will be happy about this feature and the good news is that
it is offered in Community Edition. In other words, it is free! Please
keep in touch because we are releasing new versions, of course, new
features. If you have any questions or comments,&nbsp; just send an email to
contact at antmedia dot io or fill the contact form.</p>

