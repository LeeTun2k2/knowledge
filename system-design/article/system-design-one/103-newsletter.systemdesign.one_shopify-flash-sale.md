[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Shopify Handles Flash Sales at 32 Million Requests per Minute

### #16: Learn More - Awesome Shopify Engineering (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 19, 2023

50

9

2

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines flash sales at Shopify. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/shopify-flash-sale/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there lived a Shopify server.

He had a single purpose in life: to help people sell products online.

[![Shopify Flash Sale](https://substackcdn.com/image/fetch/$s_!TsUF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F58992685-a418-44a1-b3c9-e32b7b39fb48_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!TsUF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F58992685-a418-44a1-b3c9-e32b7b39fb48_1200x630.gif)

He was the sweet child of Shopify engineers - and built with a simple architecture.

He used Nginx for load balancing because of its fast event loop. And Redis for caching expensive operations.

He was living a Happy life.

[![Shopify Flash Sale; Software Architecture](https://substackcdn.com/image/fetch/$s_!5haF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe9643d6c-ae1c-4f37-8942-a9473d4256b6_1200x630.png)](https://substackcdn.com/image/fetch/$s_!5haF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe9643d6c-ae1c-4f37-8942-a9473d4256b6_1200x630.png)Cartoon Architecture of Shopify

Until one day.

An Evil social media influencer, Pam entered the picture.

And ruined it all.

She wanted to sell a limited edition of shoes in a short time at a discount price - a Flash sale.

[![Shopify Flash Sale; Scalable Software Architecture](https://substackcdn.com/image/fetch/$s_!DlOu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5fa8d77f-f70c-4cdc-876c-26e4c55b05b5_1200x630.png)](https://substackcdn.com/image/fetch/$s_!DlOu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5fa8d77f-f70c-4cdc-876c-26e4c55b05b5_1200x630.png)

So she opened the Bird app and Instagram - and hyped about it.

Shopify server knew what was coming for him: massive traffic.

[![Shopify Flash Sale](https://substackcdn.com/image/fetch/$s_!vAK5!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0dd9b7ac-5080-4376-938b-0c47905b8d71_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!vAK5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0dd9b7ac-5080-4376-938b-0c47905b8d71_1200x630.gif)

.He got Scared and called Shopify engineers for help.

[![Shopify Flash Sale; Architecture](https://substackcdn.com/image/fetch/$s_!Jxoy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F903bede6-fc2c-4407-b7e4-cd96044ae7c5_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!Jxoy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F903bede6-fc2c-4407-b7e4-cd96044ae7c5_1200x630.gif)

So they thought hard.

And came up with 10 simple ideas to handle the Flash sale.

* * *

## Shopify Flash Sale

Here are 10 ideas that Shopify used to handle Flash sales at 32 million requests per minute:

### 1\. Low Timeout

A user wouldn’t wait more than a few seconds for an action to finish.

Besides a client would _waste computing resources_ and increase costs by waiting on an unresponsive server.

[![Shopify Flash Sale; Timeout](https://substackcdn.com/image/fetch/$s_!Pk0e!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3897f072-8130-4b57-aad9-4641e112e5d9_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!Pk0e!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3897f072-8130-4b57-aad9-4641e112e5d9_1200x630.gif)

So they reduced timeout wherever they could in the system.

### 2\. Circuit Breaker

A degraded service is better than a completely down service.

And connecting to a server that failed many times in a short time is bad. Because it prevents the server from becoming healthy again.

[![Shopify Flash Sale; Circuit breaker](https://substackcdn.com/image/fetch/$s_!udYt!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff241e5c6-de2f-4a79-88e6-183ab0ff9fb3_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!udYt!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff241e5c6-de2f-4a79-88e6-183ab0ff9fb3_1200x630.gif)

So they installed the circuit breaker pattern. It stops requests if the circuit breaker is open and protects the database and API server.

### 3\. Rate Limit

Performance degrades if the number of requests exceeds system capacity.

[![Shopify Flash Sale; Rate Limiter](https://substackcdn.com/image/fetch/$s_!vhhu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1c4b1003-f2e9-48a6-96f3-9134443081d8_1200x630.png)](https://substackcdn.com/image/fetch/$s_!vhhu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1c4b1003-f2e9-48a6-96f3-9134443081d8_1200x630.png)

So they used rate limiting and load shedding to _prevent extra requests_.

Also they installed the back pressure pattern. It allows many requests to go through without breaking the service.

Besides they set up fair queueing to give priority to users who came first.

### 4\. Monitoring

Monitoring and alerting the important metrics helps to identify server failure risks quickly.

[![Shopify Flash Sale; Monitoring](https://substackcdn.com/image/fetch/$s_!nJEl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F08a6cbe2-c2f6-4e13-bed5-d2b223febb6a_1200x630.png)](https://substackcdn.com/image/fetch/$s_!nJEl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F08a6cbe2-c2f6-4e13-bed5-d2b223febb6a_1200x630.png)

So they monitored the following metrics:

  * Latency: time it takes to process a unit of work

  * Traffic: the rate at which new work comes into the system in requests per minute

  * Errors: the rate at which unexpected things occur

  * Saturation: system load relative to its total capacity

### 5\. Structured Logging

They needed logs to debug and understand what happened in a web request.

So they stored logs in a central place because there were many servers creating logs. And they wanted to keep it searchable.

[![Shopify Flash Sale; Logging](https://substackcdn.com/image/fetch/$s_!9P9x!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F433e5320-b233-4488-9ed9-ec4307d714cf_1200x630.png)](https://substackcdn.com/image/fetch/$s_!9P9x!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F433e5320-b233-4488-9ed9-ec4307d714cf_1200x630.png)

They structured logs in a machine-readable format to parse and index quickly.

Besides they included _correlation ID_ to allow tracing.

### 6\. Idempotent Keys

The probability of an unreliable event to occur in a distributed system is high, especially with high traffic.

And retry of a failed request should be done safely. For example, they didn’t want to double charge a customer’s card.

[![Shopify Flash Sale; Idempotency Keys](https://substackcdn.com/image/fetch/$s_!ztRs!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F65275322-a87a-4c26-b3d5-032499ada71e_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!ztRs!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F65275322-a87a-4c26-b3d5-032499ada71e_1200x630.gif)

So they sent a _unique idempotency key_ with each request.

### 7\. Consistent Reconciliation

They relied on financial partners. And merged financial data to achieve data consistency.

[![Shopify Flash Sale; Data reconciliation](https://substackcdn.com/image/fetch/$s_!V2ii!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F59ab29ac-95a9-48c8-9e90-48e96b7f0642_1200x630.png)](https://substackcdn.com/image/fetch/$s_!V2ii!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F59ab29ac-95a9-48c8-9e90-48e96b7f0642_1200x630.png)

They tracked any data mismatch and automated the fix.

### 8\. Load Testing

They used load testing to find system bottlenecks and to install protection mechanisms.

[![Shopify Flash Sale; Load testing](https://substackcdn.com/image/fetch/$s_!RnKs!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F39d1105f-653f-4fcd-a2eb-adf735fb4dca_1200x630.png)](https://substackcdn.com/image/fetch/$s_!RnKs!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F39d1105f-653f-4fcd-a2eb-adf735fb4dca_1200x630.png)

They did load testing by simulating a large volume of traffic. 

### 9\. Incident Management and Retrospective

They set up proper incident management to improve service reliability. 

And involved 3-roles in incident management:

  * Incident Manager on Call (IMOC): coordinates the incident

  * Support Response Manager (SRM): responsible for public communication

  * Service Owner: responsible for restoring stability

[![Shopify Flash Sale; Incident management](https://substackcdn.com/image/fetch/$s_!XMEx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F14cbc4ea-6aa2-4658-a1b5-2b51b3a4ea13_1200x630.png)](https://substackcdn.com/image/fetch/$s_!XMEx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F14cbc4ea-6aa2-4658-a1b5-2b51b3a4ea13_1200x630.png)

Also they did incident retrospectives after an incident occurred. This helped them to:

  * Dig deep into what happened

  * Identify wrong assumptions about the system

Besides they came up with action items based on the discussion. And implemented them to prevent the same failure from occurring again.

### 10\. Data Isolation

They kept people’s data in different database shards. So if a database shard crashes it wouldn’t affect another.

[![Shopify Flash Sale; data isolation](https://substackcdn.com/image/fetch/$s_!nDFr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F234891cd-e041-47b0-8813-f2c8483f6155_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!nDFr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F234891cd-e041-47b0-8813-f2c8483f6155_1200x630.gif)

But they shared the stateless workers between shards to get the best performance.

And replicated the database across many data centers for high availability.

* * *

The probability of failures still exists but they reduced the risk of downtime. And limited the scope of impact.

And everybody lived Happily ever after.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/shopify-flash-sale?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

Did you enjoy this post? Then don't forget to hit the Like button ❤️

[![7 Simple Ways to Fail System Design Interview](https://substackcdn.com/image/fetch/$s_!-Sxa!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F541c2154-853f-4a25-b513-7cbe54c4fb47_1280x720.png)7 Simple Ways to Fail System Design Interview[NK](https://substack.com/profile/135589200-nk)·October 10, 2023[Read full story](https://newsletter.systemdesign.one/p/design-system-newsletter)](https://newsletter.systemdesign.one/p/design-system-newsletter)

[![How Giphy Delivers 10 Billion GIFs a Day to 1 Billion Users](https://substackcdn.com/image/fetch/$s_!QOCr!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9ed955a6-ad0b-423f-90c3-c8f5a2fb1ff1_1280x720.gif)How Giphy Delivers 10 Billion GIFs a Day to 1 Billion Users[NK](https://substack.com/profile/135589200-nk)·October 12, 2023[Read full story](https://newsletter.systemdesign.one/p/cdn-explained)](https://newsletter.systemdesign.one/p/cdn-explained)

* * *

## References

  * https://shopify.engineering/building-resilient-payment-systems

  * https://www.usenix.org/conference/srecon16europe/program/presentation/stolarsky

  * https://www.infoq.com/presentations/shopify-architecture-flash-sale/

  * Photo by [Roberto Cortese](https://unsplash.com/@robertocortese?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/F1I4IN86NiE?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

50

9

2

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Stephen Kazibwe's avatar](https://substackcdn.com/image/fetch/$s_!AF-M!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fc5fd3711-f254-4656-b80b-2d3168e9207c_144x144.png)](https://substack.com/profile/75166292-stephen-kazibwe?utm_source=comment)

[Stephen Kazibwe](https://substack.com/profile/75166292-stephen-kazibwe?utm_source=substack-feed-item)

[Nov 3, 2023](https://newsletter.systemdesign.one/p/shopify-flash-sale/comment/42985182 "Nov 3, 2023, 9:54 AM")

Liked by Neo Kim

Great Article. Thanks for sharing.

ReplyShare

[![Gregor Ojstersek's avatar](https://substackcdn.com/image/fetch/$s_!TiaG!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1b7fdc30-d8c4-45f2-b0df-0b60baf9d4f4_1000x1000.jpeg)](https://substack.com/profile/106098672-gregor-ojstersek?utm_source=comment)

[Gregor Ojstersek](https://substack.com/profile/106098672-gregor-ojstersek?utm_source=substack-feed-item)

[Oct 21, 2023](https://newsletter.systemdesign.one/p/shopify-flash-sale/comment/42245368 "Oct 21, 2023, 1:37 PM")

Liked by Neo Kim

Great article! Enjoyed the story like edition with a lot of added visuals.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/shopify-flash-sale/comment/42245368)

[7 more comments...](https://newsletter.systemdesign.one/p/shopify-flash-sale/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture