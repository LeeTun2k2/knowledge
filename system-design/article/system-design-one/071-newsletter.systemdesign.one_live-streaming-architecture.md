[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Facebook Scaled Live Video to a Billion Users

### #47: Break Into Live Streaming Architecture (6 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 24, 2024

92

1

9

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines Facebook’s live video architecture. If you want to learn more, find the references at the bottom of the page._

  * _[Share this post](https://newsletter.systemdesign.one/p/live-streaming-architecture/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

July 2018 - Thailand.

A group of boys and their coach had a football training session.

And they decide to do an outing for team building.

[![Live Streaming Architecture](https://substackcdn.com/image/fetch/$s_!MPvH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff6353623-dae0-4363-a820-6656fa883aea_800x500.gif)](https://substackcdn.com/image/fetch/$s_!MPvH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff6353623-dae0-4363-a820-6656fa883aea_800x500.gif)

So they visit the Tham Luang cave.

But heavy rains flooded the cave and trapped them inside.

And rescue is difficult due to narrow cave passages.

18 days passed…

A rescue effort with an international team started.

While they streamed it via Facebook live video.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

#### [TheFutureParty (Featured)](https://news.futureparty.com/subscribe?utm_source=systemdesign&utm_medium=crosspromos&utm_campaign=newsletter)

Understand the future of business, entertainment, and culture, all in a 5-minute daily email.

[Join TheFutureParty](https://news.futureparty.com/subscribe?utm_source=systemdesign&utm_medium=crosspromos&utm_campaign=newsletter)

* * *

A Facebook live video is a one-to-many stream. That means everyone with access to live video can watch it.

The broadcaster sends live video to the Facebook infrastructure. Then they process it and forward it to viewers.

[![Facebook Live Video High-Level Architecture](https://substackcdn.com/image/fetch/$s_!dlLG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F28cdec9d-99aa-4a56-85ec-a826c0cdacd2_1048x533.png)](https://substackcdn.com/image/fetch/$s_!dlLG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F28cdec9d-99aa-4a56-85ec-a826c0cdacd2_1048x533.png)Facebook Live Video High-Level Architecture

They use real-time messaging protocol (**[RTMP](https://en.wikipedia.org/w/index.php?title=Real-Time_Messaging_Protocol)**) to send live video from the broadcaster to the Facebook infrastructure. It’s based on TCP and splits the live video into separate audio and video streams.

They run the live stream server to transcode live video into different bit rates.

**Transcoding** is the process of converting a video to different formats. It’s necessary because each client device supports a different format. While the **bit rate** represents the data in each second of the video.

Then they create 1-second video segments in [MPEG-DASH](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP) format and send those to viewers.

**DASH** is a streaming protocol over HTTP. It consists of a manifest file and media files. Imagine the manifest file as a collection of pointers to media files. They update the manifest file whenever new video segments are created. Hence the viewer can request the media files in the correct order.

[![Live Video Traffic](https://substackcdn.com/image/fetch/$s_!65cq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd397fb28-dd05-4130-b4d1-ab3048c82d7a_968x531.png)](https://substackcdn.com/image/fetch/$s_!65cq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd397fb28-dd05-4130-b4d1-ab3048c82d7a_968x531.png)Live Video Traffic

This rescue operation became extremely popular on Facebook.

And 7.1 million people across the world started watching it concurrently.

So it created a traffic spike.

* * *

## Live Streaming Architecture

Scaling live video is a hard problem.

Here’s how Facebook solves it:

### 1\. Scalability

There’s a risk of the live stream server failing while processing the video.

[![Selecting Live Stream Server Using Consistent Hashing](https://substackcdn.com/image/fetch/$s_!NABk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F11ec40fd-09a6-47c3-bac5-9c171eee9c03_1245x696.png)](https://substackcdn.com/image/fetch/$s_!NABk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F11ec40fd-09a6-47c3-bac5-9c171eee9c03_1245x696.png)Selecting Live Stream Server Using Consistent Hashing

So they run many live stream servers and use **[consistent hashing](https://systemdesign.one/consistent-hashing-explained/)** to select one. It automatically selects the next server if the active server fails.

Also they use stream ID as the consistent hash key. Hence broadcaster could reconnect to the same server if their network connection drops.

### 2\. Network Bandwidth

Viewers may have different network bandwidths as people use mobile internet.

[![Converting Live Video Into Different Resolutions for Adaptive Bitrate Streaming](https://substackcdn.com/image/fetch/$s_!Pu0P!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F655d3e06-5dc3-4add-8386-f00abcdc5dff_800x500.png)](https://substackcdn.com/image/fetch/$s_!Pu0P!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F655d3e06-5dc3-4add-8386-f00abcdc5dff_800x500.png)Converting Live Video Into Different Resolutions for Adaptive Bitrate Streaming

So they do **adaptive bitrate** streaming. This means a live video gets broken down into smaller parts. And each part gets converted into different resolutions.

Simply put, the video quality changes based on network bandwidth.

Besides they measure the upload bandwidth of the broadcaster. And then change the video resolution to match the bandwidth.

### 3\. Latency

Viewers from another continent may experience latency due to their distance from the live stream server.

[![Origin Server vs Edge Server](https://substackcdn.com/image/fetch/$s_!rl9W!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F025542ac-f2cc-4b47-b5c1-c3080899675f_800x500.gif)](https://substackcdn.com/image/fetch/$s_!rl9W!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F025542ac-f2cc-4b47-b5c1-c3080899675f_800x500.gif)Origin Server vs Edge Server

So they set up extra servers in a 2-level hierarchy.

They installed edge servers closer to the viewers. It stores the live video segments for low latency.

And installed origin servers to reduce the live stream server load.

[![Server Hierarchy in Facebook Live Video](https://substackcdn.com/image/fetch/$s_!-DRE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7925cc97-9569-47c5-a62f-986fe5feb784_800x500.png)](https://substackcdn.com/image/fetch/$s_!-DRE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7925cc97-9569-47c5-a62f-986fe5feb784_800x500.png)Server Hierarchy in Facebook Live Video

Imagine each continent has an origin server, while each country has many edge servers.

Put another way, there’s a one-to-many relationship between an origin server and edge servers. So many edge servers send requests to the same origin server.

While there’s a one-to-many relationship between a live server and origin servers. Hence many origin servers send requests to the same live server.

[![Request Workflow](https://substackcdn.com/image/fetch/$s_!x2fK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F38395c0a-3d04-47ce-bd64-74fba1102389_1441x286.png)](https://substackcdn.com/image/fetch/$s_!x2fK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F38395c0a-3d04-47ce-bd64-74fba1102389_1441x286.png)Request Workflow

So when a viewer requests a video segment, they serve it from the edge server if it got cached. Otherwise the request gets forwarded to the origin server and live stream server.

### 4\. Thundering Herd

An edge server can handle 200k requests per second. Thus reducing the traffic reaching the origin server.

But if many people watch a live video concurrently, there’s a risk of the [thundering herd problem](https://en.wikipedia.org/wiki/Thundering_herd_problem) in the edge server.

[![Edge Server Internal Architecture](https://substackcdn.com/image/fetch/$s_!L9S3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcaaded11-88c0-4d9c-8774-b90f27222faf_1305x512.png)](https://substackcdn.com/image/fetch/$s_!L9S3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcaaded11-88c0-4d9c-8774-b90f27222faf_1305x512.png)Edge Server Internal Architecture

So they built the edge server with an HTTP proxy and a **caching layer**.

This means the viewer first checks whether a specific video segment is in the edge server cache.

The HTTP proxy then requests the origin server for that video segment only if it’s not in the cache.

Also they store video segments in different cache servers within an edge server to load balance the traffic.

Besides they built the origin server with the same architecture. And it forwards the request to the live stream server on a cache miss.

Then the video segment gets cached in each server to handle future requests.

[![Thundering Herd Problem on Edge Server](https://substackcdn.com/image/fetch/$s_!ijih!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F097c888b-f25f-487f-a3ea-760a47d75d13_1335x339.png)](https://substackcdn.com/image/fetch/$s_!ijih!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F097c888b-f25f-487f-a3ea-760a47d75d13_1335x339.png)Thundering Herd Problem on Edge Server

Yet statistically 2 out of 100 requests will reach the origin server.

At Facebook’s scale, this will be a big problem.

[![Request Coalescing](https://substackcdn.com/image/fetch/$s_!fMK1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe77ad894-6e49-40e3-a744-f2abf542c87a_1335x330.png)](https://substackcdn.com/image/fetch/$s_!fMK1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe77ad894-6e49-40e3-a744-f2abf542c87a_1335x330.png)Request Coalescing

So they do **request coalescing**.

That means all concurrent requests for a specific video segment get stored in a queue. While only a single request gets forwarded to the server.

Then all the requests in the queue get that video segment at once after the server responds. Thus reducing the server load and preventing the thundering herd problem.

Also many edge servers may concurrently request a specific video segment from an origin server. So they do request coalescing at the origin server to reduce the live stream server load.

Besides they replicate the video segments across edge servers for better performance.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

This case study shows that scalability must be built into the design at an early stage.

And Facebook Live remains one of the largest streaming services in the world.

While all 12 boys and their coach were safely rescued from the cave.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/live-streaming-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Razorpay Scaled to Handle Flash Sales at 1500 Requests per Second](https://substackcdn.com/image/fetch/$s_!azzb!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6eac0216-6d38-4af2-b4fc-9a6e4469e4ea_1280x720.gif)How Razorpay Scaled to Handle Flash Sales at 1500 Requests per Second[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 17, 2024[Read full story](https://newsletter.systemdesign.one/p/payment-gateway-architecture)](https://newsletter.systemdesign.one/p/payment-gateway-architecture)

[![How Stripe Prevents Double Payment Using Idempotent API](https://substackcdn.com/image/fetch/$s_!teAD!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe205302f-4dce-42be-be58-a294e5a64e95_1280x720.gif)How Stripe Prevents Double Payment Using Idempotent API[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 9, 2024[Read full story](https://newsletter.systemdesign.one/p/idempotent-api)](https://newsletter.systemdesign.one/p/idempotent-api)

* * *

### References

  * [Scaling Facebook Live](https://atscaleconference.com/videos/scaling-facebook-live/)

  * [Scaling Facebook Live-2](https://atscaleconference.com/videos/scaling-facebook-live-2/)

  * [Scaling Facebook Live Videos to a Billion Users](https://www.youtube.com/watch?v=IO4teCbHvZw)

  * [Sachin Kulkarni Describes the Architecture behind Facebook Live](https://www.infoq.com/podcasts/sachin-kulkarni-facebook-live/)

  * [Under the hood: Broadcasting live video to millions](https://engineering.fb.com/2015/12/03/ios/under-the-hood-broadcasting-live-video-to-millions/)

  * [Live video solutions: Solving the thundering herd problem](https://www.facebook.com/watch/?ref=embed_video&v=10153675295382200)

  * [Adaptive Bitrate Streaming](https://cloudinary.com/glossary/adaptive-bitrate-streaming)

  * [Understanding Request Coalescing](https://support.bunny.net/hc/en-us/articles/6762047083922-Understanding-Request-Coalescing)

  * [Tham Luang cave rescue](https://en.wikipedia.org/wiki/Tham_Luang_cave_rescue)

92

1

9

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Daniel Moka's avatar](https://substackcdn.com/image/fetch/$s_!UYvv!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb31d65c1-d5b9-40e6-8350-f82cbd17233e_400x400.jpeg)](https://substack.com/profile/5505375-daniel-moka?utm_source=comment)

[Daniel Moka](https://substack.com/profile/5505375-daniel-moka?utm_source=substack-feed-item)

[May 24, 2024](https://newsletter.systemdesign.one/p/live-streaming-architecture/comment/57195637 "May 24, 2024, 2:14 PM")

Solid share my friend, loved the visuals as always! Keep these coming!

ReplyShare

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture