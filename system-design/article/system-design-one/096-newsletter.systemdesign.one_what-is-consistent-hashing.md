[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Everything You Need to Know About Consistent Hashing

### #23: Read Now - Algorithm to Build a Planet-Scale Distributed Cache (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Nov 21, 2023

55

2

7

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

  *  _[Share this post](https://newsletter.systemdesign.one/p/what-is-consistent-hashing/?action=share) _& I'll send you some rewards for the referrals.

Imagine you own a website and it became popular.

So you install a cache to reduce the load on the origin server.

But soon the cache server will hit its limits and result in cache misses.

A simple solution is to replicate the cache server. Yet only a limited amount of data set can be cached.

So cache server must be partitioned to store more data.

[![Consistent hashing; replication vs partitioning](https://substackcdn.com/image/fetch/$s_!rF9U!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7961f29f-901e-4a0e-8d10-f586e193bc65_2050x367.png)](https://substackcdn.com/image/fetch/$s_!rF9U!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7961f29f-901e-4a0e-8d10-f586e193bc65_2050x367.png)Data Replication vs Partitioning

But partitioning the cache to handle dynamic load is a _difficult_ problem.

A naive approach is **Static Hash Partitioning**. Here’s how it works:

  1. Hash the data key

  2. Do a modulo operation of the generated hash code against the number of cache servers. This finds you the cache server ID

  3. Store data key in the cache server

    
    
    Cache Server ID = Hash(data key) mod n
    
    where n is the number of cache servers

[![Consistent hashing; Static hash partition](https://substackcdn.com/image/fetch/$s_!3mG2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7c65b095-e8c3-4efa-a123-db867651ac96_1453x1049.png)](https://substackcdn.com/image/fetch/$s_!3mG2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7c65b095-e8c3-4efa-a123-db867651ac96_1453x1049.png)Static Hash Partitioning

But existing mapping between data keys and cache servers will break if a cache server fails.

Also installing an extra cache server to handle more load will break the mapping.

A workaround is to rehash the data keys. But it’s an expensive operation.

So _static hash partitioning won’t handle a dynamic load_ without service degradation.

A simple solution to this problem is consistent hashing.

* * *

## [The .NET Weekly (Featured)](https://www.milanjovanovic.tech/?utm_source=neo)

[![The .NET Weekly](https://substackcdn.com/image/fetch/$s_!rekW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ad4ec7f-9613-4484-82bd-d8ac2d05bda9_2000x400.png)](https://substackcdn.com/image/fetch/$s_!rekW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ad4ec7f-9613-4484-82bd-d8ac2d05bda9_2000x400.png)

Want to become a better software engineer? Each week, Milan sends one piece of practical advice about .NET and software architecture. It's a 5-minute read (or less) and comes every Saturday morning. Join 32,000+ engineers here:

[Try it](https://www.milanjovanovic.tech/?utm_source=neo)

* * *

## Consistent Hashing

Consistent hashing places the cache servers on a virtual ring structure. It's called the **hash ring**.

The output space of the hash function gets mapped onto a circular space to create the hash ring. In other words, the biggest hash value gets wrapped around the smallest hash value.

[![Consistent hashing; server position](https://substackcdn.com/image/fetch/$s_!N3Mk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4ed6074-00de-40af-9daf-79befdd6c034_1054x1262.png)](https://substackcdn.com/image/fetch/$s_!N3Mk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4ed6074-00de-40af-9daf-79befdd6c034_1054x1262.png)Servers in Hash Ring

This is how consistent hashing finds the **position of a cache server** :

  1. Hash the IP address or domain name of the cache server

  2. Base convert generated hash code

  3. Do modulo operation on the hash code against the number of positions in the hash ring

  4. Place the cache server in the hash ring

[![Consistent hashing; Data insertion](https://substackcdn.com/image/fetch/$s_!y_v2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22c3a648-0028-4c4c-9d89-192821f3c475_1092x1430.png)](https://substackcdn.com/image/fetch/$s_!y_v2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22c3a648-0028-4c4c-9d89-192821f3c475_1092x1430.png)Data Insertion in Hash Ring

This is how consistent hashing finds the cache server to **store a data key** :

  1. Hash the data key using the same hash function

  2. Base convert generated hash code

  3. Traverse the hash ring in the clockwise direction until a cache server

  4. Store the data key in the cache server

Each cache server becomes responsible for the region between itself and its predecessor.

[![Consistent hashing; Server failure](https://substackcdn.com/image/fetch/$s_!-mS7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9c3e8e74-6bc6-46f6-b833-46bd217041d7_1210x1135.png)](https://substackcdn.com/image/fetch/$s_!-mS7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9c3e8e74-6bc6-46f6-b833-46bd217041d7_1210x1135.png)Data Movement on a Server Failure

The data keys get moved to the immediate cache server in the clockwise direction if a cache server fails. While the remaining cache servers remain unaffected.

Besides only data keys that fall within the range of the new cache server get moved out when a cache server gets provisioned. 

So _consistent hashing reduces the data movement_ when the number of servers changes.

Also the data keys need not be uniformly distributed across cache servers in the hash ring.

[![Consistent hashing; Virtual nodes](https://substackcdn.com/image/fetch/$s_!gpmH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F56f25b26-a702-4338-a157-8b1fe52ac529_1127x1168.png)](https://substackcdn.com/image/fetch/$s_!gpmH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F56f25b26-a702-4338-a157-8b1fe52ac529_1127x1168.png)Virtual Nodes in Hash Ring

So a single cache server can be assigned to many positions on the hash ring. 

This can be done by hashing server ID through different hash functions. It’s called **virtual nodes**. And it prevents hot spots in the hash ring.

* * *

## How to Implement Consistent Hashing?

A self-balancing binary search tree can be used to store the server positions in the hash ring. Because it offers _logarithmic time complexity_ for search, insert, and delete operations.

[![Consistent hashing implementation](https://substackcdn.com/image/fetch/$s_!iA23!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47d3ce15-811f-4c0b-9112-409e498fffe5_2989x1168.png)](https://substackcdn.com/image/fetch/$s_!iA23!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47d3ce15-811f-4c0b-9112-409e498fffe5_2989x1168.png)Consistent Hashing Implementation

Here is what happens when a data key gets inserted:

  1. Hash the data key

  2. Find the cache server by searching the binary search tree in logarithmic time

  3. Store the data key in the cache server

Besides each cache server can keep a binary search tree to track the data keys stored by it.

It makes data movement easier when a cache server gets provisioned or decommissioned.

[![Consistent hashing; Asymptotic complexity](https://substackcdn.com/image/fetch/$s_!R9Uk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6927e98-b7c4-4133-b22e-8af46da7dfc9_1620x510.png)](https://substackcdn.com/image/fetch/$s_!R9Uk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6927e98-b7c4-4133-b22e-8af46da7dfc9_1620x510.png)Asymptotic Complexity; k = number of data keys, n = number of cache servers

* * *

## Consistent Hashing Use Cases

Other popular use cases of consistent hashing are:

  * Partitioning data in Amazon Dynamo and Apache Cassandra databases

  * Load balancing video stream traffic in Vimeo 

  * Distributing video content across CDN by Netflix

  * Finding a specific Discord server by the Discord client

## Consistent Hashing Advantages

The advantages of consistent hashing are:

  * Easy horizontal scalability

  * Minimal data movement when the number of servers change

  * Easy partitioning of data

## Consistent Hashing Disadvantages

The disadvantages of consistent hashing are:

  * Hot spots in the hash ring could cause cascading failure

  * Risk of non-uniform data distribution

  * Server capacity is not taken into account 

Consistent hashing is widely used in distributed systems. So it’s important to understand it.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/what-is-consistent-hashing?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![Everything You Need to Know About Micro Frontends](https://substackcdn.com/image/fetch/$s_!tijZ!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fedb1b927-4c3a-4b8d-a9e2-650da7093495_1280x720.png)Everything You Need to Know About Micro Frontends[NK](https://substack.com/profile/135589200-nk)·November 14, 2023[Read full story](https://newsletter.systemdesign.one/p/micro-frontends)](https://newsletter.systemdesign.one/p/micro-frontends)

[![How Does Netflix Work?](https://substackcdn.com/image/fetch/$s_!mkdo!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1d73ba16-80c9-4237-ba4a-95b88698d1bc_1280x720.gif)How Does Netflix Work?[NK](https://substack.com/profile/135589200-nk)·November 16, 2023[Read full story](https://newsletter.systemdesign.one/p/how-does-netflix-work)](https://newsletter.systemdesign.one/p/how-does-netflix-work)

* * *

## References

  * https://github.com/papers-we-love/papers-we-love/blob/main/distributed_systems/consistent-hashing-and-random-trees.pdf

  * https://systemdesign.one/consistent-hashing-explained/

55

2

7

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Karane Vieira's avatar](https://substackcdn.com/image/fetch/$s_!kLTT!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F04b3903f-5252-41d5-a55d-d52f4f83b35f_96x96.jpeg)](https://substack.com/profile/115439202-karane-vieira?utm_source=comment)

[Karane Vieira](https://substack.com/profile/115439202-karane-vieira?utm_source=substack-feed-item)

[Dec 26, 2023](https://newsletter.systemdesign.one/p/what-is-consistent-hashing/comment/46053427 "Dec 26, 2023, 12:56 PM")

Liked by Neo Kim

Really nice article. I was not familiar with this solution. In the implementation section, maybe you should add where to store the balanced tree. Should that be in Zookeeper? In a separate cache server reserved just for the application configuration?

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/what-is-consistent-hashing/comment/46053427)

[1 more comment...](https://newsletter.systemdesign.one/p/what-is-consistent-hashing/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture