[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Load Balancing Algorithms Really Work ⭐

### #72: Break Into Load Balancing Algorithms (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 19, 2025

131

6

12

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines 6 popular load balancing algorithms. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/load-balancing-algorithms/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, there lived 2 QA engineers named John and Paul.

They worked for a tech company named Hooli.

Although bright, they never got promoted.

So they were sad and frustrated.

[![Load Balancing Algorithms](https://substackcdn.com/image/fetch/$s_!mDgf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5c55996-9355-40c1-8f65-22b617c51ff5_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!mDgf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5c55996-9355-40c1-8f65-22b617c51ff5_1200x630.gif)

Until one day, they had a smart idea to build a photo-sharing app.

And their growth skyrocketed every day.

So they scaled by installing more servers.

But uneven traffic distribution caused server overload.

So they set up a load balancer for each service.

Think of **load balancer** as a component that distributes traffic evenly among servers.

Yet each service has a different workload and usage pattern.

Onward.

* * *

### [Securing AI agents and non-human identities - Sponsor](https://solutions.cerbos.dev/securing-ai-agents-non-human-identities-in-enterprises?utm_campaign=system_design_may_2025&utm_source=ebook&utm_medium=email&utm_content=NHI_ebook&utm_term=)

[![](https://substackcdn.com/image/fetch/$s_!4UjR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa2481a10-a02c-4312-8415-32006a63c0d7_3840x2160.png)](https://solutions.cerbos.dev/securing-ai-agents-non-human-identities-in-enterprises?utm_campaign=system_design_may_2025&utm_source=ebook&utm_medium=email&utm_content=NHI_ebook&utm_term=)

NHIs surged with the rise of AI agents, microservices, and distributed cloud systems. This ebook gives you a practical roadmap to secure NHIs in your architecture, with Zero Trust principles at the core:

  * Real-world examples of AI and NHI specific security threats, illustrated by incidents from Okta, GitHub, and Microsoft + OWASP research

  * 12 security principles with 35 practical steps for risk-informed NHI governance

  * A vendor landscape and evaluation checklist to help you build out your NHI security infrastructure

[Download the ebook](https://solutions.cerbos.dev/securing-ai-agents-non-human-identities-in-enterprises?utm_campaign=system_design_may_2025&utm_source=ebook&utm_medium=email&utm_content=NHI_ebook&utm_term=)

* * *

## Load Balancing Algorithms

Here’s how they load balance traffic across different services:

### 1\. Round Robin

One weekend, their app became trending on the play store.

Because of this, many users tried to log in at the same time. Yet it’s necessary to distribute traffic evenly among auth servers.

So they installed the round robin algorithm on the load balancer.

[![Round Robin Algorithm](https://substackcdn.com/image/fetch/$s_!Zxev!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F59b4201f-60c9-4b9f-8ede-ac4f619be384_948x672.png)](https://substackcdn.com/image/fetch/$s_!Zxev!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F59b4201f-60c9-4b9f-8ede-ac4f619be384_948x672.png)Round Robin Algorithm

Here’s how it works:

  * The load balancer keeps a list of servers

  * It then forwards requests to the servers in sequential order

Once it reaches the end of the list, it starts again from the first server.

This approach is simple to set up and understand. Yet it doesn't consider how long a request takes, so slow requests might overload the server.

Life was good.

### 2\. Least Response Time

Until one day, a celebrity uploads a photo on the app. 

Because of that, millions of users check their feed. Yet some servers handling the feed might be slow due to garbage collection and [CPU pressure](https://www.netdata.cloud/blog/understanding-linux-cpu-consumption-load-and-pressure-for-performance-optimisation/).

So they use the least response time algorithm to route the requests.

[![Least Response Time Algorithm](https://substackcdn.com/image/fetch/$s_!YIYk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbd1f6462-b898-4b1f-ac0e-2439ea42263c_993x672.png)](https://substackcdn.com/image/fetch/$s_!YIYk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbd1f6462-b898-4b1f-ac0e-2439ea42263c_993x672.png)Least Response Time Algorithm

Here’s how it works:

  * The load balancer monitors the response time of servers

  * It then forwards the request to the server with the fastest response time

  * If 2 servers have the same latency, the server with the fewest connections gets the request

This approach has the lowest latency, yet there’s an overhead with server monitoring. Besides latency spikes might cause wrong routing decisions.

Life was good again.

### 3\. Weighted Round Robin

But one day, they noticed a massive spike in photo uploads.

Each photo gets processed to reduce storage costs and improve user experience. While processing some photos is complex and expensive.

So they installed the weighted round robin algorithm on the load balancer.

[![Weighted Round Robin Algorithm](https://substackcdn.com/image/fetch/$s_!h6Ti!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F602b6889-9ee9-40d1-8952-090212d3e33a_948x672.png)](https://substackcdn.com/image/fetch/$s_!h6Ti!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F602b6889-9ee9-40d1-8952-090212d3e33a_948x672.png)Weighted Round Robin Algorithm

Here’s how it works:

  * The load balancer assigns a weight to each server based on its capacity

  * It then forwards requests based on server weight; the more the weight, the higher the requests

Imagine **weighted round robin** as an extension of the round robin algorithm. It means servers with higher capacity get more requests in sequential order.

This approach offers better performance. Yet scaling needs manual updates to server weights, thus increasing operational costs.

### 4\. Adaptive

Their growth became inevitable; they added support for short videos.

A video gets transcoded into different formats for low bandwidth usage. Yet transcoding is expensive, and some videos could be lengthy.

So they installed the adaptive algorithm on the load balancer.

[![Adaptive Load Balancing Algorithm](https://substackcdn.com/image/fetch/$s_!Op6c!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc84e2191-f316-476b-8c28-6403c5a441f7_948x672.png)](https://substackcdn.com/image/fetch/$s_!Op6c!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc84e2191-f316-476b-8c28-6403c5a441f7_948x672.png)Adaptive Load Balancing Algorithm

Here’s how it works:

  * An agent runs on each server, which sends the server status to the load balancer in real-time

  * The load balancer then routes the requests based on server metrics, such as CPU and memory usage

Put simply, servers with lower load receive more requests. It means better fault tolerance. Yet it’s complex to set up, also the agent adds an extra overhead.

Let’s keep going!

### 5\. Least Connections

Until one day, users started to binge-watch videos on the app.

This means long-lived server connections. Yet a server can handle only a limited number of them.

So they installed the least connections algorithm on the load balancer.

[![Least Connections Algorithm](https://substackcdn.com/image/fetch/$s_!CsGz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa9b83731-7e94-4a8a-90c7-0af21dc10e3b_948x672.png)](https://substackcdn.com/image/fetch/$s_!CsGz!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa9b83731-7e94-4a8a-90c7-0af21dc10e3b_948x672.png)Least Connections Algorithm

Here’s how it works:

  * The load balancer tracks the active connections to the server

  * It then routes requests to the server with fewer connections

It ensures a server doesn’t get overloaded during peak traffic. Yet tracking the number of active connections makes it complex. Also session affinity needs extra logic.

Life was good again.

### 6\. IP Hash

But one day, they noticed a spike in usage of their chat service.

Yet session state is necessary to track conversations in real-time.

So they installed the IP hash algorithm on the load balancer. It allows **sticky sessions** by routing requests from a specific user to the same server.

[![IP Hash Algorithm](https://substackcdn.com/image/fetch/$s_!a123!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F024c2b5c-ac5d-4a6b-b79c-cfaa03abec72_948x672.png)](https://substackcdn.com/image/fetch/$s_!a123!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F024c2b5c-ac5d-4a6b-b79c-cfaa03abec72_948x672.png)IP Hash Algorithm

Here’s how it works:

  * The load balancer uses a hash function to convert the client’s IP address into a number

  * It then finds the server using the number

This approach avoids the need for an external storage for sticky sessions.

Yet there’s a risk of server overload if IP addresses aren’t random. Also many clients might share the same IP address, thus making it less effective. 

* * *

There are 2 ways to set up a load balancer: a hardware load balancer or a software load balancer.

A hardware load balancer runs on a separate physical server. Although it offers high performance, it's expensive.

So they set up a software load balancer. It runs on general-purpose hardware. Besides it's easy to scale and cost-effective.

And everyone lived happily ever after.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋 Say hello on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 150K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/load-balancing-algorithms?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

**TL;DR** 🕰️

You can find a [summary of this article here](https://www.linkedin.com/posts/nk-systemdesign-one_if-i-had-to-load-balance-traffic-here-are-activity-7330195820027277312-8m1d?utm_source=share&utm_medium=member_desktop&rcm=ACoAAEALjXEBnGWKup7Y3FjFIJ1ocWkZGqToedo). Consider a repost if you find it helpful.

* * *

[![How DNS Works 🔥](https://substackcdn.com/image/fetch/$s_!NXq7!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8cbe1b81-1f1a-4e58-be8b-359a4c2847f7_1280x720.png)How DNS Works 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 2, 2025[Read full story](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work)](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work)

[![How JWT Works ✨](https://substackcdn.com/image/fetch/$s_!Xv0p!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5b48f3f4-fdda-4da8-9eb3-288289bb9475_1280x720.png)How JWT Works ✨[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 10, 2025[Read full story](https://newsletter.systemdesign.one/p/how-jwt-works)](https://newsletter.systemdesign.one/p/how-jwt-works)

* * *

### References

  * [What is load balancing?](https://www.cloudflare.com/en-gb/learning/performance/what-is-load-balancing/)

  * [Round Robin Load Balancing](https://www.vmware.com/topics/round-robin-load-balancing)

  * [Least response time method](https://docs.netscaler.com/en-us/citrix-adc/current-release/load-balancing/load-balancing-customizing-algorithms/leastresponsetime-method.html)

  * [How do load balancer algorithms distribute client traffic across servers?](https://kemptechnologies.com/load-balancer/load-balancing-algorithms-techniques)

  * [How Load Balancing Algorithms Work, and How to Choose the Right One](https://deploy.equinix.com/blog/comparing-load-balancing-algorithms/)

  * [IP Hash Load Balancing](https://www.withcoherence.com/articles/ip-hash-load-balancing-nginx-configuration-guide)

  * [What is Adaptive Load Balancing?](https://www.skudonet.com/blog/what-is-adaptive-load-balancing/)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

131

6

12

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[May 19, 2025](https://newsletter.systemdesign.one/p/load-balancing-algorithms/comment/118357214 "May 19, 2025, 12:19 PM")

Liked by Neo Kim

Funny thing about load balancing, there’s no one “best” algorithm.

What works great for one service might totally mess up another. 

Sometimes simple round robin just wins because it’s dead easy. Other times, you need the fancy stuff like adaptive or IP hash.

Depends on the traffic, the bottlenecks, and how much pain you can handle when things go sideways.

Good one, Neo

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/load-balancing-algorithms/comment/118357214)

[![Rolas Najera's avatar](https://substackcdn.com/image/fetch/$s_!D3R7!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff332d19f-96a0-49ff-a5cf-d28ef9aaec11_3456x3456.jpeg)](https://substack.com/profile/123329765-rolas-najera?utm_source=comment)

[Rolas Najera](https://substack.com/profile/123329765-rolas-najera?utm_source=substack-feed-item)

[May 20, 2025](https://newsletter.systemdesign.one/p/load-balancing-algorithms/comment/118597079 "May 20, 2025, 6:17 AM")

Liked by Neo Kim

I loved it! Thanks, Neo!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/load-balancing-algorithms/comment/118597079)

[4 more comments...](https://newsletter.systemdesign.one/p/load-balancing-algorithms/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture