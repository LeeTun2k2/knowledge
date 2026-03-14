[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Cloudflare Was Able to Support 55 Million Requests per Second With Only 15 Postgres Clusters

### #32: Learn More - Awesome PostgreSQL Scalability (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jan 12, 2024

177

16

15

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Cloudflare scaled to 55 million requests per second with only 15 Postgres clusters. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/postgresql-scalability/?action=share) & I'll send you some rewards for the referrals._

July 2009 - California, United States.

A team of entrepreneurs creates a content delivery network (**CDN**) to make the Internet faster and more reliable. And called it Cloudflare.

They faced various challenges in the early stages of development.

Yet their growth rate was spectacular.

[![Overall Internet Traffic; PostgreSQL Scalability](https://substackcdn.com/image/fetch/$s_!-Kdi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc33909ff-acb7-401a-b921-d0db843302c7_800x500.png)](https://substackcdn.com/image/fetch/$s_!-Kdi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc33909ff-acb7-401a-b921-d0db843302c7_800x500.png)Overall Internet Traffic

Now they serve 20% of Internet traffic at 55 million HTTP requests per second.

And they did it with only 15 PostgreSQL clusters.

A **cluster** is a group of database servers.

They use Postgres to store the metadata of services and handle the Online Transaction Processing (**OLTP**) workload.

Yet supporting many tenants having different load conditions within a cluster is a hard problem.

A **tenant** is an isolated data space for a specific user or group.

[![](https://substackcdn.com/image/fetch/$s_!dbzr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F33b89eb8-2e83-40bc-b278-c648fde37647_800x60.png)](https://substackcdn.com/image/fetch/$s_!dbzr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F33b89eb8-2e83-40bc-b278-c648fde37647_800x60.png)

## PostgreSQL Scalability

Here’s how they use PostgreSQL at extreme scalability:

### 1\. Resource Usage

Most clients compete with each other for Postgres connections. 

But Postgres connections are expensive because each connection is a separate process at the operating system (**OS**) level.

And each tenant has a unique workload, so it's difficult to create a global throttle value.

Besides throttling misbehaving tenants manually is a huge effort. 

A single tenant might issue an expensive query. Thus they might starve the neighboring tenants and block them.

Also a query is difficult to isolate once it reaches the database server.

[![Connection Pooling With PgBouncer](https://substackcdn.com/image/fetch/$s_!4srJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F76dfaf50-d653-4f97-8985-ddb997f13333_1128x697.png)](https://substackcdn.com/image/fetch/$s_!4srJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F76dfaf50-d653-4f97-8985-ddb997f13333_1128x697.png)Connection Pooling With PgBouncer

So they use _PgBouncer_ as a connection pooler in front of Postgres. 

**PgBouncer** acts as a TCP proxy that holds a pool of connections to Postgres.

A tenant connects to the PgBouncer instead of Postgres directly. Thus it limits the number of Postgres connections to prevent connection starvation.

Also constant opening and closing of database connections are expensive. PgBouncer avoids it through persistent connections.

Besides they use PgBouncer to throttle tenants issuing expensive queries at runtime.

### 2\. Thundering Herd Problem

Thundering herd occurs when many clients query a server concurrently. It degrades database performance.

[![Thundering Herd Problem](https://substackcdn.com/image/fetch/$s_!Kgfu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4208b291-dd8a-4b20-a63f-073a2c59b04f_1024x805.jpeg)](https://substackcdn.com/image/fetch/$s_!Kgfu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4208b291-dd8a-4b20-a63f-073a2c59b04f_1024x805.jpeg)[Thundering Herd](https://en.wikipedia.org/wiki/The_Thundering_Herd_%281925_film%29)

The state of an application gets initialized when redeployed. It results in the application creating many connections to the database at once.

Thus it causes a thundering herd as tenants compete with each other for Postgres connections.

They use _PgBouncer_ to throttle the number of Postgres connections created by a specific tenant.

### 3\. Performance

They don't run Postgres in the cloud. Instead on bare metal servers without virtualization for high performance.

[![Load Balancing Traffic Between Database Instances](https://substackcdn.com/image/fetch/$s_!Knhf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F049b9ed0-0473-4279-92d5-aeca8aea14b1_924x336.png)](https://substackcdn.com/image/fetch/$s_!Knhf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F049b9ed0-0473-4279-92d5-aeca8aea14b1_924x336.png)Load Balancing Traffic Between Database Instances

They use _HAProxy_ as an L4 load balancer. 

PgBouncer forwards queries to HAProxy. 

And HAProxy load balance traffic across primary and secondary database read replicas.

### 4\. Concurrency

The performance degrades if there are many tenants issuing concurrent queries.

[![Congestion Avoidance Algorithm Throttling Tenants](https://substackcdn.com/image/fetch/$s_!c-jy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F322b11cf-9066-4858-930f-ff7f4428ee15_1328x819.png)](https://substackcdn.com/image/fetch/$s_!c-jy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F322b11cf-9066-4858-930f-ff7f4428ee15_1328x819.png)Congestion Avoidance Algorithm Throttling Tenants

So they use the _TCP Vegas congestion avoidance algorithm_ to throttle tenants. It finds the optimal number of packets that can be sent concurrently. 

The algorithm does that by sampling each tenant's transaction round trip time (**RTT**) to Postgres.

It then increases the connection pool size as long as the RTT doesn't degrade.

Thus it throttles traffic before resource starvation happens.

### 5\. Ordering Queries

They rank queries at the PgBouncer layer using queues. 

The queries get ordered in the queue based on their historical resource consumption. Put another way, the queries that need more resources get moved to the end of the queue.

[![Ordering Queries in Priority Queue](https://substackcdn.com/image/fetch/$s_!7YcD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2e8d8ad-3cf4-4a66-9892-fce6680f6b3f_1353x683.png)](https://substackcdn.com/image/fetch/$s_!7YcD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2e8d8ad-3cf4-4a66-9892-fce6680f6b3f_1353x683.png)Ordering Queries in Priority Queue

Also they enable priority queueing only in peak traffic to prevent resource starvation. In other words, a query doesn't wait forever at the end of the queue in normal traffic.

This approach improves the latency of most queries. 

Yet a higher latency is observed for tenants that issue expensive queries in peak traffic.

### 6\. High Availability 

They use the _Stolon_ cluster manager for the high availability of Postgres.

[Stolon](https://github.com/sorintlab/stolon) replicates data across Postgres instances.

[![High Availability of Data Layer With Stolon](https://substackcdn.com/image/fetch/$s_!skDG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F706c4f9f-139a-406f-a963-4a4b6ef6c880_1631x337.png)](https://substackcdn.com/image/fetch/$s_!skDG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F706c4f9f-139a-406f-a963-4a4b6ef6c880_1631x337.png)High Availability of Database With Stolon

Besides Stolon elects Postgres leaders and does failover in peak traffic.

They replicate a database cluster across 2 regions for high availability.

While each cluster consists of 3 database servers within a single region.

The writes get routed to the primary region. 

And data get asynchronously replicated to the secondary region. The reads are routed to the secondary region.

Besides they test connectivity between components to find network partitions proactively. 

They do chaos testing for resilience. And set up extra network switches and routes to avoid network partitions.

They use the _pg_rewind_ tool for synchronizing Postgres clusters. It replays missing writes to the primary server that came back online after a failover.

* * *

They run more than a hundred database servers including replicas.

And offer services like DNS Resolver, Firewall, and DDoS Protection.

They use a combination of OS resource management, queueing theory, congestion algorithms, and even statistics for PostgreSQL scalability.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/postgresql-scalability?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Uber Finds Nearby Drivers at 1 Million Requests per Second](https://substackcdn.com/image/fetch/$s_!wM7I!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1eac74fb-4f58-4723-8d82-7f9b7a178ae1_1280x720.gif)How Uber Finds Nearby Drivers at 1 Million Requests per Second[NK](https://substack.com/profile/135589200-nk)·January 4, 2024[Read full story](https://newsletter.systemdesign.one/p/how-does-uber-find-nearby-drivers)](https://newsletter.systemdesign.one/p/how-does-uber-find-nearby-drivers)

[![How PayPal Was Able to Support a Billion Transactions per Day With Only 8 Virtual Machines](https://substackcdn.com/image/fetch/$s_!-L42!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F77318b8c-c1b0-4531-babe-464e68c08f82_1280x720.gif)How PayPal Was Able to Support a Billion Transactions per Day With Only 8 Virtual Machines[NK](https://substack.com/profile/135589200-nk)·December 26, 2023[Read full story](https://newsletter.systemdesign.one/p/actor-model)](https://newsletter.systemdesign.one/p/actor-model)

* * *

## References

  * Source taken from [Vignesh Ravichandran](https://viggy28.dev/)

  * [Performance isolation in a multi-tenant database environment](https://blog.cloudflare.com/performance-isolation-in-a-multi-tenant-database-environment/)

  * [Cloudflare: Performance isolation in multi-tenant DB](https://www.youtube.com/watch?v=DvblO-f2bqQ)

  * [An introduction to stolon: cloud native PostgreSQL high availability](https://sgotti.dev/post/stolon-introduction/)

  * [Cloudflare dishes up the stats on internet traffic in 2023](https://www.theregister.com/2023/12/13/cloudflare_internet_traffic_2023/)

  * [Open sourcing our fork of PgBouncer](https://blog.cloudflare.com/open-sourcing-our-fork-of-pgbouncer)

  * [PostgreSQL pg_rewind documentation](https://www.postgresql.org/docs/current/app-pgrewind.html)

177

16

15

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Marcos F. Lobo 🗻🧭's avatar](https://substackcdn.com/image/fetch/$s_!7roK!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fff9211d7-f06d-4d11-b17c-4f1af3d2df5a_3264x1836.jpeg)](https://substack.com/profile/40136239-marcos-f-lobo?utm_source=comment)

[Marcos F. Lobo 🗻🧭](https://substack.com/profile/40136239-marcos-f-lobo?utm_source=substack-feed-item)

[Jan 12, 2024](https://newsletter.systemdesign.one/p/postgresql-scalability/comment/47171534 "Jan 12, 2024, 6:43 PM")

Liked by Neo Kim

It would be interesting to know, related to the performance section, what kind of discs they are using in the baremetal Postgres servers. I could imagine they are some advanced SSD disks. When I was working at CERN I witnessed changing the discs for the storage for Ceph 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/postgresql-scalability/comment/47171534)

[![Neha Thakur's avatar](https://substackcdn.com/image/fetch/$s_!SFki!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F889c849e-9e81-4f61-8944-4de848506234_144x144.png)](https://substack.com/profile/11262968-neha-thakur?utm_source=comment)

[Neha Thakur](https://substack.com/profile/11262968-neha-thakur?utm_source=substack-feed-item)

[Jan 12, 2024](https://newsletter.systemdesign.one/p/postgresql-scalability/comment/47155936 "Jan 12, 2024, 3:42 PM")

Liked by Neo Kim

Learned some exciting features of postgres

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/postgresql-scalability/comment/47155936)

[14 more comments...](https://newsletter.systemdesign.one/p/postgresql-scalability/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture