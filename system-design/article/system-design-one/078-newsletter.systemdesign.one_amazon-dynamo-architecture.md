[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Amazon Scaled E-commerce Shopping Cart Data Infrastructure

### #41: Break Into Amazon Dynamo White Paper (8 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Mar 26, 2024

109

2

12

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the Amazon Dynamo white paper. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/amazon-dynamo-architecture/?action=share) & I'll send you some rewards for the referrals._

The holiday season, 2004.

Amazon's e-commerce platform is running the Oracle enterprise database. 

[![Scaling a Traditional Database](https://substackcdn.com/image/fetch/$s_!m6DK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb57d0e5-7eab-4232-8dbc-4f7ec39bdb74_1035x702.png)](https://substackcdn.com/image/fetch/$s_!m6DK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb57d0e5-7eab-4232-8dbc-4f7ec39bdb74_1035x702.png)Scaling a Relational Database

They set up clustering and leader-follower replication topology for scalability. Yet the database couldn’t scale well for critical services like the shopping cart. And caused several outages during peak shopping hours with 10+ million requests a day.

They could only scale reads by adding database followers. While they had to vertically scale the database leader to scale writes. That means write operations suffered from a single point of failure.

Yet each customer must be able to view and add items to the shopping cart all the time. Otherwise they will lose money. Also the shopping cart infrastructure should be scalable.

So they did post-mortem debugging. And found that 70% of their operations need only a simple key-value data model. That means a relational database would be inefficient for their use case.

So they modeled the shopping cart as a key-value service. The cart ID is the key while the items in the shopping cart are represented as a list of values.

Also they didn’t want to lose any item added to the shopping cart by customers. In other words, they want extremely high write availability and durability.

[![Amazon Dynamo Architecture](https://substackcdn.com/image/fetch/$s_!daA4!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F04748728-5dc0-4177-aae3-9711cf921c2e_800x500.gif)](https://substackcdn.com/image/fetch/$s_!daA4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F04748728-5dc0-4177-aae3-9711cf921c2e_800x500.gif)

So a group of distributed systems experts designed a horizontally scalable distributed database. They created it to scale for both reads and writes.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Amazon Dynamo Architecture

They didn't create new technologies. Instead combined many distributed systems techniques to solve the problems. Here’s how they did it:

### 1\. Partitioning Data

They partitioned the data to solve the write scalability problem. Because each partition could accept writes in parallel.

[![Partitioning Data for Write Scalability](https://substackcdn.com/image/fetch/$s_!hGlw!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fea05c727-0b40-4579-87db-e830d2a98497_967x721.png)](https://substackcdn.com/image/fetch/$s_!hGlw!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fea05c727-0b40-4579-87db-e830d2a98497_967x721.png)Partitioning Data for Write Scalability

A simple approach is to partition and sort data. Yet it could create a hot shard problem as some shards might receive more traffic than others.

[![Regular Hashing to Partition Data](https://substackcdn.com/image/fetch/$s_!my6z!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a0bd50d-36f2-4008-ba84-75d79bc587fb_1323x722.png)](https://substackcdn.com/image/fetch/$s_!my6z!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a0bd50d-36f2-4008-ba84-75d79bc587fb_1323x722.png)Regular Hashing to Partition Data

An alternative approach is regular hashing. The idea is to hash the data keys and do modulo-operation on the result against the number of servers. Thus finding the server to interact with. And it distributes data more uniformly.

But it won't scale while adding or removing servers. Because data associations will break and cause large data movement.

[![Partitioning Data Using Consistent Hashing](https://substackcdn.com/image/fetch/$s_!vBjU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F032686b0-0fb5-4344-b6bd-5749eaab8d49_915x908.png)](https://substackcdn.com/image/fetch/$s_!vBjU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F032686b0-0fb5-4344-b6bd-5749eaab8d49_915x908.png)Partitioning Data Using Consistent Hashing

So they use consistent hashing to solve this problem. Think of consistent hashing as a distributed systems technique to reduce data movement while adding or removing servers.

**Consistent hashing** treats the output range of the hash function as a ring. That means the largest hash value wraps around to the smallest hash value. And each server gets a random position on the hash ring.

They find the position of a server by hashing the server ID. And doing a modulo-operation on that result against the number of available positions on the hash ring.

While they add a data item to the server by hashing its key. And doing a modulo-operation on that result against the number of available positions on the hash ring. After that, they traverse the ring in a clockwise direction to find the responsible server. 

Put another way, a server is responsible for data that falls in the region between it and its predecessor server on the hash ring.

In the example above, the data key falls between 75 and 100 range. So the server at position 100 stores the data.

Besides they store the server positions in a sorted data structure. So a server could be found in logarithmic time complexity.

[![Data Movement When a Server Is Added in Consistent Hashing](https://substackcdn.com/image/fetch/$s_!O-1p!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F88ff7ac8-0f62-4d8d-aca4-c90232819908_904x908.png)](https://substackcdn.com/image/fetch/$s_!O-1p!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F88ff7ac8-0f62-4d8d-aca4-c90232819908_904x908.png)Data Movement When a Server Is Added in Consistent Hashing

The data movement is low when a new server gets added to the ring. Because they move only the data in the range that falls between the new server and the preceding server. Thus making it easier to scale.

Life was better.

Until one day when they noticed an unbalanced load across the servers.

So they introduced virtual nodes. It’s a variant of consistent hashing.

**Virtual nodes** mean a server position in the hash ring doesn’t map only to a single physical server. Instead it could contain many physical servers. Thus reducing the risk of hot shards and making it easy to include heterogeneous servers.

### 2\. Ensuring Data Durability

But one fine day a server fails and they lose some data.

[![Replicating Data Across Servers for Durability](https://substackcdn.com/image/fetch/$s_!8dVh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4704fb75-1c60-43d5-bda6-accd61358a05_904x908.png)](https://substackcdn.com/image/fetch/$s_!8dVh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4704fb75-1c60-43d5-bda6-accd61358a05_904x908.png)Replicating Data Across Servers for Durability

So they replicate data asynchronously across N number of servers for durability. That means they store a data item in the next server on the ring. And also in the next (N-1) servers. While N is the replication factor.

Yet they need data consistency.

So they use **sloppy quorum**.

[![Achieving Consistency Through Sloppy Quorum](https://substackcdn.com/image/fetch/$s_!JW2i!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff2e0aeb3-ac70-4955-9c7e-b091f06c6b26_742x810.png)](https://substackcdn.com/image/fetch/$s_!JW2i!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff2e0aeb3-ac70-4955-9c7e-b091f06c6b26_742x810.png)Achieving Consistency Through Sloppy Quorum

At least a single server will have the latest data using sloppy quorum.

And they consider the system consistent if it satisfies the equation: R+W > N.

R represents the read quorum. That means the number of servers that must reply to consider a read operation successful.

While W represents the write quorum. That means the number of servers that must reply to consider a write operation successful.

So the set of servers in reads and writes will overlap considering the equation and the system will return the latest data.

Life was good again.

### 3\. Resolving Data Conflicts

Yet one day concurrent writes with sloppy quorum caused conflict on a data item.

They could resolve the conflict by applying the last write.

A simple solution to find the last write is to use the system clock - Network Time Protocol (**NTP**). But clock skew is normal in distributed systems, thus it's unreliable. That means different clocks run at different rates.

[![](https://substackcdn.com/image/fetch/$s_!D8zW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fed0d8b36-ad7c-448d-b1ca-11b705ccab07_1920x1086.png)](https://substackcdn.com/image/fetch/$s_!D8zW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fed0d8b36-ad7c-448d-b1ca-11b705ccab07_1920x1086.png)[Vector Clocks to Find the Causality of Events](https://en.wikipedia.org/wiki/Vector_clock)

So they use vector clocks to find data version history and merge them during reads. In other words, they use vector clocks to find causality between versions. 

The different data versions get automatically merged if there are no conflicts. Otherwise the client logic must resolve the conflicts.

The idea behind **vector clocks** is simple. Each server maintains a vector of integers and the integers start at zero. The server increments its integer whenever an event gets sent.

While the receiver finds the maximum between its integer and the received value. So event A occurred before event B if all integers in A are lesser than those of event B.

Also the whole vector gets sent along with an event. That means vector clocks are passed between the system and the client.

They have solved another problem.

### 4\. Handling Temporary Failures

Imagine their write quorum is 3. That means 3 servers must reply to consider a write operation successful.

Yet one morning, one of those servers was temporarily down.

[![Writing Data Temporarily to the Next Healthy Server Using Hinted Handoff](https://substackcdn.com/image/fetch/$s_!O7Un!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F13e71655-8a9c-4435-95af-df7ac3602249_1017x1025.png)](https://substackcdn.com/image/fetch/$s_!O7Un!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F13e71655-8a9c-4435-95af-df7ac3602249_1017x1025.png)Writing Data Temporarily to the Next Healthy Server Using Hinted Handoff

So they use **hinted handoff**. It stores the data temporarily in the next healthy server. That means instead of the next N servers, it picks the next N healthy servers.

While the temporary server sends the data once the failed server is back online. Thus offering high write availability.

And life was good.

### 5\. Handling Permanent Failures

Until one day when the server storing temporary data from hinted handoff also crashed.

And it happened before sending temporary data to the server that was back online after a failure.

[![Synchronising Data Using the Merkle Tree](https://substackcdn.com/image/fetch/$s_!0GIe!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3ce43437-8f5f-4e37-a8cc-9fa8cbc6d95f_1854x1024.png)](https://substackcdn.com/image/fetch/$s_!0GIe!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3ce43437-8f5f-4e37-a8cc-9fa8cbc6d95f_1854x1024.png)Synchronizing Data Using the Merkle Tree

So they use **Merkle trees** to synchronize data between servers in the background.

Think of it as a data structure that allows finding data differences efficiently.

They find data differences by comparing the hash value of the root in the Merkle trees. After that, they check the children nodes if the parent nodes aren’t the same. Thus in the worst case, they need logarithmic time complexity to transfer data.

Things are looking better.

### 6\. Detecting Server Membership

But one day they add more servers to scale the system.

And it was important to find the available servers in the system. Yet a centralized approach using heartbeats isn’t scalable.

[![Tracking System State Information Using Gossip Protocol](https://substackcdn.com/image/fetch/$s_!LNMB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66316fce-9025-40c2-aaf8-5097bbaa6ecb_1288x656.png)](https://substackcdn.com/image/fetch/$s_!LNMB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66316fce-9025-40c2-aaf8-5097bbaa6ecb_1288x656.png)Tracking System State Information Using Gossip Protocol

So they use **gossip protocol** to track the system state. It finds the servers by pinging random servers periodically.

While each server transfers its entire membership list to a set of servers on each epoch. And a server is considered dead if it’s unavailable for a specific number of epochs.

Gossip protocol is a decentralized approach to service discovery and failure detection. Also the system would reach eventual consistency in logarithmic time with gossip protocol.

It wasn't a perfect system. But they chose the tradeoffs carefully to solve problems.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

3 years later…

They published the results in a white paper and called it Amazon Dynamo.

It laid the foundation for many NoSQL databases.

And distributed databases like Apache Cassandra, Riak, and Amazon DynamoDB are based on Amazon Dynamo principles.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/amazon-dynamo-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Khan Academy Scaled to 30 Million Users](https://substackcdn.com/image/fetch/$s_!w2xB!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa4ed11fa-b75a-4068-a610-37a2f83988ed_1280x720.gif)How Khan Academy Scaled to 30 Million Users[Neo Kim](https://substack.com/profile/135589200-neo-kim)·March 12, 2024[Read full story](https://newsletter.systemdesign.one/p/khan-academy-architecture)](https://newsletter.systemdesign.one/p/khan-academy-architecture)

[![How Tinder Scaled to 1.6 Billion Swipes per Day](https://substackcdn.com/image/fetch/$s_!gLsW!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fad4f699a-dc60-4921-a6c2-e40d861617c0_1280x720.gif)How Tinder Scaled to 1.6 Billion Swipes per Day[Neo Kim](https://substack.com/profile/135589200-neo-kim)·March 19, 2024[Read full story](https://newsletter.systemdesign.one/p/tinder-architecture)](https://newsletter.systemdesign.one/p/tinder-architecture)

* * *

### References

  * [Dynamo: Amazon’s Highly Available Key-value Store](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)

  * [A Decade of Dynamo: Powering the next wave of high-performance, internet-scale applications](https://www.allthingsdistributed.com/2017/10/a-decade-of-dynamo.html)

  * [Amazon DynamoDB – a Fast and Scalable NoSQL Database Service Designed for Internet Scale Applications](https://www.allthingsdistributed.com/2012/01/amazon-dynamodb.html)

  * [Amazon’s DynamoDB — 10 years later](https://www.amazon.science/latest-news/amazons-dynamodb-10-years-later)

  * [Dynamo – A Followup and Re-Rebuttals](https://jsensarma.com/dynamo-part-i-a-followup-and-re-rebuttals/)

  * [Consistent Hashing Explained](https://systemdesign.one/consistent-hashing-explained/)

  * [Gossip Protocol](https://systemdesign.one/gossip-protocol/)

  * [Hinted Handoff](https://systemdesign.one/hinted-handoff/)

  * [What Is Service Discovery?](https://systemdesign.one/what-is-service-discovery/)

  * [Consistency Patterns](https://systemdesign.one/consistency-patterns/)

  * Image taken from [Vector Clocks](https://en.wikipedia.org/wiki/Vector_clock#/media/File:Vector_Clock.svg)

109

2

12

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Han's avatar](https://substackcdn.com/image/fetch/$s_!NjE4!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcaff092f-4c23-45b4-93ef-d3db6d26552f_144x144.png)](https://substack.com/profile/1887782-han?utm_source=comment)

[Han](https://substack.com/profile/1887782-han?utm_source=substack-feed-item)

[Mar 27, 2024](https://newsletter.systemdesign.one/p/amazon-dynamo-architecture/comment/52560582 "Mar 27, 2024, 5:55 AM")

Liked by Neo Kim

For the merkle tree, is tree designed such all data blocks are on the leafs? 

If yes, then i assume any difference between 2 data blocks will cause all parent hashes to be different. So when searching from root downwards, we also to go all the way from root to the leafs. There won't be a case of the search stopping somewhere in the middle right?

"Thus in the worst case, they need logarithmic time complexity to transfer data."

So i don't understand why is it a worst case. I thought it is always the case that we have to search until leafs, which are where the data blocks exist, and anything above leaf level are meaningless hashes

Also could you clarify statements in section 4+5?

"While the temporary server sends the data once the failed server is back online. Thus offering high write availability."

Temporary server send data to where? The failed server after it's recovered?

" it happened before sending temporary data to the server that was back online after a failure."

Which server is sending to which server?

\- Failed (before failing) to temporary?

\- Temporary to Failed (after recovering)?

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/amazon-dynamo-architecture/comment/52560582)

[1 more comment...](https://newsletter.systemdesign.one/p/amazon-dynamo-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture