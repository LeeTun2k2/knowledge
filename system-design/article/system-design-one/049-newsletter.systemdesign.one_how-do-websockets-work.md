[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Do Websockets Work ✨

### #67: A Simple Introduction to Websockets (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Feb 28, 2025

196

22

19

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how websockets work. You’ll find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-do-websockets-work/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there lived a student named Jonas.

Although extremely bright, he struggled with chemistry lessons. 

And spent countless hours doing homework.

So he was sad and frustrated.

[![Google Docs use websockets for real time communication](https://substackcdn.com/image/fetch/$s_!2WOd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2961af29-d6f1-4cbe-82b8-03684275ade3_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!2WOd!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2961af29-d6f1-4cbe-82b8-03684275ade3_1200x630.gif)

Until one day, he decides to ask for help with homework from his friend, Emma.

And shares a Google Document with her.

The changes have to be visible in real-time to both of them, which means a bidirectional communication pattern is necessary.

Let’s quickly look into potential communication patterns to handle this use case:

#### 1\. HTTP Request Response

[![How HTTP Request-Response Works](https://substackcdn.com/image/fetch/$s_!BWWb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F07159f2c-cba6-493c-ab3f-c6f43f9a41c4_1011x678.png)](https://substackcdn.com/image/fetch/$s_!BWWb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F07159f2c-cba6-493c-ab3f-c6f43f9a41c4_1011x678.png)How HTTP Request-Response Works

The client requests the server for an update, and the server responds after processing it. Although this approach is simple and efficient, the client has to request the server again for an update. So it isn’t real-time.

#### 2\. Short Polling

[![How HTTP Polling Works](https://substackcdn.com/image/fetch/$s_!ibvh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd1b39042-747c-4078-9a23-f4894454a5ab_1035x676.png)](https://substackcdn.com/image/fetch/$s_!ibvh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd1b39042-747c-4078-9a23-f4894454a5ab_1035x676.png)How Short Polling Works

Imagine **polling** as automatically refreshing the browser every few seconds to check a stock’s price.

The client _periodically_ asks the server for an update, while the server returns an empty response if nothing has changed. It can track changes in real time.

Yet there are some problems with this approach:

  * Responsiveness depends on the interval between requests.

  * Extra overhead from connection requests and empty responses.

So it’s inefficient for this use case.

#### 3\. Long Polling

[![How Long Polling Works](https://substackcdn.com/image/fetch/$s_!E2OH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67a40195-9988-4464-9f8b-248c7e6f323b_1011x675.png)](https://substackcdn.com/image/fetch/$s_!E2OH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67a40195-9988-4464-9f8b-248c7e6f323b_1011x675.png)How Long Polling Works

Think of **long polling** as asking a restaurant waiter for food, and then waiting at the counter until it's ready.

The client requests, and the server responds only when there’s an update. 

Put simply, the server doesn’t send an empty response; instead, it _holds_ the request and waits for an update to occur. Yet the server sends an empty response on timeout. 

And client has to make an extra request to receive every change.

Although it allows real-time communication, there are some problems with this approach:

  * The server has to maintain many open connections, and it consumes resources.

  * The client has to make a separate request for sending data to the server.

So it’s inefficient for this use case.

#### 4\. Server-Sent Events

[![How Server-Sent Events Works](https://substackcdn.com/image/fetch/$s_!2Uq7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8304c443-d921-49d5-a2f9-03317a210364_1048x677.png)](https://substackcdn.com/image/fetch/$s_!2Uq7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8304c443-d921-49d5-a2f9-03317a210364_1048x677.png)How Server-Sent Events Works

Think of **SSE** like a radio broadcast, it's one way and listeners can't talk back to the radio station.

The client requests the server, and it creates an open connection with the client.

The server then updates the client whenever anything changes. Yet the communication is _unidirectional_ , and the client has to make a separate request to send data to the server.

So it's inefficient for this use case.

Ready for the best part?

* * *

### [Trae - Sponsored](https://www.trae.ai/?utm_source=ads&utm_medium=win_edm&utm_campaign=systemdesignone)

[![Trae AI](https://substackcdn.com/image/fetch/$s_!4e9g!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F640a7440-225a-49c9-abfe-725dcc32a991_1270x692.png)](https://www.trae.ai/?utm_source=ads&utm_medium=win_edm&utm_campaign=systemdesignone)

Meet Trae, an adaptive AI IDE that transforms how you work, collaborating with you to run faster:

  * Simply describe your project needs in natural language, and watch it built by Trae.

  * Upload your design files and let Trae assist in translating your visual concepts into clean, workable code.

  * As you code, Trae learns your project patterns and provides contextually relevant suggestions to help you code more efficiently.

[Get Trae right now!](https://www.trae.ai/?utm_source=ads&utm_medium=win_edm&utm_campaign=systemdesignone)

* * *

## How Do Websockets Work

Google Docs use [websockets](https://www.rfc-editor.org/rfc/rfc6455) for bidirectional communication in real-time.

Imagine **websockets** as a telephone call, both people can talk at the same time.

Here’s how it works:

### 1\. Opening a Websocket Connection

[![How Websocket Handshake Works](https://substackcdn.com/image/fetch/$s_!0GyA!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd5370f9-9349-4df9-9646-b559b1471637_1324x691.png)](https://substackcdn.com/image/fetch/$s_!0GyA!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd5370f9-9349-4df9-9646-b559b1471637_1324x691.png)How Websocket Handshake Works

The technique of opening a websockets connection is called the **handshake**.

Here’s how it works:

  * The client sends an HTTP GET request to the server asking for a websocket connection.

  * The HTTP request includes specific headers.

The server returns an HTTP [101 Switching Protocols](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/101) response code if it’s ready.

[![How Websockets Work](https://substackcdn.com/image/fetch/$s_!MkwI!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff11cac2f-624f-4776-9758-e183d27ec937_1034x676.png)](https://substackcdn.com/image/fetch/$s_!MkwI!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff11cac2f-624f-4776-9758-e183d27ec937_1034x676.png)How Websockets Work

The handshake is complete and opens a Transmission Control Protocol (**[TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)**) connection for bidirectional communication.

The client and server can exchange data in both directions in real-time. It supports text and binary data formats.

Onward.

### 2\. Transferring Data Through Websockets

[![A Websocket Message Split Into Frames](https://substackcdn.com/image/fetch/$s_!NSym!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F856155c5-c193-40b1-b2b3-8daef18a489d_987x303.png)](https://substackcdn.com/image/fetch/$s_!NSym!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F856155c5-c193-40b1-b2b3-8daef18a489d_987x303.png)Websocket Message Split Into Frames

A websocket message can be split into frames for performance. A frame contains information such as payload data in binary syntax. 

[![Sending Frames Through a Websocket Connection](https://substackcdn.com/image/fetch/$s_!MoOd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F21c95b15-fab3-439e-82f3-75b1447b5a8b_1011x677.png)](https://substackcdn.com/image/fetch/$s_!MoOd!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F21c95b15-fab3-439e-82f3-75b1447b5a8b_1011x677.png)Sending Frames Through a Websocket Connection

A frame allows the sender to have only a small buffer to keep data. The receiver then assembles the frames to reconstruct the original websocket message.

### 3\. Closing a Websocket Connection

The technique of closing a websocket connection is called **closing handshake**.

The client or server can close the connection. A websocket frame with a specific code is used to signal the close. The connection is closed after the receiver acknowledges it.

The server then deallocates the resources.

* * *

Websocket is efficient because it can send updates through a single connection.

Yet it needs a persistent and stateful connection between the client and server. While a server can handle only a limited websocket connections because of memory and socket limitations.

Although good for specific use cases, it isn’t a silver bullet for real-time communication.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/) | [Bluesky](https://bsky.app/profile/systemdesign.one)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a tech audience, you may want to [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-do-websockets-work?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Apple Pay Handles 41 Million Transactions a Day Securely 💸](https://substackcdn.com/image/fetch/$s_!6aLm!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff75ede44-6b6f-4e22-ae4d-dc762586955d_1280x720.gif)How Apple Pay Handles 41 Million Transactions a Day Securely 💸[Neo Kim](https://substack.com/profile/135589200-neo-kim)·March 27, 2025[Read full story](https://newsletter.systemdesign.one/p/how-does-apple-pay-work)](https://newsletter.systemdesign.one/p/how-does-apple-pay-work)

[![1 Simple Technique to Scale Microservices Architecture 🚀](https://substackcdn.com/image/fetch/$s_!MCWQ!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F84c96d29-cf27-47f0-9e1c-78ca1fa5f461_1280x720.gif)1 Simple Technique to Scale Microservices Architecture 🚀[Neo Kim](https://substack.com/profile/135589200-neo-kim)·February 4, 2025[Read full story](https://newsletter.systemdesign.one/p/how-to-scale-microservices)](https://newsletter.systemdesign.one/p/how-to-scale-microservices)

* * *

### References

  * [Short-polling vs Long-polling for real-time web applications?](https://stackoverflow.com/questions/4642598/short-polling-vs-long-polling-for-real-time-web-applications)

  * [Using server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)

  * [HTTP Long Polling - What it is and when to use it](https://ably.com/topic/long-polling)

  * [The WebSocket Protocol](https://www.rfc-editor.org/rfc/rfc6455)

  * [The WebSocket API and protocol explained](https://ably.com/topic/websockets)

  * [WebSocket protocol](https://www.wallarm.com/what/a-simple-explanation-of-what-a-websocket-is)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

196

22

19

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Bert Beckmann's avatar](https://substackcdn.com/image/fetch/$s_!EYWw!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd1016eba-4739-458e-8a30-e67a181908c1_144x144.png)](https://substack.com/profile/60352-bert-beckmann?utm_source=comment)

[Bert Beckmann](https://substack.com/profile/60352-bert-beckmann?utm_source=substack-feed-item)

[Feb 28, 2025](https://newsletter.systemdesign.one/p/how-do-websockets-work/comment/96858552 "Feb 28, 2025, 2:57 PM")

Liked by Neo Kim

so if "Although good for specific use cases, it isn’t a silver bullet for real-time communication."

what is the silver bullet (the best solution for real time communication) ?

If google docs uses websockets - how does google docs handle the server limited websocket connections issue?

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-do-websockets-work/comment/96858552)

[![Sanidhya Rathore's avatar](https://substackcdn.com/image/fetch/$s_!03vU!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faa534ebc-41f7-4eda-bed6-e2e603bfddd4_475x475.jpeg)](https://substack.com/profile/105663655-sanidhya-rathore?utm_source=comment)

[Sanidhya Rathore](https://substack.com/profile/105663655-sanidhya-rathore?utm_source=substack-feed-item)

[Mar 3, 2025](https://newsletter.systemdesign.one/p/how-do-websockets-work/comment/97564502 "Mar 3, 2025, 5:03 AM")

Liked by Neo Kim

Interesting Read!

ReplyShare

[20 more comments...](https://newsletter.systemdesign.one/p/how-do-websockets-work/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture