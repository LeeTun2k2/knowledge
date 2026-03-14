[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Tinder Scaled to 1.6 Billion Swipes per Day

### #40: Break Into Tinder Architecture (7 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Mar 19, 2024

189

19

15

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines Tinder's architecture. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/tinder-architecture/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there lived a student named Kenji in Okinawa, Japan.

[![Kenji; Tinder Architecture](https://substackcdn.com/image/fetch/$s_!jzLY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5891f09b-dd30-47f4-b1e2-73516083dfe1_800x500.gif)](https://substackcdn.com/image/fetch/$s_!jzLY!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5891f09b-dd30-47f4-b1e2-73516083dfe1_800x500.gif)

He’s moving to Australia for higher studies.

But one day, his girlfriend breaks up with him.

So he was sad.

He hears about a platform to connect people called Tinder.

Although an introvert, he decides to try it.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Tinder Architecture

Here’s a simplified version of Tinder architecture:

### Chapter 1: Get Over the Breakup

Kenji creates a Tinder profile and adds extra information.

[![Creating a User Profile](https://substackcdn.com/image/fetch/$s_!SpRN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Facc5d36d-65c3-4fa2-9b48-8184c5e85901_1790x274.png)](https://substackcdn.com/image/fetch/$s_!SpRN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Facc5d36d-65c3-4fa2-9b48-8184c5e85901_1790x274.png)Creating a User Profile

They store the user information in a key-value database like Amazon DynamoDB. And use Dynamo Streams to push out changes on a table to different places automatically.

Also the user information gets added to the message queue to update the location index. They use the location index to find nearby users efficiently.

[![Public Traffic Routed Through API Gateway](https://substackcdn.com/image/fetch/$s_!MySx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6150c02e-5782-4f78-b6f9-59a0c1d30531_1153x233.png)](https://substackcdn.com/image/fetch/$s_!MySx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6150c02e-5782-4f78-b6f9-59a0c1d30531_1153x233.png)Public Traffic Routed Through API Gateway

They provide public APIs through an API Gateway. That means it acts as a central entry point for user requests to the infrastructure. And handles user authorization and security rules.

They run around 500 microservices. And use service mesh to communicate between the services. Imagine service mesh as a network infrastructure for managing microservices communication efficiently.

### Chapter 2: The Lady From Okinawa

Kenji is shown Tinder profiles of people living nearby.

It’s difficult to find nearby people only based on a person’s latitude and longitude values.

Also they don’t divide the world map into evenly spaced grids to avoid the hot-shard problem. Because the grids in the ocean will be empty. While grids in big cities will have many users. A **hot shard** is an excessive load on a single partition.

Instead they use the S2 library. **S2** is a square-shaped hierarchical geospatial indexing system created by Google.

It’s a stable library supporting many programming languages. Put another way, they use S2 to recommend people in real time and shard the location database.

[![Representing the World Map Using S2 Cells](https://substackcdn.com/image/fetch/$s_!QJD5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb9da6531-eea4-40aa-906d-dea6a11b2e1e_800x500.gif)](https://substackcdn.com/image/fetch/$s_!QJD5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb9da6531-eea4-40aa-906d-dea6a11b2e1e_800x500.gif)Representing the World Map Using S2 Cells

S2 divides Earth’s surface into cells on a flat grid, giving each cell a unique identifier with a 64-bit integer. In other words, a small cell represents a small area of the earth.

They store the users physically closer to each other in the same database shard. Thus reducing the need to query many shards to find nearby users.

And S2 is hierarchical. That means the cell size varies from square centimeters to square kilometers.

Also it supports finding a specific cell using the latitude and longitude. And provides the functionality to find the surrounding cells of a specific cell.

[![Hilbert Curve Preserving Spatial Locality](https://substackcdn.com/image/fetch/$s_!uG3u!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F461ea178-29f3-4c10-b290-ae7653fcd2ea_1345x665.png)](https://substackcdn.com/image/fetch/$s_!uG3u!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F461ea178-29f3-4c10-b290-ae7653fcd2ea_1345x665.png)[Hilbert Curve Preserving Spatial Locality](https://en.wikipedia.org/wiki/Hilbert_curve)

S2 is based on the Hilbert curve. Imagine the **Hilbert curve** as a line that covers every point in a square by folding and looping in a special way. While two points that are close in the Hilbert curve are also close in physical space. That means it preserves spatial locality.

Each small Hilbert curve represents an S2 cell. While four adjacent cells form a bigger cell, and a quadtree represents a 2D Hilbert curve. A **[quadtree](https://en.wikipedia.org/wiki/Quadtree)** is a tree data structure where each node has exactly four children.

[![Finding Nearby Users](https://substackcdn.com/image/fetch/$s_!QHNV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb909022f-95b5-4a4a-b9f9-290a86f31060_1848x274.png)](https://substackcdn.com/image/fetch/$s_!QHNV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb909022f-95b5-4a4a-b9f9-290a86f31060_1848x274.png)Finding Nearby Users

They query the location index (S2) to find nearby users. It returns all the database shards close to a specific location. After that, they query all the relevant database shards in parallel to get the list of users in those shards. 

On average they query 3 database shards to find nearby users within a 160 km radius. Also they filter the results based on user preferences before recommending people.

[![Lady from Okinawa; Tinder Architecture](https://substackcdn.com/image/fetch/$s_!8ZQ6!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3394d7b-5a2f-49e7-abf1-838d3992aaf3_800x500.gif)](https://substackcdn.com/image/fetch/$s_!8ZQ6!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3394d7b-5a2f-49e7-abf1-838d3992aaf3_800x500.gif)

Kenji meets up with a lady from Okinawa.

But it didn’t work out because she wasn't interested in a long-distance relationship.

### Chapter 3: Okinawa to Sydney

Kenji thought matching on Tinder with someone in Sydney, Australia would make more sense.

So he uses Tinder’s feature called Passport to change his location.

[![Changing the User Location](https://substackcdn.com/image/fetch/$s_!wiF_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F78a5ca52-c463-499e-870b-5d841327b71d_1790x274.png)](https://substackcdn.com/image/fetch/$s_!wiF_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F78a5ca52-c463-499e-870b-5d841327b71d_1790x274.png)Changing the User Location

They update the index on user location changes. So people from the new location could see the user. In other words, they add the user to the new location index and remove the user from the old index.

Yet these operations aren’t atomic, so there’s a risk of failures.

Until one day Kenji updated the profile to the new location. And immediately changes back to the original location. But his user data still pointed to the new location as the operations weren’t executed in the same order.

[![The Problem Without Guaranteed Ordering](https://substackcdn.com/image/fetch/$s_!oLx3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdc9b4bd3-3fa3-4286-b57d-cac8e983be7c_1174x1005.png)](https://substackcdn.com/image/fetch/$s_!oLx3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdc9b4bd3-3fa3-4286-b57d-cac8e983be7c_1174x1005.png)The Problem Without Guaranteed Ordering

So they use Apache Kafka to solve this problem. Think of **Kafka** as a distributed streaming platform for large-scale data processing. 

They sent the same user data to the same Kafka partitions. While Kafka consumers acquire locks on the partitions to avoid contention. Thus providing a FIFO queue implementation with ordering guarantees and very high throughput.

Also they checkpoint Kafka so the processing could resume after a process crash.

### Chapter 4: Zoe and Kenji

Kenji kept swiping on Tinder. 

And days passed.

[![Matching Users](https://substackcdn.com/image/fetch/$s_!gHaw!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1b6b59f6-7fbe-4141-ae28-8b6e5a6a32f0_1694x248.png)](https://substackcdn.com/image/fetch/$s_!gHaw!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1b6b59f6-7fbe-4141-ae28-8b6e5a6a32f0_1694x248.png)Matching Users

They send swipes to a data stream like Amazon Kinesis. And run match workers to read the data stream and check whether there’s a match from the Likes cache. 

While the Likes cache stores the information about people a user has liked.

[![Notifying Users About a Match](https://substackcdn.com/image/fetch/$s_!IuiG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd8e01656-101d-4946-bcff-0c3b7ff9b296_1505x249.png)](https://substackcdn.com/image/fetch/$s_!IuiG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd8e01656-101d-4946-bcff-0c3b7ff9b296_1505x249.png)Notifying Users About a Match

They use WebSockets to notify users if there’s a match. Thus giving a real-time experience. **WebSockets** is a real-time, bidirectional communication protocol.

[![Storing Disliked Profiles by a User](https://substackcdn.com/image/fetch/$s_!KmrJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd7e92b51-fbfd-40e4-8723-c881c97969be_1814x238.png)](https://substackcdn.com/image/fetch/$s_!KmrJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd7e92b51-fbfd-40e4-8723-c881c97969be_1814x238.png)Storing Profiles Disliked by a User

They put the disliked profiles by a person into a data storage like Amazon S3. And use that information for data analysis to improve user recommendations.

[![Match; Tinder Architecture](https://substackcdn.com/image/fetch/$s_!E1B_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d539faf-9090-4469-9519-76602f111dd5_800x500.gif)](https://substackcdn.com/image/fetch/$s_!E1B_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d539faf-9090-4469-9519-76602f111dd5_800x500.gif)

Until one day. Kenji matched with Zoe. 

A lady from Sydney.

### Chapter 5: Problems Are Guidelines

They calculate the unique number of users to find the load on a shard. And users within a shard are usually in the same time zone. 

[![The Traffic Pattern of Two Different Shards](https://substackcdn.com/image/fetch/$s_!PAL-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faa1f798d-2fb2-4937-90a5-002f7252e82a_821x543.png)](https://substackcdn.com/image/fetch/$s_!PAL-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faa1f798d-2fb2-4937-90a5-002f7252e82a_821x543.png)The Traffic Pattern of Two Different Shards

A hot shard problem will likely occur due to time zone differences. That means the peak traffic will differ across locations due to time differences. And cause unbalanced traffic across shards.

Yet a single shard in a physical server would make it worse. So they randomly assign many shards to the same physical server to prevent the hot shard problem.

[![Cache-Aside Pattern to Avoid Hot Shard Problem](https://substackcdn.com/image/fetch/$s_!o0L_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fce2da0bf-b981-4f34-9ad6-1719f275b0cb_1186x411.png)](https://substackcdn.com/image/fetch/$s_!o0L_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fce2da0bf-b981-4f34-9ad6-1719f275b0cb_1186x411.png)Caching to Avoid Hot Shard Problem

Also most data operations at Tinder are read operations. So they use Redis cache for scale. 

They use the cache-aside pattern. In other words, they check the cache for data. And if it’s absent, they fall back to the database. Also the cache gets updated with the data.

So the cache layer solves the read hot shard problem. While they do rate limiting to handle the write hot shard problem.

Besides they do exponential backoff with jitter on a failure. And run a background job periodically to synchronize the data stores.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

Tinder remains one of the largest dating platforms. And handle around 26 million matches a day.

[![Happy end; Tinder Architecture](https://substackcdn.com/image/fetch/$s_!d7lN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff92cd38f-346a-4d66-8ccd-e5ccd83306b1_800x500.gif)](https://substackcdn.com/image/fetch/$s_!d7lN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff92cd38f-346a-4d66-8ccd-e5ccd83306b1_800x500.gif)

While Kenji and Zoe lived happily ever after.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/tinder-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Khan Academy Scaled to 30 Million Users](https://substackcdn.com/image/fetch/$s_!w2xB!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa4ed11fa-b75a-4068-a610-37a2f83988ed_1280x720.gif)How Khan Academy Scaled to 30 Million Users[Neo Kim](https://substack.com/profile/135589200-neo-kim)·March 12, 2024[Read full story](https://newsletter.systemdesign.one/p/khan-academy-architecture)](https://newsletter.systemdesign.one/p/khan-academy-architecture)

[![How Amazon S3 Achieves 99.999999999% Durability](https://substackcdn.com/image/fetch/$s_!obVc!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3645f110-2311-45c9-8e01-e50ea90e94e0_1280x720.gif)How Amazon S3 Achieves 99.999999999% Durability[Neo Kim](https://substack.com/profile/135589200-neo-kim)·March 5, 2024[Read full story](https://newsletter.systemdesign.one/p/amazon-s3-durability)](https://newsletter.systemdesign.one/p/amazon-s3-durability)

* * *

### References

  * [Geosharded Recommendations Part 1: Sharding Approach](https://medium.com/tinder/geosharded-recommendations-part-1-sharding-approach-d5d54e0ec77a)

  * [Geosharded Recommendations Part 2: Architecture](https://medium.com/tinder/geosharded-recommendations-part-2-architecture-3396a8a7efb)

  * [Geosharded Recommendations Part 3: Consistency](https://medium.com/tinder/geosharded-recommendations-part-3-consistency-2d2cb2f0594b)

  * [Taming ElastiCache with Auto-discovery at Scale](https://medium.com/tinder/taming-elasticache-with-auto-discovery-at-scale-dc5e7c4c9ad0)

  * [Powering Tinder® — The Method Behind Our Matching](https://www.tinderpressroom.com/powering-tinder-r-the-method-behind-our-matching/)

  * [Now More Than Ever, Having Someone To Talk To Can Make A World Of Difference](https://www.tinderpressroom.com/now-more-than-ever-having-someone-to-talk-to-can-make-a-world-of-difference)

  * [AWS re: Invent 2017: Tinder and DynamoDB: It's a Match! Massive Data Migration, Zero (DAT328)](https://www.youtube.com/watch?v=Lq4aNihcS8A)

  * [Tinder system architecture](https://xie.infoq.cn/article/b717a40affb80efa6f9cfee77)

  * [Building resiliency at scale at Tinder with Amazon ElastiCache](https://aws.amazon.com/blogs/database/building-resiliency-at-scale-at-tinder-with-amazon-elasticache/)

  * [How we built the Tinder API Gateway](https://medium.com/tinder/how-we-built-the-tinder-api-gateway-831c6ca5ceca)

  * [Geometry on the Sphere: Google's S2 Library](https://docs.google.com/presentation/d/1Hl4KapfAENAOf4gv-pSngKwvS_jwNVHRPZTTDzXXn6Q/view?pli=1#slide=id.i0)

  * [Google’s S2, geometry on the sphere, cells, and Hilbert curve](https://blog.christianperone.com/2015/08/googles-s2-geometry-on-the-sphere-cells-and-hilbert-curve/)

  * [Using Apache Kafka as a Scalable, Event-Driven Backbone for Service Architectures](https://www.confluent.io/blog/apache-kafka-for-service-architectures/)

  * Image taken from [Hilbert 3D](https://en.wikipedia.org/wiki/File:Hilbert3d-step3.png)

  * Image taken from [Hilbert curve](https://en.wikipedia.org/wiki/File:Hilbert_curve_3.svg)

189

19

15

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Jade Wilson's avatar](https://substackcdn.com/image/fetch/$s_!tGfL!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F98108c42-35f4-479c-b526-096d180d5288_625x625.jpeg)](https://substack.com/profile/199051090-jade-wilson?utm_source=comment)

[Jade Wilson](https://substack.com/profile/199051090-jade-wilson?utm_source=substack-feed-item)

[Mar 19, 2024](https://newsletter.systemdesign.one/p/tinder-architecture/comment/51987047 "Mar 19, 2024, 3:00 PM")

Liked by Neo Kim

I love how you tie the different user's into this, it makes for a great read. Thanks for sharing🙏🙌

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/tinder-architecture/comment/51987047)

[![Osinachi Okpara's avatar](https://substackcdn.com/image/fetch/$s_!WBh8!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F73e79c5f-3379-4f2e-b461-ae04de992149_792x792.png)](https://substack.com/profile/7000855-osinachi-okpara?utm_source=comment)

[Osinachi Okpara](https://substack.com/profile/7000855-osinachi-okpara?utm_source=substack-feed-item)

[Mar 19, 2024](https://newsletter.systemdesign.one/p/tinder-architecture/comment/51980793 "Mar 19, 2024, 1:39 PM")

Liked by Neo Kim

Basically, the algorithm of...love! 😅

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/tinder-architecture/comment/51980793)

[17 more comments...](https://newsletter.systemdesign.one/p/tinder-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture