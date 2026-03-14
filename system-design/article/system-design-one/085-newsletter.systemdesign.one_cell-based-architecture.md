[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How 0.1% Companies Do Hyperscaling

### #34: And Cell Based Architecture Explained Like You’re Twenty (7 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jan 28, 2024

71

10

4

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how cell based architecture works. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/cell-based-architecture/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there lived a software architect named John.

He worked for a startup that went through a mind-boggling growth rate.

Although explosive growth is a good problem to have, scalability issues became impossible to solve.

[![Cell based architecture; Hyperscale](https://substackcdn.com/image/fetch/$s_!kNLz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9b01dfa9-fa2c-4aa6-86e2-6d5eac8c3394_1280x838.png)](https://substackcdn.com/image/fetch/$s_!kNLz!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9b01dfa9-fa2c-4aa6-86e2-6d5eac8c3394_1280x838.png)Hyperscale

**Hypergrowth** occurs when growth exceeds the expected level by more than 40%. Yet most companies with hypergrowth _fail_ due to scalability problems.

So they need to _hyperscale_.

**Hyperscale** is the architecture’s ability to scale quickly based on the changing demand. It needs extreme parallelization and fault isolation.

So he was frustrated.

[![Titanic designer](https://substackcdn.com/image/fetch/$s_!6oeF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9d417b56-8d63-4566-8f57-d235fa4d6746_160x150.gif)](https://substackcdn.com/image/fetch/$s_!6oeF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9d417b56-8d63-4566-8f57-d235fa4d6746_160x150.gif)[Titanic’s Designer Talking About Its Resilience](https://giphy.com/gifs/titanic-gif-ship-shipping-af34tVk53Li4E)

Until one weekend he watches the film Titanic on television.

The scene in which the ship’s designer talks about its resilience caught his attention.

[![Vertical Partition Walls Dividing the Ship’s Interior Into Watertight Compartments](https://substackcdn.com/image/fetch/$s_!qc26!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2fea266d-829c-4dad-955a-c6905360448a_800x500.png)](https://substackcdn.com/image/fetch/$s_!qc26!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2fea266d-829c-4dad-955a-c6905360448a_800x500.png)Vertical Partition Walls Dividing the Ship’s Interior Into Watertight Compartments

They partitioned the ship's interior using vertical walls. Thus compartments become water-tight and self-contained. And the ship wouldn't sink unless many compartments were affected.

Put another way, the walls prevent seawater flooding from affecting the entire ship.

He had a eureka moment. And writes down notes with a pencil before going to bed.

His notes from that day laid the foundation of cell based architecture.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### [Pointer (Featured)](https://www.pointer.io/?utm_source=systemdesign&utm_medium=email&utm_campaign=crosspromo)

If you find system design useful, consider checking out Pointer.io. It’s a reading club for software developers read by CTOs, engineering managers, and senior developers. They send out super high-quality engineering-related content and it’s completely free!

[![Pointer-io](https://substackcdn.com/image/fetch/$s_!werO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faf1db0e8-eeb7-4469-b9f6-9d4dd64602a7_564x564.png)](https://substackcdn.com/image/fetch/$s_!werO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faf1db0e8-eeb7-4469-b9f6-9d4dd64602a7_564x564.png)

[Sign up for free](https://www.pointer.io/?utm_source=systemdesign&utm_medium=email&utm_campaign=crosspromo)

* * *

## Cell Based Architecture

Here’s how cell based architecture works:

### 1\. Implementation

A system is divided into cells and the traffic gets routed between the cells using a cell router.

A **cell** might contain many services, load balancers, and databases. And it’s technology-agnostic.

Put another way, a cell is a completely self-contained instance of the application. So it’s independently deployable and observable.

The customer traffic gets routed to the right cells via a thin layer called the **cell router**.

[![Cell Based Architecture](https://substackcdn.com/image/fetch/$s_!bDIg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F340e0eb6-854a-4417-9d09-f85a91e28e60_952x684.png)](https://substackcdn.com/image/fetch/$s_!bDIg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F340e0eb6-854a-4417-9d09-f85a91e28e60_952x684.png)Cell Based Architecture

The failure of a cell doesn’t affect another one because they are _separated at the logical level_. In other words, cell based architecture _prevents single points of failure_.

A cell could be created using an _infrastructure as a code script_ or any programming language. While each cell gets a name and a version identifier.

The components within a cell communicate with each other through supported network mechanisms. While external communication happens on standard network protocols via a gateway.

[![Reducing Scope of Impact With Cell Based Architecture](https://substackcdn.com/image/fetch/$s_!EdTH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fca819557-cb86-487b-89d3-3f3fb81d478c_1313x826.png)](https://substackcdn.com/image/fetch/$s_!EdTH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fca819557-cb86-487b-89d3-3f3fb81d478c_1313x826.png)Reducing Scope of Impact With Cell Based Architecture

A cell shouldn’t share its state with others and handle only a subset of the total traffic. Thus the impact of a failure like a bad code deployment is reduced.

If a cell fails, only the customers in that specific cell are affected. So the _blast radius is lower_. 

Put another way, if a system contains 10 cells and serves 100 requests. The failure of a single cell affects only 10% of requests.

**Blast radius** is the approximate number of customers affected by a failure.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### 2\. Key Concepts

Here are some key concepts of cell based architecture:

#### a) Customer Placement

The cells get partitioned using a _partition key_. A simple or composite partition key can be used to distribute the traffic between cells.

And the _Customer ID_ is a candidate partition key for most use cases.

[![Mapping Customers to Right Cells](https://substackcdn.com/image/fetch/$s_!XeYs!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F64d84257-59ff-402b-a1ec-f07c8ca260cc_1299x353.png)](https://substackcdn.com/image/fetch/$s_!XeYs!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F64d84257-59ff-402b-a1ec-f07c8ca260cc_1299x353.png)Mapping Customers to Right Cells

The cell router forwards requests to cells based on the partition key. [Consistent hashing](https://newsletter.systemdesign.one/p/what-is-consistent-hashing), or range-based mapping algorithms can be used to map customers to cells.

[![Provisioning Cells Based on the Number of Customers](https://substackcdn.com/image/fetch/$s_!vb_f!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb704b92f-02c1-425e-900d-d9a95c8536a7_800x500.gif)](https://substackcdn.com/image/fetch/$s_!vb_f!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb704b92f-02c1-425e-900d-d9a95c8536a7_800x500.gif)Provisioning Cells Based on the Number of Customers

The number of customers supported by a specific cell depends on its capacity. And a service can be scaled out by adding more cells.

#### b) Cell Router

A simple router can be implemented with a DNS. While an API gateway and a NoSQL database like DynamoDB can be used for complex routing.

The routing layer must be kept simple and horizontally scalable to prevent failures.

[![Routing Traffic to the Right Cell](https://substackcdn.com/image/fetch/$s_!09s6!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcaa6a24b-3137-496f-a9d8-f885656d7b87_934x603.png)](https://substackcdn.com/image/fetch/$s_!09s6!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcaa6a24b-3137-496f-a9d8-f885656d7b87_934x603.png)Routing Traffic to the Right Cell

A dedicated gateway can be installed on each cell for communication. And it becomes the single access point to the cell. Alternatively a shared gateway can be set up via a centralized deployment.

The _gateway_ provides a well-defined interface to a subset of APIs, events, or streams.

#### c) Cell Size

Each cell must have a _fixed maximum size_. Thus the risk of non-linear scaling factors and contention points gets reduced. 

Also scaling out and stress testing is easier with fixed-size cells. So the mean time between failures (**MTBF**) is higher.

And the number of hosts that need to be touched for deployments and diagnosis is reduced. So the mean time to recovery (**MTTR**) is lower.

Yet the _cell boundary_ depends on the business domain and organizational structure.

[![Cell Size vs Benefits](https://substackcdn.com/image/fetch/$s_!WfnG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc0c4dbc5-819c-454c-8c47-7cfc57e5af35_1423x864.png)](https://substackcdn.com/image/fetch/$s_!WfnG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc0c4dbc5-819c-454c-8c47-7cfc57e5af35_1423x864.png)Cell Size vs Benefits

The blast radius is smaller when there are many small cells. Yet capacity is better used with a few large cells. So an _optimal cell size_ must be chosen for best performance.

#### d) Cell Deployment

A deployment must be _tested on a single cell_ before rolling it to others for safety.

[![Cell Deployment Occurring in Waves](https://substackcdn.com/image/fetch/$s_!sGMV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66744b0a-6f4c-41e6-b88f-397a3b92b469_1397x965.png)](https://substackcdn.com/image/fetch/$s_!sGMV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66744b0a-6f4c-41e6-b88f-397a3b92b469_1397x965.png)Cell Deployment in Waves

So the cells get _deployed in waves_ and the metrics are monitored. A deployment is rolled back if there is a failure and a new wave gets introduced.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### 3\. Use Cases

Some use cases where cell based architecture is a good fit are:

  * Applications that need high availability

  * High-scale systems that are too big to fail

  * Systems with low recovery time objective (**RTO**)

  * Systems with many combinations of test cases but insufficient coverage

The cell boundaries provide resilience against failures like buggy feature deployments. And it avoids poison pill requests by limiting the scope of impact.

**Resilience** is the ability of the system to recover from a failure quickly.

A **poison pill request** occurs when a request triggers a failure mode across the system.

[![Benefits of Cell Based Architecture](https://substackcdn.com/image/fetch/$s_!TWuD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3aa4a339-633d-4ee3-a1f4-630aa2cf713e_1046x1109.png)](https://substackcdn.com/image/fetch/$s_!TWuD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3aa4a339-633d-4ee3-a1f4-630aa2cf713e_1046x1109.png)Benefits of Cell Based Architecture

The cell based architecture makes the design more modular and reduces failover problems.

Besides issues due to misbehaving clients, data corruption, and operational mistakes gets prevented.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### 4\. Best Practices

Here’s a list of best practices with cell based architecture:

  * Start with more than a single cell from day 1 to get familiar with the architecture

  * Consider the current tech stack as cell zero and add a router layer

  * Perform a failure mode analysis of the cell to find its resilience

  * A single team could own an entire cell for simplicity. But with cell boundary on the bounded context

  * Cells should communicate via versioned and well-defined APIs

  * Cells must be secured through policies in API gateways

  * Cells should throttle and scale independently

  * The dependencies between cells reduce the benefits of cell based architecture. So they should be kept minimum

  * There shouldn’t be shared resources like databases to avoid a global state

  * The cells should get deployed in waves

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### 5\. Anti-Patterns

Here’s a list of common anti-patterns with cell based architecture:

  * Growing the cell size without limits

  * Deploying code to every cell at once

  * Sharing state between cells

  * Adding complex logic to the routing layer

  * Increased interactions between cells

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### 6\. Cell Based Architecture vs Microservices

A cell could represent a _bounded context_. And it can be implemented as a monolith, a set of microservices, or serverless functions.

**Bounded Context** in Domain-Driven Design (**DDD**) is the explicit domain model boundary.

[![Cell Based Architecture vs Microservices](https://substackcdn.com/image/fetch/$s_!auMp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0432f808-21d0-40e7-a6bd-6f8e85ccaf3c_1139x997.png)](https://substackcdn.com/image/fetch/$s_!auMp!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0432f808-21d0-40e7-a6bd-6f8e85ccaf3c_1139x997.png)[Microservices vs Cell Based Architecture](https://imgflip.com/)

Cell based architecture applies the fundamental concepts of microservices across the entire application. For example, microservices allow the creation of small and separate services. If a service is down, the system will remain operational. 

While the cell based architecture creates a copy of the entire production environment or the bounded context. So only a few users get affected by a cell failure.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

A service failure is inevitable. But its impact can be controlled.

Companies like Slack, DoorDash, and Amazon run cell based architecture. 

Yet it isn’t an alternative to microservices architecture. Instead a way to approach microservices with more agility and confidence. 

Also it increases the costs and complexity of the architecture due to infrastructure redundancy. So it’s important to understand the trade-offs and use it only if necessary.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/cell-based-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Hashnode Generates Feed at Scale](https://substackcdn.com/image/fetch/$s_!J2_D!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F06af69cc-23c5-4e78-bb48-56a53fcbfce8_1280x720.gif)How Hashnode Generates Feed at Scale[Neo Kim](https://substack.com/profile/135589200-neo-kim)·January 21, 2024[Read full story](https://newsletter.systemdesign.one/p/feed-architecture)](https://newsletter.systemdesign.one/p/feed-architecture)

[![How Cloudflare Was Able to Support 55 Million Requests per Second With Only 15 Postgres Clusters](https://substackcdn.com/image/fetch/$s_!HiKr!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fef97562a-380e-41fa-8f6d-747c86968e77_1280x720.gif)How Cloudflare Was Able to Support 55 Million Requests per Second With Only 15 Postgres Clusters[Neo Kim](https://substack.com/profile/135589200-neo-kim)·January 12, 2024[Read full story](https://newsletter.systemdesign.one/p/postgresql-scalability)](https://newsletter.systemdesign.one/p/postgresql-scalability)

* * *

## References

  * [Cell-Based Architecture](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md)

  * [How we adopted a cell-based architecture](https://interactengineering.io/cell-based-architecture-aws/)

  * [Reducing the Scope of Impact with CellBased Architecture](https://docs.aws.amazon.com/pdfs/wellarchitected/latest/reducing-scope-of-impact-with-cell-based-architecture/reducing-scope-of-impact-with-cell-based-architecture.pdf)

  * [A Decentralized Reference Architecture for Cloud-native Applications](https://www.youtube.com/watch?v=zhRUaAVB1zo)

  * [Cell-Based Architecture: A New Decentralized Approach for Cloud Native Patterns](https://thenewstack.io/cell-based-architecture-a-new-decentralized-approach-for-cloud-native-patterns/)

  * [Back to Basics: Cell-Based Architecture](https://www.youtube.com/watch?v=RbD0uK1lmz4)

  * [Cell Based Architecture for Early Stage SaaS](https://www.youtube.com/watch?v=kJECSpVwM7Q)

  * [Case Study: AWS Fargate under the hood](https://www.youtube.com/watch?v=Hr-zOaBGyEA)

  * [Case Study: Slack’s Migration to a Cellular Architecture](https://slack.engineering/slacks-migration-to-a-cellular-architecture/)

  * [Case Study: DoorDash’s Journey to a cell-based microservices architecture on AWS for hyperscale](https://www.youtube.com/watch?v=ReRrhU-yRjg)

  * [Images created with imgflip](https://imgflip.com/)

  * [Titanic Giphy](https://giphy.com/gifs/titanic-gif-ship-shipping-af34tVk53Li4E)

71

10

4

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Petar Ivanov's avatar](https://substackcdn.com/image/fetch/$s_!Hqxs!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb236a7ab-735e-49d2-bbe8-98b1f901b169_500x500.jpeg)](https://substack.com/profile/10269058-petar-ivanov?utm_source=comment)

[Petar Ivanov](https://substack.com/profile/10269058-petar-ivanov?utm_source=substack-feed-item)

[Jan 28, 2024](https://newsletter.systemdesign.one/p/cell-based-architecture/comment/48295803 "Jan 28, 2024, 12:47 PM")

Liked by Neo Kim

That's a great outline of the Cell-Based Architecture, Neo! And to be honest, I haven't heard about it till now. And as you shared, that's a great addition and enhancement to the microservices.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/cell-based-architecture/comment/48295803)

[![lakhan jindam's avatar](https://substackcdn.com/image/fetch/$s_!WEDy!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdf6c40dd-e632-433b-98ea-0c1baaa9acd1_96x96.png)](https://substack.com/profile/96149610-lakhan-jindam?utm_source=comment)

[lakhan jindam](https://substack.com/profile/96149610-lakhan-jindam?utm_source=substack-feed-item)

[Jan 28, 2024](https://newsletter.systemdesign.one/p/cell-based-architecture/comment/48298629 "Jan 28, 2024, 1:46 PM")

Liked by Neo Kim

I think its gaining some traction now, in my current company we are already evaluating and doing a POC for it. Would be happy to write about it once its in action until then its difficult to say.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/cell-based-architecture/comment/48298629)

[8 more comments...](https://newsletter.systemdesign.one/p/cell-based-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture