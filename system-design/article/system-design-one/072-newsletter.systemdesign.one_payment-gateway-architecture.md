[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Razorpay Scaled to Handle Flash Sales at 1500 Requests per Second

### #46: A Case Study on Payment Gateway Scalability (5 min read)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 17, 2024

86

4

4

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

2020 - India.

IPL, the most famous cricket league in the world is about to start.

And more than 23 million raving fans of cricket in India will stream it.

[![Payment Gateway Architecture](https://substackcdn.com/image/fetch/$s_!zdXF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4369ade9-9c62-45a1-9c19-8333524a8542_800x500.gif)](https://substackcdn.com/image/fetch/$s_!zdXF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4369ade9-9c62-45a1-9c19-8333524a8542_800x500.gif)

While companies sell food at a discount via flash sales minutes before the game starts.

And accepts online payment via Razorpay, a shiny payment gateway service.

Flash sales create a traffic spike with transactions reaching 1500 requests per second.

Although it’s possible to serve this traffic, scaling the infrastructure quickly to handle it could be difficult.

_This post outlines how Razorpay scales to handle flash sales. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/payment-gateway-architecture/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

#### [Refind – Brain food, delivered daily (Featured)](https://refind.com/n/c?put=MlHF&att=rfnd)

Loved by 450,000+ curious minds. Every day Refind analyzes thousands of articles and sends you only the best. Subscribe for free today.

[![Refind](https://substackcdn.com/image/fetch/$s_!pZgP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe07ccd1d-09a9-4b8a-a21e-cd9d71bf8e4a_800x800.jpeg)](https://substackcdn.com/image/fetch/$s_!pZgP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe07ccd1d-09a9-4b8a-a21e-cd9d71bf8e4a_800x800.jpeg)

[Join Refind](https://refind.com/n/c?put=MlHF&att=rfnd)

* * *

## Payment Gateway Architecture

Here are their scalability techniques for flash sale:

### 1\. Rate Limit the Traffic

They rate limit the traffic to prevent server overload.

[![Rate Limiting Unwanted Traffic](https://substackcdn.com/image/fetch/$s_!qmur!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F89bc2ab3-eb22-43df-a8a6-5c1987044574_1638x401.png)](https://substackcdn.com/image/fetch/$s_!qmur!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F89bc2ab3-eb22-43df-a8a6-5c1987044574_1638x401.png)Rate Limiting Unwanted Traffic

And use a Nginx proxy server as the rate limiter. It gets deployed as a [sidecar](https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar) and runs a dedicated cache for rate limiting. Imagine the **sidecar pattern** as extending a service by attaching an extra container.

Besides they use the [fixed window algorithm](https://redis.io/learn/develop/java/spring/rate-limiting/fixed-window) for efficient rate limiting. It uses a single atomic counter per key with an expiry time (**TTL**).

### 2\. Connection Pooling

They use MySQL as the main database. While clients compete with each other for a database connection during flash sales.

They run PHP on the application layer. But PHP uses a process model for execution. So sharing resources between processes isn’t possible. Hence PHP doesn’t support database connection pooling natively.

This means the application layer holds database connections while waiting for results. So connection starvation is likely to happen if the queries get expensive.

Also the database performance degrades if the number of idle connections increases.

[![Database Proxy for Connection Pooling](https://substackcdn.com/image/fetch/$s_!XHvC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67e12ef7-7162-4454-a2eb-2a827d58d4da_1258x772.png)](https://substackcdn.com/image/fetch/$s_!XHvC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67e12ef7-7162-4454-a2eb-2a827d58d4da_1258x772.png)Database Proxy for Connection Pooling

So they use [ProxySQL](https://proxysql.com/) as a database proxy. It holds a pool of connections to the database.

And a tenant connects to ProxySQL instead of MySQL directly. Thus limiting the number of MySQL connections to avoid connection starvation.

Think of a **tenant** as an isolated data space for a specific user.

Also constant opening and closing of database connections could be expensive. While ProxySQL prevents it via persistent connections.

Besides ProxySQL caches the query results for low latency.

They deploy ProxySQL as a sidecar to keep the application layer stateless. And set up a fallback to connect directly with MySQL if ProxySQL fails for high availability.

### 3\. Avoid the Thundering Herd

[Thundering herd](https://en.wikipedia.org/wiki/Thundering_herd_problem) occurs when many clients query the server concurrently during flash sales. That means bad performance and downtime.

[![3. Avoid the Thundering Herd](https://substackcdn.com/image/fetch/$s_!Kgfu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4208b291-dd8a-4b20-a63f-073a2c59b04f_1024x805.jpeg)](https://substackcdn.com/image/fetch/$s_!Kgfu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4208b291-dd8a-4b20-a63f-073a2c59b04f_1024x805.jpeg)[Thundering Herd](https://en.wikipedia.org/wiki/The_Thundering_Herd_%281925_film%29)

So they use these techniques to prevent the thundering herd problem:

  * Throttle incoming traffic

  * Add [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff) by the client

  * Include caching

Besides they use ProxySQL to throttle tenants issuing expensive database queries.

### 4\. Autoscaling Isn’t Enough

It takes around 4 minutes for a newly provisioned server to become healthy. So they don't rely only on autoscaling to handle traffic spikes.

[![Autoscaling](https://substackcdn.com/image/fetch/$s_!qUND!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd88eafaf-075b-4f3e-9a31-00a603c178de_1227x292.png)](https://substackcdn.com/image/fetch/$s_!qUND!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd88eafaf-075b-4f3e-9a31-00a603c178de_1227x292.png)Autoscaling

Instead they prewarm their infrastructure. And run baked container images to reduce the deployment time.

They do [capacity planning](https://systemdesign.one/back-of-the-envelope/) based on estimated transactions and scale their servers horizontally. Also they scale down the infrastructure after flash sales with autoscaling.

### 5\. Smart Routing

They should forward the traffic only to external bank gateways that are operational.

[![Routing Traffic to an Operational Gateway](https://substackcdn.com/image/fetch/$s_!V6ni!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2d2ad369-0f68-48c2-8c34-6e41fd35b689_1641x633.png)](https://substackcdn.com/image/fetch/$s_!V6ni!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2d2ad369-0f68-48c2-8c34-6e41fd35b689_1641x633.png)Routing Traffic to an Operational Gateway

So they use routing rules based on machine learning. It considers the success and failure events from payments. And then predicts the success probability of each external gateway.

### 6\. Testing

They must resolve system bottlenecks for better performance.

[![Load Testing](https://substackcdn.com/image/fetch/$s_!CrNl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d14c69e-4251-4d4a-b4e2-aa3bc2f99268_972x529.png)](https://substackcdn.com/image/fetch/$s_!CrNl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d14c69e-4251-4d4a-b4e2-aa3bc2f99268_972x529.png)Load Testing

So they do load testing using an open-source tool called [k6](https://k6.io/docs/#what-is-k6). It checks the system's performance under an expected load. And provides information about latency and throughput.

### 7\. Flywheel Effect

They profile the system for bottlenecks.

[![Feedback Loop](https://substackcdn.com/image/fetch/$s_!xLIQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43a13e36-3d06-4dab-b262-6b7080cddaf9_1586x977.png)](https://substackcdn.com/image/fetch/$s_!xLIQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43a13e36-3d06-4dab-b262-6b7080cddaf9_1586x977.png)Feedback Loop

And put their learning in a constant loop. It helped to improve their performance.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

They run critical services like payments and orders in separate microservices. Because it gives scalability.

This case study shows that simple and proven techniques can solve most scalability problems.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/payment-gateway-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How McDonald’s Food Delivery Platform Handles 20,000 Orders per Second](https://substackcdn.com/image/fetch/$s_!4pfj!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1cf94d9e-994e-44c5-af84-f5c0a240726c_1280x720.gif)How McDonald’s Food Delivery Platform Handles 20,000 Orders per Second[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 3, 2024[Read full story](https://newsletter.systemdesign.one/p/mcdonalds-architecture)](https://newsletter.systemdesign.one/p/mcdonalds-architecture)

[![How Stripe Prevents Double Payment Using Idempotent API](https://substackcdn.com/image/fetch/$s_!teAD!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe205302f-4dce-42be-be58-a294e5a64e95_1280x720.gif)How Stripe Prevents Double Payment Using Idempotent API[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 9, 2024[Read full story](https://newsletter.systemdesign.one/p/idempotent-api)](https://newsletter.systemdesign.one/p/idempotent-api)

* * *

### References

  * [IPL: Razorpay’s second innings](https://engineering.razorpay.com/ipl-razorpays-second-innings-ae7c86b0894c)

  * [Auto Incident Management for improved Systems Availability and Developer Productivity](https://engineering.razorpay.com/how-automation-contributed-to-improved-systems-availability-developer-productivity-16c992fdb7da)

  * [Razorpay’s Authentication Revamp: Turbocharging Performance](https://engineering.razorpay.com/razorpays-authentication-revamp-turbocharging-performance-b8bb9d750fe8)

  * [Never Have I Ever - Gone Live without Perf](https://engineering.razorpay.com/never-have-i-ever-gone-live-without-perf-470c2114d50d)

  * [ProxySQL Website](https://proxysql.com/)

  * [k6 documentation](https://k6.io/docs/#what-is-k6)

  * [Load Testing vs Stress Testing: What's the Difference and Why It Matters?](https://apitoolkit.io/blog/load-testing-vs-stress-testing-differences/)

  * [How to Implement Fixed Window Rate Limiting using Redis](https://redis.io/learn/develop/java/spring/rate-limiting/fixed-window)

  * [Why does PHP not support a database connection pool?](https://www.quora.com/Why-does-PHP-not-support-database-connection-pool)

86

4

4

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[May 17, 2024](https://newsletter.systemdesign.one/p/payment-gateway-architecture/comment/56617657 "May 17, 2024, 2:50 PM")

Great lessons on scalability @systemdesignone

ReplyShare

[![Fran Soto's avatar](https://substackcdn.com/image/fetch/$s_!XWMk!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10f90fdb-11ac-48b4-8f51-6a59e07763d2_1149x1149.png)](https://substack.com/profile/170998285-fran-soto?utm_source=comment)

[Fran Soto](https://substack.com/profile/170998285-fran-soto?utm_source=substack-feed-item)

[May 19, 2024](https://newsletter.systemdesign.one/p/payment-gateway-architecture/comment/56742583 "May 19, 2024, 6:04 AM")

Now every time I read "thundering herd" I'll think of a flash sale and people running.

Good article, Neo!

ReplyShare

[2 more comments...](https://newsletter.systemdesign.one/p/payment-gateway-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture