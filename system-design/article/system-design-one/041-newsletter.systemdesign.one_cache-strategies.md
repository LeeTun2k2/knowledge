[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Everything You Need to Know About Cache Strategies ⭐

### #74: A Simple Introduction to Cache Strategies (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jun 13, 2025

103

10

9

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines popular cache strategies. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/cache-strategies/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there lived a junior software engineer named Asahi.

He worked for a tech company named Hooli.

Although smart, he never received the opportunity to work on any interesting project.

So he was sad and frustrated.

[![cache strategies](https://substackcdn.com/image/fetch/$s_!oKlN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7db66c6-291a-4c0c-939c-a8c2ebe7581f_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!oKlN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7db66c6-291a-4c0c-939c-a8c2ebe7581f_1200x630.gif)

Until one day, when he had the idea of applying for a new job.

And the interviewer asked him 1 simple question,

_“Can you list 5 popular cache strategies?”._

Yet he failed to answer it, and the interview was over in 7 minutes.

So he studied it later to fill the knowledge gap.

Onward.

* * *

### [Shape the future of AI with Outlier—Sponsor](https://tiny.outlier.ai/3b3y7fbs)

[![](https://substackcdn.com/image/fetch/$s_!ysaF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6f0421c-8b22-4806-843f-817f59bdda1d_1920x1080.png)](https://tiny.outlier.ai/3b3y7fbs)

[Outlier](https://tiny.outlier.ai/3b3y7fbs) is hiring experienced developers to help train cutting-edge AI models with expert human feedback. Work flexibly from anywhere, get paid weekly (up to $50/hr), and apply your real-world dev skills to real AI challenges—no AI experience required.

[Apply here](https://tiny.outlier.ai/3b3y7fbs)

* * *

## Cache Strategies

He failed the interview, so you don’t have to.

And here’s how you can answer it:

A **cache** is used to store frequently accessed data, thus serving future requests faster. Also it keeps data in memory for low latency. Yet the available memory is limited. So frequent updates to the cache are necessary.

The technique of storing, retrieving, and managing data in a cache for performance and optimal memory usage is called a **cache strategy**.

### 1\. Cache Aside Strategy

[![Cache Aside Strategy](https://substackcdn.com/image/fetch/$s_!GYjx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F114bf12a-df3b-4b2b-b10e-a58e9b22ff08_1584x335.gif)](https://substackcdn.com/image/fetch/$s_!GYjx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F114bf12a-df3b-4b2b-b10e-a58e9b22ff08_1584x335.gif)

The cache aside is one of the most popular cache strategies. 

It’s also called _lazy loading_ because data gets cached only when it’s queried.

Here’s how it works:

  1. The app tries to read data from the cache

  2. It then reads data from the database on a cache miss

  3. The data gets written to the cache

A **cache miss** means the queried data is unavailable in the cache.

Besides the cache doesn't interact with the database directly; instead, the app does.

This strategy is easy to implement. But there’s a risk of data inconsistency and extra latency because of network round trips.

It’s used in read-heavy workloads, such as configuration data or user profiles.

Ready for the next technique?

### 2\. Write Through Strategy

[![Write Through Cache Strategy](https://substackcdn.com/image/fetch/$s_!vdiD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3ffb465-b245-46c0-ad0b-30535383de82_1584x335.gif)](https://substackcdn.com/image/fetch/$s_!vdiD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3ffb465-b245-46c0-ad0b-30535383de82_1584x335.gif)

The write-through cache strategy ensures strong consistency between the cache and the database.

Here’s how it works:

  1. The app writes to the cache

  2. The cache then __ writes __ to the database _synchronously._

The app doesn't interact with the database directly; instead, the cache does.

Although this strategy offers data consistency, the latency on writes is higher. Also the cache space might get wasted with infrequently accessed data.

It’s used where write rate is low, and when data freshness is critical.

### 3\. Read Through Strategy

[![Read Through Cache Strategy](https://substackcdn.com/image/fetch/$s_!E9Ho!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F05be620d-f144-4302-ab8d-ac53e48a3b90_1584x335.gif)](https://substackcdn.com/image/fetch/$s_!E9Ho!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F05be620d-f144-4302-ab8d-ac53e48a3b90_1584x335.gif)

The read-through cache strategy installs the cache between the app and the database. This means all reads go through the cache.

Here’s how it works:

  1. The app reads data from the cache

  2. Then it reads data from the database on a cache miss

  3. The data gets written to the cache and then returned to the app

The app doesn't interact with the database directly; instead, the cache does.

Although this strategy offers low latency, there’s a risk of data inconsistency.

It’s used in read-heavy workloads, such as a newsfeed or a product catalog.

Let’s keep going!

### 4\. Write Back Strategy

[![Write Back Cache Strategy](https://substackcdn.com/image/fetch/$s_!mKIn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9267df15-d04d-45dd-9c25-56673dde00d6_1584x335.gif)](https://substackcdn.com/image/fetch/$s_!mKIn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9267df15-d04d-45dd-9c25-56673dde00d6_1584x335.gif)

The write-back strategy writes data to the cache for batching and performance. This means write latency is low.

Here’s how it works:

  1. The app writes to the cache directly

  2. The cache then writes data __ to the database _asynchronously_

This strategy offers better write performance through batching. Yet there’s a risk of data loss if the cache fails before writing to the database.

It’s used in write-heavy workloads where throughput is more important than durability.

### 5\. Write Around Strategy

[![Write Around Cache Strategy](https://substackcdn.com/image/fetch/$s_!Mb5w!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1725533c-9acc-4581-8915-30b5ca1d981b_1584x335.gif)](https://substackcdn.com/image/fetch/$s_!Mb5w!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1725533c-9acc-4581-8915-30b5ca1d981b_1584x335.gif)

Here’s how the write-around cache strategy works:

  1. The app writes to the database

  2. It tries to read data from the cache later

  3. The app reads from the database on a cache miss

  4. It then updates the cache

Although this strategy optimizes cache storage for frequently accessed data, there is a risk of increased latency because of cache misses.

It’s used for large data objects where updates happen rarely.

* * *

#### **TL;DR:**

The 2 popular cache implementations are Redis and Memcached.

  * Read heavy workload: use cache aside or read through strategies

  * Consistency vs throughput: use write-through or write-back strategies

  * Avoid caching one-off writes: use write-around strategy

It’s important to pick the right cache strategy based on your needs and tradeoffs.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋 Find me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 150K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/cache-strategies?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

**TL;DR** 🕰️

You can find a summary of this article [here](https://www.linkedin.com/posts/nk-systemdesign-one_if-i-had-to-set-up-a-cache-here-are-5-strategies-activity-7339258136458915841-ylOY). Consider a repost if you find it helpful.

* * *

[![How API Gateway Actually Work ⭐](https://substackcdn.com/image/fetch/$s_!vRoW!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F98649508-84dd-4f6b-872c-d2550b4cc7a5_1280x720.png)How API Gateway Actually Work ⭐[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 29, 2025[Read full story](https://newsletter.systemdesign.one/p/how-api-gateway-works)](https://newsletter.systemdesign.one/p/how-api-gateway-works)

[![How Load Balancing Algorithms Really Work ⭐](https://substackcdn.com/image/fetch/$s_!Agws!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdf63a17b-ea6d-4f14-9a1f-d571bf065f4c_1280x720.png)How Load Balancing Algorithms Really Work ⭐[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 19, 2025[Read full story](https://newsletter.systemdesign.one/p/load-balancing-algorithms)](https://newsletter.systemdesign.one/p/load-balancing-algorithms)

* * *

### References

  * [Introduction to database caching](https://www.prisma.io/dataguide/managing-databases/introduction-database-caching)

  * [From cache to in-memory data grid. Introduction to Hazelcast](https://www.slideshare.net/slideshow/from-cache-to-in-memory-data-grid-introduction-to-hazelcast/34802471)

  * [Using Read-through Cache & Write-through Cache](https://www.alachisoft.com/resources/articles/readthru-writethru-writebehind.html)

  * [Redis official site](https://redis.io/)

  * [Memcached official site](https://memcached.org/)

103

10

9

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Kevin Naughton Jr.'s avatar](https://substackcdn.com/image/fetch/$s_!k-1n!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F32751fef-8ccd-4ad3-a72d-c1aefb4654b7_400x400.jpeg)](https://substack.com/profile/201111637-kevin-naughton-jr?utm_source=comment)

[Kevin Naughton Jr.](https://substack.com/profile/201111637-kevin-naughton-jr?utm_source=substack-feed-item)

[Jun 13, 2025](https://newsletter.systemdesign.one/p/cache-strategies/comment/125489812 "Jun 13, 2025, 1:06 PM")

Liked by Neo Kim

congrats on 150k Neo! :)

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/cache-strategies/comment/125489812)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Jun 13, 2025](https://newsletter.systemdesign.one/p/cache-strategies/comment/125472669 "Jun 13, 2025, 11:57 AM")

Liked by Neo Kim

Cache is probably the closest thing we have to a silver bullet. 

But only if you're willing to aim it carefully, invalidation is still hard.

Good breakdown, Neo.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/cache-strategies/comment/125472669)

[8 more comments...](https://newsletter.systemdesign.one/p/cache-strategies/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture