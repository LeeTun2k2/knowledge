[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Slack Architecture That Powers Billions of Messages a Day

### #18: Read Now - Awesome Slack Architecture (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 26, 2023

38

1

1

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines Slack's architecture. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/messaging-architecture/?action=share) & I'll send you some rewards for the referrals._

Slack is a real-time messaging app.

They built Slack with 2 API types: web and real-time.

The web API handles user sessions over HTTP.

While the real-time API handles chat messages, typing indicators, and presence status. The real-time API uses WebSockets for bidirectional communication.

[![Messaging Architecture; WebSockets](https://substackcdn.com/image/fetch/$s_!S3nT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F23b2742c-8390-4c5e-86ff-cf9353e8040f_1068x911.png)](https://substackcdn.com/image/fetch/$s_!S3nT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F23b2742c-8390-4c5e-86ff-cf9353e8040f_1068x911.png)Real-Time API with WebSockets

I think HTTP/2 combined with Server-Sent Events is another option for bidirectional communication. Because HTTP/2 offers multiplexing by reusing the same TCP connection.

Also Slack API gets paginated to reduce latency and bandwidth usage. And they do it using cursor-based pagination _._

**Cursor-based pagination** works by maintaining a pointer to a specific item in an ordered dataset. And client requests include the pointer to get only items after that.

Slack runs on the LAMP (Linux-Apache-MySQL-PHP) stack.

[![Messaging Architecture; Slack Tech Stack](https://substackcdn.com/image/fetch/$s_!_U7E!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F616ea989-438f-45dd-9586-341eeda32a53_1231x426.png)](https://substackcdn.com/image/fetch/$s_!_U7E!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F616ea989-438f-45dd-9586-341eeda32a53_1231x426.png)Slack Tech Stack

They used MySQL database to store chat messages because:

  * It’s a proven technology

  * It offers mature tooling support

  * There are many experienced engineers available in the SQL domain

  * The relational data model is a great discipline

But MySQL by default favors strong consistency. So they set up _MySQL as an eventually consistent database_ for high availability.

Also Slack desktop client got built with ElectronJS and ReactJS.

## Messaging Architecture

A messaging app needs 3 things to work:

  * Validity: every user gets the published message

  * Integrity: a chat message doesn’t get delivered to a user more than once

  * Total order: chat messages get delivered in the same order for every user

This is similar to atomic broadcast in distributed systems and it’s impossible to implement.

So they _loosened some constraints_ to create Slack. They did it by relaxing the end-to-end property of the system based on usage patterns.

They built Slack with a client-server architecture.

[![Messaging Architecture; Slack System Design](https://substackcdn.com/image/fetch/$s_!q80L!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1ba1a33f-188e-4c1e-8824-c76fc7963b96_1707x1131.png)](https://substackcdn.com/image/fetch/$s_!q80L!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1ba1a33f-188e-4c1e-8824-c76fc7963b96_1707x1131.png)Slack Architecture

The chat server is a PHP monolith that does CRUD operations on the chat database.

The gateway server is a stateful in-memory service. It pushes chat messages to the client over WebSockets.

Also _[consistent hashing](https://newsletter.systemdesign.one/p/what-is-consistent-hashing)_ maps Slack channels to gateway servers.

Vitess shards MySQL with channel-id as the shard key to reduce conflicts.

**Vitess** is a topology management service for MySQL and helps to scale out easily.

They do service registry using Consul. A **service registry** lets services find each other and communicate.

The job queue defers non-critical tasks like indexing chat messages in search. They created their own job queue without using a third-party solution like Kafka. Because they wanted to meet their needs with little operational complexity.

They do SSL termination with Envoy Edge proxy. Besides it provides hot restarts to achieve high availability.

A hot restart works by avoiding client connection drops on code change deployments.

Yet Slack’s initial payload size increased as the number of users grew. And resulted in high latency. So they created a new service (**snapshot service**) to get low latency and high performance. It’s an application-level edge query engine backed by a cache server.

The snapshot service provided just-in-time annotation. It does it by predicting data objects that might get queried next by the client. And pushes the data objects proactively to prevent an extra network call.

Besides mobile users reply to a chat message using the web API. It allowed mobile users to avoid the extra work of creating a WebSocket connection.

They _salted_ the chat messages to prevent the same message shown more than once. The **salt** is a unique but random token.

Also they installed load balancers between different system components. And stored the media files shared in chat messages in AWS S3. The frequently accessed media assets get cached in CDN to reduce latency.

They added logic to fetch the newer chat messages from the server using the last-seen timestamp of the user.

The _logical clock_ (vector clock) preserves the ordering of chat messages. 

The logical clock finds the causal relationships between events in a distributed system. And does it by including a counter that gets incremented on every chat message.

Besides Thrift data serialization format gave them high performance.

### Slack App Workflow

[![Messaging Architecture; Slack App Workflow](https://substackcdn.com/image/fetch/$s_!tmVE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1bf79d8c-3f91-49bc-8666-1565553084f5_3826x1291.png)](https://substackcdn.com/image/fetch/$s_!tmVE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1bf79d8c-3f91-49bc-8666-1565553084f5_3826x1291.png)Messaging Workflow

I'll summarize the Slack messaging workflow. The client delivers a message to the chat server. The chat server then requests the job queue to index the message in search. The message then gets routed to the gateway server using [consistent hashing](https://newsletter.systemdesign.one/p/what-is-consistent-hashing).

Also the gateway server keeps an on-disk buffer of uncommitted sends. Because it helps to recover from crashes and keeps the core Slack always operational.

* * *

Slack grew to support billions of messages a day. And run 5 Million simultaneous sessions at peak.

My _takeaways_ from this case study are:

  * Optimality changes with growth and it’s important to find the end-to-end part of the problem

  * Complexity isn’t bad if it solves a problem

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you to everybody who supports this newsletter. Consider sharing this post with your friends and get rewards.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/messaging-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Shopify Handles Flash Sales at 32 Million Requests per Minute](https://substackcdn.com/image/fetch/$s_!z8uP!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feaa2de74-874a-4ba9-bea7-1f86709d567b_1280x720.png)How Shopify Handles Flash Sales at 32 Million Requests per Minute[NK](https://substack.com/profile/135589200-nk)·October 19, 2023[Read full story](https://newsletter.systemdesign.one/p/shopify-flash-sale)](https://newsletter.systemdesign.one/p/shopify-flash-sale)

[![How LinkedIn Scaled to 930 Million Users](https://substackcdn.com/image/fetch/$s_!bR1z!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a003379-beed-429c-b36d-0b13f6fdd344_1280x720.png)How LinkedIn Scaled to 930 Million Users[NK](https://substack.com/profile/135589200-nk)·October 17, 2023[Read full story](https://newsletter.systemdesign.one/p/scalable-software-architecture)](https://newsletter.systemdesign.one/p/scalable-software-architecture)

* * *

## References

  * https://gotoams.nl/2018/sessions/440/scaling-slack

  * https://systemdesign.one/slack-architecture/

  * https://slack.engineering/real-time-messaging/

  * Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/slack-text-bmmcfZqSjBU?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

38

1

1

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Jignesh Patil's avatar](https://substackcdn.com/image/fetch/$s_!FZLI!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F631cead5-f419-4ce2-ba55-e4f26bd7bd0f_1284x1288.jpeg)](https://substack.com/profile/30257168-jignesh-patil?utm_source=comment)

[Jignesh Patil](https://substack.com/profile/30257168-jignesh-patil?utm_source=substack-feed-item)

[Oct 31, 2023](https://newsletter.systemdesign.one/p/messaging-architecture/comment/42836907 "Oct 31, 2023, 8:22 PM")

Liked by Neo Kim

Salting messages, great. It will be great to know the relationship between messages of users and admin privilege. 

ReplyShare

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture