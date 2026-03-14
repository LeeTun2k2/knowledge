[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Disney+ Hotstar Scaled to 25 Million Concurrent Users

### #6: You Need to Read This - Amazing Scalability Patterns (6 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 17, 2023

68

6

3

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

February 2015 - Mumbai, India.

There was an increasing demand for online streaming in India.

Star India, a 21st Century Fox subsidiary, decided to take the opportunity. And launched Hotstar.

Hotstar is a subscription-based video streaming service. They hit the _25 million concurrent users_ record from the raving fans of live cricket in India.

_This post outlines the scalability techniques from Hotstar._ _If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/hotstar-scaling/?action=share) & I'll send you some rewards for the referrals._

* * *

## Hotstar Scaling

Here are the scalability techniques:

### 1\. Autoscaling Is Not Enough

They _didn't_ rely on _autoscaling_ to handle traffic spikes. Because it took time for a newly provisioned server to become healthy. The observed time range was 90 seconds.

[![Hotstar Scaling; Autoscaling](https://substackcdn.com/image/fetch/$s_!qUND!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd88eafaf-075b-4f3e-9a31-00a603c178de_1227x292.png)](https://substackcdn.com/image/fetch/$s_!qUND!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd88eafaf-075b-4f3e-9a31-00a603c178de_1227x292.png)Autoscaling

Instead they prewarmed their infrastructure _ahead_ of major sports events. They did it by estimating the peak concurrent traffic. The historical traffic patterns helped them with estimations.

They _prewarmed_ their infrastructure with _autoscaling_. Also tuned their servers to run at full capacity.

### 2\. Avoid the Thundering Herd

The thundering herd problem occurs if many concurrent clients query a server. It results in degraded performance.

[![Hotstar Scaling; Thundering Herd](https://substackcdn.com/image/fetch/$s_!Kgfu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4208b291-dd8a-4b20-a63f-073a2c59b04f_1024x805.jpeg)](https://substackcdn.com/image/fetch/$s_!Kgfu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4208b291-dd8a-4b20-a63f-073a2c59b04f_1024x805.jpeg)[The Thundering Herd Problem](https://en.wikipedia.org/wiki/The_Thundering_Herd_%281925_film%29)

Their _techniques_ to avoid the thundering herd problem:

  * Add jitter to client requests

  * Activate client caching

  * Allow exponential backoff by the client

### 3\. Apply the Pareto Principle

They studied the traffic patterns to _identify_ _important_ _components_. 

The result: subscription engine, metadata engine, and streaming infrastructure. And they put full _focus_ on the high availability of important components.

[![Hotstar Scaling; Pareto principle](https://substackcdn.com/image/fetch/$s_!oC3B!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4bafdc4b-1a12-4991-b92a-c78d0abe65ad_2413x1162.png)](https://substackcdn.com/image/fetch/$s_!oC3B!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4bafdc4b-1a12-4991-b92a-c78d0abe65ad_2413x1162.png)Rank the important components

Also they tweaked each component. It allowed them to meet unique scalability needs.

Besides they kept their system design simple. It enabled them to achieve operational excellence.

They added multi-level redundancy to overcome server failures.

### 4\. Don’t Repeat Yourself

They used a _multi-level cache_ to improve read scalability. Their key idea was to reduce the traffic that reaches the origin server. And prevent repeating expensive operations.

[![Hotstar scaling; Multi-level cache](https://substackcdn.com/image/fetch/$s_!1XZs!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb5f3737-77fd-483a-bc48-02e4772c9535_873x742.png)](https://substackcdn.com/image/fetch/$s_!1XZs!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb5f3737-77fd-483a-bc48-02e4772c9535_873x742.png)Multi-level cache to scale

They used caching with the right time-to-live (**TTL**). This allowed them to keep the system functional even when the origin server failed.

Also they monitored the TTL of every cache level. Because they wanted to prevent the thundering herd and avoid data consistency issues.

### 5\. Rate Limit the Traffic

They rate limited the traffic. So, they were able to prevent unwanted traffic from overloading the servers.

[![Hotstar scaling; Rate limiter](https://substackcdn.com/image/fetch/$s_!ywGC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3739e2a7-a25b-49dd-b057-2e37c3783f27_1824x436.png)](https://substackcdn.com/image/fetch/$s_!ywGC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3739e2a7-a25b-49dd-b057-2e37c3783f27_1824x436.png)Rate limit unwanted traffic

Also they kept a denial list. This allowed them to stop suspicious traffic from degrading the system.

### 6\. Enable Panic Mode

They implemented panic mode on the server. This allowed them to inform the client of server overload. 

Also they configured the client to exponentially back off. This technique provided _high availability_.

[![Hotstar Scaling; Panic mode](https://substackcdn.com/image/fetch/$s_!aTb9!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb053980-d17e-48be-8557-43802be8c55a_500x756.png)](https://substackcdn.com/image/fetch/$s_!aTb9!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb053980-d17e-48be-8557-43802be8c55a_500x756.png)Favor essential services in panic mode; Created with [imgflip](https://imgflip.com/)

Here is what they did in panic mode: _favor_ the availability of _important components_. And _gracefully degrade_ _non-essential_ components. For example, their recommendations service. This allowed them to free up and reuse the servers that ran non-essential components.

Also they allowed users to _bypass_ the non-essential components. This improved the reliability.

### 7\. Testing

They identified bottlenecks through tests.

[![Hotstar scaling; Testing](https://substackcdn.com/image/fetch/$s_!PJPq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F58d80990-b667-4738-ba63-53b82a19cdfd_1500x1255.png)](https://substackcdn.com/image/fetch/$s_!PJPq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F58d80990-b667-4738-ba63-53b82a19cdfd_1500x1255.png)Test to identify bottlenecks

This is how they did it:

_Chaos testing_ checks whether the system will remain functional if certain services fail.

_Performance testing_ checks whether the system will degrade when the number of users grows. They used the [flood.io](https://www.flood.io/) tool to run performance tests.

_Load testing_ checks the system performance under an expected number of concurrent users. They used the [gatling.io](https://gatling.io/) tool to run load tests.

### 8\. Orchestrate the Infrastructure

They considered reliable _infrastructure_ to be the foundation of scalability.

[![Hotstar scaling; Containers and orchestration](https://substackcdn.com/image/fetch/$s_!yTfX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3555b629-ba16-4f89-a450-ed74cc82a28a_943x754.png)](https://substackcdn.com/image/fetch/$s_!yTfX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3555b629-ba16-4f89-a450-ed74cc82a28a_943x754.png)Containers and orchestration

They _avoided configuration tools_ like Chef and Puppet. Because it added further delay to the newly provisioned server from becoming healthy.

Instead they used _baked container images_. It included everything necessary to become healthy after provisioning. This allowed quick scalability.

Also they used _proven technologies_ to simplify infrastructure operations. For example, Kubernetes.

### 9\. Everything Is a Trade-off

Cloud providers have limitations. AWS sets a limit on the _number of servers_ available for a specific [instance type](https://aws.amazon.com/ec2/instance-types/). It reduced the server availability for Hotstar during peak traffic. Their workaround was to _use different instance types_ \- and get more servers.

[![Hotstar scaling; Everything is a trade-off](https://substackcdn.com/image/fetch/$s_!HcH5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F353de602-d66c-472c-80a2-61873a7dcdca_970x897.png)](https://substackcdn.com/image/fetch/$s_!HcH5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F353de602-d66c-472c-80a2-61873a7dcdca_970x897.png)Everything is a trade-off

Also AWS _doesn't allow fine-tuning autoscaling_ logic. So, they added a custom autoscaling script to meet their needs.

### 10\. Tweak the Software

Unique problems need a unique solution. So does system design.

[![Hotstar scalability; Specialised software](https://substackcdn.com/image/fetch/$s_!wgyy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd61c16ce-66eb-477e-86ba-8a9518109478_2344x1103.png)](https://substackcdn.com/image/fetch/$s_!wgyy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd61c16ce-66eb-477e-86ba-8a9518109478_2344x1103.png)Tweak software for better performance

They studied the data patterns to tweak their system. For example, they tweaked the operating system kernel and application libraries. This improved performance.

### 11\. Flywheel Effect

They identified bottlenecks by profiling. For example, they observed that users with limited network bandwidth had poor user experience. So, they tweaked the bit-rate settings.

[![Hotstar scalability; Continuous feedback loop](https://substackcdn.com/image/fetch/$s_!6qCQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3c3370d1-e631-4500-a7d4-9801fa6b3a72_1629x1040.png)](https://substackcdn.com/image/fetch/$s_!6qCQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3c3370d1-e631-4500-a7d4-9801fa6b3a72_1629x1040.png)Continuous feedback loop

They put learning from failures in a loop. And this constant loop improved their performance.

### 12\. Keep an Open Communication Channel

Monitoring important system metrics was crucial for _performance_.

[![Hotstar scaling; Open communication](https://substackcdn.com/image/fetch/$s_!DWYr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fee1406c0-9876-468f-8692-1ba2037d885e_634x529.png)](https://substackcdn.com/image/fetch/$s_!DWYr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fee1406c0-9876-468f-8692-1ba2037d885e_634x529.png)Ensure that key stakeholders are on the same page

So, they maintained an open communication channel between stakeholders. This helped to _align_ on the important system _metrics_.

### 13\. Grow the Scalability Toolbox

They knew that there was _no silver bullet_ to scalability. Because every tool has its limitations. So, they used a combination of many tools and techniques to scale. And kept introducing new scalability techniques.

[![Hotstar scaling; Scalability toolbox](https://substackcdn.com/image/fetch/$s_!pRXc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F49ac43bf-cd7e-400f-b0a2-3c2388a3bbc4_1376x538.png)](https://substackcdn.com/image/fetch/$s_!pRXc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F49ac43bf-cd7e-400f-b0a2-3c2388a3bbc4_1376x538.png)Scalability toolbox

Also they studied the _traffic pattern_ to get the best out of each tool.

* * *

This case study shows that preparing for failures is important to scale. Hotstar claims that its current scalability techniques will handle _50 million concurrent users_.

Walt Disney acquired Hotstar as part of its acquisition of 21st Century Fox. And Disney+ Hotstar remains a major player in India's streaming industry.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/whatsapp-engineering?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjoxMzU1ODkyMDAsInBvc3RfaWQiOjEzNTk4NjUwNiwiaWF0IjoxNzAyOTA1NjA0LCJleHAiOjE3MDU0OTc2MDQsImlzcyI6InB1Yi0xNTExODQ1Iiwic3ViIjoicG9zdC1yZWFjdGlvbiJ9.bEcfsA8y-NXfI-ZIyT0n9s3t606G94soeu-3g65iq_4)

* * *

[![8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers](https://substackcdn.com/image/fetch/$s_!88vE!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94e067cc-6ade-44bf-9818-5dc20a260541_1280x720.png)8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers[NK](https://substack.com/profile/135589200-nk)·August 27, 2023[Read full story](https://newsletter.systemdesign.one/p/whatsapp-engineering)](https://newsletter.systemdesign.one/p/whatsapp-engineering)

[![Tumblr Shares Database Migration Strategy With 60+ Billion Rows](https://substackcdn.com/image/fetch/$s_!JUzq!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7d7a33d0-f8dd-4b24-828c-66d166d90723_1280x720.png)Tumblr Shares Database Migration Strategy With 60+ Billion Rows[NK](https://substack.com/profile/135589200-nk)·September 10, 2023[Read full story](https://newsletter.systemdesign.one/p/how-to-migrate-a-mysql-database)](https://newsletter.systemdesign.one/p/how-to-migrate-a-mysql-database)

* * *

Word-of-mouth referrals like yours help this community grow - Thank you.

[![System Design Newsletter Feedback](https://substackcdn.com/image/fetch/$s_!Xm3t!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F65746305-197f-48f7-b333-ceebc85ae1c2_3273x522.png)](https://substackcdn.com/image/fetch/$s_!Xm3t!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F65746305-197f-48f7-b333-ceebc85ae1c2_3273x522.png)Thanks for the feedback

 _Get featured in the newsletter: Write your feedback on this post, and tag me on[Twitter](https://twitter.com/systemdesign42), [LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/), and [Substack Notes](https://substack.com/@systemdesignone)._

* * *

### References

  * Saxena, A. (2018). _Scaling Is Not An Accident_. [online] Medium. Available at: https://blog.hotstar.com/scaling-is-not-an-accident-895140ac84c0 [Accessed 15 Sep. 2023].

  * Saxena, A. (2019). _Scaling the Hotstar Platform for 50M_. [online] Medium. Available at: https://blog.hotstar.com/scaling-the-hotstar-platform-for-50m-a7f96a019add.

  * www.youtube.com. (n.d.). _Scaling hotstar.com for 25 million concurrent viewers_. [online] Available at [YouTube](https://www.youtube.com/watch?v=QjvyiyH4rr0) [Accessed 15 Sep. 2023].

  * Wikipedia Contributors (2022). _Disney+ Hotstar_. [online] Wikipedia. Available at: https://en.wikipedia.org/wiki/Disney%2B_Hotstar.

68

6

3

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Tarun Pahuja's avatar](https://substackcdn.com/image/fetch/$s_!D6ls!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6848cd5-190f-4656-8a9a-e819d4f029d8_96x96.jpeg)](https://substack.com/profile/85668596-tarun-pahuja?utm_source=comment)

[Tarun Pahuja](https://substack.com/profile/85668596-tarun-pahuja?utm_source=substack-feed-item)

[Oct 19, 2023](https://newsletter.systemdesign.one/p/hotstar-scaling/comment/42110082 "Oct 19, 2023, 3:36 AM")

Liked by Neo Kim

I think you should pick one aspect of scaling and explain it in deep. 

ReplyShare

[2 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/hotstar-scaling/comment/42110082)

[![Andre Nader's avatar](https://substackcdn.com/image/fetch/$s_!eM3b!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F84ada029-6f66-4d8f-89af-e69d93446845_1648x1872.jpeg)](https://substack.com/profile/14824538-andre-nader?utm_source=comment)

[Andre Nader](https://substack.com/profile/14824538-andre-nader?utm_source=substack-feed-item)

[Sep 20, 2023](https://newsletter.systemdesign.one/p/hotstar-scaling/comment/40417093 "Sep 20, 2023, 11:39 PM")

Liked by Neo Kim

I appreciate the mention! I think that is the first time I’ve noticed Substack alert me to it. Definitely a smart notification for Substack team to have built, they just fostered a new writer to writer connection.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/hotstar-scaling/comment/40417093)

[4 more comments...](https://newsletter.systemdesign.one/p/hotstar-scaling/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture