Let’s have look at WebRTC Peer-to-Peer Communication procedure.

![WebRTC_P2P](https://i1.wp.com/antmedia.io/wp-content/uploads/2018/11/peer2peer-1.png)


## Basics of WebRTC Peer-to-Peer Connection

For WebRTC Peer-To-Peer Communication, two clients create a direct connection between each other after completing signaling operations. First, both clients get media sources. Then one of them starts signaling procedures. As stated in the below figure, Browser 1 creates an SDP offer and sends to another side. Browser 2 gets that offer and creates an SDP answer and sends to Browser 1. After the SDP change phase, Browser 1 sends Ice Candidate to its peer then Browsers send Ice Candidate also. These sendings are performed several times until they get the required network information of another part. Finally, they create direct WebRTC communication between them.

![WebRTC_P2P](https://i1.wp.com/antmedia.io/wp-content/uploads/2019/01/webRTC-BasicsOfHowItWorks2.png)

(* Retrieved from https://developer.mozilla.org/en-US/docs/Web/Guide/API/WebRTC/Peer-to-peer_communications_with_WebRTC)

## How to Establish Peer to Peer WebRTC Connection using Ant Media Server

Ant Media Server supports WebRTC Peer-To-Peer communication in addition to 1-N and N-N communication. Mobile (Android, IOS ) and Web (JS) SDKs provide P2P communication functionalities to developers. The steps of WebSocket messaging mechanism to create WebRTC P2P connections are described below;

**1. Peers connect to Ant Media Server through WebSocket.**

> ws://SERVER_NAME:5080/WebRTCAppEE/websocket

**2. The client sends join JSON command to the server with stream name parameter.**

> {
    command : "join",
    streamId : "stream1",
}

**3. When the second peer joins the stream, the server sends start JSON command to the first peer**

> {
    command : "start",
    streamId : "stream1",
}

**4. The first peer create offer SDP and send to the server with takeConfiguration command,**

> {
   command : "takeConfiguration",
   streamId : "stream1",
   type : "offer",  
   sdp : "${SDP_PARAMETER}"
}

**5. The second peer creates answer SDP and sends to the server with takeConfiguration command**

> {
   command : "takeConfiguration",
   streamId : "stream1",
   type : "answer",  
   sdp : "${SDP_PARAMETER}"
}

**6. Each peer gets ice candidates several times and sends to each other with takeCandidate command through the server.**

> {
    command : "takeCandidate",
    streamId : "stream1",
    label : "${CANDIDATE.SDP_MLINE_INDEX}",
    id : "${CANDIDATE.SDP_MID}",
    candidate : "${CANDIDATE.CANDIDATE}"
}

**7. Clients send leave JSON command to leave the room**

> {
    command : "leave",
    streamId: "stream1"
}












