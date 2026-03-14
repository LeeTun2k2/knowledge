[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Canva Supports Real-Time Collaboration for 135 Million Monthly Users

### #36: Learn More - Awesome RSocket (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Feb 18, 2024

103

13

10

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Canva does real-time collaboration for millions of users. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/rsocket/?action=share) & I'll send you some rewards for the referrals._

December 2020 - Sydney, Australia.

Two UX designers want to edit the same file at once.

And they use a design app called Hooli.

One of them changes the heading. While the other deletes it being unaware of the user’s action.

And it caused a conflict in the file.

So they were frustrated.

[![RSocket](https://substackcdn.com/image/fetch/$s_!WHzh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F449e6b4a-72b6-41f6-8951-d2e7080802ba_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!WHzh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F449e6b4a-72b6-41f6-8951-d2e7080802ba_1200x630.gif)

Until one day when they hear about _Canva_ at a design conference.

They tried it and were dazzled by the real-time collaboration feature.

* * *

A request-response model over HTTP protocol isn’t enough for real-time collaboration.

Instead a _bidirectional_ protocol for pushing data to the clients in real time is needed.

Yet it becomes difficult at a high scale because many connections must be maintained simultaneously. And it’s complex to keep them reliable.

_**Long polling**_ sends a new request after each response. So it increases latency. Also state management and concurrency become difficult.

While _**SSE**_ on HTTP/2 needs an extra protocol on top of it to become bidirectional.

And _**WebSockets**_ in a _microservices_ architecture increases the server load. Also every connection needs some memory. Thus it wouldn't scale.

Besides WebSockets is a transport layer protocol. So it’s hard to find out the data packets are for a specific backend only using WebSockets.

[![Difficulties With Real-Time Protocol](https://substackcdn.com/image/fetch/$s_!TQEy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F75491ca8-456b-41c4-8d48-2779014207d3_1179x1168.png)](https://substackcdn.com/image/fetch/$s_!TQEy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F75491ca8-456b-41c4-8d48-2779014207d3_1179x1168.png)Difficulties With Real-Time Protocol

Most network protocols don’t satisfy the use cases needed for real-time collaboration.

Here are some of these use cases:

  * Message-driven communication to avoid extra processing on the sender-receiver side

  * High-performance communication to reduce system costs

  * Support various communication patterns for flexibility

  * Stable and simple protocol for resilience

  * Support many platforms and programming languages

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## RSocket

So they use _RSocket_ for real-time collaboration.

**RSocket** is an application layer protocol and it supports Reactive Streams semantics.

**Reactive Streams** is an initiative to standardize _asynchronous_ stream processing. Its principles are based on the [Reactive Manifesto](https://www.reactivemanifesto.org/).

Also RSocket supports client-server and server-server communication.

[![Origin Story of RSocket](https://substackcdn.com/image/fetch/$s_!Zlbp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F36e621d8-589c-44f4-8e5e-e49820d9205e_500x582.jpeg)](https://substackcdn.com/image/fetch/$s_!Zlbp!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F36e621d8-589c-44f4-8e5e-e49820d9205e_500x582.jpeg)[Origin Story of RSocket](https://imgflip.com/)

A group of companies created RSocket to build _resilient_ and _reactive_ microservices.

And here’s how RSocket offers extreme scalability:

### 1\. Performance

WebSockets doesn't support _multiplexing_.

While RSocket solves the multiplexing problem by creating many _channels_ within a network connection. Besides it’s possible to mix parts of one message with another on the wire.

**Multiplexing** divides the capacity of a network connection into many logical channels. And each logical channel transfers a separate data stream.

[![Multiplexing With RSocket](https://substackcdn.com/image/fetch/$s_!wetQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fecf567b4-b180-4261-a240-2054631ef85f_1200x630.png)](https://substackcdn.com/image/fetch/$s_!wetQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fecf567b4-b180-4261-a240-2054631ef85f_1200x630.png)Multiplexing in RSocket

RSocket communication is asynchronous.

And Reactive Streams like _[RxJava](https://github.com/ReactiveX/RxJava)_ can be used to implement RSocket.

### 2\. Data Format

RSocket uses the _binary protocol_ on top of TCP, WebSockets, or [Aeron](https://aeron.io/). Put another way, it’s transport layer agnostic.

For example, RSocket encodes JSON and sends it in binary data format.

[![Binary Messaging in RSocket](https://substackcdn.com/image/fetch/$s_!TWzC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc4ac938d-fe96-4c58-9673-72229d63fbdf_1200x630.png)](https://substackcdn.com/image/fetch/$s_!TWzC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc4ac938d-fe96-4c58-9673-72229d63fbdf_1200x630.png)Binary Messaging in RSocket

An RSocket message consists of data and metadata. Yet data and metadata can be in different data formats.

Besides RSocket supports RPC and event-based messaging. And sends real messages instead of just bytes.

### 3\. Flexibility

RSocket is _supported_ by most programming languages.

And it offers _application-level flow control_. So there's no need to implement buffering or windowing on the application. Instead the consumable amount of messages can be specified.

Besides it allows adding data for logical stream identification and infrastructure.

### 4\. Resilience

Each channel within an RSocket connection does backpressure independently. Put another way, there’s data flow control on each logical stream. 

Slowing down data flow to prevent overwhelming the receiver is called **Backpressure**.

Without backpressure, pending requests might bring down the entire system. Thus it reduces the _blast radius_.

Besides RSocket allows implementation of backpressure only on certain channels within a connection. Because it makes sense to buffer data for services like analytics. But drop data for critical services to have strong consistency.

[![Resilience via Reactive Streams](https://substackcdn.com/image/fetch/$s_!XxpU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff6c0445d-d904-4bbc-844c-f63efe79e4f1_1200x630.png)](https://substackcdn.com/image/fetch/$s_!XxpU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff6c0445d-d904-4bbc-844c-f63efe79e4f1_1200x630.png)Resilience via Reactive Streams

The client could inform the server about the number of messages it expects. And the server would reserve its capacity to return the exact number of messages. It’s called **Leasing**.

Also the server can inform the client about its capacity via data frames. Thus acting as a natural _rate limiter_ and _circuit breaker_.

Besides RSocket supports keepalive heartbeat signals.

### 5\. Communication Pattern

Once a connection is created, the _client_ vs _server_ distinction gets removed in RSocket.

And both sides become symmetrical via peer-to-peer communication. Put another way, each side can initiate the interactions.

[![Communication Patterns](https://substackcdn.com/image/fetch/$s_!1jQk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F28b6d400-17ae-46b4-b66a-3006567ae07b_1200x630.png)](https://substackcdn.com/image/fetch/$s_!1jQk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F28b6d400-17ae-46b4-b66a-3006567ae07b_1200x630.png)Communication Patterns

RSocket supports these communication patterns:

  * Request-Response: send and receive a message 

  * Request-Stream: send a message and receive a stream of messages

  * Request-Channel: send streams of messages in both directions

  * Fire-and-Forget: send a message one way

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Canva Architecture

The client connects to the WebSocket Gateway server over HTTP.

While the connection between the WebSocket Gateway server and backends is via RSocket.

And they run Java on the backend.

[![Canva Architecture](https://substackcdn.com/image/fetch/$s_!Svvn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86659bc0-b998-4afc-9eae-e2d847dc5804_1202x769.png)](https://substackcdn.com/image/fetch/$s_!Svvn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86659bc0-b998-4afc-9eae-e2d847dc5804_1202x769.png)Canva Architecture

They manage many channels within a single RSocket connection. Thus the total number of connections is kept low.

Besides the _least-loaded_ algorithm is used to load balance the RSocket channels. Because connections to backends are mostly long-lived.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

Real-time streaming offers a better user experience. Yet it's complex to implement.

And a wrong protocol could increase the system costs.

RSocket is a cloud-native protocol designed for high performance. It's slowly gaining popularity, so use it if necessary.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

#### NK’s Recommendations

  * [Crushing Tech Education](https://blog.crushingtecheducation.com/): Join a community of 5,000 engineers and technical managers dedicated to learning system design. Also consider subscribing to the [YouTube channel](https://www.youtube.com/@crushingtecheducation/videos).

Author: [Eugene Shulga](https://open.substack.com/users/136563906-eugene-shulga?utm_source=mentions)

  * [Leading Developers](https://zaidesanton.substack.com/): If you are a Development Team Leader, Engineering Manager, or considering that career path - try this newsletter.

Author: [Anton Zaides](https://open.substack.com/users/121956618-anton-zaides?utm_source=mentions)

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/rsocket?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Disney+ Hotstar Delivered 5 Billion Emojis in Real Time](https://substackcdn.com/image/fetch/$s_!0053!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1c8de39-6df0-4eed-a42b-227cb449519d_1280x720.gif)How Disney+ Hotstar Delivered 5 Billion Emojis in Real Time[Neo Kim](https://substack.com/profile/135589200-neo-kim)·February 10, 2024[Read full story](https://newsletter.systemdesign.one/p/hotstar-architecture)](https://newsletter.systemdesign.one/p/hotstar-architecture)

[![How 0.1% Companies Do Hyperscaling](https://substackcdn.com/image/fetch/$s_!NJSC!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc32fd27d-5dd6-45fb-9e55-c33ea16793ee_1280x720.gif)How 0.1% Companies Do Hyperscaling[Neo Kim](https://substack.com/profile/135589200-neo-kim)·January 28, 2024[Read full story](https://newsletter.systemdesign.one/p/cell-based-architecture)](https://newsletter.systemdesign.one/p/cell-based-architecture)

* * *

### References

  * [Enabling real-time collaboration with RSocket](https://www.canva.dev/blog/engineering/enabling-real-time-collaboration-with-rsocket/)

  * [RSocket](https://rsocket.io/)

  * [RSocket Protocol](https://github.com/rsocket/rsocket/blob/master/Protocol.md)

  * [Reactive Streams](https://www.reactive-streams.org/)

  * [Canva Statistics 2024](https://www.demandsage.com/canva-statistics/)

  * [The Reactive Manifesto](https://www.reactivemanifesto.org/)

  * [RSocket Spring](https://docs.spring.io/spring-framework/reference/rsocket.html)

  * [RxJS: Reactive Extensions For JavaScript](https://github.com/ReactiveX/rxjs)

103

13

10

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Sai teja's avatar](https://substackcdn.com/image/fetch/$s_!n7Iv!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fblack.png)](https://substack.com/profile/92287589-sai-teja?utm_source=comment)

[Sai teja](https://substack.com/profile/92287589-sai-teja?utm_source=substack-feed-item)

[Feb 18, 2024](https://newsletter.systemdesign.one/p/rsocket/comment/49813871 "Feb 18, 2024, 1:41 PM")

Liked by Neo Kim

WebSockets actually operate at the Application layer, not the Transport layer, according to the OSI model. This is a common misconception because WebSockets provide a way to establish a persistent, full-duplex communication channel over a single TCP connection, which operates at the Transport layer

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/rsocket/comment/49813871)

[![Konstantin Borimechkov's avatar](https://substackcdn.com/image/fetch/$s_!BeGt!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F532e6c77-82f4-44ff-b207-c5f7ce5acb27_620x620.jpeg)](https://substack.com/profile/145622751-konstantin-borimechkov?utm_source=comment)

[Konstantin Borimechkov](https://substack.com/profile/145622751-konstantin-borimechkov?utm_source=substack-feed-item)

[Feb 28, 2024](https://newsletter.systemdesign.one/p/rsocket/comment/50521071 "Feb 28, 2024, 8:04 AM")

Liked by Neo Kim

Amazing blog post! Having heard of RSocket before and this gave an amazing overview of this protocol!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/rsocket/comment/50521071)

[11 more comments...](https://newsletter.systemdesign.one/p/rsocket/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture