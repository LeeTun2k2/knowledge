[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Kafka Works

### #91: Learn Everything About Apache Kafka’s Architecture, Including Brokers, KRaft, Topic Partitions, Tiered Storage, Exactly Once, Kafka Connect, Kafka Schema Registry and Kafka Streams

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)[![Stanislav Kozlovski's avatar](https://substackcdn.com/image/fetch/$s_!0lM5!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03bc8810-0db9-40c9-a28b-1a4752b7a135_800x800.jpeg)](https://substack.com/@stanislavkozlovski)

[Neo Kim](https://substack.com/@systemdesignone) and [Stanislav Kozlovski](https://substack.com/@stanislavkozlovski)

Sep 25, 2025

520

11

74

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the Kafka architecture._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-kafka-works/?action=share) & I'll send you some rewards for the referrals._

Open any article about Kafka today, and you will see the same words being used to describe it:

> _“It’s an open-source, distributed, durable, very scalable, fault-tolerant pub/sub message system with rich integration and stream processing capabilities.”_

While it’s true, it isn’t helpful for a novice reader being introduced to Kafka for the first time. Today, we will explain Kafka by **precisely breaking down** what every word in that definition means. Every definition will start with the 💡 symbol. At the end, you will have a complete high-level understanding of Apache Kafka.

* * *

I want to introduce [Stanislav Kozlovski](http://linkedin.com/in/stanislavkozlovski) (“The Kafka Guy”) as a guest author.

[![](https://substackcdn.com/image/fetch/$s_!1YEl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F88c3c102-45da-4e5d-a559-5b0b14036e98_1200x630.png)](http://linkedin.com/in/stanislavkozlovski)

He’s an Apache Kafka committer and software engineer who worked at Confluent (the Kafka company) during its hypergrowth period. 

Check out his newsletter and social media:

  * [2 Minute Streaming](https://blog.2minutestreaming.com/) (Flagship newsletter)

  * [Big Data Stream](https://bigdata.2minutestreaming.com/)

  * [LinkedIn](http://linkedin.com/in/stanislavkozlovski)

  * [Twitter](https://x.com/BdKozlovski)

Currently, he has taken a break from engineering to consult companies and write extensively about Apache Kafka and data engineering.

* * *

#### Apache Kafka

Apache Kafka is one of the most popular open-source projects in the data infrastructure space. It is a standard tool in every data engineer’s catalog, used by over 70% of Fortune 500 companies and 150,000 organizations.[1](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-1-174189967)

Kafka is a messaging system that was originally developed by LinkedIn in 2010. In 2011, it was open-sourced and donated to the Apache Foundation.[2](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-2-174189967)

> **💡** _**open-source (1/8)**_

Today, Confluent is a public company and widely known as the company behind Kafka. It has expanded its business to offer a more complete data streaming platform on top of Kafka.

Nowadays, Kafka is more than a simple messaging system: it’s a larger ecosystem of components that form a _****_**streaming platform**[3](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-3-174189967). It is frequently called the swiss army knife of data infrastructure.

Onward.

* * *

### **[CodeRabbit: Free AI Code Reviews in CLI (Sponsor)](https://coderabbit.link/neo)**

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

#### Kafka’s Story

Why did Kafka become so widely used?

It solved a very important problem - the problem of data integration at scale. LinkedIn had to connect different services to one another. A naive way of achieving this would be to create many custom point-to-point integrations (called **data pipelines**) between each service. That would have resulted in an **O (N^2)** mess that would break often and be impossible to maintain when N is in the hundreds**.**

[![](https://substackcdn.com/image/fetch/$s_!-az8!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F44381f1d-2081-42cf-b9b7-b1200d42284d_1999x750.png)](https://substackcdn.com/image/fetch/$s_!-az8!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F44381f1d-2081-42cf-b9b7-b1200d42284d_1999x750.png)

Apache Kafka flips this problem on its head. Instead of creating custom pipelines per connection, it encourages organizations to:

  1. Store their data in a central location (Kafka)

  2. Use a single standard API (the Kafka API)

  3. Have applications subscribe and consume this data in real time

This decouples writers from readers, as writers simply publish to Kafka, and readers subscribe to Kafka.

The data gets durably persisted to disk for a limited amount of time (e.g., 7 days). Kafka is ideal and was built with read-fanout use cases in mind, where the same message needs to get read by multiple systems. As such, it’s common for the system’s read throughput to be a multiple of its write throughput.

[![](https://substackcdn.com/image/fetch/$s_!xlWg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86c7c46a-ccb0-45b0-b69d-b007a8fbb270_1600x880.png)](https://substackcdn.com/image/fetch/$s_!xlWg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86c7c46a-ccb0-45b0-b69d-b007a8fbb270_1600x880.png)

> 💡 _**pub-sub messaging system (2/8)**_ \- this is what a pub/sub messaging system is

With Kafka, organizations don’t need to maintain dozens of fragile custom point-to-point pipelines that break whenever a single VM restarts. The data can be written to Kafka once and read as many times as necessary by whatever system needs it.

> In this article, we won’t talk further about the use cases of Kafka. If you’re more interested in the reason behind Kafka, I recently covered in-depth why LinkedIn created Kafka. It made the front page of Hacker News.

> ✅ Check it out [here](https://bigdatastream.substack.com/p/why-was-apache-kafka-created).

* * *

## The Basic Kafka Concepts

Okay, let’s dive into Kafka now! To truly understand the system, we need to start from the basics.

Let’s examine its core data structure:

### The Log Data Structure

Kafka is built upon the [simple log data structure](https://topicpartition.io/definitions/the-log).

It is append-only; you can only add records to the end of the log (no deletes or updates allowed). Reads go from left to right, in the order the records were added.

Each record in the log has a unique monotonically increasing number called an **offset**. The offset refers to the record and denotes its order.

[![](https://substackcdn.com/image/fetch/$s_!nCew!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F412801e6-4a1b-4311-ab1a-68fa1d42129b_1456x819.png)](https://substackcdn.com/image/fetch/$s_!nCew!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F412801e6-4a1b-4311-ab1a-68fa1d42129b_1456x819.png)

The API of the log data structure is very simple:

[![](https://substackcdn.com/image/fetch/$s_!UMlg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7cbb6cc7-00ab-4566-8848-3053e2baa234_1999x984.png)](https://substackcdn.com/image/fetch/$s_!UMlg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7cbb6cc7-00ab-4566-8848-3053e2baa234_1999x984.png)

Kafka keeps the log structure on disk. The log’s sequential operations work very well with HDDs. Hard drives offer very high throughput for sequential[4](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-4-174189967) reads and writes. This differs from random reads and writes, where HDDs don’t perform well.

#### Records

  *  _**{record, message, event}**_ : an entry in the log. I use these words interchangeably when describing data in Kafka.

Each message is essentially a key-value pair; it consists of a ``byte[] key`` and a ``byte[] value`` (although other metadata like offset, timestamp, and custom headers exist too). The key is optional; it is valid for a message to only have a value.

[![](https://substackcdn.com/image/fetch/$s_!yFwB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb21a0081-a5da-4e34-9203-7efd6d37eb1d_1999x897.png)](https://substackcdn.com/image/fetch/$s_!yFwB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb21a0081-a5da-4e34-9203-7efd6d37eb1d_1999x897.png)

The key thing to remember is that the key/value pairs are **raw bytes**.

Kafka does not inherently support types (e.g., int64, string, etc.) nor schemas (specific message structures).

It is the client-side code’s responsibility to apply schemas:

  * **When writing** : producer clients convert (serialize) the objects into bytes.

  * **When reading** : consumer clients parse (deserialize) the raw bytes from the network into the object.

### Topics & Partitions

#### Topics

One log is not enough. You want to separate your data into categories. Just as in a database, you would create separate tables for user accounts and customer orders; in Kafka, you would create separate topics.

It’s common for a Kafka cluster to have hundreds to thousands of topics.

#### Partitions

Kafka is a distributed system designed to scale much further than what a single machine can handle. As such, it uses **sharding**.

A topic is sharded into one or more partitions.

Each partition is a separate instance of the log data structure.

While a topic can have just one partition, it’s very common for it to have dozens, since this helps with parallelization of reads (more on that later).

[![](https://substackcdn.com/image/fetch/$s_!kVL_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F97951e01-e10d-479c-9022-840a40706cb2_1600x880.png)](https://substackcdn.com/image/fetch/$s_!kVL_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F97951e01-e10d-479c-9022-840a40706cb2_1600x880.png)

### Clients & The API

Kafka doesn’t use HTTP. It uses its own TCP-based protocol. This means that you need more custom code to send and receive requests; you can’t just use any HTTP library.

Kafka provides its own libraries that implement the underlying protocol. The main clients you’d care about are the **Producer** and the **Consumer**.

  * Producer: the class that’s used for writing data to Kafka

  * Consumer: the class that’s used for reading data from Kafka

The Apache Kafka project offers a Java library that implements these:
    
    
    import org.apache.kafka.clients.producer.KafkaProducer;
    
    import org.apache.kafka.clients.consumer.KafkaConsumer;

The Producer class allows you to **send messages** to a topic. You can explicitly choose the partition or allow Kafka to do it automatically for you.

[![](https://substackcdn.com/image/fetch/$s_!CeRS!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff802585f-fda1-4fec-8142-3331044acf43_1999x467.png)](https://substackcdn.com/image/fetch/$s_!CeRS!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff802585f-fda1-4fec-8142-3331044acf43_1999x467.png)

The Consumer, on the other hand, lets you **subscribe** to and**read** the stream of messages coming in from a topic (or specific partition):

[![](https://substackcdn.com/image/fetch/$s_!JGid!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb82d66a-0863-48f2-8327-234def847299_1999x948.png)](https://substackcdn.com/image/fetch/$s_!JGid!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb82d66a-0863-48f2-8327-234def847299_1999x948.png)

This is simply the most important API I can show you concisely; a lot more exist. Kafka may seem simple, but it has many details to learn to be effective with it.

* * *

## Kafka as a Distributed System

### Brokers

Apache Kafka is designed to be a _**distributed**_ system - one meant to scale horizontally by adding more nodes. As such, any normal deployment of Kafka consists of at least _**three nodes**_.

  * _**Broker**_ : an instance of the Kafka server. This is what we call a node in the system.

  * _**Cluster**_ : all the brokers in the system.

> 💡 _**distributed (3/8)**_

### Scalability

Kafka has a ton of interesting performance optimizations (more on this in another article). Its greatest strength is its horizontal scalability.

> 💡 _**scalable (4/8)**_

The [Log data structure](https://topicpartition.io/definitions/the-log) is key to Kafka’s scalability - writes on it are O(1) and lock-free. This is because records are simply **appended to the end** and cannot be updated, nor individually deleted.

Messages within a partition are independent of each other. They have no higher-level guarantees like unique keys. This reduces the need for locking and allows Kafka to append to the Log structure as fast as the disk will allow.

Because each partition is a separate log, and you can add more brokers to the cluster, your scale is limited by how many brokers you can add.

Nothing theoretically stops you from having a Kafka cluster that accepts **50 GiB/s of writes** and then scaling it 2x to **100 GiB/s**.[5](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-5-174189967)

[![](https://substackcdn.com/image/fetch/$s_!2T4U!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F468a703f-19d0-471d-a783-efcf71bd3c18_1600x900.png)](https://substackcdn.com/image/fetch/$s_!2T4U!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F468a703f-19d0-471d-a783-efcf71bd3c18_1600x900.png)

### Replication

A partition in Kafka doesn’t live only on one broker - it is replicated across many.

A configurable setting (called **replication factor**) denotes how many copies should exist. The default and most common is **three**.

In other words, we have three copies (called **replicas**) of the Log data structure. These replicas live on the disks of different brokers.

Replication is done for many reasons, one of which is data durability[6](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-6-174189967): when three copies of the data exist, one disk failing won’t lead to data loss.

In modern cloud deployments, brokers are spread across different [availability zones](https://blog.2minutestreaming.com/p/basic-aws-networking-costs#:~:text=AZ%20\(Availability%20Zone\)%20%2D%20a%20physically%20isolated%20location%20with%20one%20or%20more%20data%20centers%2C%20inside%20a%20region). This ensures very high durability and availability. Even in the unlikely event of a whole data center burning down, the Kafka cluster would still survive.

> 💡 _**durable (5/8)**_

### Leaders

Once you start maintaining copies of data in a distributed system, you open yourself to a lot of edge cases. Keeping new data in sync is tricky. The copies must match, and the system needs to somehow agree on the latest state.

> There is a whole class of complex algorithms in computer science called [distributed consensus](https://sre.google/sre-book/managing-critical-state/), which handle this.

Kafka uses a straightforward single-leader replication model. At any time, one replica serves as the leader. The other two replicas act as followers (i.e., hot standbys).

Only the leader accepts new writes - it serves as the source of truth of the log. The followers actively replicate data from the leader. Reads can be served from both the leader and its followers.

When a broker node goes offline, the system notices it. Other brokers then take over the leadership of the partitions the dead broker led. This is how Kafka offers high availability.

> 💡 _**fault-tolerant (6/8)**_

[![](https://substackcdn.com/image/fetch/$s_!v4hU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F82a3d9fb-bd86-4760-83ec-8ff82edd6dd4_1600x880.png)](https://substackcdn.com/image/fetch/$s_!v4hU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F82a3d9fb-bd86-4760-83ec-8ff82edd6dd4_1600x880.png) A topic with 4 partitions and a replication factor of 3, with different brokers leading different replicas of the partitions. Brokers with follower replicas send fetch requests to the brokers with leader replicas to replicate the data

### The Metadata Log

In a distributed system, all nodes must agree on the latest state of the cluster. Brokers must coordinate on certain metadata changes, like electing a new leader. This is again a distributed consensus problem. Because it’s too complex for the purposes of introducing Kafka, we will simply gloss over how Kafka solves it.

Kafka uses a **centralized coordination model**[7](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-7-174189967). The central coordinator is none other than… **a log**.

Kafka durably stores all metadata changes in a special _**single-partition**_ topic called `__cluster_metadata`. This storage model inherits all the benefits from topics. It gets fault-tolerance, durability, and most importantly for metadata, **ordering**.

Each record in the log represents a **single cluster event** (a delta). When replayed fully in the same order, a node can deterministically rebuild the same cluster end state.[8](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-8-174189967)  
  
Here is a visual example of how it works in Kafka:

[![](https://substackcdn.com/image/fetch/$s_!YlTG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e6ffdc5-8d48-4a57-a3f3-ae58570b3e26_1600x1100.png)](https://substackcdn.com/image/fetch/$s_!YlTG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e6ffdc5-8d48-4a57-a3f3-ae58570b3e26_1600x1100.png)A representation of how cluster events represent changes in the cluster state, and how applying one after the other results in the same end-state

In other words, the cluster metadata topic partition is the source of truth for the latest metadata in Kafka.

Every broker in the cluster is subscribed to this topic. In real time, each broker pulls the latest committed updates. When a new record is fetched, the broker applies it to its in-memory metadata. This builds the broker’s idea of the latest state of the cluster.

If every broker is a follower of the partition, a natural question arises - who is the leader? What node gets to decide what new metadata is written to this partition?

### Controllers

Controllers serve as the control plane for a Kafka cluster. They’re special kinds of brokers that don’t host regular topics - they only do specific cluster-management operations. Usually, you’d deploy three controller brokers.

[![](https://substackcdn.com/image/fetch/$s_!2FVQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb25eae72-8faf-49ea-b094-1441e36df720_1600x1100.png)](https://substackcdn.com/image/fetch/$s_!2FVQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb25eae72-8faf-49ea-b094-1441e36df720_1600x1100.png)Every broker reads the __cluster_metadata log and replays the event logs to recreate the latest cluster state, regardless of whether it’s a controller broker or a regular broker.

At any one time, there is only one active controller - the leader of the log. Only the active controller can write to the log. The other controllers serve as hot standbys (followers).

The active controller is responsible for making all metadata decisions in the cluster, like electing new partition leaders (when a broker dies), creating new topics, changing configs at runtime, etc.

Most importantly, it’s responsible for determining broker liveness.[9](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-9-174189967)

Every broker issues **[heartbeats](https://martinfowler.com/articles/patterns-of-distributed-systems/heartbeat.html)** to the active controller. If a broker does not send heartbeats for 6 seconds straight, it is fenced off from the cluster by the controller. The controller then assigns other brokers to act as the leaders for the partitions the fenced (dead) broker led.

[![](https://substackcdn.com/image/fetch/$s_!PgiQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8b2c3903-9d0f-4f80-b2eb-32678e9f71f3_1600x1100.png)](https://substackcdn.com/image/fetch/$s_!PgiQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8b2c3903-9d0f-4f80-b2eb-32678e9f71f3_1600x1100.png)

The careful reader will now ask:

> If the active controller is responsible for electing partition leaders, who’s responsible for electing the `__cluster_metadata` leader?

The `__cluster_metadata` partition is special. A custom distributed consensus algorithm is used to elect its leader.

### KRaft

Leader election in a distributed system is a subset of the consensus problem. Many consensus algorithms exist, like Raft, Paxos, Zab, and so on.

Kafka uses its own [Raft](https://raft.github.io/)-inspired algorithm called KRaft (Kafka Raft).

KRaft has two key roles:

  1. Elect the active controller 👑

     1. The controller nodes comprise a Raft quorum

     2. The quorum runs a Raft election protocol to elect a leader of the `__cluster_metadata` partition. The leader of that partition is the active controller

  2. Agree on the latest state of the metadata log

     1. Metadata updates are first appended to the Raft log on the active controller.

     2. They are marked committed only when a majority of the quorum has persisted them.

The active controller determines the leaders for **all the other** regular topic partitions. It writes it to the metadata log, and once committed by the controller quorum, the decision is set in stone.

In other words, the way leader election in Kafka works is:

  * Leader election _**between the controllers**_ (picking the active one) is done through a variant of Raft (KRaft)

  * Leader election _**between regular brokers**_ is done through the controller.

KRaft is a relatively recent algorithm in Apache Kafka. For many years, Kafka [used ZooKeeper](https://stanislavkozlovski.medium.com/apache-kafkas-distributed-system-firefighter-the-controller-broker-1afca1eae302). Back then, there was just one controller. It performed the same tasks as today, but critically also had the responsibilities of a regular broker. Its decisions were persisted in [ZooKeeper](https://en.wikipedia.org/wiki/Apache_ZooKeeper), which used the Zab consensus algorithm behind the scenes.

This coordinator-based leader election model differs from other systems. For example, RedPanda[10](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-10-174189967) uses a separate Raft quorum per partition.

[![](https://substackcdn.com/image/fetch/$s_!_BUE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F24484c2c-1764-47e9-a347-1eabea0fdf47_1600x880.png)](https://substackcdn.com/image/fetch/$s_!_BUE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F24484c2c-1764-47e9-a347-1eabea0fdf47_1600x880.png)

* * *

## Other Features That Set Kafka Apart

### Data Retention

A key motivation in Kafka’s design was to add the ability to replay historical data and decouple data retention from clients. Alternative messaging systems would store messages as long as no client has consumed them, and once consumed, delete them.

Kafka flips this model - it offers a simple time-based SLA as the message retention policy. A message is automatically deleted if it has been retained in the broker longer than a certain period, typically 7 days. The fact that the Log data structure’s O(1) performance doesn’t degrade with a larger data size makes this feasible.

Through this model, Kafka offers the feature of **replayability** \- the ability to reprocess old historical messages. That is extremely useful in cases where, for example, a consumer has had a dormant bug in it for a while and erroneously processed messages. When the bug is fixed, the correct logic can be rerun on the same messages.

### Tiered Storage

Unfortunately, at scale, it becomes extremely tricky to manage so much historical data. A cluster with 1 GB/s of producer bandwidth would collect 1,772 TB worth of data across the cluster[11](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-11-174189967). Even spread across 100 brokers, it’s still 17TB worth of data that each broker would have to host on its disk.

With so much state, [a lot of problems start piling up](https://blog.2minutestreaming.com/p/apache-kafka-kip-405-tiered-storage):

  * ❌ The system becomes inelastic. This happens because any action or incident requires a massive amount of data to be moved. That takes a long time.

  * ❌ Further, the way the cloud is priced - [durably hosting data yourself](https://aiven.io/blog/16-ways-tiered-storage-makes-kafka-better#cost) on [HDDs tends to cost](https://getkafkanated.substack.com/p/how-to-size-your-kafka-tiered-storage-cluster) **10x more** than storing it in S3.

[![](https://substackcdn.com/image/fetch/$s_!YwzE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7b36d9ac-fff5-4881-81ab-3ce07b034a48_1456x819.png)](https://substackcdn.com/image/fetch/$s_!YwzE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7b36d9ac-fff5-4881-81ab-3ce07b034a48_1456x819.png)

The Kafka community found an ingenious way to solve [all these problems](https://aiven.io/blog/16-ways-tiered-storage-makes-kafka-better#1-simpler-operations) with one simple idea → outsource them to S3.

While it may sound overly simple or lazy, it is an **extremely elegant solution**. S3 is a [marvel of software engineering](https://bigdata.2minutestreaming.com/p/how-aws-s3-scales-with-tens-of-millions-of-hard-drives) \- it is maintained by hundreds of bright Amazon engineers. It is most likely the largest scale storage system known to man.

Kafka uses a pluggable interface to store cold data in a secondary storage tier. All three cloud object stores [are supported](https://github.com/Aiven-Open/tiered-storage-for-apache-kafka) as the secondary tier, and you are free to extend it further.

In essence, the data path in modern Kafka looks like this:

  * **Hotset Tier** : Write a message to a Kafka broker, which gets replicated across the replicas in the cluster. The message is stored on disk across all three nodes.

    * The message is asynchronously offloaded to S3 (the secondary cold tier)

  * **Cold Tier** : After a configurable amount of time (e.g., 12 hours), the message is deleted from the brokers. Its only source of truth is left in S3. It expires from S3 after a separate configurable period.

[![](https://substackcdn.com/image/fetch/$s_!pCb7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7864668e-90d6-4936-a644-be6e6ae63520_1456x819.png)](https://substackcdn.com/image/fetch/$s_!pCb7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7864668e-90d6-4936-a644-be6e6ae63520_1456x819.png)

You can still read the cold historical data from Kafka using the regular APIs. The only change is that the broker now fetches it from S3 instead of its own disk. 

This results in slightly higher latencies when fetching historical data, but can be alleviated through caching. Latency for hot data can improve because it makes it cost-effective to deploy performant SSDs (instead of HDDs). Throughput remains the same very high number. Kafka as a system becomes much more elastic because it no longer needs to move massive amounts of data whenever new brokers are added or removed.

Storing large amounts of data in Kafka also ends up becoming more than 10x cheaper.

[![](https://substackcdn.com/image/fetch/$s_!nAHf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F64dbcef7-7855-4a77-bba2-79c9d59cc986_1456x819.png)](https://substackcdn.com/image/fetch/$s_!nAHf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F64dbcef7-7855-4a77-bba2-79c9d59cc986_1456x819.png)

### Consumer Groups & Read Parallelization

Recall that the log is read sequentially and in order:

  * A topic is split into partitions because it’s Big Data™ - a single node shouldn’t be able to consume the whole topic. You need many consumers to handle the high volume of topic data.

  * **Only one consumer** is meant to read from a partition at a time. This is done to ensure message order without needing locks.

  * These consumers need to coordinate on how to split partitions between each other.

  * At the same time, Kafka’s goal is to allow **parallel** consumption (multiple readers) of **the same** partition(s) for high read-fanout cases.

Kafka addresses this through**consumer** **groups**. These groups are a set of consumer client instances (typically on separate nodes) that operate as one coherent unit. They distribute the work among themselves.

Each consumer group reads topics independently at its own pace. Consumers within the same group split the partitions between each other.

Consumer Groups support dynamic membership - you can scale consumption up or down by adding or removing members at runtime.

In essence, read throughput in Kafka can be scaled in two different ways:

  1. Add more **consumers** to your group

     * e.g., your topic went from 10 MB/s to 20 MB/s of throughput, and your two existing consumers can’t keep up. Add more so they take up the extra load.

  2. Add more consumer **groups**

     * e.g., your topic is being consumed, but you’d like a new, separate application type to process the data too. For example, a nightly accounting job wants to catch up with the last day’s worth of payments.

[![](https://substackcdn.com/image/fetch/$s_!e37q!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0eddea17-0b6e-40c9-ae07-a842b81817a9_1456x1001.png)](https://substackcdn.com/image/fetch/$s_!e37q!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0eddea17-0b6e-40c9-ae07-a842b81817a9_1456x1001.png)Two different consumer groups reading the same Kafka topic. The “fraud detection” group is expanding by adding a new consumer. A new “accounting” consumer group consisting of two consumers is starting up too.

### The Consumer Group Membership Protocol

Consumers within a group form a _**distributed processing system.**_ Unsurprisingly, we hit more distributed systems’ problems - how do we coordinate the consumers? They need to:

  * Establish consensus on progress (up to what offset did they read to)

  * Handle liveness (did a consumer go offline, and how do we take over its work)

  * Handle dynamic membership (did a new consumer come online)

  * Distribute work between themselves (which consumer will take which partition)

Kafka consumers within the same group don’t talk to each other. They coordinate indirectly through a Kafka broker. The broker that leads a certain group is called the **Group Coordinator**.

Kafka again uses a **centralized coordination model** \- the Group Coordinator makes the decisions. Consumers heartbeat to the coordinator and, through a pull-based model, inform themselves of what work they should do.

#### The __consumer_offsets Topic

The Group Coordinator also acts as the “database” which stores the progress of each consumer.

Consumer Groups store a simple mapping of _**`{partition, offset}**_ ` in a special Kafka topic called _**`__consumer_offsets`**_. This helps them save the progress on what record they have _**read up to.**_

When a consumer reads messages, it commits the offset up to which it has processed the log via the coordinator broker. This regular checkpointing allows for smooth failovers. In the event of failure, the consumer can restart and resume from where it ended, or another consumer can come and pick the work back up.

The special offsets topic has many partitions spread throughout brokers. Each group is associated with a particular partition. The leader of the particular partition that the group is associated with acts as the Group Coordinator for that group. In that sense, every broker in the cluster can act as a coordinator for some consumer group. This prevents hot spots where one broker handles all the consumer groups in the cluster.

[![](https://substackcdn.com/image/fetch/$s_!fKBi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcfa1305a-7f3a-42a6-8779-552e351fbbef_1456x1001.png)](https://substackcdn.com/image/fetch/$s_!fKBi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcfa1305a-7f3a-42a6-8779-552e351fbbef_1456x1001.png)Two different consumer groups reading the same Kafka topic

The consumer group protocol is a critical part of Kafka.

It is generic enough to support other use cases beyond consumer groups. It therefore provides**a way for external distributed systems to establish leader election and durably persist state through Kafka**.

Keep this in mind: the next three systems we’ll discuss depend on the group protocol to work as distributed systems.

But first, a little about transactions:

### Transactions & Exactly Once Processing

I will try to keep this brief because it can get pretty complicated - Kafka supports **transactions**. But they aren’t quite like database transactions - it’s more about message visibility control.

A transaction in Kafka means that:

  * A producer can send many messages. Those messages can go to different topics or partitions. They can also reach various brokers.

  * Those messages will **atomically** either be committed or aborted across all brokers.

Technically, this happens _from the perspective_ _of a consumer._ In other words, the messages are still written to the topics, but consumers can be configured to skip non-committed ones. This is client-side filtering at work.

Marking transactions as committed or aborted is achieved through a two-phase commit protocol. It again relies on a centralized coordination model. A Transaction Coordinator broker makes this work. It’s pretty complex.

[![](https://substackcdn.com/image/fetch/$s_!h6st!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbadb53ae-4d07-4c02-821e-8e41856e9731_1456x1001.png)](https://substackcdn.com/image/fetch/$s_!h6st!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbadb53ae-4d07-4c02-821e-8e41856e9731_1456x1001.png)A producer writing messages to multiple topics and partitions in the same transaction. It manages and commits the transaction via the broker acting as the Transaction Coordinator

The important thing with transactions is that they enable message deduplication in common cases.

  * **⚡️ Network/broker blips** : if the network drops the broker response packets, or the broker restarts, the same producer client will idempotently[12](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-12-174189967) write its message without creating duplicates.

  * **💥 Producer client blips** : if the producer itself restarts from a clean state, it will fetch its monotonically increasing ID and bump an epoch. This way, a potentially old zombie instance with the old epoch can’t interfere with the transaction.

This doesn’t remove all cases of duplicates. Edge cases from external systems can still exist.[13](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-13-174189967)

However, when reads and writes only involve Kafka (and no other external system), exactly once processing is possible.💡

This is actively supported and used in Kafka Streams, as we will cover shortly 👇

* * *

### Other Kafka Components

The [Apache Kafka GitHub project](https://github.com/apache/kafka) consists of a few components, two of which we already covered:

  * **Kafka Core** : the brokers, controllers, and coordinators (back-end).

  * **Kafka Clients** : the Kafka client libraries (producer, consumer).

These are two more that we will focus on now:

  * **Kafka Streams**

  * **Kafka Connect**

#### Kafka Streams

A _**stream processor**_ is an application that:

  * continuously processes messages

  * works performantly and at scale

  * emits the results in real-time

  * supports complex stateful operations like windowed aggregations and joins of multiple streams of data[14](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-14-174189967)

**Kafka Streams** is a high-level [client-side stream processing](https://github.com/apache/kafka/blob/trunk/streams/src/main/java/org/apache/kafka/streams/KafkaStreams.java) Java library for Kafka. It offers rich higher-level stream processing APIs that basically do this:

  1. continuously read a stream of messages from a set of Kafka input topics

  2. perform processing on these messages (map, filter, joins, sums, stateful window aggregations, etc.)

  3. continuously write the results into an output topic

Here is a simple pseudocode example of its declarative API:

[![](https://substackcdn.com/image/fetch/$s_!uKgV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb5c3b7c8-9425-489d-a201-ec4d8a1c53ba_1999x1069.png)](https://substackcdn.com/image/fetch/$s_!uKgV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb5c3b7c8-9425-489d-a201-ec4d8a1c53ba_1999x1069.png)

This code continuously counts the sum of human page views over the last minute and produces it in a new topic. Here is an example of what the Kafka topics would look like:

[![](https://substackcdn.com/image/fetch/$s_!Ee47!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5cdaf677-5aca-4458-b1bb-8fb0d8d6d53f_1456x1001.png)](https://substackcdn.com/image/fetch/$s_!Ee47!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5cdaf677-5aca-4458-b1bb-8fb0d8d6d53f_1456x1001.png)An example of how the records in the source page view topic get processed into page traffic sums

This API is intended to be used within your own Java applications. It works like the consumer. It helps you scale by spreading the stream processing work over multiple applications (just like consumer groups do). One difference is that it also lets you spread the work through **threads**. It uses the same consumer group protocol underneath to coordinate work between instances.

[![](https://substackcdn.com/image/fetch/$s_!76zL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F92b1f285-20ee-41d7-acd3-8c2757b7be56_1456x1001.png)](https://substackcdn.com/image/fetch/$s_!76zL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F92b1f285-20ee-41d7-acd3-8c2757b7be56_1456x1001.png)An example two-node stream processing job

It is technically possible to achieve this with your own code using the simple producer/consumer libraries, but it’d be a lot of work. Kafka Streams is a higher-level abstraction above both clients with a ton of extra processing, orchestration, and stateful logic on top. 👌

Kafka Streams only works with Kafka. It takes input from a Kafka topic and sends output to another Kafka topic. This setup allows Kafka Streams to guarantee exactly once processing by using Kafka Transactions. Practically speaking, this means that it can _**atomically**_ process data.

For example, it could read a set of payment messages in a Kafka topic, calculate the sum, and persist the result in another Kafka topic, with a 100% guarantee that no message was lost or double-counted in the process.

> If interested in more, here is a quick introduction to [Kafka Streams](https://bigdata.2minutestreaming.com/p/what-is-kafka-streams-api-guide).

#### Schema Registry

Kafka does not support types (e.g., int64) nor schemas[15](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-15-174189967). Messages are just raw bytes[16](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-16-174189967). To process a message, like summing order payment values, you need to be able to parse the message structure and the exact value field. How is this achieved, then?

There’s no “official” way, because the open source Apache project does not support schemas. There is a common convention, though. That is to use an external HTTP service with a database that stores the schemas.

In practice, a Kafka topic acts as this database. It stores a simple pair of _**{schema, topic}**_. Kafka clients connect to the service, download the schema, and use it whenever they serialize/deserialize messages.

The first such registry was a [source-available project](https://github.com/confluentinc/schema-registry) by Confluent called Schema Registry. However, without official Apache support or a truly open-source license, the ecosystem has fragmented. There are many different service implementations[17](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-17-174189967) today. Many Kafka users also opt to manage schemas in their own unconventional ways.

The way the end-to-end path conventionally works with schemas:

  1. Producers decide on a schema, associate it with a topic, and register it in the registry

  2. Producers then serialize the message in the correct structure (including the unique schema ID in the message) and write it to Kafka

  3. Consumers download the message, parse the schema ID, then fetch (and cache) the schema from the registry

  4. Consumers use the schema to deserialize the message

[![](https://substackcdn.com/image/fetch/$s_!be6u!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe7617c20-bf48-4c91-aa6c-031d19d21620_1456x1001.png)](https://substackcdn.com/image/fetch/$s_!be6u!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe7617c20-bf48-4c91-aa6c-031d19d21620_1456x1001.png)

#### Kafka Connect

Kafka [was created to solve LinkedIn’s data integration problem](https://bigdatastream.substack.com/p/why-was-apache-kafka-created). It is meant to move data between different systems. This can be difficult because each system can have its own API, its own protocol (TCP, HTTP, JDBC, etc.), its own format (XML, JSON, Protobuf, Avro), and different compatibility guarantees.

Kafka Connect helps standardize this. Connect is both a framework (set of APIs) and a **runtime** for plugins that connect Kafka with external systems[18](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-18-174189967).

For the end user, it’s a no-code/low-code framework to plumb popular systems to Kafka and back (think ElasticSearch, Snowflake, PostgreSQL, BigQuery).

This ensures a single, standardized way to integrate systems together. The tricky bits of code that guarantee fault-tolerance, ordering, and exactly-once processing are written once (in the form of plugins) and battle-tested.

[![](https://substackcdn.com/image/fetch/$s_!aKO3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd4d18c16-872c-4e57-af3f-9b5e409ad95a_1456x1001.png)](https://substackcdn.com/image/fetch/$s_!aKO3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd4d18c16-872c-4e57-af3f-9b5e409ad95a_1456x1001.png)An example of Kafka Connect integrating data into Kafka. Source Connectors read data from MongoDB and write to Kafka. Sink Connectors read data from Kafka and write it to Snowflake.

Connect has three main terms one should know about:

  * **Connect Workers** : the simple nodes that form a distributed Connect Cluster

  * **Connect Herder** : a worker that acts as the manager of the cluster. It exposes a REST API with which users can check the status of tasks, start new ones, etc

  * **Connectors** : the plugins (or libraries) that run on the workers. They contain the code needed to integrate with other systems

    * A **Source** Connector reads data from an external system and writes it to Kafka (System->Kafka)

    * A **Sink** Connector reads data from Kafka and writes it to an external system (Kafka->System)

The end user spins up a cluster with several Worker nodes. They install the particular Connector plugin jars on these nodes. Then, they start the integration with a simple HTTP POST request.

This again forms another distributed processing system. The Herder leader election, general cluster membership, and the distribution of new tasks throughout the group of Workers are all done transparently via Kafka’s Consumer Group protocol.

Essentially, Connect is a lot of plugin-specific integration logic on top of the regular KafkaProducer and KafkaConsumer APIs. [Hundreds of Connector plugins](https://www.confluent.io/hub/) exist, which give Kafka its incredibly rich integration capabilities.

> **💡** _**rich integration (8/8)**_

* * *

### 🎬 Conclusion

With that, we’ve gone over all the important internals of Apache Kafka. I repeat the introductory sentence, which you can now hopefully understand much better:

> 💡 Kafka is an open-source, distributed, durable, very scalable, fault-tolerant pub/sub messaging system with rich integration and stream processing capabilities.

It achieves this through many internal details, including but not limited to:

  * The Producer & Consumer libraries & APIs

  * Topics, Partitions, and Replicas

  * Brokers and KRaft Controller Quorums

  * Idempotency, Transactions, and Exactly-Once Processing

  * Tiered Storage

  * Consumer Groups & the Consumer Group Protocol

  * Kafka Streams

  * Kafka Connect

  * Schema Registry

This is why Kafka is the Swiss army knife of data engineering.

It is a very active open-source project that is constantly evolving. A few notable features that are currently being worked on are:

  * [Queues](https://blog.2minutestreaming.com/p/apache-kafka-share-group-queues-kip-932): the ability to read a partition with queue-like semantics. Queues have no ordering but allow for multiple consumers to read from the same log with per-record acknowledgement. This is different from Kafka’s exclusive one-consumer-per-partition model. In that model, consumers read data in order and only know “I’ve read **up until** this message”.

  * [Diskless Topics](https://blog.2minutestreaming.com/p/diskless-kafka-topics-kip-1150): the ability to host topic partitions in a leaderless way. This happens by leveraging object storage (S3) as the data layer instead of brokers’ disks. This feature can cut cloud costs by **90%** \+ (!), further boost scalability, and simplify managing Kafka.

  * [Iceberg Topics](https://aiven.io/blog/iceberg-topics-for-apache-kafka-zero-etl-zero-copy): the ability to store your Kafka data in an [open table format](https://bigdata.2minutestreaming.com/p/meet-your-new-data-lakehouse-s3-iceberg) (Iceberg) in a zero-copy way.

Many alternative proprietary systems compete with Apache Kafka. The one common denominator is that they all use the same Kafka protocol and API. They just implement it differently. These include, but are not limited to, Confluent Kora, AWS MSK, RedPanda, WarpStream, BufStream, Aiven Inkless, AutoMQ, and more.

The space is incredibly rich and rapidly evolving. If you’re further interested in the world of event streaming and Apache Kafka, make sure to stay in touch. 🙂

* * *

👋 I'd like to thank [Stanislav](http://linkedin.com/in/stanislavkozlovski) for writing this newsletter!

  * Also subscribe to his newsletters: **[2 Minute Streaming](https://blog.2minutestreaming.com/)** and **[Big Data Stream](https://bigdata.2minutestreaming.com/)**.

It’ll help you master Kafka quickly.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋 Find me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 170K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter.

You are now 170,001+ readers strong, very close to 171k. Let’s try to get 171k readers by 28 September. Consider sharing this post with your friends and get rewards.

Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-kafka-works?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

Apache®, Apache Kafka®, Kafka, and the Kafka logo are either registered trademarks or trademarks of the Apache Software Foundation in the United States and/or other countries. No endorsement by The Apache Software Foundation is implied by the use of these marks.

[1](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-1-174189967)

This adoption data is an estimation by Confluent, the company founded by the creators of Kafka. I have no good way to confirm it.

[2](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-2-174189967)

That’s why its official name is “Apache Kafka”, although we still often say Kafka for short.

[3](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-3-174189967)

A Streaming Platform means a system that allows you to store and process a large volume of streams of data. For example, a company like Uber has millions of drivers constantly streaming their GPS coordinates to Uber’s backend systems. A streaming platform can scale this data and process it in real time as it comes. This helps Uber make use of the data (e.g, recompute the route, figure out where there is traffic, etc).

[4](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-4-174189967)

A sequential read means reading bytes laid out contiguously on the physical drive. Random reads mean the opposite - you have to jump to different parts of the drive to read the bytes.

[5](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-5-174189967)

Theoretically, you can do this by using very good high-end hardware with ample networking capacity. An example could be 75 brokers each accepting 512 MiB/s of writes. Modern SSDs can do this without a problem. Practically speaking, it would be very hard to operate and require custom code. Therefore most prefer to split such workloads into many clusters.

[6](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-6-174189967)

**Durability** refers to long-term data protection - ensuring that data never gets lost or corrupted.

[7](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-7-174189967)

**Centralized coordination** in distributed systems means all the nodes rely on a single authority, like a coordinator or a leader. This authority makes decisions, enforces rules, and keeps the state consistent. Alternative interesting models include things like quorums, gossip, and CRDTs.

[8](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-8-174189967)

**Stream-table duality** is the simple idea that a stream of events and a table are two sides of the same coin. Any mutation to a table (update/delete/insert) is in itself an event. The table simply represents the end state of all events. If you start from scratch and replay all the events, you reach the same table.  
_**Event-sourcing**_ , on the other hand, is the design of a system around this log-based stream of events.  
The ideas have their roots in **materialized view theory** in databases and in **change data capture**. (Some sources, if you’re extra curious:[ stream-table duality](https://medium.com/event-driven-utopia/the-duality-of-streams-and-tables-why-it-matters-ed9bb17e7505) and [event-sourcing](https://martinfowler.com/eaaDev/EventSourcing.html))

[9](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-9-174189967)

**Liveness** is a tricky distributed systems term that basically means that the system will eventually make progress (won’t freeze up). In the context of broker liveness, it means that a dead broker will get fenced, so partitions don’t get stuck on a dead node. This allows the cluster to move forward. Liveliness technically also means that an alive broker will eventually be unfenced.

[10](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-10-174189967)

[RedPanda](https://github.com/redpanda-data/redpanda) is a C++ rewrite of Kafka, purpose-built to offer ultra-low latencies. It is fully compatible with the Kafka protocol & API. However, it takes some different design choices. For example, it uses a Raft quorum for each partition. It features a thread-per-core execution model based on the [Seastar](https://seastar.io/) library (similar to [Scylla](https://www.scylladb.com/), the C++ [Cassandra](https://cassandra.apache.org/_/index.html) rewrite). It is also simpler because it directly ships with a schema registry and HTTP proxy.

[11](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-11-174189967)

1 GiB/s * 60 (seconds) * 60 (minutes) * 24 (hours) * 7 (days) * 3 (replication) == 1,814,400 GiB == 1,771.875 TiB; Not to mention you want to run your disks with ample free space capacity (e.g., 50%) - you’d need to provision ~3500TB worth of disks!

[12](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-12-174189967)

**Idempotency** means not repeating the same action twice. If a “Create User Bob” request is sent twice, an idempotent system would create the user only once. In Kafka, this is achieved by associating a unique monotonically increasing ID with each message. So you could send the message (“Create-User-Bob, 1”) twice, but Kafka will accept it only once because of the unique ID. This is not foolproof, though, because the unique ID comes from the Kafka Producer client. Two producers can therefore create the same message with different IDs.

[13](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-13-174189967)

Most simply said, imagine you have an HTTP service receiving requests. The service processes the request “Create user Bob” and successfully produces the message to Kafka. Before the service responds with an HTTP response to the user, it crashes. The user then retries the same HTTP request, and the new service produces the same “Create user Bob” into Kafka. From Kafka’s PoV, this is fine because it sees both as separate messages. The HTTP service evidently does not support handling idempotent requests and exactly-once processing.

[14](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-14-174189967)

**Stream processing** \- the easiest way to understand it is through the opposite extreme - **batch processing**. Imagine you are Tesla and are collecting data from your fleet of cars. At the end of each business day, you run a big report. The report joins data from multiple sources and creates a dashboard that an executive in Tesla sees. They use it to see summaries like the number of kilometers crossed, times they had to charge the Tesla, how often X feature was used. It runs once a day and calculates data after a cut-off date (e.g., end of day). Stream Processing would be the opposite - it would have the cars continuously emit data like their tire pressure (psi). It would then perform windowed aggregations on this data to understand, in real time, what’s happening to the car. If the tire pressure went from 45psi to 41psi over the course of 15 minutes, you’d know **the tire is losing pressure**. If the tire pressure went from 45psi to 20psi over the course of _**10 seconds**_ , you’d know you **blew out a tire**. Tesla could implement this example by deploying a lot of Kafka Stream jobs in their back-end. (assuming the data exists in Kafka)

[15](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-15-174189967)

A _**schema**_ basically means the expected structure of your data. A database table has a very strict schema - you know what type each field is and what fields there are (e.g id BIGINT, name VARCHAR, cost DECIMAL, is_premium BOOLEAN). You can’t add fields or types that don’t match, like a string for an ID. A JSON object by itself is schemaless - you can modify it however you want and it’s still a valid JSON object (the only question is whether your server will accept it, and that depends on the modification). In the same way, a blob of bytes doesn’t have a strict structure - you can add any garbage in there. There is a project that adds a strict structure to JSON objects called [JSON Schema](https://json-schema.github.io/json-schema/example1.html). Similarly, there are projects that add strict structure to blobs of bytes ([Protobuf](https://protobuf.dev/), [Avro](https://avro.apache.org/)). It’s important to have schemas because they are critical to validating and catching bugs in your data early.

[16](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-16-174189967)

I believe this was a[ big mistake](https://bigdata.2minutestreaming.com/i/170964904/schemaless-kafka) by the project. Every important use case, like Connect, Streams, and general processing, must understand the data’s structure.

[17](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-17-174189967)

A few schema registry implementations are[ Karaspace](https://github.com/Aiven-Open/karapace) (Apache-licensed),[ AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/schema-registry-integrations.html) (proprietary),[ ApiCurio](https://github.com/Apicurio/apicurio-registry) (Apache-licensed),[ Buf Schema Registry](https://buf.build/product/bsr) (proprietary),[ and Redpanda](https://github.com/redpanda-data/redpanda) (mixed licenses).

[18](https://newsletter.systemdesign.one/p/how-kafka-works#footnote-anchor-18-174189967)

A **runtime** here means that you deploy the Connect software, and then, via `curl`, schedule extra pre-defined code to run on top of it (plugins). A _**framework**_ means that you’re free to write your own plugins that use the API if you’d like.

520

11

74

Share

PreviousNext

[![Stanislav Kozlovski's avatar](https://substackcdn.com/image/fetch/$s_!0lM5!,w_52,h_52,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03bc8810-0db9-40c9-a28b-1a4752b7a135_800x800.jpeg)](https://substack.com/@stanislavkozlovski?utm_source=byline)| A guest post by| [Stanislav Kozlovski](https://substack.com/@stanislavkozlovski?utm_campaign=guest_post_bio&utm_medium=web)7yr experience with Kafka; committer; writes concisely about kafka and big data engineering| [Subscribe to Stanislav](https://bigdata.2minutestreaming.com/subscribe?)  
---|---  
  
#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Favour Lawrence's avatar](https://substackcdn.com/image/fetch/$s_!jSq3!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86627fc9-c8d0-49f8-8d0c-5dd8e22b79d9_2500x2000.jpeg)](https://substack.com/profile/99824971-favour-lawrence?utm_source=comment)

[Favour Lawrence](https://substack.com/profile/99824971-favour-lawrence?utm_source=substack-feed-item)

[Sep 26](https://newsletter.systemdesign.one/p/how-kafka-works/comment/160124876 "Sep 26, 2025, 1:29 PM")

Liked by Stanislav Kozlovski, Neo Kim

This article is so comprehensive. One can find their way round kafka with this .

ReplyShare

[![Dinesh Solanki's avatar](https://substackcdn.com/image/fetch/$s_!dSG2!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb5a7bfb7-c08f-4f05-bceb-5da669d7e823_96x96.png)](https://substack.com/profile/327876431-dinesh-solanki?utm_source=comment)

[Dinesh Solanki](https://substack.com/profile/327876431-dinesh-solanki?utm_source=substack-feed-item)

[Sep 26](https://newsletter.systemdesign.one/p/how-kafka-works/comment/160022994 "Sep 26, 2025, 3:33 AM")

Liked by Stanislav Kozlovski, Neo Kim

This article is awesome. Explaining every capabilities of the tool so that we can go deeper as needed. As a beginner in kafka, this is very helpful. Thanks!

ReplyShare

[9 more comments...](https://newsletter.systemdesign.one/p/how-kafka-works/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture