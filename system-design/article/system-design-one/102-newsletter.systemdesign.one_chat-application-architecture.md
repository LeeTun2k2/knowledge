[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Wechat Architecture That Powers 1.67 Billion Monthly Users

### #17: Read Now - Architecture Overview Chat Application (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 24, 2023

28

4

1

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines WeChat's architecture. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/chat-application-architecture/?action=share) & I'll send you some rewards for the referrals._

WeChat is the most popular messaging app in China with 1.67 billion monthly active users.

They went from 491 concurrent users in 2011 to 400+ million concurrent users.

Their development principle was simple: release first and then optimize. And it allowed them to validate the product idea before scaling it.

## Chat Application Architecture

[![Message Workflow; Chat Application Architecture](https://substackcdn.com/image/fetch/$s_!C8Mc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F30586666-088c-43dc-9e27-4b8f8c15e642_2504x487.png)](https://substackcdn.com/image/fetch/$s_!C8Mc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F30586666-088c-43dc-9e27-4b8f8c15e642_2504x487.png)Messaging Workflow

When a message gets sent, the WeChat server stores it because the receiver _might_ be offline. And a notification gets delivered to the receiver when it's back online again.

The receiver then fetches the message from the server.

[![Message queue; Chat Application Architecture](https://substackcdn.com/image/fetch/$s_!PVZQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb71a9a32-84f6-40a8-9576-8d0bd619a0f3_1347x977.png)](https://substackcdn.com/image/fetch/$s_!PVZQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb71a9a32-84f6-40a8-9576-8d0bd619a0f3_1347x977.png)Message Queue Delivering Messages

They used an asynchronous message queue to deliver chat messages to the receiver. Because processing time is higher in some use cases.

For example, message delivery in group chat is a time-consuming operation.

Also they restricted the number of people in a group chat to 500. And tracked each person’s last read message index to deliver unread messages.

WeChat servers run C++. And services communicate with each other via Svrkit: an RPC framework.

They built WeChat with _N-layer Architecture_ : Access, Logic, and Storage. 

**N-layer architecture** partitions application logic into different logical layers and offers scalability.

[![WeChat Architecture; Chat Application Architecture](https://substackcdn.com/image/fetch/$s_!efV2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4957c66a-d9fa-4528-b431-76d5f5aef401_1365x1214.png)](https://substackcdn.com/image/fetch/$s_!efV2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4957c66a-d9fa-4528-b431-76d5f5aef401_1365x1214.png)WeChat N-layer Architecture

#### Access Layer

It handled network calls: client-initiated requests and server-initiated pushes.

I don’t know the network protocol that WeChat uses but WebSocket is a good choice. Because it offers bi-directional communication between the client and the server.

#### Logic Layer

It contains business logic and provides an abstract interface to the client.

They broke the business logic into separate modules based on functionality and importance. And kept the modules independently deployable.

#### Storage Layer

It provided data access and included databases: MySQL and SDB.

SDB is a simple string key-value database.

Besides they used Memcached to improve data access efficiency and reduce database calls.

They stored the last-read message index and contacts of a person in SDB. Because it's highly performant and reliable.

And ran SDB in the _leader-follower_ (asynchronous) replication topology. SDB followers handled reads when the leader crashed.

They stored account data and chat messages in MySQL. And ran MySQL in _multi-leader_ topology to scale writes.

They routed reads to MySQL followers. But reads that needed strong consistency got routed to MySQL leader.

Yet replication lag and the single point of failure remained a problem with this setup.

So they installed the _KVSvr algorithm_ on top of MySQL and SDB.

**KVSvr** is a distributed algorithm based on the Quorum protocol. It gives a strong consistency guarantee, asynchronous data replication, and high write performance. Also it cached data for improved read performance.

### Data Synchronization

A person’s messages, contacts, and account data get stored on the server. And the client needs to synchronize it.

So they created a snapshot of the data on the server and sent it to the client. The snapshot consisted of key-value pairs.

### Multi Data Center

Their initial data center was in Shanghai and they wanted to grow further. So they installed extra data centers in Hong Kong and Canada.

They wanted each data center to be self-independent. So they deployed every service in each data center. It allowed them to route traffic to a healthy data center if one of them failed.

Yet data consistency between data centers remained a problem. Because the latency between data centers was high.

Also they needed to avoid business logic problems due to eventual consistency.

So they segmented users. The traffic from China got routed to the Shanghai data center. While international users got routed to data centers outside China.

And data got replicated asynchronously between data centers. This setup reduced complexity and prevented consistency problems.

[![Sync Multi Data Center; Chat Application Architecture](https://substackcdn.com/image/fetch/$s_!RjVC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3fffb7f1-77c8-4b36-8604-625a005594cb_3407x2252.png)](https://substackcdn.com/image/fetch/$s_!RjVC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3fffb7f1-77c8-4b36-8604-625a005594cb_3407x2252.png)Synchronization between Data Centers

They used a Quorum-based queue to synchronize data between data centers for reliability.

And coordinated operations across data centers for special cases such as global unique account ID creation.

[![Replication; Chat Application Architecture](https://substackcdn.com/image/fetch/$s_!9o2f!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F839e34fc-d630-48ea-9c08-96a70e5e6099_2821x1788.png)](https://substackcdn.com/image/fetch/$s_!9o2f!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F839e34fc-d630-48ea-9c08-96a70e5e6099_2821x1788.png)Data Replication across Data Centers

They decided to use the eventual consistency model for group chat because it met their needs.

* * *

WeChat was extended to support voice messages, games, and mobile payment. And it has become China's app for everything.

There are still many open questions about WeChat architecture. But I couldn't find extra information about it. So please share in the comments if you find anything helpful.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!8hRX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1221b8f2-505c-484e-8adc-83ea002c18cd_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

A big thank you to everybody who supports this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/chat-application-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How LinkedIn Scaled to 930 Million Users](https://substackcdn.com/image/fetch/$s_!bR1z!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a003379-beed-429c-b36d-0b13f6fdd344_1280x720.png)How LinkedIn Scaled to 930 Million Users[NK](https://substack.com/profile/135589200-nk)·October 17, 2023[Read full story](https://newsletter.systemdesign.one/p/scalable-software-architecture)](https://newsletter.systemdesign.one/p/scalable-software-architecture)

[![How Shopify Handles Flash Sales at 32 Million Requests per Minute](https://substackcdn.com/image/fetch/$s_!z8uP!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feaa2de74-874a-4ba9-bea7-1f86709d567b_1280x720.png)How Shopify Handles Flash Sales at 32 Million Requests per Minute[NK](https://substack.com/profile/135589200-nk)·October 19, 2023[Read full story](https://newsletter.systemdesign.one/p/shopify-flash-sale)](https://newsletter.systemdesign.one/p/shopify-flash-sale)

* * *

## References

  * https://www.infoq.cn/article/the-road-of-the-growth-weixin-background

  * https://github.com/radareorg/sdb

  * https://en.wikipedia.org/wiki/WeChat

28

4

1

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Omar Al Raisi's avatar](https://substackcdn.com/image/fetch/$s_!1eab!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3ef806d2-0909-42f5-b1f5-4ff9e2975d56_144x144.png)](https://substack.com/profile/146231475-omar-al-raisi?utm_source=comment)

[Omar Al Raisi](https://substack.com/profile/146231475-omar-al-raisi?utm_source=substack-feed-item)

[Oct 24, 2023](https://newsletter.systemdesign.one/p/chat-application-architecture/comment/42401198 "Oct 24, 2023, 2:26 PM")

Liked by Neo Kim

Awesome work

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/chat-application-architecture/comment/42401198)

[![Toan Tran's avatar](https://substackcdn.com/image/fetch/$s_!5Rvt!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe961183f-8102-4694-890f-069e4da0ade8_144x144.png)](https://substack.com/profile/98290977-toan-tran?utm_source=comment)

[Toan Tran](https://substack.com/profile/98290977-toan-tran?utm_source=substack-feed-item)

[Nov 29, 2023](https://newsletter.systemdesign.one/p/chat-application-architecture/comment/44435537 "Nov 29, 2023, 2:01 AM")Edited

Liked by Neo Kim

Thank you for the article.

I would like to clarify information here

> When a message gets sent, the WeChat server stores it because the receiver might be offline. And a notification gets delivered to the receiver when it's back online again.

From WeChat document, it seems like they will only store your message in case the receiver is offline. They store messages in a short amount of time 

[https://help.wechat.com/cgi-bin/micromsg-bin/oshelpcenter?t=help_center/topic_detail&opcode=2&plat=2&lang=en&id=160317aebr7v160317e6jj22&Channel=helpcenter](https://help.wechat.com/cgi-bin/micromsg-bin/oshelpcenter?t=help_center/topic_detail&opcode=2&plat=2&lang=en&id=160317aebr7v160317e6jj22&Channel=helpcenter)

> We do not permanently retain the content of any messages on our servers whether they are text, audio or rich media files such as photos, videos, or documents, unless you or your recipient saves them as a Favorite. Once 72 hours has lapsed since you sent your chat message, or 120 hours for images, audio, videos, and files, WeChat permanently deletes the content of the message on our servers. Upon deletion, neither WeChat nor any and third party will be able to view the content of your message.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/chat-application-architecture/comment/44435537)

[2 more comments...](https://newsletter.systemdesign.one/p/chat-application-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture