[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How LinkedIn Scaled to 930 Million Users

### #15: Software Architecture Evolution at LinkedIn (3 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 17, 2023

62

9

5

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

This post outlines how LinkedIn evolved its software architecture over time. I will walk you through their architectural evolution in 11 stages. Each stage shows how they changed the architecture to meet increasing scalability needs.

_If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/scalable-software-architecture/?action=share) & I'll send you some rewards for the referrals._

## Scalable Software Architecture

LinkedIn started with 2,700 users in 2003 and now has around 930 million users. Here is the chronological order of their evolutionary stages:

### 1\. The Monolith

They installed a single monolith application and a few databases to do all the work.

[![Scalable Software Architecture; Monolith](https://substackcdn.com/image/fetch/$s_!AxnR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffe4d79a4-691e-4912-9455-06543c526011_621x1022.png)](https://substackcdn.com/image/fetch/$s_!AxnR!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffe4d79a4-691e-4912-9455-06543c526011_621x1022.png)

### 2\. Graph Service

As number of users grew, they wanted to manage connections between users in an efficient way.

[![Scalable Software Architecture; Graph service](https://substackcdn.com/image/fetch/$s_!JNxm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd7e2964-5863-4278-8ca9-e5dd0d2bb4d9_1368x964.png)](https://substackcdn.com/image/fetch/$s_!JNxm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd7e2964-5863-4278-8ca9-e5dd0d2bb4d9_1368x964.png)

So they created a separate _in-memory_ graph service and communicated with it via RPC.

### 3\. Scaling Database

The database became a performance bottleneck. As a workaround, they scaled it vertically by adding CPU and memory. But soon it hit limitations and became expensive.

[![Scalable Software Architecture; Replica database](https://substackcdn.com/image/fetch/$s_!jHje!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9165aacf-2517-4389-b76e-1c7e9058e465_1042x552.png)](https://substackcdn.com/image/fetch/$s_!jHje!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9165aacf-2517-4389-b76e-1c7e9058e465_1042x552.png)

So they installed (replica) follower databases to scale further. It served reads.

But this solution worked only for the medium term because it didn’t scale the writes. So they _partitioned_ the database to scale writes.

### 4\. Service-Oriented Architecture

They needed high availability but it was hard for the app server to keep up with high traffic.

So they broke the monolith into many small _stateless services_ :

  * Frontend service - presentation logic

  * Mid-tier service - API access to data models

  * Backend data service - access to the database

[![Scalable Software Architecture; Service oriented architecture](https://substackcdn.com/image/fetch/$s_!H9RV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fecb91d3c-4d3c-4d35-8d9c-a6362a4de8f4_627x1563.png)](https://substackcdn.com/image/fetch/$s_!H9RV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fecb91d3c-4d3c-4d35-8d9c-a6362a4de8f4_627x1563.png)

They kept the services stateless because it is easy to scale out by replication. Besides they did load testing and performance monitoring.

LinkedIn had 750 services in 2015.

### 5\. Caching

Caching was the right choice for them to meet scalability needs with hypergrowth.

[![Scalable Software Architecture; Caching](https://substackcdn.com/image/fetch/$s_!pUj2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7433a645-87db-4a88-8506-8801d5bf6b47_780x464.png)](https://substackcdn.com/image/fetch/$s_!pUj2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7433a645-87db-4a88-8506-8801d5bf6b47_780x464.png)

Also they relied on CDN and browser cache.

Besides they stored precomputed results in the database.

### 6\. Birth of Kafka

They needed data to flow into the data warehouse and Hadoop for analytics. So they created Kafka to stream and queue data.

[![Scalable Software Architecture; Kafka](https://substackcdn.com/image/fetch/$s_!lthy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6fbc7d90-ddc7-4551-b873-25f375839c0c_1396x1227.png)](https://substackcdn.com/image/fetch/$s_!lthy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6fbc7d90-ddc7-4551-b873-25f375839c0c_1396x1227.png)

Kafka is a distributed pub-sub messaging platform.

### 7\. Scaling Engineering Teams

They put the entire focus on improving tooling, deployment, infrastructure, and developer productivity. And _paused_ feature development.

[![Scalable Software Architecture; Scaling engineering teams](https://substackcdn.com/image/fetch/$s_!6Umi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0abae63b-a457-4ce7-b2b0-34008d1b64b7_800x500.png)](https://substackcdn.com/image/fetch/$s_!6Umi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0abae63b-a457-4ce7-b2b0-34008d1b64b7_800x500.png)

It improved their engineering agility to build scalable products.

### 8\. Birth of Rest.li Framework

They wanted to decouple services, so they switched to RESTful API and sent JSON over HTTP.

[![Scalable Software Architecture; Rest.li framework](https://substackcdn.com/image/fetch/$s_!ibwT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc82b918e-2c5d-45eb-b057-0b2a89d0d79c_523x1023.webp)](https://substackcdn.com/image/fetch/$s_!ibwT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc82b918e-2c5d-45eb-b057-0b2a89d0d79c_523x1023.webp)

And built the Rest.li web framework to abstract many parts of data exchange.

### 9\. Super Blocks

Service-oriented architecture caused many downstream calls and it became a problem. So they grouped the backend services to create a single access API.

### 10\. Multi Data Center

They needed high availability and wanted to avoid single points of failure.

[![Scalable Software Architecture; Multi data center](https://substackcdn.com/image/fetch/$s_!CLz5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F222ce539-13b9-42c3-b7b8-617c18569e6d_800x500.gif)](https://substackcdn.com/image/fetch/$s_!CLz5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F222ce539-13b9-42c3-b7b8-617c18569e6d_800x500.gif)

So they replicated data across many data centers. And redirected user requests to nearby data centers.

### 11\. Ditch JSON

JSON data serialization became a performance bottleneck. So they moved from JSON to Protobuf to reduce latency.

* * *

LinkedIn followed the idea: keep it simple. And changed their architecture based on needs. They remain the biggest network for Professionals in 2023.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/scalable-software-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Giphy Delivers 10 Billion GIFs a Day to 1 Billion Users](https://substackcdn.com/image/fetch/$s_!QOCr!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9ed955a6-ad0b-423f-90c3-c8f5a2fb1ff1_1280x720.gif)How Giphy Delivers 10 Billion GIFs a Day to 1 Billion Users[NK](https://substack.com/profile/135589200-nk)·October 12, 2023[Read full story](https://newsletter.systemdesign.one/p/cdn-explained)](https://newsletter.systemdesign.one/p/cdn-explained)

[![7 Simple Ways to Fail System Design Interview](https://substackcdn.com/image/fetch/$s_!-Sxa!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F541c2154-853f-4a25-b513-7cbe54c4fb47_1280x720.png)7 Simple Ways to Fail System Design Interview[NK](https://substack.com/profile/135589200-nk)·October 10, 2023[Read full story](https://newsletter.systemdesign.one/p/design-system-newsletter)](https://newsletter.systemdesign.one/p/design-system-newsletter)

* * *

## References

  * https://engineering.linkedin.com/architecture/brief-history-scaling-linkedin

  * <https://newsletter.systemdesign.one/p/protocol-buffers-vs-json>

62

9

5

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Akram EL Basri's avatar](https://substackcdn.com/image/fetch/$s_!xT5G!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fec37dae9-7525-46c1-9ca4-9fa9bd561616_450x450.png)](https://substack.com/profile/208507929-akram-el-basri?utm_source=comment)

[Akram EL Basri](https://substack.com/profile/208507929-akram-el-basri?utm_source=substack-feed-item)

[Mar 12, 2024](https://newsletter.systemdesign.one/p/scalable-software-architecture/comment/51518222 "Mar 12, 2024, 8:18 PM")

Liked by Neo Kim

I adore your articles man !

ReplyShare

[![Anton Zaides's avatar](https://substackcdn.com/image/fetch/$s_!m9AG!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe37a1acd-c9a1-4968-b60d-907005004d84_1728x1728.jpeg)](https://substack.com/profile/121956618-anton-zaides?utm_source=comment)

[Anton Zaides](https://substack.com/profile/121956618-anton-zaides?utm_source=substack-feed-item)

[Oct 24, 2023](https://newsletter.systemdesign.one/p/scalable-software-architecture/comment/42388777 "Oct 24, 2023, 9:53 AM")

Liked by Neo Kim

Great article :)

I would have loved to see the whole architecture in each step, to see how it 'evolves' (and looks at the end). 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/scalable-software-architecture/comment/42388777)

[7 more comments...](https://newsletter.systemdesign.one/p/scalable-software-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture