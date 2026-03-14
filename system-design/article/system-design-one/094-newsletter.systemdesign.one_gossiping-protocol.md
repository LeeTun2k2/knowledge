[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Everything You Need to Know About Gossip Protocol

### #25: Learn More - How Distributed Systems Gossip (6 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Nov 28, 2023

34

5

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

  *  _[Share this post](https://newsletter.systemdesign.one/p/gossiping-protocol/?action=share) & I'll send you some rewards for the referrals._

The 2 main problems in distributed systems are state management and communication.

A peer-to-peer service like gossip protocol can be used to solve them.

The gossip protocol handles system state with high availability.

Also application-level data can be piggybacked in gossip messages as key-value pairs.

The gossip protocol is also called the _epidemic protocol_. Because the messages get transferred like how epidemics spread.

* * *

## [Break into tech in 6 weeks 💻 (Featured)](https://www.entrylevel.net/)

[![EntryLevel](https://substackcdn.com/image/fetch/$s_!Nusk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1cace244-6622-4315-b817-dcdb3f2f969e_1790x726.png)](https://substackcdn.com/image/fetch/$s_!Nusk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1cace244-6622-4315-b817-dcdb3f2f969e_1790x726.png)

Learn tech skills in 6 weeks to create a portfolio and land a remote job.

[Try it](https://www.entrylevel.net/)

* * *

## Why Use Gossip Protocol?

There are different ways to broadcast a message in distributed systems. They are:

#### 1\. Point-To-Point Broadcast

[![Point-To-Point Broadcast](https://substackcdn.com/image/fetch/$s_!WuNJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5c7bbd9c-6fdb-43f8-baea-d59c8def5cdf_1075x367.png)](https://substackcdn.com/image/fetch/$s_!WuNJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5c7bbd9c-6fdb-43f8-baea-d59c8def5cdf_1075x367.png)Point-To-Point Broadcast

The producer sends the messages directly to the consumer. Also the producer retries if the consumer fails to accept the message. 

Yet the message will be lost if both the producer and consumer fail simultaneously.

#### 2\. Eager Reliable Broadcast

[![Eager Reliable Broadcast](https://substackcdn.com/image/fetch/$s_!UmYu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4dfa05b6-c4a9-4feb-a7b0-8919c340b445_1012x723.png)](https://substackcdn.com/image/fetch/$s_!UmYu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4dfa05b6-c4a9-4feb-a7b0-8919c340b445_1012x723.png)Eager Reliable Broadcast

Each server broadcasts a message to every other server in the system. 

Although it’s fault-tolerant, this approach is problematic because:

  * High bandwidth usage due to _O(n^2)_ messages broadcast to _n_ number of servers 

  * Network bottleneck due to _O(n)_ linear broadcast

  * Extra storage is needed to maintain the list of nodes

#### 3\. Gossip Protocol

[![Gossip Protocol](https://substackcdn.com/image/fetch/$s_!puhO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4152eaa8-da1c-41fb-860c-31f931c620de_864x432.gif)](https://substackcdn.com/image/fetch/$s_!puhO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4152eaa8-da1c-41fb-860c-31f931c620de_864x432.gif)Gossip Protocol

Each server periodically sends the messages to a set of random servers. And the entire system will receive a message eventually.

Gossip protocol is a good choice for communication in a large-scale system because:

  * Each server transfers only a limited number of messages

  * Limited bandwidth usage

  * Tolerant to network and server failures

The gossip protocol is reliable because many servers retransmit the messages.

Yet gossip protocol can be used to keep nodes consistent only if:

  * Operations are commutative

  * Serializability is not needed

The number of servers that receive a message from a particular server is called the **Fanout**.

The number of gossip rounds needed to transfer a specific message across the entire system is called the **Cycle**.

A case study of a gossiping system with 128 servers needed less than 2 percent of CPU and 60 KBps of bandwidth.

Here are some _gossip protocol simulations_ :

  * [Serf Convergence Simulator](https://www.serf.io/docs/internals/simulator.html)

  * [Gossip Simulator](https://flopezluis.github.io/gossip-simulator/)

### Gossip Protocol Properties

There is no formal definition for gossip protocol. But it’s expected to have certain properties:

  * A peer server must be selected randomly

  * Each server stores only location information. And is unaware of the entire system state

  * Interactions between servers are periodic and pairwise

### Gossip Algorithm

Each server maintains a list of servers and their metadata.

Here is how the gossip algorithm works:

  1. Gossip periodically to a random server

  2. The server inspects the received gossip message

  3. The server merges the message with the highest version to local data

### Gossip Protocol Implementation

The gossip protocol uses the _peer sampling service_ to find the peer servers.

Here is how the peer sampling service works:

  1. Initialize each server with a partial view of the system

  2. Merge the server’s view with a peer server’s view on gossip exchange

A server initiating a gossip exchange sends a _gossip digest synchronization_ message. It contains a list of _gossip digests_.

Here is a sample schema of gossip digest:
    
    
    EndPointState:
    10.0.1.41
    
    HeartBeatState: 
    generation: 1259904231, version: 681
    
    ApplicationState: 
    "average-load": 2.7, generation: 3659909691, version: 42
    
    ApplicationState: 
    "bootstrapping": pxLpassF9XD8Kymj, generation: 1281909615, version: 91
    

The gossip messages get sent over User Datagram Protocol (**UDP**) or Transmission Control Protocol (**TCP**). 

A server is considered healthy if the heartbeat counter keeps incrementing. So the heartbeat counter of a server is incremented on each gossip exchange.

The gossip protocol removes the data from a server using a tombstone. The **tombstone** is a special data entry to invalidate a data key without the actual deletion of the data.

* * *

## Gossip Protocol Types

There are 3 types of gossip protocols. They are categorized based on message transfer time and the network traffic created.

#### 1\. Anti-Entropy Gossip Protocol

This variant sends an unbounded number of messages without termination.

It’s usually used to reduce the entropy between replicas of a stateful service like the database. The server with the newest message sends it to other servers.

Yet it causes high bandwidth usage due to the transfer of the entire dataset.

#### 2\. Rumor-Mongering Gossip Protocol

This variant’s cycle is more frequent compared to the anti-entropy cycle. So it will likely flood the network.

Yet rumor-mongering protocol uses less bandwidth because only the latest changes get transferred.

#### 3\. Aggregation Gossip Protocol

This variant creates a system-wide value by sampling data across each server. And then combining them.

* * *

## How Gossip Protocol Spreads Messages

There are different ways to spread gossip messages. So it should be chosen based on the service needs and available network conditions. 

The 3 ways to spread gossip messages are:

#### 1\. Push Model

The server with the newest message sends it to a random set of servers.

The push model is efficient if there are only a few messages because it avoids the traffic overhead.

#### 2\. Pull Model

Each server actively polls a random set of servers for newer messages.

The pull model is efficient if there are many new messages.

#### 3\. Push-Pull Model

The server pushes the newest messages and also polls for newer messages.

The push model is efficient in the initialization phase when there are only a few active servers.

While the pull model becomes efficient if there are many active servers.

* * *

## Gossip Protocol Use Cases

The gossip protocol can be used to implement:

  * First-in-first-out (**FIFO**) broadcast

  * Causality broadcast

  * Total order broadcast

Some of the popular use cases of gossip protocol are:

  * Spreading server state across the system in Amazon S3 

  * Detecting failures and tracking server membership in Amazon Dynamo 

  * Propagating server metadata in the Redis cluster

  * Spreading the nonce value across the mining servers in Bitcoin

  * Electing the leader and detecting agent failures in Consul

  * Transferring [consistent hash ring](https://newsletter.systemdesign.one/p/what-is-consistent-hashing) state in Riak database 

## Gossip Protocol Advantages

The advantages of gossip protocol are:

  * Fault-tolerant: messages flow via many routes making it tolerant against unreliable networks

  * Scalable: each server interacts with a limited number of servers and sends a fixed number of messages. Also a server doesn’t wait for an acknowledgement

  * Decentralized: it uses a peer-to-peer communication model

## Gossip Protocol Disadvantages

The disadvantages of gossip protocol are:

  * Eventually consistent: it’s slower compared to multicast. Also gossip behavior depends on network topology

  * Difficult to debug and test: non-deterministic and distributed nature makes it hard to debug and reproduce failures

  * High bandwidth usage: A single message will get sent many times across different servers

The takeaway is to avoid gossiping in the real world. But gossip in the world of distributed systems when eventual consistency is fine.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/gossiping-protocol?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![Everything You Need to Know About Consistent Hashing](https://substackcdn.com/image/fetch/$s_!BUmN!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47e1805e-49ac-4d7c-ae59-f74554f58b44_1280x720.gif)Everything You Need to Know About Consistent Hashing[NK](https://substack.com/profile/135589200-nk)·November 21, 2023[Read full story](https://newsletter.systemdesign.one/p/what-is-consistent-hashing)](https://newsletter.systemdesign.one/p/what-is-consistent-hashing)

[![What Happens When You Type google.com in the Browser?](https://substackcdn.com/image/fetch/$s_!JDEg!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc4f1940e-7b40-4498-8e3f-3012ae74dc06_1280x720.png)What Happens When You Type google.com in the Browser?[NK](https://substack.com/profile/135589200-nk)·November 26, 2023[Read full story](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser)](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser)

* * *

## References

  * https://www.cs.cornell.edu/projects/Quicksilver/public_pdfs/2007PromiseAndLimitations.pdf

  * https://systemdesign.one/gossip-protocol/

34

5

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture