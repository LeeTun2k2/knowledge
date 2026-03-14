[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Instagram Scaled to 2.5 Billion Users

### #49: Break Into Instagram Scalability Techniques (7 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jun 11, 2024

∙ Paid

155

3

14

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Instagram scaled its infrastructure. If you want to learn more, find references at the bottom of the page._

  * _[Share this post](https://newsletter.systemdesign.one/p/instagram-infrastructure/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, 2 Stanford graduates decided to make a location check-in app.

Yet they noticed photo sharing was the most used feature of their app.

So they pivoted to create a photo-sharing app and called it Instagram.

As more users joined in the early days, they bought new hardware to scale.

[![Vertical Scaling](https://substackcdn.com/image/fetch/$s_!x0aU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbde515ca-3609-4de1-a9f8-71be1ff1b008_976x711.png)](https://substackcdn.com/image/fetch/$s_!x0aU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbde515ca-3609-4de1-a9f8-71be1ff1b008_976x711.png)Vertical Scaling

But scaling vertically became expensive after a specific point.

Yet their growth was skyrocketing.

And hit 30 million users in less than 2 years.

So they scaled out by adding new servers.

[![Horizontal Scaling](https://substackcdn.com/image/fetch/$s_!0BIH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F84affe40-7da2-40c7-b82f-e98a8990a45d_1282x871.jpeg)](https://substackcdn.com/image/fetch/$s_!0BIH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F84affe40-7da2-40c7-b82f-e98a8990a45d_1282x871.jpeg)Horizontal Scaling

Although it temporarily solved their scalability issues, there were newer problems from hypergrowth.

Here are some of them:

### 1\. Resource Usage:

More hardware allowed them to scale out.

Yet each server wasn’t used to maximum capacity.

Put simply, resources got wasted as their server throughput wasn’t high.

### 2\. Data Consistency:

They installed data centers across the world to handle massive traffic.

And data [consistency](https://systemdesign.one/consistency-patterns/) is needed for a better user experience.

But it became difficult with many data centers.

### 3\. Performance:

They run Python on the application server.

This means they have processes instead of threads due to global interpreter lock (**[GIL](https://en.wikipedia.org/wiki/Global_interpreter_lock)**).

So there were some performance [limitations](https://stackoverflow.com/questions/617787/why-should-i-use-a-thread-vs-using-a-process).

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Instagram Infrastructure

The smart engineers at Instagram used simple ideas to solve these difficult problems.

Here’s how they provide extreme scalability:

### 1\. Resource Usage:

Python is beautiful.

But C is [faster](https://stackoverflow.com/questions/13853053/what-makes-c-faster-than-python).

So they replaced stable functions that are extensively used with [Cython](https://cython.org/) and C/C++.

It reduced their CPU usage.

Also they didn’t buy expensive hardware to scale up.

Instead reduced CPU instructions needed to finish a task.

This means running good code.

Thus handling more users on each server.

[![Shared Memory vs Private Memory](https://substackcdn.com/image/fetch/$s_!icIv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F411534a6-e2c5-4efe-ace4-3b92a180e802_1249x406.png)](https://substackcdn.com/image/fetch/$s_!icIv!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F411534a6-e2c5-4efe-ace4-3b92a180e802_1249x406.png)Shared Memory vs Private Memory

Besides the number of Python processes on a server is limited by the system memory available.

While each process has a private memory and access to shared memory.

That means more processes can run if common data objects get moved to shared memory.

So they moved data objects from private memory to shared memory if a single copy was enough.

Also removed unused code to reduce the amount of code in memory.

### 2\. Data Consistency:

They use [Cassandra](https://cassandra.apache.org/_/index.html) to support the Instagram user feed.

Each Cassandra node can be deployed in a different data center and synchronized via eventual consistency.

Yet running Cassandra nodes in different _continents_ makes latency worse and data consistency difficult.

[![Optimal Data Placement With Separate Cassandra Clusters](https://substackcdn.com/image/fetch/$s_!VFpq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F77dff89b-d8c5-4e85-ad09-b3a276fd5807_800x500.gif)](https://substackcdn.com/image/fetch/$s_!VFpq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F77dff89b-d8c5-4e85-ad09-b3a276fd5807_800x500.gif)Optimal Data Placement With Separate Cassandra Clusters

So they don’t run a single Cassandra cluster for the whole world.

Instead separate ones for each continent.

This means the Cassandra cluster in Europe will store the data of European users. While the Cassandra cluster in the US will store the data of the US users.

Put another way, the number of replicas isn’t equal to the number of data centers.

Yet there’s a [risk of downtime](https://bluexp.netapp.com/blog/aws-availability-zones-architecture-how-to-select#:~:text=However%2C%20if%20the%20entire%20region,your%20application%20can%20continue%20functioning.) with separate Cassandra clusters for each region.

[![Keeping a Single Replica in Another Region for Disaster Readiness](https://substackcdn.com/image/fetch/$s_!OTjl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5344627c-f678-4d8f-b5ff-1ae6a75a8bd3_800x500.gif)](https://substackcdn.com/image/fetch/$s_!OTjl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5344627c-f678-4d8f-b5ff-1ae6a75a8bd3_800x500.gif)Keeping Single Replica in Another Region for Disaster Readiness

So they keep a single replica in another region and use [quorum](https://en.wikipedia.org/wiki/Quorum_%28distributed_computing%29) to route requests with an extra latency.

Besides they use [Akkio](https://engineering.fb.com/2018/10/08/core-infra/akkio/) for optimal data placement. It splits data into logical units with strong locality.

Imagine **Akkio** as a service that knows when and how to move data for low latency.

They route every user request through the global load balancer. And here’s how they find the right Cassandra cluster for a request:

[![Finding the Correct Cassandra Cluster for a User Request](https://substackcdn.com/image/fetch/$s_!aPeW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F24cc33e3-6f54-40e6-920c-3dfe65a126fd_1818x909.png)](https://substackcdn.com/image/fetch/$s_!aPeW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F24cc33e3-6f54-40e6-920c-3dfe65a126fd_1818x909.png)Finding the Correct Cassandra Cluster for a User Request

  1. The user sends a request to the app but it doesn’t know the user's Cassandra cluster

  2. The app forwards the request to the Akkio proxy

  3. The Akkio proxy asks the cache layer

  4. The Akkio proxy will talk to the location database on a cache miss. It’s replicated across every region as it has a small dataset

  5. The Akkio proxy returns the user's Cassandra cluster information to the app

  6. The app directly accesses the right Cassandra cluster based on the response

  7. The app caches the information for future requests

The Akkio requests take around 10 ms. So the extra latency is tolerable.

Also they migrate the user data if a user moves to another continent and settles down there.

And here’s how they migrate data if a user moves between continents:

[![Data Migration When a User Moves to Another Continent](https://substackcdn.com/image/fetch/$s_!pL0L!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd6fa6b0-531e-4f79-aa55-013985dedb37_1551x903.png)](https://substackcdn.com/image/fetch/$s_!pL0L!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd6fa6b0-531e-4f79-aa55-013985dedb37_1551x903.png)Data Migration When a User Moves to Another Continent

  1. The Akkio proxy knows the user's Cassandra cluster location from the generated response

  2. They maintain a counter in the access database. It records the number of accesses from a specific region for each user

  3. The Akkio proxy increments the access database counter with each request. They could find the exact location of the user based on the IP address

  4. Akkio migrates user data to another Cassandra cluster if the counter exceeds a limit

Besides they store user information, friendships, and picture metadata in [PostgreSQL](https://www.postgresql.org/).

Yet they get more read requests than write requests.

[![Scaling Out PostgreSQL](https://substackcdn.com/image/fetch/$s_!TJhR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8d8f7e12-9e95-4973-8332-5da39a9fcb13_967x676.png)](https://substackcdn.com/image/fetch/$s_!TJhR!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8d8f7e12-9e95-4973-8332-5da39a9fcb13_967x676.png)Scaling Out PostgreSQL

So they run PostgreSQL in [leader-follower](https://www.postgresql.org/docs/8.4/high-availability.html) replication topology.

And route write requests to the leader. While read requests get routed to the follower on the same data center.

### 3\. Performance:

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Finstagram-infrastructure&utm_source=paywall&utm_medium=web&utm_content=145237784)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Finstagram-infrastructure&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture