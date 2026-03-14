[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Meta Achieves 99.99999999% Cache Consistency 🎯

### #52: Break Into Meta Engineering (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jul 18, 2024

∙ Paid

186

3

19

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

A fundamental way to scale a distributed system is to avoid coordination between components.

And cache helps to avoid the coordination needed to access the database. So cache data correctness is important for scalability.

  * _[Share this post](https://newsletter.systemdesign.one/p/cache-consistency/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, Facebook ran a simple tech stack - PHP & MySQL.

But as more users joined, they faced scalability problems.

So they set up a distributed cache[1](https://newsletter.systemdesign.one/p/cache-consistency#footnote-1-145887884).

Although it temporarily solved their scalability issue, maintaining fresh cache data became difficult. Here’s a common race condition:

  1. The client queries the cache for a value not present in it

  2. So the cache queries the database for the value: x = 0

  3. In the meantime, the value in the database gets changed: x = 1

  4. But the cache invalidation event reaches the cache first: x = 1

  5. Then the value from cache fill reaches the cache: x = 0

[![Race Condition During Cache Invalidation](https://substackcdn.com/image/fetch/$s_!CZZZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd4b0feb-f94b-4067-9f71-a40a7afc8e70_1514x1023.png)](https://substackcdn.com/image/fetch/$s_!CZZZ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd4b0feb-f94b-4067-9f71-a40a7afc8e70_1514x1023.png)Race Condition During Cache Invalidation

Now database: x = 1, while cache: x = 0. So there’s cache inconsistency.

Yet their growth rate was explosive. And became the third most visited site in the world.

[![Cache Consistency](https://substackcdn.com/image/fetch/$s_!LTLQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fac3f31fe-c5e3-4f80-8b99-52d7bac53149_800x500.gif)](https://substackcdn.com/image/fetch/$s_!LTLQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fac3f31fe-c5e3-4f80-8b99-52d7bac53149_800x500.gif)

Now they serve a quadrillion (1015) requests per day. So even a 1% cache miss rate is expensive - 10 trillion cache fills[2](https://newsletter.systemdesign.one/p/cache-consistency#footnote-2-145887884) a day.

_This post outlines how Meta uses observability to improve cache consistency. It doesn’t cover how they invalidate cache_**,**_but how they find when to invalidate cache. You will find references at the bottom of this page if you want to go deeper._

This case study assumes a simple data model and the database & cache are aware of each other.

* * *

## Cache Consistency

Cache **inconsistency** feels like data loss from a user’s perspective.

So they created an _observability_ solution.

And here’s how they did it:

### 1\. Monitoring 📈

They created a separate service to monitor cache inconsistency & called it **Polaris**.

[![Polaris Monitoring Cache Inconsistency](https://substackcdn.com/image/fetch/$s_!1TFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F06c91dfe-41fd-4fd1-8614-f1f1bf00b6ca_1352x408.png)](https://substackcdn.com/image/fetch/$s_!1TFk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F06c91dfe-41fd-4fd1-8614-f1f1bf00b6ca_1352x408.png)Polaris Monitoring Cache Inconsistency

Here’s how Polaris works:

  * It acts like a cache server & receives cache invalidation events

  * Then it queries cache servers to find data inconsistency

  * It queues inconsistent cache servers & checks again later

  * It checks data correctness during writes, so finding cache inconsistency is faster

  * Simply put, it measures cache inconsistency

Besides there’s a risk of network partition between distributed cache & Polaris. So they use a separate invalidation event stream between the client & Polaris.

A simple fix for cache inconsistency is to query the database.

But there’s a risk of database overload at a high scale. So Polaris queries the database at timescales of 1, 5, or 10 minutes. It lets them back off efficiently & improve accuracy.

### 2\. Tracing 🔎

Debugging a distributed cache without logs is hard.

And they wanted to find out why cache inconsistency occurs each time. Yet logging every data change isn’t scalable as it’s _write-heavy_. While the cache is for a read-heavy workload. So they created a **tracing library** & embedded it on each cache server.

[![Logging Data Changes Occuring During the Race Condition Window](https://substackcdn.com/image/fetch/$s_!Pen5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F65b76c37-f8d9-41c1-9f5b-80c7ad44b6c0_1155x244.png)](https://substackcdn.com/image/fetch/$s_!Pen5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F65b76c37-f8d9-41c1-9f5b-80c7ad44b6c0_1155x244.png)Logging Data Changes Occurring During the Race Condition Window

Here’s how it works:

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fcache-consistency&utm_source=paywall&utm_medium=web&utm_content=145887884)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fcache-consistency&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture