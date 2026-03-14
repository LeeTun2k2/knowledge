[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# 8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers

### #1: Learn More - Awesome WhatsApp Engineering (6 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Aug 27, 2023

767

25

69

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the incredible story of WhatsApp co-founder Jan Koum. And the engineering techniques used to scale WhatsApp. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/whatsapp-engineering/?action=share) & I'll send you some rewards for the referrals._

January 2008 - California, United States.

Jan Koum, an engineer at Yahoo, applies for work at Facebook - _rejected_.

It was not the end - he moved on with his life _._

He buys an iPhone next year and immediately recognizes the huge potential of the new App Store.

So he decided to build an instant messenger with some of his former coworkers from Yahoo. And named it WhatsApp. The vision behind WhatsApp was to replace the expensive SMS.

With 1 million people signing up each day, the growth rate of WhatsApp was mind-boggling.

WhatsApp was able to support _50 billion messages a day_ from 450 million daily active users. And they did it with only _32 engineers_.

Although explosive product growth is a good problem to have, Jan Koum and the WhatsApp team had to adopt the best engineering practices to overcome the challenges.

* * *

## WhatsApp Engineering

WhatsApp engineering practices to meet extreme scalability were:

### 1\. Single Responsibility Principle

They put _product focus_ only on the core feature - _messaging_.

And didn’t bother to build an advertising network or a social media platform.

[![WhatsApp Engineering; Single responsibility principle](https://substackcdn.com/image/fetch/$s_!JOaV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff7395296-30ad-4a27-acb0-d9bb668163b5_3218x1175.png)](https://substackcdn.com/image/fetch/$s_!JOaV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff7395296-30ad-4a27-acb0-d9bb668163b5_3218x1175.png)Single Responsibility Principle

Also they _eliminated feature creep_ at all costs.

Feature creep occurs when you add excessive features to a product. And make it difficult to use.

Besides they focused on the reliability of WhatsApp over everything else.

### 2\. Technology Stack

They used Erlang to build the core functionalities of WhatsApp servers. Because it:

  * Provides high scalability with a tiny footprint

  * And supports hot-loading

Threads are a native feature of Erlang. But in Java or C++ threads belong to the operating system. So there is no need to save the entire CPU state in Erlang. And this makes context switching cheaper.

Hot loading makes it easier to deploy code changes without a server restart. Or traffic redirection. In simple words, Hot loading offers high availability.

### 3\. Why Reinvent the Wheel?

_Don’t reinvent the wheel_ \- either use open source or buy a commercial solution.

[![WhatsApp Engineering; Do not reinvent the wheel](https://substackcdn.com/image/fetch/$s_!s239!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67c9df99-7049-428c-b7c9-65b2220d3259_1398x709.png)](https://substackcdn.com/image/fetch/$s_!s239!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67c9df99-7049-428c-b7c9-65b2220d3259_1398x709.png)Don’t Reinvent the Wheel

Ejabberd is an open-source real-time messaging server written in Erlang.

And they built WhatsApp _on top of ejabberd_. Also they rewrote some of the ejabberd core components to meet their needs.

Besides WhatsApp leveraged third-party services such as Google Push to provide push notifications.

### 4\. Cross-Cutting Concerns

They put huge _emphasis_ on _cross-cutting concerns_ _to improve product quality_.

Cross-cutting concerns are things that affect many parts of a product. And are hard to separate. For example, monitoring and alerting the health of the services.

[![WhatsApp engineering; Cross-cutting concerns](https://substackcdn.com/image/fetch/$s_!GrSs!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F27bf54b7-ffea-432b-909a-698a240820b4_1589x1437.png)](https://substackcdn.com/image/fetch/$s_!GrSs!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F27bf54b7-ffea-432b-909a-698a240820b4_1589x1437.png)Cross-Cutting Concerns

And they improved the _software development process_ with _Continuous integration_ and _Continuous delivery._

Continuous integration is the process of merging the code changes regularly into a central repository.

Continuous delivery is the process of code deployment to a testing or production environment.

### 5\. Scalability

WhatsApp __ used _diagonal scaling_ to keep the costs and operational complexity low.

Horizontal scaling is the process of increasing the number of machines in the resource pool.

Vertical scaling is the process of increasing the capacity of an existing machine, such as the CPU or memory.

And diagonal scaling is a hybrid of horizontal and vertical scaling. The computing resources get added both vertically and horizontally.

[![WhatsApp engineering; Scalability](https://substackcdn.com/image/fetch/$s_!ohzS!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5874f06d-4984-4c36-9042-241b2809287d_2019x1521.png)](https://substackcdn.com/image/fetch/$s_!ohzS!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5874f06d-4984-4c36-9042-241b2809287d_2019x1521.png)Scalability

They ran WhatsApp servers on the FreeBSD operating system. Because they had previous experience with FreeBSD while working at Yahoo. Besides FreeBSD offered a reliable network stack.

Also they fine-tuned FreeBSD to accommodate _2 million+ connections per server_. And modified kernel parameters such as files and sockets.

They _overprovisioned servers_ to handle sudden traffic spikes and keep headroom for failures. For example, failures such as network partitions or hardware faults.

### 6\. Flywheel Effect

They _measured_ the metrics such as CPU, context switches, and system calls. Then _identified_ and _eliminated_ the _bottlenecks_. And they did this at regular intervals.

[![WhatsApp Engineering; Continuous feedback cycle](https://substackcdn.com/image/fetch/$s_!rwcp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc19510e6-36df-4f88-aa0f-b8e5f9770775_1360x721.png)](https://substackcdn.com/image/fetch/$s_!rwcp!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc19510e6-36df-4f88-aa0f-b8e5f9770775_1360x721.png)Continuous Feedback Cycle

The _continuous feedback cycle_ tremendously improved the performance of WhatsApp.

### 7\. Quality

They used load testing to _identify single points of failure._

Load testing is the process of measuring the performance of the system under the anticipated load.

[![WhatsApp Engineering; Load testing](https://substackcdn.com/image/fetch/$s_!0Uvv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe0cb8209-2b85-427e-8f11-3821b67cd133_1483x1537.png)](https://substackcdn.com/image/fetch/$s_!0Uvv!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe0cb8209-2b85-427e-8f11-3821b67cd133_1483x1537.png)Load Testing

And they used artificial production traffic and DNS configuration changes for load testing.

### 8\. Small Team Size

The _communication paths_ between engineers increase quadratically as the team size grows. This is a recipe for _degraded productivity_.

[![WhatsApp Engineering; Communication paths between engineers](https://substackcdn.com/image/fetch/$s_!jAFM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1d8f3e5a-e37f-4b96-affe-25aefd1630d2_1378x1160.png)](https://substackcdn.com/image/fetch/$s_!jAFM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1d8f3e5a-e37f-4b96-affe-25aefd1630d2_1378x1160.png) Communication Paths Between Engineers

So they kept the team size small - _32 engineers_.

* * *

WhatsApp is one of the most successful instant messengers in the market.

In 2014, the same Facebook that rejected Jan Koum acquired WhatsApp for a whopping 19 billion USD.

According to Forbes, Jan Koum has a net worth of 14 billion USD in 2023.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/whatsapp-engineering?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

* * *

[![This Is How Quora Shards MySQL to Handle 13+ Terabytes](https://substackcdn.com/image/fetch/$s_!oxfV!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fef236a61-e44d-4b75-929a-a99dce9d4e56_1280x720.png)This Is How Quora Shards MySQL to Handle 13+ Terabytes[NK](https://substack.com/profile/135589200-nk)·September 3, 2023[Read full story](https://newsletter.systemdesign.one/p/mysql-sharding)](https://newsletter.systemdesign.one/p/mysql-sharding)

[![Tumblr Shares Database Migration Strategy With 60+ Billion Rows](https://substackcdn.com/image/fetch/$s_!JUzq!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7d7a33d0-f8dd-4b24-828c-66d166d90723_1280x720.png)Tumblr Shares Database Migration Strategy With 60+ Billion Rows[NK](https://substack.com/profile/135589200-nk)·September 10, 2023[Read full story](https://newsletter.systemdesign.one/p/how-to-migrate-a-mysql-database)](https://newsletter.systemdesign.one/p/how-to-migrate-a-mysql-database)

* * *

## References

  * http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html

  * https://www.shopify.com/partners/blog/feature-creep

  * https://stackoverflow.com/questions/2708033/technically-why-are-processes-in-erlang-more-efficient-than-os-threads

  * https://www.ejabberd.im/index.html

  * https://en.wikipedia.org/wiki/Jan_Koum

  * https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment

  * https://www.nops.io/blog/horizontal-vs-vertical-scaling/

  * https://www.javatpoint.com/scaling-in-cloud-computing

  * https://www.businessinsider.com/whatsapp-built-using-erlang-and-freebsd-2015-10

  * https://www.blazemeter.com/blog/performance-testing-vs-load-testing-vs-stress-testing

  * Thumbnail Photo by [Anton from Pexels](https://www.pexels.com/photo/a-person-holding-a-cellphone-with-logo-on-the-screen-4132538/)

  * Rick Reed. [WhatsApp: Half a billion unsuspecting FreeBSD users](https://www.youtube.com/watch?v=TneLO5TdW_M)

  * Anton Lavrik. (2018) [A Reflection on Building the WhatsApp Server - Code BEAM](https://www.youtube.com/watch?v=LJx6mUEFAqQ)

767

25

69

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Math police's avatar](https://substackcdn.com/image/fetch/$s_!i9GT!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fpurple.png)](https://substack.com/profile/165546635-math-police?utm_source=comment)

[Math police](https://substack.com/profile/165546635-math-police?utm_source=substack-feed-item)

[Aug 28, 2023](https://newsletter.systemdesign.one/p/whatsapp-engineering/comment/39176019 "Aug 28, 2023, 8:02 AM")

Liked by Neo Kim

Hi,

You write "The communication paths between engineers increase exponentially as the team grows in size.", but in fact the total number of communication paths between n nodes is equal to n * (n-1) / 2, which is not an exponential growth, but an polynomial growth (quadratic to be exact), i.e. O(n^2). 

ReplyShare

[4 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/whatsapp-engineering/comment/39176019)

[![Bogdan Veliscu's avatar](https://substackcdn.com/image/fetch/$s_!J8QA!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8794f76d-22a4-4475-b0d3-8af9c9cc21b3_800x800.png)](https://substack.com/profile/115684107-bogdan-veliscu?utm_source=comment)

[Bogdan Veliscu](https://substack.com/profile/115684107-bogdan-veliscu?utm_source=substack-feed-item)

[Mar 17, 2024](https://newsletter.systemdesign.one/p/whatsapp-engineering/comment/51829247 "Mar 17, 2024, 10:02 AM")

Liked by Neo Kim

Great read, Neo! In short don’t reinvent the wheel and avoid complexity as long as possible.

ReplyShare

[23 more comments...](https://newsletter.systemdesign.one/p/whatsapp-engineering/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture