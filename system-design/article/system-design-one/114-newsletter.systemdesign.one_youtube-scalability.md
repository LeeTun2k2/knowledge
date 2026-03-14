[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# 11 Reasons Why YouTube Was Able to Support 100 Million Video Views a Day With Only 9 Engineers

### #5: Read Now - YouTube Top 11 Scalability Techniques (6 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 16, 2023

39

2

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines YouTube's scalability in its early days. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/youtube-scalability/?action=share) & I'll send you some rewards for the referrals._

February 2005 - California, United States.

3 early employees from PayPal wanted to build a platform to share videos. 

They co-founded YouTube in their garage.

Yet they had limited financial resources. So they funded YouTube through credit card debt and infrastructure borrowing. The financial limitations forced them to create innovative scalability techniques.

In the next year, they hit 100 million video views a day. And they did this with only 9 engineers.

* * *

## YouTube Scalability

Here are the 11 YouTube scalability techniques:

### 1\. Flywheel Effect

They took a _scientific approach_ to scalability: collect and analyze system data.

[![Scalability loop; YouTube scalability](https://substackcdn.com/image/fetch/$s_!aoYR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F01b04421-9742-4366-80d6-86492dba4da2_1847x1216.png)](https://substackcdn.com/image/fetch/$s_!aoYR!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F01b04421-9742-4366-80d6-86492dba4da2_1847x1216.png)Scalability Loop

Their workflow was a constant loop: _identify_ and _fix bottlenecks_. 

This approach avoided the need for high-end hardware and reduced hardware costs.

### 2\. Tech Stack

They kept their tech stack _simple_ and used _proven_ technologies.

[![YouTube tech stack; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!L7CD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F85923639-484a-490a-b97f-9416b5ae69b8_2062x638.png)](https://substackcdn.com/image/fetch/$s_!L7CD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F85923639-484a-490a-b97f-9416b5ae69b8_2062x638.png)YouTube Tech Stack

MySQL stored metadata: video titles, tags, descriptions, and user data. Because it was easy to fix issues in MySQL.

[Lighttpd](https://www.lighttpd.net/) web server served video.

Suse Linux as the operating system. They used Linux tools to inspect the system behavior: strace, ssh, rsync, vmstat, and tcpdump.

Python ran on the application server. Because it offered many reusable libraries and they _didn’t want to reinvent the wheel_. In other words, Python allowed rapid and flexible development. Python was never a bottleneck based on their measurements.

Yet they used Python-to-C compiler and C-language extensions to run CPU-intensive tasks.

### 3\. Keep It Simple

They considered _software architecture_ to be the _root_ of scalability. They _didn’t_ follow _buzzwords_ to scale. But kept the architecture simple - making code reviews easier. And it allowed them to rearchitect fast to meet changing needs. For example, they pivoted from a dating site to a video-sharing site.

They kept the network path simple. Because network appliances have scalability limitations.

[![Hardware costs; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!HMSe!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff7c87721-ea7f-4f9f-a1b6-977f5728683b_1573x1177.png)](https://substackcdn.com/image/fetch/$s_!HMSe!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff7c87721-ea7f-4f9f-a1b6-977f5728683b_1573x1177.png)Hardware Costs

Also they used commodity hardware. It allowed them to reduce power consumption and maintenance fees - and keep the costs low.

Besides they kept the scale-aware code opaque to application development.

### 4\. Choose Your Battles

They _outsourced_ their _problems_. Because they wanted to focus on important things. They didn’t have the time or resources to build their infrastructure to serve popular videos. So they put the _popular videos_ on a third-party _[CDN](https://en.wikipedia.org/wiki/Content_delivery_network)_. The benefits:

  * Low latency due to fewer network hops from the user

  * High performance because it served videos from memory

  * High availability because of automatic replication

They served _less popular videos_ from a [colocated](https://en.wikipedia.org/wiki/Colocation_centre) data center. And used the software RAID to improve the performance through multi-disk access parallelism. Also tweaked their servers to prevent cache thrashing.

They kept their infrastructure in a colocated data center for 2 reasons. To tweak servers with ease to meet their needs and to negotiate their contracts.

[![Choose your fights; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!1n2k!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd93becdb-6aec-4171-90c7-98f3c9c0d479_2261x1729.png)](https://substackcdn.com/image/fetch/$s_!1n2k!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd93becdb-6aec-4171-90c7-98f3c9c0d479_2261x1729.png)Outsourcing Problems to Free Up Resources

Each video had 4 thumbnails. So, they faced problems in serving small objects: lots of disk seeks, and filesystem limits. So, they _put thumbnails_ in [BigTable](https://research.google/pubs/pub27898/). It is a distributed data store with many benefits:

  * Avoids small file problems by clustering files

  * Improved performance

  * Low latency with a multi-level cache

  * Easy to provision

Also they _faked data_ to _prevent expensive transactions_. For example, they faked the video view count and asynchronously updated the counter. A popular technique today to approximate correctness is the [bloom filter](https://systemdesign.one/bloom-filters-explained/). It’s a probabilistic data structure.

### 5\. Pillars of Scalability

They relied on _3 pillars of scalability_ : stateless, replication, and partitioning.

[![Pillars of scalability; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!Ruks!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1a59ed2b-ece4-45bb-882c-371821ee772b_1292x1094.png)](https://substackcdn.com/image/fetch/$s_!Ruks!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1a59ed2b-ece4-45bb-882c-371821ee772b_1292x1094.png)3 Pillars of Scalability

They kept their web servers _stateless_. And scaled it out via replication.

They _replicated_ the database server for read scalability and high availability. And load balanced the traffic among replicas. But this approach caused problems: replication lag and issues with write scalability.

[![replication vs partitioning; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!pesQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb8481fc3-2959-4c5e-b21e-d6cedd5b2629_1737x633.png)](https://substackcdn.com/image/fetch/$s_!pesQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb8481fc3-2959-4c5e-b21e-d6cedd5b2629_1737x633.png)Replication vs Partitioning

So they _partitioned_ the database for improved write scalability, cache locality, and performance. Also it reduced their hardware costs by 30%.

Besides they studied data access patterns to determine the partition level. For example, they studied popular queries, joins, and transactional consistency. And chose the user as the partition level.

### 6\. Solid Engineering Team

A knowledgeable team is an important asset to scalability.

[![engineering team; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!odOp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4e7dbd1e-5b31-4069-9f97-64a86b534d10_3019x696.png)](https://substackcdn.com/image/fetch/$s_!odOp!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4e7dbd1e-5b31-4069-9f97-64a86b534d10_3019x696.png)Cross-disciplinary Team

They kept the _team size small_ for improved communication - 9 engineers. And their team was great at _cross-disciplinary skills_.

### 7\. Don’t Repeat Yourself

They used _cache_ to prevent repeating expensive operations. It allowed them to scale reads.

[![Cache; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!Q-cl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3da7685d-c297-4269-bf1c-ad57447f2c05_1100x821.png)](https://substackcdn.com/image/fetch/$s_!Q-cl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3da7685d-c297-4269-bf1c-ad57447f2c05_1100x821.png)Multi-level Cache to Scale

Also they implemented caching at _many_ levels - it reduced latency.

### 8\. Rank Your Stuff

[![Pareto principle; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!W8Je!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66d9ab14-f746-493e-b641-654253c2c0e0_2413x1138.png)](https://substackcdn.com/image/fetch/$s_!W8Je!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66d9ab14-f746-493e-b641-654253c2c0e0_2413x1138.png)Rank Important Traffic; 80-20 Rule

They ranked _video-watch_ traffic over everything else. So they kept a dedicated cluster of resources for video-watch traffic. It provided high availability.

### 9\. Prevent the Thundering Herd

The thundering herd problem occurs if many concurrent clients query a server. It degrades performance.

[![Thundering herd; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!O7T7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1caba58a-a9df-44d6-99ba-127d94c4444b_1024x805.jpeg)](https://substackcdn.com/image/fetch/$s_!O7T7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1caba58a-a9df-44d6-99ba-127d94c4444b_1024x805.jpeg)[The Thundering Herd Problem](https://en.wikipedia.org/wiki/The_Thundering_Herd_%281925_film%29)

So they added _jitter_ to prevent the thundering herd problem. For example, they added jitter to the cache expiry of popular videos.

### 10\. Play the Long Game

They _focused_ on _macro-level_ of things: algorithms, and scalability. They did quick hacks to buy more time to build long-term solutions. For example, stubbing bad API with Python to prevent short-term problems.

[![Take risks; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!HOor!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F07782fa3-4c06-4505-9f3a-0841b403cc4c_1622x1190.png)](https://substackcdn.com/image/fetch/$s_!HOor!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F07782fa3-4c06-4505-9f3a-0841b403cc4c_1622x1190.png)Take Risks

They _tolerated_ _imperfection_ in their components. When hit a bottleneck: they either rewrote the component or got rid of it.

They _traded_ off _efficiency_ for _scalability_. For example:

  * They chose Python over C

  * They kept clear boundaries between components to scale out. And tolerated latency

  * They optimized the software to be fast enough. But didn’t obsess with machine efficiency

  * They served video from a server location based on bandwidth availability. And not based on latency

### 11\. Adaptive Evolution

They _tweaked_ the system to meet their needs. Examples:

  * Critical components used [RPC](https://en.wikipedia.org/wiki/Remote_procedure_call) instead of HTTP REST. It improved performance

  * Custom [BSON](https://bsonspec.org/) as the data serialization format. It offered high-performance

  * Eventual consistency in certain parts of the application for scalability. For example, the read-your-writes [consistency model](https://systemdesign.one/consistency-patterns/) for user comments

  * Studied Python to prevent common pitfalls. Also relied on profiling

  * Customized open-source software

  * Optimized database queries

  * Made non-critical real-time tasks asynchronous

[![Coding principles; YouTube Scalability](https://substackcdn.com/image/fetch/$s_!_Kh0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd1866d7-4ae4-41c9-9287-1dcf9c7567c8_1554x1543.png)](https://substackcdn.com/image/fetch/$s_!_Kh0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd1866d7-4ae4-41c9-9287-1dcf9c7567c8_1554x1543.png)Coding Principles

They didn’t waste time writing code to restrict people. Instead adopted _great engineering practices_ \- coding conventions to improve their code structure.

* * *

Google acquired YouTube in 2006. And they remain the market leader in video sharing with 5 billion video views a day.

According to Forbes, the founders of YouTube have a net worth of 100+ million USD.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/whatsapp-engineering?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjoxMzU1ODkyMDAsInBvc3RfaWQiOjEzNTk4NjUwNiwiaWF0IjoxNzAzMDE1NjEwLCJleHAiOjE3MDU2MDc2MTAsImlzcyI6InB1Yi0xNTExODQ1Iiwic3ViIjoicG9zdC1yZWFjdGlvbiJ9.kQdwsEKrUzn2IHUiShGEOn27zPuOXpEF2u7BAnqNKF4)

* * *

[![8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers](https://substackcdn.com/image/fetch/$s_!88vE!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94e067cc-6ade-44bf-9818-5dc20a260541_1280x720.png)8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers[NK](https://substack.com/profile/135589200-nk)·August 27, 2023[Read full story](https://newsletter.systemdesign.one/p/whatsapp-engineering)](https://newsletter.systemdesign.one/p/whatsapp-engineering)

[![Tumblr Shares Database Migration Strategy With 60+ Billion Rows](https://substackcdn.com/image/fetch/$s_!JUzq!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7d7a33d0-f8dd-4b24-828c-66d166d90723_1280x720.png)Tumblr Shares Database Migration Strategy With 60+ Billion Rows[NK](https://substack.com/profile/135589200-nk)·September 10, 2023[Read full story](https://newsletter.systemdesign.one/p/how-to-migrate-a-mysql-database)](https://newsletter.systemdesign.one/p/how-to-migrate-a-mysql-database)

* * *

## References

  * Seattle Conference on Scalability: YouTube Scalability. (2012). _YouTube_. Available at: [YouTube](https://www.youtube.com/watch?v=w5WVu624fY8) [Accessed 14 Sep. 2023].

  * www.youtube.com. (n.d.). _Scalability at YouTube_. [online] Available at [YouTube](https://www.youtube.com/watch?t=602&v=G-lGCC4KKok&feature=youtu.be) [Accessed 14 Sep. 2023].

  * didip (2008). _Super Sizing Youtube with Python_. [online] Available at: https://www.slideshare.net/didip/super-sizing-youtube-with-python.

  * Wikipedia Contributors (2019). _YouTube_. [online] Wikipedia. Available at: https://en.wikipedia.org/wiki/YouTube.

  * highscalability.com. (n.d.). _YouTube Architecture - High Scalability -_. [online] Available at: http://highscalability.com/youtube-architecture.

39

2

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![SWE's avatar](https://substackcdn.com/image/fetch/$s_!drWN!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8b84c39b-8414-443e-8ed1-fcea47e3eef7_720x720.webp)](https://substack.com/profile/136563906-swe?utm_source=comment)

[SWE](https://substack.com/profile/136563906-swe?utm_source=substack-feed-item)

[Nov 8, 2023](https://newsletter.systemdesign.one/p/youtube-scalability/comment/43284114 "Nov 8, 2023, 3:11 PM")

Liked by Neo Kim

Good summary

ReplyShare

[![Akshay Soni's avatar](https://substackcdn.com/image/fetch/$s_!i9GT!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fpurple.png)](https://substack.com/profile/198299300-akshay-soni?utm_source=comment)

[Akshay Soni](https://substack.com/profile/198299300-akshay-soni?utm_source=substack-feed-item)

[Mar 21, 2024](https://newsletter.systemdesign.one/p/youtube-scalability/comment/52144989 "Mar 21, 2024, 3:24 PM")

Great Article

ReplyShare

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture