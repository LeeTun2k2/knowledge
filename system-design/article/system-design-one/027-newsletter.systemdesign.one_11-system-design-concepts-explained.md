[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# 11 System Design Concepts Explained, Simply

### #88: Break Into System Design (9 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)[![Dr. Ashish Bamania's avatar](https://substackcdn.com/image/fetch/$s_!1rS7!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff41b7f65-55d7-4099-969a-931c2ddd2f5f_612x612.png)](https://substack.com/@drashishbamania)

[Neo Kim](https://substack.com/@systemdesignone) and [Dr. Ashish Bamania](https://substack.com/@drashishbamania)

Sep 16, 2025

251

11

37

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines 11 must-know system design concepts._

  * _[Share this post](https://newsletter.systemdesign.one/p/11-system-design-concepts-explained/?action=share) & I'll send you some rewards for the referrals._

Systems design is an important part of all software engineering tasks you will work on in your career.

While junior developers may get by with writing functional code, senior engineers are valued for their ability to keep the bigger picture in mind.

> _How will this system handle a million users?_
> 
> _What happens when a critical component fails?_
> 
> _How do we maintain performance as users and their data grow exponentially?_

These are some questions that form the foundation of every ‘effortlessly’ running big tech app out there.

Onward.

* * *

I want to introduce [Ashish Bamania](https://www.linkedin.com/in/ashishbamania/) as a guest author.

[![](https://substackcdn.com/image/fetch/$s_!1Z8U!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F08f941e3-3860-4825-9e6e-1541febdf3c9_1200x630.png)](https://www.linkedin.com/in/ashishbamania/)

He's a self-taught software engineer and works as an emergency physician.

If you're getting started with system design, I highly recommend getting his book: **[Systems Design In 100 Images](https://bamaniaashish.gumroad.com/l/systemsdesign/SYSTEMS25)**. It'll help you learn the fundamentals quickly.

(You'll also get a 25% discount when you use the code—SYSTEMS25.)

* * *

[![](https://substackcdn.com/image/fetch/$s_!EeiQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6791e1be-9495-46bf-bffd-d76026585a75_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!EeiQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6791e1be-9495-46bf-bffd-d76026585a75_1080x1080.webp)

Systems design teaches you how to answer those questions well. It is how you go from “ _barely making things work_ ” to “ _making things work at scale_ ”.

* * *

### [CodeRabbit: Free AI Code Reviews in CLI (Sponsor)](https://coderabbit.link/neo)

[![CodeRabbit CLI](https://substackcdn.com/image/fetch/$s_!5LSQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0df5bd04-5caf-4644-bf13-41d11dbf2cb3_1999x1208.png)](https://coderabbit.link/neo)

**[CodeRabbit CLI](https://coderabbit.link/neo)** is an AI code review tool that runs directly in your terminal. It provides intelligent code analysis, catches issues early, and integrates seamlessly with AI coding agents like Claude Code, Codex CLI, Cursor CLI, and Gemini to ensure your code is production-ready before it ships.

  * Enables pre-commit reviews of both staged and unstaged changes, creating a multi-layered review process.

  * Fits into existing Git workflows. Review uncommitted changes, staged files, specific commits, or entire branches without disrupting your current development process.

  * Reviews specific files, directories, uncommitted changes, staged changes, or entire commits based on your needs.

  * Supports programming languages including JavaScript, TypeScript, Python, Java, C#, C++, Ruby, Rust, Go, PHP, and more.

  * Offers free AI code reviews with rate limits so developers can experience senior-level reviews at no cost.

  * Flags hallucinations, code smells, security issues, and performance problems.

  * Supports guidelines for other AI generators, AST Grep rules, and path-based instructions.

[Install CodeRabbit CLI](https://coderabbit.link/neo)

* * *

## System Design Concepts

Here are all the key concepts that you need to understand systems design and become a better engineer:

### 1\. Scalability

[Scalability](https://en.wikipedia.org/wiki/Scalability) is a system’s ability to handle increased load while maintaining acceptable performance.

[![](https://substackcdn.com/image/fetch/$s_!NqIe!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F864ef851-407a-4875-a462-570e07670bdc_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!NqIe!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F864ef851-407a-4875-a462-570e07670bdc_1080x1080.webp)

These are two ways to scale systems:

  1. **[Horizontal scaling](https://en.wikipedia.org/wiki/Scalability#Horizontal_or_scale_out)** : adding more machines/ servers to handle increased load by distributing work across them.

  2. **[Vertical scaling](https://en.wikipedia.org/wiki/Scalability#Vertical_or_scale_up)** : increasing the power of existing machines/ servers by adding more CPU, RAM, or storage to handle increased load.

> _Horizontal scaling means ‘scaling out’ to different servers._
> 
> _Vertical scaling means ‘scaling up’ existing servers._

[![](https://substackcdn.com/image/fetch/$s_!wwgm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a7b7fbf-3eab-43b6-9f47-b8779cdf0e97_1456x713.webp)](https://substackcdn.com/image/fetch/$s_!wwgm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a7b7fbf-3eab-43b6-9f47-b8779cdf0e97_1456x713.webp)

### 2\. Throughput & Bandwidth

Both terms are used interchangeably, but they have different meanings.

[Bandwidth](https://en.wikipedia.org/wiki/Bandwidth_\(computing\)) refers to the amount of data that can potentially travel through a network within a period.

[Throughput](https://en.wikipedia.org/wiki/Network_throughput) refers to the amount of data that actually transfers during a specified period.

[![](https://substackcdn.com/image/fetch/$s_!-2vx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F56cfba5d-20bc-4b4a-9ff6-b01097058332_2000x896.webp)](https://substackcdn.com/image/fetch/$s_!-2vx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F56cfba5d-20bc-4b4a-9ff6-b01097058332_2000x896.webp)

### **3\. Concurrency vs. Parallelism**

Many engineers confuse these terms, so it’s important to understand their meanings clearly.

[Parallelism](https://en.wikipedia.org/wiki/Parallel_computing) refers to executing multiple tasks simultaneously across different processor cores or machines.

[Concurrency](https://en.wikipedia.org/wiki/Concurrency_\(computer_science\)) means executing multiple tasks simultaneously, either by running them in parallel or by rapidly switching between them on the same processor core.

> _Parallelism is a subset of concurrency._

[![](https://substackcdn.com/image/fetch/$s_!dRuY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febdcffa8-92c6-44c6-9a02-f95cae65e2ab_1400x686.webp)](https://substackcdn.com/image/fetch/$s_!dRuY!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febdcffa8-92c6-44c6-9a02-f95cae65e2ab_1400x686.webp)

### **4\. Consistency, Availability & Partition Tolerance**

These three terms are crucial for understanding the trade-offs and limitations when designing distributed systems.

  * **[Consistency](https://en.wikipedia.org/wiki/Consistency_\(database_systems\))** : The guarantee that a system of all machines connected in a distributed manner sees the same data at the same time.

[![](https://substackcdn.com/image/fetch/$s_!WGxp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2285315d-a0ba-455c-9ee4-47847113f539_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!WGxp!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2285315d-a0ba-455c-9ee4-47847113f539_1080x1080.webp)

  * **[Availability](https://en.wikipedia.org/wiki/Availability)** : The degree to which a system remains operational and responsive to requests.

[![](https://substackcdn.com/image/fetch/$s_!5yjF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2712848b-c836-4bb2-964c-a49c86d42e15_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!5yjF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2712848b-c836-4bb2-964c-a49c86d42e15_1080x1080.webp)

  * **[Partition Tolerance](https://en.wikipedia.org/wiki/Network_partition) : **The capability of a distributed system to continue operating despite network failures between the connected machines.

[![](https://substackcdn.com/image/fetch/$s_!PHyS!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F371327a3-2cb6-4702-afd5-6c71a7c687b0_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!PHyS!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F371327a3-2cb6-4702-afd5-6c71a7c687b0_1080x1080.webp)

These three concepts are related to each other through the CAP theorem.

Let’s understand what it is next.

### **5\. CAP Theorem**

The [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) states that in a distributed system, you can guarantee at most two out of these three properties (consistency, availability, and partition tolerance) but never all three simultaneously.

[![](https://substackcdn.com/image/fetch/$s_!fYIg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faebcbf06-dcfe-4387-b7f0-e9f1ffdfe0c1_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!fYIg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faebcbf06-dcfe-4387-b7f0-e9f1ffdfe0c1_1080x1080.webp)

This leads to systems that either have:

  * High Consistency and Partition Tolerance (for example, databases such as MongoDB and HBase).

  * High Consistency and Availability (for example, traditional relational databases such as PostgreSQL that run on a single machine).

  * High availability and Partition tolerance (for example, databases such as Cassandra and CouchDB).

### **6\. PACELC Theorem**

The CAP theorem is further extended by the PACELC theorem.

While the CAP theorem states that with network partitioning, a distributed system must choose between availability and consistency.

The [PACELC theorem](https://en.wikipedia.org/wiki/PACELC_design_principle) adds to this by stating that, where there is no network partitioning, the distributed system still faces a trade-off between Latency and Consistency.

The acronym PACELC stands for Partition (P), Availability (A), Consistency (C), Else (E), Latency (L), Consistency (C).

[![](https://substackcdn.com/image/fetch/$s_!bM6e!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F77565b7a-9865-4656-8580-e22dbc397700_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!bM6e!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F77565b7a-9865-4656-8580-e22dbc397700_1080x1080.webp)

We have not yet discussed what latency is, so let’s talk about it next.

### **7\. Latency**

In distributed systems, [Latency](https://en.wikipedia.org/wiki/Latency_\(engineering\)) is the time it takes for a request to go through the system and return a response.

[![](https://substackcdn.com/image/fetch/$s_!Cw5Y!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4e1b60e-7236-4bf1-8da8-c65436efc3e9_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!Cw5Y!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4e1b60e-7236-4bf1-8da8-c65436efc3e9_1080x1080.webp)

### **8\. Techniques That Reduce Latency**

Multiple techniques are used to decrease latency. Some of them are:

  * [Data replication](https://en.wikipedia.org/wiki/Replication_\(computing\)) and keeping copies of data in various regions closer to the users, as with [Content Delivery Networks (CDNs](https://en.wikipedia.org/wiki/Content_delivery_network)).

[![](https://substackcdn.com/image/fetch/$s_!5B6S!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F34c2f8d4-b6d4-4065-be23-fb54c38f1ca8_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!5B6S!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F34c2f8d4-b6d4-4065-be23-fb54c38f1ca8_1080x1080.webp)

  * **[Caching](https://en.wikipedia.org/wiki/Cache_\(computing\))** : a technique where frequently accessed results are stored in memory in a cache server rather than constantly querying the central database (a slow operation) for results.

[![](https://substackcdn.com/image/fetch/$s_!ZmPZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1a20c427-ab37-4726-b5b2-f9c23abe2f9a_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!ZmPZ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1a20c427-ab37-4726-b5b2-f9c23abe2f9a_1080x1080.webp)

  * **[Sharding](https://en.wikipedia.org/wiki/Shard_\(database_architecture\)) : **a technique**** where data is divided into smaller subsets called shards and distributed across multiple machines/ nodes.

In this way, when the system is queried, each query only reaches the nodes that store the relevant shards, which process the request, reducing load and response time.

[![](https://substackcdn.com/image/fetch/$s_!oDsA!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3e534ab1-6b99-4825-9888-a78f47b0a6dd_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!oDsA!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3e534ab1-6b99-4825-9888-a78f47b0a6dd_1080x1080.webp)

  * **[Load balancing](https://en.wikipedia.org/wiki/Load_balancing_\(computing\)) :** a technique of distributing incoming network requests across multiple servers to prevent any single server from becoming overloaded.

[![](https://substackcdn.com/image/fetch/$s_!mOEO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F32e52739-1622-479f-a513-c42d9a2ab294_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!mOEO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F32e52739-1622-479f-a513-c42d9a2ab294_1080x1080.webp)

There are multiple algorithms for load balancing, and some popular ones are:

  * **Round Robin:** Sending requests to servers in a fixed cyclical order.

  * **Least Connections:** Sending requests to the server with the fewest active connections.

  * **Least Response Time:** Sending requests to the server with the fastest response time.

  * **Weighted Round Robin** : Sending requests to servers in a fixed cyclical order, but proportional to each server's capacity.

Next, let’s learn about databases.

### **9\. Relational vs. Non-Relational Databases**

The two main types of databases are:

  * **[Relational/ SQL databases](https://en.wikipedia.org/wiki/Relational_database) : **These databases organize data into tables with predefined schemas and use SQL (Structured Query Language) to query them. For example, PostgreSQL and MySQL.

  * **[Non-relational/NoSQL databases:](https://en.wikipedia.org/wiki/NoSQL) **These databases do not rely on fixed table schemas and store and query data in flexible ways. For example, MongoDB, Redis, and Cassandra.

[![](https://substackcdn.com/image/fetch/$s_!3Iod!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0a036eeb-97f5-46ef-a8f5-e6b8cfd5afb3_2000x903.webp)](https://substackcdn.com/image/fetch/$s_!3Iod!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0a036eeb-97f5-46ef-a8f5-e6b8cfd5afb3_2000x903.webp)

### **10\. Transactions & Their Types**

The next concept that you must know about is a [Transaction](https://en.wikipedia.org/wiki/Database_transaction), which is a logical unit of work that groups one or multiple operations.

The two types of transactions in distributed systems are:

  * ACID transactions

  * BASE transactions

#### **ACID Transactions**

The acronym ACID stands for:

  * **Atomicity** : A transaction either completes entirely or fails, and no partial transactions occur.

  * **Consistency** : A system remains in a valid state before and after each transaction, as per all rules and constraints.

  * **Isolation** : Transactions running in parallel do so as if they’re the only ones executing. No two transactions interfere with each other’s intermediate states.

  * **Durability** : Once a transaction is committed, its changes are permanently saved and are unaffected by system failures.

[![](https://substackcdn.com/image/fetch/$s_!TChV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F82c91ecc-f323-4496-9d75-bbdeaac5170f_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!TChV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F82c91ecc-f323-4496-9d75-bbdeaac5170f_1080x1080.webp)

#### **BASE Transactions**

The acronym BASE stands for:

  * **Basically Available** : The system remains operational even when parts of it fail, although some data may be temporarily inaccessible.

  * **Soft State** : Data can be temporarily inconsistent across different parts of the system. These temporary inconsistencies are resolved at a later time.

  * **Eventual Consistency** : Given sufficient time, all parts of the system synchronize and eventually converge to a consistent state.

[![](https://substackcdn.com/image/fetch/$s_!SAWJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4107c32-df59-499f-92af-5e5986839a46_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!SAWJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4107c32-df59-499f-92af-5e5986839a46_1080x1080.webp)

Relational/ SQL databases use ACID transactions to maintain strict data integrity and relationships.

NoSQL databases use BASE transactions to achieve high scalability and performance by relaxing consistency requirements.

BASE transactions help large-scale applications, such as those from Amazon, Google, and Meta, work around the CAP/PACELC trade-offs by prioritizing availability and low latency over strict consistency.

### **11\. APIs**

The last important concept that we discuss in this article is [Application Programming Interfaces](https://en.wikipedia.org/wiki/API) (APIs).

These are sets of rules that allow different applications to communicate with each other.

Think of an API as a waiter in a restaurant. A client requests the waiter. The waiter (the API) takes the order to the kitchen (the Server), and gets back with the food for the client (the Response).

[![](https://substackcdn.com/image/fetch/$s_!INcy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff3f87ee5-9522-43b9-a964-c208ca997c7a_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!INcy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff3f87ee5-9522-43b9-a964-c208ca997c7a_1080x1080.webp)

Three types of Web API architectural styles are popular and important to know about:

  * **[REST (Representational State Transfer)](https://en.wikipedia.org/wiki/REST) : **This is the most common architectural style for designing web APIs. REST APIs use standard HTTP methods (`GET`, `POST`, `PUT`, `DELETE`), are stateless (so each request contains all the information for the server to process it), and usually return data in JSON format.

[![](https://substackcdn.com/image/fetch/$s_!6wEJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4871f174-af20-4488-8fbf-4de8fec66ae6_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!6wEJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4871f174-af20-4488-8fbf-4de8fec66ae6_1080x1080.webp)

  * **[SOAP (Simple Object Access Protocol)](https://en.wikipedia.org/wiki/SOAP) : **An older, highly structured API protocol that uses XML for message format. It is popularly used in enterprise environments because of its built-in error handling and security features.

[![](https://substackcdn.com/image/fetch/$s_!cpYL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc4ea7448-0baa-4ca7-9d76-dea57344f612_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!cpYL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc4ea7448-0baa-4ca7-9d76-dea57344f612_1080x1080.webp)

  * **[GraphQL](https://en.wikipedia.org/wiki/GraphQL)** : This is a newer API architectural style developed by Facebook (now Meta) that allows clients to request the exact data they need. Unlike REST, where multiple endpoints may be required, GraphQL uses a single endpoint to query for specific data requirements.

[![](https://substackcdn.com/image/fetch/$s_!bvHK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d46a2eb-110d-4856-a4ed-063584b69183_1080x1080.webp)](https://substackcdn.com/image/fetch/$s_!bvHK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d46a2eb-110d-4856-a4ed-063584b69183_1080x1080.webp)

* * *

Now that you understand the basic concepts that are useful in designing systems that work at scale, it’s time to explore how they are applied.

Look at the apps you use every day and think about how they might work behind the scenes.

_How does Substack handle millions of blogs?_

_How does WhatsApp deliver messages instantly worldwide?_

_How does your bank app handle millions of users without crashing?_

_Why doesn’t YouTube buffer when millions watch videos all at once?_

After understanding the design principles behind these existing systems, start applying these concepts in your own work.

Practice redesigning small projects you’ve built before. Talk to other engineers to get their perspectives on how they would handle a problem.

Remember, every expert was once a beginner. I am sure that you will design systems that handle millions of users before you know it.

* * *

👋 I'd like to thank [Ashish](https://www.linkedin.com/in/ashishbamania/) for writing this newsletter!

  * Also try his newsletters: **[Into AI](https://intoai.pub/)** and **[Into Quantum](https://intoquantum.pub/)**.

Don't forget to **[get his book](https://bamaniaashish.gumroad.com/l/systemsdesign/SYSTEMS25)** at a special 25% discount. It’ll help you understand system design fundamentals quickly.

[![](https://substackcdn.com/image/fetch/$s_!sU-3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1f82e9e3-a63c-4a5f-b76e-29191fefe7a0_2000x1000.webp)](https://bamaniaashish.gumroad.com/l/systemsdesign/SYSTEMS25)

* * *

Unlock access to every deep dive article by becoming a paid subscriber:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋 Say hi on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 170K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter.

You are now 171,001+ readers strong, very close to 172k. Let’s try to get 172k readers by 20 September. Consider sharing this post with your friends and get rewards.

Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/11-system-design-concepts-explained?utm_source=substack&utm_medium=email&utm_content=share&action=share)

251

11

37

Share

PreviousNext

[![Dr. Ashish Bamania's avatar](https://substackcdn.com/image/fetch/$s_!1rS7!,w_52,h_52,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff41b7f65-55d7-4099-969a-931c2ddd2f5f_612x612.png)](https://substack.com/@drashishbamania?utm_source=byline)| A guest post by| [Dr. Ashish Bamania](https://substack.com/@drashishbamania?utm_campaign=guest_post_bio&utm_medium=web)I help you to become great at AI and Quantum Computing 👨🏻‍💻| [Subscribe to Dr.](https://www.intoai.pub/subscribe?)  
---|---  
  
#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Ellie Daw's avatar](https://substackcdn.com/image/fetch/$s_!Ea8N!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a988a61-3788-45bc-96b5-55d0ee01d41f_1664x1664.png)](https://substack.com/profile/29331720-ellie-daw?utm_source=comment)

[Ellie Daw](https://substack.com/profile/29331720-ellie-daw?utm_source=substack-feed-item)

[Sep 16](https://newsletter.systemdesign.one/p/11-system-design-concepts-explained/comment/156628549 "Sep 16, 2025, 5:04 PM")

Liked by Neo Kim, Dr. Ashish Bamania

This was the perfect amount of detail - thank you!

ReplyShare

[2 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/11-system-design-concepts-explained/comment/156628549)

[![Ishwarya Rajendrababu's avatar](https://substackcdn.com/image/fetch/$s_!0Kb-!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9e0a5ff7-caa0-4de0-8d12-1dec7e7564f8_144x144.png)](https://substack.com/profile/154841634-ishwarya-rajendrababu?utm_source=comment)

[Ishwarya Rajendrababu](https://substack.com/profile/154841634-ishwarya-rajendrababu?utm_source=substack-feed-item)

[Oct 2](https://newsletter.systemdesign.one/p/11-system-design-concepts-explained/comment/162252772 "Oct 2, 2025, 5:02 PM")

Liked by Neo Kim, Dr. Ashish Bamania

where do you see consistent hashing which provides fault tolerance in a distributed system fall into the above categories?

ReplyShare

[1 reply](https://newsletter.systemdesign.one/p/11-system-design-concepts-explained/comment/162252772)

[9 more comments...](https://newsletter.systemdesign.one/p/11-system-design-concepts-explained/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture