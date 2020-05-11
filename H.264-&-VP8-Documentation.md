In this documentation, we're going to explain simply how to use H.264 & VP8 in Ant Media Server.

![H264-VP8 General Settings](https://antmedia.io/wp-content/uploads/2020/05/H264-VP8-general.png)

VP8 and H.264 are mandatory codecs in WebRTC according to [https://tools.ietf.org/html/rfc7742](RFC 7742). On the other hand, not all browsers support VP8 and H.264 codecs at the same time which may be caused by some licensing issues. 

Ant Media Server supports H.264 for 1.x versions and now Ant Media Server supports  VP8 for 2.x versions including transcoding, clustering. So that all of the devices can play WebRTC streams within their browsers.

1. [H.264 & VP8 Enabled](#1-h264--vp8-enabled)
2. [Only H.264 Enabled](#2-only-h264-enabled)
3. [Only VP8 Enabled](#3-only-vp8-enabled)

## 1. H.264 & VP8 Enabled

![H.264 & VP8 Enabled](https://antmedia.io/wp-content/uploads/2020/05/H264VP8.png)

H.264 and VP8 enabled section, browser choose which is convenient for the support. If you choose this section, you should enable Adaptive Bitrate.

## 2. Only H.264 Enabled

![Only H.264 Enabled](https://antmedia.io/wp-content/uploads/2020/05/Only-H.264-Enabled.png)

This section supports only H.264 enabled.

## 3. Only VP8 Enabled

![Only VP8 Enabled](https://antmedia.io/wp-content/uploads/2020/05/Only-VP8-Enabled.png)

This section supports only VP8 enabled. In this section, you cannot use HLS Streaming & MP4 Recording.
VP8 section can use the H.264 unsupported browsers/devices. You can check your device VP8 supports in here -> https://mozilla.github.io/webrtc-landing/pc_test_no_h264.html
