[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Why Is Redis a Distributed Swiss Army Knife 💭

### #50: Break Into Redis Use Cases (5 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jun 20, 2024

344

3

35

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _Note: This isn’t a sponsored post. I wrote it for someone who has never used Redis. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/redis-use-cases/?action=share) & I'll send you some rewards for the referrals._

**Why it matters:** Redis is often used to build high-performance apps and solve various problems without reinventing the wheel.

**Between the lines:** Redis changed their licensing recently. Yet there are many forks [1](https://newsletter.systemdesign.one/p/redis-use-cases#footnote-1-145546875).

It’s hackathon day at a tech company named Hooli.

And 2 software engineers want to create a fancy little app.

[![redis use cases](https://substackcdn.com/image/fetch/$s_!NY3c!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcec70f06-f599-40ab-8a37-3bcad53afb89_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!NY3c!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcec70f06-f599-40ab-8a37-3bcad53afb89_1200x630.gif)

Yet they were experienced only with a few tools - including Redis.

So they decide to reuse their skills in every possible way and build a prototype.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Redis Use Cases

Here’s how they built the app:

### 1\. Caching:

They installed a web server to handle API requests.

Yet most likely the same data will often get queried from the database. So they set up a Redis cache in front of the database.

A **cache** is a fast, in-memory data store.

[![Caching Database Response](https://substackcdn.com/image/fetch/$s_!1Spd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb8df299e-f438-4945-a29a-cc6b46a4bcf3_1985x376.png)](https://substackcdn.com/image/fetch/$s_!1Spd!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb8df299e-f438-4945-a29a-cc6b46a4bcf3_1985x376.png)Caching Database Response

How it works:

  * The database response is cached in the Redis [hash](https://redis.io/docs/latest/develop/data-types/hashes/) data structure

  * The database query gets hashed and is stored as the cache key

  * The cache gets queried on every request to check if the data is present

  * Data is in cache: response gets served by the cache - **cache hit**

  * Data isn’t in the cache: query hits the database and then fills the cache - **cache miss**

[![Speed Comparison](https://substackcdn.com/image/fetch/$s_!TgJ1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6112eefa-1bc3-4116-b50e-485d6c7b8e79_1087x810.png)](https://substackcdn.com/image/fetch/$s_!TgJ1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6112eefa-1bc3-4116-b50e-485d6c7b8e79_1087x810.png) Speed Comparison

It offers:

  * Faster response - 100x

  * Reduced database load

### 2\. Queueing:

They added extra servers to query and process data - named **query workers**.

Yet some queries took longer to finish.

So they set up Redis [streams](https://redis.io/docs/latest/develop/data-types/streams/) to queue requests. It allowed them to process requests asynchronously.

A Redis **stream** is a data structure that acts like an append-only log. This means every entry is immutable.

[![Queueing Requests for Asynchronous Processing](https://substackcdn.com/image/fetch/$s_!nPCp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffbec1322-2125-4c22-ace3-b762fd692235_2631x411.png)](https://substackcdn.com/image/fetch/$s_!nPCp!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffbec1322-2125-4c22-ace3-b762fd692235_2631x411.png)Queueing Requests for Asynchronous Processing

How it works:

  1. The request gets forwarded on a cache miss

  2. The request gets wrapped in a message and gets assigned a unique identifier

  3. The message gets added to the queue

  4. The query worker consumes the message if it has free capacity

  5. The query worker interacts with the database

It let them:

  * Buffer requests for asynchronous processing

  * Decouple serving and processing of requests

  * Auto-scale query workers based on load

### 3\. Locking:

Many expensive queries at once will overload the database.

So they installed a distributed [lock](https://redis.io/glossary/redis-lock/) using Redis.

A **distributed lock** orchestrates access to a shared resource from many clients.

[![Acquiring a Lock Before Querying the Database](https://substackcdn.com/image/fetch/$s_!4ksd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F917fbab0-2970-4dae-94df-2224384d7ad3_1661x564.png)](https://substackcdn.com/image/fetch/$s_!4ksd!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F917fbab0-2970-4dae-94df-2224384d7ad3_1661x564.png)Acquiring a Lock Before Querying the Database

How it works:

  * The query worker acquires a lock before talking to the database

  * An expiry time is set for the lock

  * The number of query workers that access the database at once is limited. While others must wait

It offers:

  * Resilience by avoiding many queries at once

  * Avoid noisy neighbor problem

### 4\. Throttling:

There’s a risk of lock acquisition failure. It will overload the database.

So they use Redis [streams](https://redis.io/docs/latest/develop/data-types/streams/) to throttle messages.

[![Re-Inserting Messages to the Queue for Throttling](https://substackcdn.com/image/fetch/$s_!Vww1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6cfefcc-e66e-43c0-9200-c61e35142f6b_1228x475.png)](https://substackcdn.com/image/fetch/$s_!Vww1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6cfefcc-e66e-43c0-9200-c61e35142f6b_1228x475.png)Re-Inserting Messages to the Queue for Throttling

How it works:

  * The query worker fails to acquire a lock, so the message gets added back to the queue

  * Also a delay gets assigned to the message before inserting it

  * And an extra delay gets added each time the same message is re-inserted into the queue - **exponential backoff**

It provides:

  * Congestion management by controlling the number of parallel database requests

### 5\. Session Store:

The web server stores the user’s data and preferences.

Yet it’s hard to scale a _stateful_ web server.

So they installed a separate session store using Redis.

[![Storing Session Data in a Separate Store](https://substackcdn.com/image/fetch/$s_!pS6Z!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbf77732b-75ac-4056-87b8-8c9dc5a9892f_1987x585.png)](https://substackcdn.com/image/fetch/$s_!pS6Z!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbf77732b-75ac-4056-87b8-8c9dc5a9892f_1987x585.png)Storing Session Data in a Separate Store

How it works:

  * Session data is stored in the Redis hash data structure

  * An expiry time is set for each user's data

  * The expiry time gets renewed whenever the user requests something

It let them:

  * Scale _stateless_ web servers easily

  * Handle traffic spikes

### 6\. Rate Limiting:

Many API requests in a short period will overload the web server.

So they set up a rate limiter with Redis.

[![Rate Limiting API Requests](https://substackcdn.com/image/fetch/$s_!PxQE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffb46537b-9ead-4e4c-af40-d1e7de9e0752_1420x415.png)](https://substackcdn.com/image/fetch/$s_!PxQE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffb46537b-9ead-4e4c-af40-d1e7de9e0752_1420x415.png)Rate Limiting API Requests

How it works:

  * A [counter](https://redis.io/glossary/storing-counters-in-redis/) is implemented using the Redis hash data structure

  * The counter represents the number of requests allowed on each API endpoint in a period

  * The counter gets decremented with each request

  * Requests get rejected if the counter becomes 0

  * The counter gets reset to its initial value after a specific period

While they deployed their app into production - it worked like a charm.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### 🔥 Bonus

Here are some more Redis use cases:

  * [Sorted sets](https://redis.io/docs/latest/develop/data-types/sorted-sets/) can be used to create a leaderboard

  * [HyperLogLog](https://redis.io/docs/latest/develop/data-types/probabilistic/hyperloglogs/) can be used to estimate the number of items in a set

  * [Pub-Sub](https://redis.io/docs/latest/develop/interact/pubsub/) can be used to implement real-time messaging

  * The [geospatial index](https://redis.io/glossary/geospatial-indexing/) can be used to find nearby points within a radius

  * [Time series](https://redis.io/docs/latest/develop/data-types/timeseries/) data type can be used for data analytics

  * The [list](https://redis.io/docs/latest/develop/data-types/lists/) data type can be used to create a social network timeline

  * The [RedisSearch](https://redis.io/search/) module supports full-text search and SQL-like query functionality. It’s useful if the search index fits in memory or changes often

  * The [RedisJSON](https://redis.io/json/) module can be used to store nested JSON data. It avoids de-serialization costs

* * *

**The bottom line:** Redis is more than just a cache. It runs in memory, thus providing faster reads and writes.

Also it can survive a crash with disk persistence. 

Besides it supports atomic operations using [transactions](https://redis.io/docs/latest/develop/interact/transactions/).

It’s single-threaded, so there is no need for locks. Thus offering high performance.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/redis-use-cases?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Instagram Scaled to 2.5 Billion Users](https://substackcdn.com/image/fetch/$s_!lU6H!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffc2f7a6a-3227-4396-aa8d-24dead87a511_1280x720.gif)How Instagram Scaled to 2.5 Billion Users[Neo Kim](https://substack.com/profile/135589200-neo-kim)·June 11, 2024[Read full story](https://newsletter.systemdesign.one/p/instagram-infrastructure)](https://newsletter.systemdesign.one/p/instagram-infrastructure)

[![How YouTube Was Able to Support 2.49 Billion Users With MySQL](https://substackcdn.com/image/fetch/$s_!LJGo!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F20f4ccdb-88ae-44ce-9bf0-b2c83220f5d2_1280x720.gif)How YouTube Was Able to Support 2.49 Billion Users With MySQL[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 31, 2024[Read full story](https://newsletter.systemdesign.one/p/vitess-mysql)](https://newsletter.systemdesign.one/p/vitess-mysql)

* * *

### References

  * [5 Redis Use Cases with Gur Dotan - Redis Labs](https://www.youtube.com/watch?v=znjGckK8abw)

  * [Redis Use Case Examples for Developers](https://redis.io/blog/5-industry-use-cases-for-redis-developers/)

  * [What is Redis Cache?](https://www.youtube.com/watch?v=Tqaqdfxi-J4&list=PL83Wfqi-zYZG_tk7VQvkPDqRua0uwfXIF)

  * [Redis Streams Explained](https://www.youtube.com/watch?v=Z8qcpXyMAiA&list=PL83Wfqi-zYZG_tk7VQvkPDqRua0uwfXIF)

  * [RedisJSON Explained](https://www.youtube.com/watch?v=V0wmD_y03iM&list=PL83Wfqi-zYZG_tk7VQvkPDqRua0uwfXIF&index=8)

  * [Redis Data Structures for Non-Redis Users](https://www.youtube.com/watch?v=ELk_W9BBTDU)

  * [Cache vs. Session Store](https://redis.io/blog/cache-vs-session-store/)

  * [Top 5 Caching Patterns](https://newsletter.systemdesign.one/p/caching-patterns)

  * [Leaderboard System Design](https://systemdesign.one/leaderboard-system-design/)

  * [This Is How Stripe Does Rate Limiting to Build Scalable APIs](https://newsletter.systemdesign.one/p/rate-limiter)

  * [Dave Nielsen: Top 5 uses of Redis as a Database | PyData Seattle 2015](https://www.youtube.com/watch?v=jTTlBc2-T9Q)

  * [AWS re: Invent 2020: Beyond caching: Advanced design patterns in Redis](https://www.youtube.com/watch?v=2WkJeofqIJg)

  * [RedisTimeSeries Explained](https://www.youtube.com/watch?v=SzcpwtLRgyk)

  * [Querying, Indexing, and Full-text Search in Redis](https://www.youtube.com/watch?v=infTV4ifNZY&list=PL83Wfqi-zYZHtpd4Glbj-NBIz7RB0Jw5u&index=15)

  * [Redis Lists Explained](https://www.youtube.com/watch?v=PB5SeOkkxQc&list=PL83Wfqi-zYZHtpd4Glbj-NBIz7RB0Jw5u)

  * [Redis Geospatial Explained](https://www.youtube.com/watch?v=qftiVQraxmI&list=PL83Wfqi-zYZHtpd4Glbj-NBIz7RB0Jw5u&index=22)

  * [Making Session Stores More Intelligent with Microservices](https://www.youtube.com/watch?v=i69TRwbxk_E)

  * [Redis HyperLogLog Explained](https://www.youtube.com/watch?v=MunL8nnwscQ&list=PL83Wfqi-zYZHtpd4Glbj-NBIz7RB0Jw5u&index=23)

  * [Redis Adopts Dual Source-Available Licensing - Hacker News](https://news.ycombinator.com/item?id=39772562)

[1](https://newsletter.systemdesign.one/p/redis-use-cases#footnote-anchor-1-145546875)

  * [Garnet](https://github.com/microsoft/garnet)

  * [Valkey](https://github.com/valkey-io/valkey)

  * [KeyDB](https://github.com/Snapchat/KeyDB)

  * [Redict](https://codeberg.org/redict/redict)

344

3

35

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![cch's avatar](https://substackcdn.com/image/fetch/$s_!DFX4!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fgreen.png)](https://substack.com/profile/88890283-cch?utm_source=comment)

[cch](https://substack.com/profile/88890283-cch?utm_source=substack-feed-item)

[Jun 22, 2024](https://newsletter.systemdesign.one/p/redis-use-cases/comment/59751446 "Jun 22, 2024, 2:57 PM")

For the "2. Queueing" use case, when there is a cache miss, since the processing of the request is decoupled, does queue worker simply update the cache after it gets data back from the db? What does user get in return when there is a cache miss ? 

ReplyShare

[![Chips Ahoy Capital's avatar](https://substackcdn.com/image/fetch/$s_!Gs9y!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4dbf68f4-5341-4c82-a22c-048b09734e70_1024x1024.jpeg)](https://substack.com/profile/237013244-chips-ahoy-capital?utm_source=comment)

[Chips Ahoy Capital](https://substack.com/profile/237013244-chips-ahoy-capital?utm_source=substack-feed-item)

[Jun 28, 2024](https://newsletter.systemdesign.one/p/redis-use-cases/comment/60341447 "Jun 28, 2024, 6:07 PM")

This is great - out of curiosity what app do you use to make your charts? 

ReplyShare

[1 reply](https://newsletter.systemdesign.one/p/redis-use-cases/comment/60341447)

[1 more comment...](https://newsletter.systemdesign.one/p/redis-use-cases/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture