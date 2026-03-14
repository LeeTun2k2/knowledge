[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Top 5 Caching Patterns

### #11: Read Now - How to Update the Cache (3 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 05, 2023

92

10

9

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the most common caching patterns. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/caching-patterns/?action=share) & I'll send you some rewards for the referrals._

A cache is a temporary storage that provides quick access to frequently used data. And it’s usually stored in memory for low latency. But available memory is limited. So it’s important to update the cache the right way. 

I will teach you the popular caching patterns in this post.

A _cache hit_ occurs when queried data is available in the cache. And a _cache miss_ occurs when queried data isn’t available in the cache.

## Caching Patterns

The 5 popular caching patterns are:

### 1\. Cache Aside

[![cache aside pattern; caching patterns](https://substackcdn.com/image/fetch/$s_!K_lM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22d7fd82-9d46-4e4e-b0a3-4a9270fb0340_600x260.gif)](https://substackcdn.com/image/fetch/$s_!K_lM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22d7fd82-9d46-4e4e-b0a3-4a9270fb0340_600x260.gif)Cache Aside Pattern

Here is the _workflow_ :

  1. Read data from the cache

  2. Read data from the database on a cache miss

  3. Write data to the cache

  4. And return data

It’s also called _lazy loading_. Because only the queried data gets cached.

Also the cache doesn't interact with the database. But the app does.

#### Advantages

  * Easy implementation

  * Finer control over cache population. Because only frequently accessed data is cached

#### Disadvantages

  * Degraded latency. Because there are many trips

  * Potential data inconsistency

#### Use Cases

  * General purpose caching

  * Read-heavy workloads

### 2\. Write Through

[![Write through pattern; Caching patterns](https://substackcdn.com/image/fetch/$s_!lQmk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f6e50eb-27a9-4da1-ad48-b55ba960f332_600x150.gif)](https://substackcdn.com/image/fetch/$s_!lQmk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f6e50eb-27a9-4da1-ad48-b55ba960f332_600x150.gif)Write Through Pattern

Here is the _workflow_ :

  1. Write data to the cache

  2. And the cache __ writes data _synchronously_ to the database

The app doesn't interact with the database. But the cache does.

#### Advantages

  * Data consistency

  * Low read latency. Because the cache contains fresh data

#### Disadvantages

  * High write latency. Because both cache and database need to be updated

  * Most data in the cache is never read

#### Use Cases

  * A low number of writes expected

  * Data freshness is important

### 3\. Read Through

[![Read Through Cache Pattern; Caching patterns](https://substackcdn.com/image/fetch/$s_!qhfs!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8546d2b-9c71-4d0f-a7f4-28e740e9caff_600x147.gif)](https://substackcdn.com/image/fetch/$s_!qhfs!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8546d2b-9c71-4d0f-a7f4-28e740e9caff_600x147.gif)Read Through Pattern

Here is the _workflow_ :

  1. Read data from the cache

  2. Read data from the database on a cache miss

  3. Write data to the cache

  4. And return data

The app doesn't interact with the database. But the cache does. 

And this is what makes it different from the cache aside pattern.

#### Advantages

  * Low read latency for frequently accessed data

  * Improved read scalability

#### Disadvantages

  * Potential data inconsistency. Because the cache might contain stale data

  * High latency on a cache miss

#### Use Cases

  * Read-heavy workloads

  * A high cache miss rate is acceptable

### 4\. Write Back

[![Write back pattern; Caching patterns](https://substackcdn.com/image/fetch/$s_!cOcQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5dce6d33-b980-41f5-a11b-3e641fa30a02_600x152.gif)](https://substackcdn.com/image/fetch/$s_!cOcQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5dce6d33-b980-41f5-a11b-3e641fa30a02_600x152.gif)Write Back Pattern

Here is the _workflow_ :

  1. Write data to the cache

  2. And the cache writes data _asynchronously_ to the database

It’s also called the _write-behind_ pattern.

#### Advantages

  * Low write latency

  * Improved performance. Because writes to the database get batched

#### Disadvantages

  * Increased risk of data loss. Because the cache might fail before writing to the database

  * Complex implementation

#### Use Cases

  * Write-heavy workloads

  * Data durability is not important

### 5\. Write Around

[![Write around pattern; Caching patterns](https://substackcdn.com/image/fetch/$s_!6ns2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fae4eb5b7-8fa9-454f-9587-1d9282485237_600x180.gif)](https://substackcdn.com/image/fetch/$s_!6ns2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fae4eb5b7-8fa9-454f-9587-1d9282485237_600x180.gif)Write Around Pattern

Here is the _workflow_ :

  1. Write data to the database

  2. Read data from the cache

  3. Read data from the database on a cache miss

  4. And write data to the cache

#### Advantages

  * Reduced risk of data loss

  * Reduced cache pollution. Because cache stores only frequently accessed data

#### Disadvantages

  * High read latency

  * High cache miss rate

#### Use Cases

  * No data update expected

  * A low number of reads expected

Redis and Memcached are popular cache implementations. And a combination of caching patterns is used in real-world systems.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/caching-patterns?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![6 Proven Guidelines on Open Sourcing From Tumblr](https://substackcdn.com/image/fetch/$s_!haH2!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F95ee48f6-b409-49b1-87cc-18c031e8e2c9_1280x720.png)6 Proven Guidelines on Open Sourcing From Tumblr[NK](https://substack.com/profile/135589200-nk)·October 1, 2023[Read full story](https://newsletter.systemdesign.one/p/open-source-guidelines)](https://newsletter.systemdesign.one/p/open-source-guidelines)

[![This Is How Stripe Does Rate Limiting to Build Scalable APIs](https://substackcdn.com/image/fetch/$s_!mvCu!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd82536df-eaae-4ef9-9adf-960b3b5851bc_1280x720.png)This Is How Stripe Does Rate Limiting to Build Scalable APIs[NK](https://substack.com/profile/135589200-nk)·September 28, 2023[Read full story](https://newsletter.systemdesign.one/p/rate-limiter)](https://newsletter.systemdesign.one/p/rate-limiter)

And don’t forget to hit the Like button if you enjoyed this post ❤️

* * *

## References

  * https://www.prisma.io/dataguide/managing-databases/introduction-database-caching

  * https://www.slideshare.net/tmatyashovsky/from-cache-to-in-memory-data-grid-introduction-to-hazelcast

  * https://www.alachisoft.com/resources/articles/readthru-writethru-writebehind.html

92

10

9

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Duck Dickson's avatar](https://substackcdn.com/image/fetch/$s_!N9MQ!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fddb88d52-5454-4b79-add0-acc72bd276d2_144x144.png)](https://substack.com/profile/110240398-duck-dickson?utm_source=comment)

[Duck Dickson](https://substack.com/profile/110240398-duck-dickson?utm_source=substack-feed-item)

[Oct 9, 2023](https://newsletter.systemdesign.one/p/caching-patterns/comment/41562615 "Oct 9, 2023, 6:34 PM")Edited

Liked by Neo Kim

This is a great idea for an article but unfortunately the descriptions are not easy to read. For example, the workflow for Read-through is WORD FOR WORD identical to Cache Aside. The words are insufficient to explain what's going on, so is the diagram. Readers have to read both the words and the diagram to stitch together hat's going on. I would encourage rewriting these to explain in more detail.

ReplyShare

[2 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/caching-patterns/comment/41562615)

[![Anton Zaides's avatar](https://substackcdn.com/image/fetch/$s_!m9AG!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe37a1acd-c9a1-4968-b60d-907005004d84_1728x1728.jpeg)](https://substack.com/profile/121956618-anton-zaides?utm_source=comment)

[Anton Zaides](https://substack.com/profile/121956618-anton-zaides?utm_source=substack-feed-item)

[Oct 5, 2023](https://newsletter.systemdesign.one/p/caching-patterns/comment/41330493 "Oct 5, 2023, 1:18 PM")

Liked by Neo Kim

Interesting! I've worked only with the Cache aside implementation, as it's the easier to maintain. 

ReplyShare

[8 more comments...](https://newsletter.systemdesign.one/p/caching-patterns/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture