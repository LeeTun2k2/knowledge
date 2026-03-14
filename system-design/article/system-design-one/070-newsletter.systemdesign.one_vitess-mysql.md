[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How YouTube Was Able to Support 2.49 Billion Users With MySQL

### #48: Break Into Vitess Architecture (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 31, 2024

340

10

22

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines Vitess architecture. If you want to learn more, find references at the bottom of the page._

  * _[Share this post](https://newsletter.systemdesign.one/p/vitess-mysql/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, 3 people working for PayPal decided to build a dating site.

Yet their business model failed.

So they pivoted to create a video-sharing site and called it YouTube.

They stored video titles, descriptions, and user data in MySQL.

As more users joined, they set up MySQL in leader-follower replication topology to scale.

[![Leader-Follower Replication Topology in Mysql](https://substackcdn.com/image/fetch/$s_!LdU0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F509f6c0a-2301-4d88-a238-00b585bec721_998x651.png)](https://substackcdn.com/image/fetch/$s_!LdU0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F509f6c0a-2301-4d88-a238-00b585bec721_998x651.png)Leader-Follower Replication Topology in MySQL

But replication in MySQL is single-threaded.

So followers couldn’t keep up with fresh data on extreme write operations to the leader.

Yet their growth rate was explosive.

[![Vitess MySQL](https://substackcdn.com/image/fetch/$s_!mR4P!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e1b29ab-d5ed-49c7-9061-a83be2a2b212_800x500.gif)](https://substackcdn.com/image/fetch/$s_!mR4P!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e1b29ab-d5ed-49c7-9061-a83be2a2b212_800x500.gif)

And hit a whopping billion users to become the second most visited site in the world.

So they scaled out by adding a cache and preloaded all the events from the MySQL [binary log](https://dev.mysql.com/doc/refman/8.0/en/binary-log.html). That means the replication becomes memory-bound and faster.

Although it temporarily solved their scalability issue, there were new problems.

Here are some of them:

### 1\. Sharding:

MySQL must be partitioned to handle storage needs.

But transactions and joins become difficult after sharding. 

So application logic should handle it.

This means application logic should find what shards to query.

And that increases the chance of downtime.

### 2\. Performance:

The leader-follower replication topology causes stale data to be read from followers.

So application logic must route the reads to the leader if fresh data is necessary.

And this needs extra logic implementation.

### 3\. Protection:

There’s a risk of some queries taking too long to return data.

Also too many MySQL connections at once can be problematic.

And might take down the database.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

* * *

## Vitess MySQL

They wanted an abstraction layer on top of MySQL for simplicity and scalability.

So they created Vitess.

Here’s how Vitess offers extreme scalability:

### 1\. Interacting with Database:

They installed a [sidecar server](https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar) in front of each MySQL instance and called it **VTTablet**.

[![Vttablet Running as a Sidecar Server](https://substackcdn.com/image/fetch/$s_!zFZ4!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6d914eb6-56cf-4f78-ac74-393870a30631_1229x549.png)](https://substackcdn.com/image/fetch/$s_!zFZ4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6d914eb6-56cf-4f78-ac74-393870a30631_1229x549.png)VTTablet Running as a Sidecar Server

It let them:

  * Control MySQL server and manage database backups

  * Rewrite expensive queries by adding the limit clause

  * Cache frequently accessed data to prevent the [thundering herd problem](https://en.wikipedia.org/wiki/Thundering_herd_problem)

### 2\. Routing SQL Queries:

They set up a stateless proxy server to route the queries and called it **VTGate**.

[![Vtgate Routing Queries to a Specific Shard](https://substackcdn.com/image/fetch/$s_!m39J!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5bd3af3e-3529-4f36-94db-4643e7608243_1365x684.png)](https://substackcdn.com/image/fetch/$s_!m39J!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5bd3af3e-3529-4f36-94db-4643e7608243_1365x684.png)VTGate Routing Queries to the Specific Shard

It let them:

  * Find the correct VTTablet to route a query based on the schema and sharding scheme

  * Keep the number of MySQL connections low via [connection pooling](https://www.prisma.io/dataguide/database-tools/connection-pooling#:~:text=Broadly%20speaking%2C%20connection%20pooling%20refers,an%20external%20tool%20or%20service.)

  * Speak MySQL protocol with the application layer

  * Act like a monolithic MySQL server for simplicity

  * Limit the number of transactions at a time for performance

[![Scaling With Many VTGate Servers](https://substackcdn.com/image/fetch/$s_!IM6f!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7e9cdd8a-e3c4-4c54-8177-cc042ece69b9_1049x851.png)](https://substackcdn.com/image/fetch/$s_!IM6f!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7e9cdd8a-e3c4-4c54-8177-cc042ece69b9_1049x851.png)Scaling With Many VTGate Servers

Besides they run many VTGate servers to scale out.

### 3\. State Information:

They set up a distributed **key-value database** to store information about schemas, sharding schemes, and roles.

[![Key-Value Database Storing Meta Information](https://substackcdn.com/image/fetch/$s_!DGLr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6d9c016-e198-4ae5-8ec2-977fb2b3e3c9_1093x768.png)](https://substackcdn.com/image/fetch/$s_!DGLr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6d9c016-e198-4ae5-8ec2-977fb2b3e3c9_1093x768.png)Key-Value Database Storing Meta Information

Also it takes care of relationships between databases like the leader and followers.

They use [Zookeeper](https://zookeeper.apache.org/) to implement the key-value database.

Besides they cache this data on VTGate for better performance.

[![Updating Key-Value Database](https://substackcdn.com/image/fetch/$s_!xZnc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67013500-29eb-4e98-97d7-a2d1a6257e52_696x307.png)](https://substackcdn.com/image/fetch/$s_!xZnc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67013500-29eb-4e98-97d7-a2d1a6257e52_696x307.png)Updating Key-Value Database

They run an HTTP server to keep the key-value database updated. And called it **VTctld**.

It gets the entire list of servers and their relationships and then updates the key-value database.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### TL;DR:

[![High-Level Architecture of Vitess](https://substackcdn.com/image/fetch/$s_!Su3p!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6622c9d0-cb91-45f2-8851-03d8cef91254_1358x768.png)](https://substackcdn.com/image/fetch/$s_!Su3p!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6622c9d0-cb91-45f2-8851-03d8cef91254_1358x768.png)High-Level Architecture of Vitess

  * VTGate: proxy server to route queries

  * Key-Value Database: configuration server for topology management

  * VTTablet: sidecar server running on each MySQL

* * *

They wrote Vitess in Go and open-sourced it.

Also it supports MariaDB.

While YouTube was able to serve 2.49 billion users with the Vitess MySQL combination.

This case study shows MySQL can easily handle internet-scale traffic.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/vitess-mysql?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Facebook Scaled Live Video to a Billion Users](https://substackcdn.com/image/fetch/$s_!CE-z!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F42f9a1a9-af26-4460-a40c-4811225f32cd_1280x720.gif)How Facebook Scaled Live Video to a Billion Users[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 24, 2024[Read full story](https://newsletter.systemdesign.one/p/live-streaming-architecture)](https://newsletter.systemdesign.one/p/live-streaming-architecture)

[![How Razorpay Scaled to Handle Flash Sales at 1500 Requests per Second](https://substackcdn.com/image/fetch/$s_!azzb!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6eac0216-6d38-4af2-b4fc-9a6e4469e4ea_1280x720.gif)How Razorpay Scaled to Handle Flash Sales at 1500 Requests per Second[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 17, 2024[Read full story](https://newsletter.systemdesign.one/p/payment-gateway-architecture)](https://newsletter.systemdesign.one/p/payment-gateway-architecture)

* * *

### References

  * [Vitess Official Site](https://vitess.io/)

  * [Vitess Architecture Official Docs](https://vitess.io/docs/19.0/overview/architecture/)

  * [Scaling YouTube's Backend: The Vitess Trade-offs - @Scale 2014 - Data](https://www.youtube.com/watch?v=5yDO-tmIoXY)

  * [Vitess: A Distributed Scalable Database Architecture](https://www.youtube.com/watch?v=SXJZuGgXINk)

  * [What Is Vitess?](https://vitess.io/docs/19.0/overview/whatisvitess/)

  * [Vitess on GitHub](https://github.com/vitessio/vitess)

  * [Scalability at YouTube](https://www.youtube.com/watch?v=G-lGCC4KKok)

  * [Vitess Supported Databases](https://vitess.io/docs/19.0/overview/supported-databases/)

  * [Do Mysql slaves run multiple threads to read the Relay log to sync up the Master's operation](https://stackoverflow.com/questions/35256926/do-mysql-slaves-run-multiple-threads-to-read-relay-log-for-sync-up-masters-oper)

  * [11 Reasons Why YouTube Was Able to Support 100 Million Video Views a Day With Only 9 Engineers](https://newsletter.systemdesign.one/p/youtube-scalability)

  * [Most Visited Websites In The World (May 2024)](https://explodingtopics.com/blog/most-visited-websites)

340

10

22

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Junaid Effendi's avatar](https://substackcdn.com/image/fetch/$s_!4vgA!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb06559f3-ee33-46f8-bfa0-50964179f235_1200x1200.png)](https://substack.com/profile/21393641-junaid-effendi?utm_source=comment)

[Junaid Effendi](https://substack.com/profile/21393641-junaid-effendi?utm_source=substack-feed-item)

[May 31, 2024](https://newsletter.systemdesign.one/p/vitess-mysql/comment/57808576 "May 31, 2024, 4:48 PM")

Liked by Neo Kim

I thought they would be using NOSQL just for scaling reason. Interesting.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/vitess-mysql/comment/57808576)

[![Ashwani Yadav's avatar](https://substackcdn.com/image/fetch/$s_!_wBO!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a7eb603-42bd-46c8-b076-ee2c1786dba9_460x460.jpeg)](https://substack.com/profile/202084123-ashwani-yadav?utm_source=comment)

[Ashwani Yadav](https://substack.com/profile/202084123-ashwani-yadav?utm_source=substack-feed-item)

[Jun 1, 2024](https://newsletter.systemdesign.one/p/vitess-mysql/comment/57863289 "Jun 1, 2024, 6:27 AM")

Thanks Neo. Why did they stick to MySQL for all this time? Why didn't they chose NoSQL? 

ReplyShare

[8 more comments...](https://newsletter.systemdesign.one/p/vitess-mysql/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture