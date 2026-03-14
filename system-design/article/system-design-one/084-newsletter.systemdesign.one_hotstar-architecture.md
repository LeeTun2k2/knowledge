[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Disney+ Hotstar Delivered 5 Billion Emojis in Real Time

### #35: Learn More - Hotstar Architecture for Emojis (5 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Feb 10, 2024

104

12

5

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Disney+ Hotstar delivered billions of emojis in real time. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/hotstar-architecture/?action=share) & I'll send you some rewards for the referrals._

February 2015 - Mumbai, India.

Disney+ Hotstar starts streaming live cricket.

Yet it lacked an immersive user experience.

So they created a live feed where people can express their moods via emojis. Thus creating an engaging live cricket experience.

[![Hotstar Architecture for Emojis](https://substackcdn.com/image/fetch/$s_!KjuW!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1f6dc012-4402-44e2-af98-31f407bd778f_800x500.gif)](https://substackcdn.com/image/fetch/$s_!KjuW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1f6dc012-4402-44e2-af98-31f407bd778f_800x500.gif)

And 55 million raving fans of cricket in India sent over _5 billion emojis in real-time_ during the 2019 Cricket World Cup.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Hotstar Architecture

They don’t do caching because emojis must be shown in real-time.

Also they split the services into smaller components to scale independently.

[![Hotstar Architecture](https://substackcdn.com/image/fetch/$s_!VVYU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F513e7007-2464-4393-9313-de772f9991e9_1274x469.png)](https://substackcdn.com/image/fetch/$s_!VVYU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F513e7007-2464-4393-9313-de772f9991e9_1274x469.png)Hotstar Architecture

Besides they avoided blocking resources and supported high concurrency via _asynchronous_ processing.

Here’s how they scaled emojis to millions of concurrent users:

### 1\. Receiving Emojis From Clients

They send emojis from the client to the server using HTTP. And run the API server in the Go programming language.

The data gets written to a local buffer before returning success to the client. Because it prevents hogging client connections on the API server.

And the buffered data is asynchronously written in batches to the _message queue_. They use [Goroutines](https://go.dev/tour/concurrency/1) while writing to the message queue for concurrency.

**Goroutines** are lightweight threads that execute functions asynchronously.

[![Asynchronous Processing on API Server](https://substackcdn.com/image/fetch/$s_!8khR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbfcf1d9c-2472-4256-ab49-c3dbd217bcaf_1286x392.png)](https://substackcdn.com/image/fetch/$s_!8khR!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbfcf1d9c-2472-4256-ab49-c3dbd217bcaf_1286x392.png)Asynchronous Processing on API Server

They use [Apache Kafka](https://kafka.apache.org/) as the message queue. It decouples system components and automatically removes the older messages on expiry.

Yet Kafka has a high operational complexity. So they use their in-house data platform, [Knol](https://blog.hotstar.com/data-democratisation-hotstar-93ebfb1e688d) to run Kafka.

The **message queue** is a common mechanism for asynchronous communication between services.

### 2\. Processing Emojis

They use [Apache Spark](https://spark.apache.org/) to process the stream of emojis from Kafka. 

**Stream processing** is a type of data processing designed for infinite data sets.

A streaming job in Spark aggregates the emojis over smaller intervals to offer a real-time experience. And the aggregated emojis get written into another Kafka.

[![Normalizing Stream of Emojis](https://substackcdn.com/image/fetch/$s_!0Ml0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc74a88b8-344b-411e-860b-a312fefdee0e_1052x293.png)](https://substackcdn.com/image/fetch/$s_!0Ml0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc74a88b8-344b-411e-860b-a312fefdee0e_1052x293.png)Normalizing Stream of Emojis

They chose Spark because it offers _micro-batching_ and good community support. Also it offers fault tolerance through checkpointing.

**Micro-batching** is batching together data records every few seconds before processing them.

While**Checkpointing** is a mechanism to store data and metadata on the file system.

Besides combining Spark and Kafka guarantees only _one-time processing_ of emojis. Thus preventing duplicates.

### 3\. Delivering Emojis to Clients

They use Python consumers to read normalized emojis from Kafka. 

And the emojis get sent to the _PubSub_ infrastructure.

While the PubSub delivers emojis over a _persistent connection_ to the clients.

They use the Message Queuing Telemetry Transport (**[MQTT](https://mqtt.org/)**) protocol to deliver the emojis.

MQTT is a lightweight messaging protocol over TCP. And it’s supported by a broad range of platforms and devices.

[![PubSub Delivering Emojis](https://substackcdn.com/image/fetch/$s_!g7Ma!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb1f3d1e3-7179-4617-8eb1-94a55d53a6f3_1302x284.png)](https://substackcdn.com/image/fetch/$s_!g7Ma!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb1f3d1e3-7179-4617-8eb1-94a55d53a6f3_1302x284.png)PubSub Delivering Emojis

They use the [EMQX](https://www.emqx.io/) (MQTT)_message broker_ to distribute messages.__ It’s based on _Erlang_ and offers very high performance.

A single machine in the EMQX cluster could handle 250k connections. 

But EMQX internally uses a distributed database called [Mnesia](https://www.erlang.org/doc/man/mnesia.html). And it was designed to support only a few machines. So it became difficult to scale beyond 2 million concurrent connections by adding more machines.

Thus they didn’t use the EMQX cluster. Instead set up a multi-cluster system using the _reverse bridge_.

[![Scaling EMQX Cluster Using Reverse Bridge](https://substackcdn.com/image/fetch/$s_!Kd9E!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F499b9414-d330-406b-8ff5-0d5225579cb3_2441x1013.png)](https://substackcdn.com/image/fetch/$s_!Kd9E!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F499b9414-d330-406b-8ff5-0d5225579cb3_2441x1013.png)Scaling EMQX Cluster Using Reverse Bridge

Each cluster contains a single publish node and many subscribe nodes. 

While the main publish node forwards the emojis to each cluster.

They run a Golang service on each cluster. And it subscribes to all the messages from the main publish node and forwards them to the publish node on every cluster. They called it the _reverse bridge_ architecture.

Besides they set up autoscaling to change the number of subscriber nodes for scalability.

* * *

Disney+ Hotstar remains a major player in India's streaming industry.

And this case study shows that a simple tech stack with proven technologies is enough for high scalability.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

#### NK’s Recommendations

  * [The Modern Software Developer](https://tmsd.substack.com/): Get tips on navigating the world of software development while getting to grips with your mental and physical well-being.

Author: [Richard Donovan](https://open.substack.com/users/82558893-richard-donovan?utm_source=mentions)

  * [Crushing Tech Education](https://blog.crushingtecheducation.com/): Join a community of 5,000 engineers and technical managers dedicated to learning system design.

Author: [Eugene Shulga](https://open.substack.com/users/136563906-eugene-shulga?utm_source=mentions)

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/hotstar-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How 0.1% Companies Do Hyperscaling](https://substackcdn.com/image/fetch/$s_!NJSC!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc32fd27d-5dd6-45fb-9e55-c33ea16793ee_1280x720.gif)How 0.1% Companies Do Hyperscaling[Neo Kim](https://substack.com/profile/135589200-neo-kim)·January 28, 2024[Read full story](https://newsletter.systemdesign.one/p/cell-based-architecture)](https://newsletter.systemdesign.one/p/cell-based-architecture)

[![How Hashnode Generates Feed at Scale](https://substackcdn.com/image/fetch/$s_!J2_D!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F06af69cc-23c5-4e78-bb48-56a53fcbfce8_1280x720.gif)How Hashnode Generates Feed at Scale[Neo Kim](https://substack.com/profile/135589200-neo-kim)·January 21, 2024[Read full story](https://newsletter.systemdesign.one/p/feed-architecture)](https://newsletter.systemdesign.one/p/feed-architecture)

* * *

## References

  * [Capturing A Billion Emojions](https://blog.hotstar.com/capturing-a-billion-emojis-62114cc0b440)

  * [Building Pubsub for 50M concurrent socket connections](https://blog.hotstar.com/building-pubsub-for-50m-concurrent-socket-connections-5506e3c3dabf)

  * [Spark Streaming vs Flink vs Storm vs Kafka Streams vs Samza: Choose Your Stream Processing Framework](https://medium.com/@chandanbaranwal/spark-streaming-vs-flink-vs-storm-vs-kafka-streams-vs-samza-choose-your-stream-processing-91ea3f04675b)

  * [Comparing Apache Kafka, Amazon Kinesis, Microsoft Event Hubs, and Google Pub/Sub](https://blog.scottlogic.com/2018/04/17/comparing-big-data-messaging.html)

  * [Data Democratisation Hotstar](https://blog.hotstar.com/data-democratisation-hotstar-93ebfb1e688d)

  * [The Hotstar Sports Bar](https://blog.hotstar.com/the-hotstar-sports-bar-a7fcfd870a12)

  * [Giphy Cricket GIF](https://media.giphy.com/media/3oeSASdA0hqt14lAnC/giphy.gif)

104

12

5

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![SWE's avatar](https://substackcdn.com/image/fetch/$s_!drWN!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8b84c39b-8414-443e-8ed1-fcea47e3eef7_720x720.webp)](https://substack.com/profile/136563906-swe?utm_source=comment)

[SWE](https://substack.com/profile/136563906-swe?utm_source=substack-feed-item)

[Feb 11, 2024](https://newsletter.systemdesign.one/p/hotstar-architecture/comment/49321221 "Feb 11, 2024, 4:48 PM")Edited

Liked by Neo Kim

Ming blowing how scalable Golang routines and Erlang mnesia can be. 

Great read and thanks for recommending my newsletter

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/hotstar-architecture/comment/49321221)

[![Duke's avatar](https://substackcdn.com/image/fetch/$s_!F1W-!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3a6b7f0f-3175-4a41-a44a-f270db3255ef_96x96.jpeg)](https://substack.com/profile/109338847-duke?utm_source=comment)

[Duke](https://substack.com/profile/109338847-duke?utm_source=substack-feed-item)

[Feb 25, 2024](https://newsletter.systemdesign.one/p/hotstar-architecture/comment/50302477 "Feb 25, 2024, 4:59 AM")

Liked by Neo Kim

"They don’t do caching because emojis must be shown in real-time."

Sorry for the noob question, but isn't the one of the point of caching is to support lower latency requests?

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/hotstar-architecture/comment/50302477)

[10 more comments...](https://newsletter.systemdesign.one/p/hotstar-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture