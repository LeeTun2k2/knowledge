[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Facebook Was Able to Support a Billion Users via Software Load Balancer ⚡

### #58: Break into Meta Engineering (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 11, 2024

119

7

10

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Facebook scaled its load balancer to a billion users. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/facebook-load-balancer/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, Facebook ran on a single server.

Life was good.

But as more users joined, they faced scalability issues.

Although adding more servers temporarily solved their scalability problems, routing traffic became difficult.

So they were sad & frustrated.

[![Facebook Load Balancer](https://substackcdn.com/image/fetch/$s_!TgIf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5df5644a-d1cd-4b6a-a8a2-ac8db133bd77_500x661.jpeg)](https://substackcdn.com/image/fetch/$s_!TgIf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5df5644a-d1cd-4b6a-a8a2-ac8db133bd77_500x661.jpeg)Image Created With [imgflip](https://imgflip.com/)

Until one morning when they had a smart idea to create a _software_ load balancer.

Yet they wanted to keep it reliable and offer extreme scalability.

So they used simple ideas to build it.

Onward.

* * *

### [This Post Summary - Instagram](https://www.instagram.com/systemdesignone/)

I wrote a summary of this post

(save it for later):

[systemdesignone](https://instagram.com/systemdesignone)

[![](https://substackcdn.com/image/fetch/$s_!_1em!,w_640,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F__ss-rehost__IG-meta-DA-5qbkt2un.jpg)](https://instagram.com/p/DA-5qbkt2un)

A post shared by [@systemdesignone](https://instagram.com/systemdesignone)

And I’d love to connect if you’re on Instagram:

[Follow Instagram](https://www.instagram.com/systemdesignone/)

* * *

## Facebook Load Balancer

Here’s how they scaled the load balancer from 0 to 1 billion users:

### 1\. One Million Users

They send encrypted data using Transport Layer Security (**[TLS](https://www.cloudflare.com/en-gb/learning/ssl/transport-layer-security-tls/)**). Think of TLS as a security protocol.

And the server must decrypt the data before processing it. This is called **[TLS termination](https://www.haproxy.com/glossary/what-is-ssl-tls-termination)**.

Yet it needs a ton of computing power.

[![Layer 7 Load Balancer Proxying Requests to the Server](https://substackcdn.com/image/fetch/$s_!Ti-Z!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F137938e6-6362-4b8b-b5c6-8cf792d3f1fe_1639x370.png)](https://substackcdn.com/image/fetch/$s_!Ti-Z!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F137938e6-6362-4b8b-b5c6-8cf792d3f1fe_1639x370.png)Layer 7 Load Balancer Proxying Requests to the Server

  * So they do TLS termination on the load balancer. And forward _decrypted_ data to servers. Thus reducing server load.

They use a **[layer 7 load balancer](https://kemptechnologies.com/load-balancer/layer-7-load-balancing)** to proxy requests. It routes traffic based on application data - URLs, HTTP headers, and so on.

### 2\. Ten Million Users

Yet a single layer 7 load balancer might crash with high traffic.

[![Layer 4 Load Balancer Routing Requests to Layer 7 Load Balancers](https://substackcdn.com/image/fetch/$s_!I2xD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa0220307-24ef-4a35-b453-042f43cb5c40_1810x291.png)](https://substackcdn.com/image/fetch/$s_!I2xD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa0220307-24ef-4a35-b453-042f43cb5c40_1810x291.png)Layer 4 Load Balancer Routing Requests to Layer 7 Load Balancers

So they added more layer 7 load balancers and put a **[layer 4 load balancer](https://www.haproxy.com/glossary/what-is-layer-4-load-balancing)** in front.

A layer 4 load balancer does Transmission Control Protocol (**[TCP](https://www.fortinet.com/resources/cyberglossary/tcp-ip)**) routing. Put simply, it doesn’t inspect application-level data. Instead forwards traffic based on IP address & port number.

Thus offering better performance.

Also they use [consistent hashing](https://systemdesign.one/consistent-hashing-explained/) for routing requests to layer 7 load balancers.

  * The IP addresses, and port numbers get hashed to find the target load balancer.

While a request gets routed to the next layer 7 load balancer if the original layer 7 load balancer fails. And future requests get routed to the original load balancer once it’s live again.

[![Service Discovery to Find Layer 7 Load Balancers](https://substackcdn.com/image/fetch/$s_!uPh_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F669ff7e1-3462-4c5a-b1be-c3f7f8fd2f89_1288x330.png)](https://substackcdn.com/image/fetch/$s_!uPh_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F669ff7e1-3462-4c5a-b1be-c3f7f8fd2f89_1288x330.png)Service Discovery to Find Layer 7 Load Balancers

  * They use Apache Zookeeper for [service discovery](https://systemdesign.one/what-is-service-discovery/). It lets the layer 4 load balancer find available layer 7 load balancers.

Besides a [TCP connection](https://www.fortinet.com/resources/cyberglossary/tcp-ip) must be created each time to send requests.

Yet it’s not efficient to create one every time due to connection overhead.

  * So they maintain _open_ TCP connections between layer 4 and layer 7 load balancers.

And keep a separate state table with the list of open connections. It lets them do failover quickly if the layer 4 load balancer fails.

### 3\. Hundred Million Users

But a single layer 4 load balancer might crash with explosive traffic.

[![Router Proxying Requests to Layer 4 Load Balancers](https://substackcdn.com/image/fetch/$s_!umTz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8a1a2d91-238e-469a-8caa-dad8c3520b21_2068x320.png)](https://substackcdn.com/image/fetch/$s_!umTz!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8a1a2d91-238e-469a-8caa-dad8c3520b21_2068x320.png)Router Proxying Requests to Layer 4 Load Balancers

So they added more layer 4 load balancers and put a **[router](https://www.cloudflare.com/en-gb/learning/network-layer/what-is-a-router/)** in front.

Here’s how the router works:

  * It uses Border Gateway Protocol (**[BGP](https://www.cloudflare.com/en-gb/learning/security/glossary/what-is-bgp/)**) to find the best routes to the layer 4 load balancer

  * It uses the Equal Cost Multi Path (**[ECMP](https://en.wikipedia.org/wiki/Equal-cost_multi-path_routing)**) routing technique to load balance traffic across those routes

Thus avoiding network congestion on a single route and making better use of bandwidth.

### 4\. One Billion Users

Yet the router is now a single point of failure.

[![Replicating Clusters Within a Data Center for High Availability](https://substackcdn.com/image/fetch/$s_!oiru!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d452945-9f69-491a-8600-9b3ab0475731_1423x427.png)](https://substackcdn.com/image/fetch/$s_!oiru!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d452945-9f69-491a-8600-9b3ab0475731_1423x427.png)Replicating Clusters Within a Data Center for High Availability

So they added more clusters within a data center - each **cluster** consists of a router and load balancers.

And set up extra data centers for more computing capacity.

[![Creation of DNS Map](https://substackcdn.com/image/fetch/$s_!nJZE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff36ddb47-f433-4116-9402-2ab0916f2a41_1051x386.png)](https://substackcdn.com/image/fetch/$s_!nJZE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff36ddb47-f433-4116-9402-2ab0916f2a41_1051x386.png)Creation of DNS Map

Besides they do smart **[DNS](https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/) load balancing** across data centers.

  * A network map is created in real time based on server capacity, server health, and so on.

The requests then get routed to the optimal server based on this network map.

* * *

They use software load balancers for flexibility.

And use commodity hardware everywhere except for the router.

While Facebook became one of the most visited sites with ~3 billion users.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/facebook-load-balancer?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Google Search Works 🔥](https://substackcdn.com/image/fetch/$s_!GiuV!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F931dac77-e59d-4100-86fb-111807a02c33_1280x720.gif)How Google Search Works 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·September 30, 2024[Read full story](https://newsletter.systemdesign.one/p/search-engine-architecture)](https://newsletter.systemdesign.one/p/search-engine-architecture)

[![Amazon Frugal Architecture Explained 💰](https://substackcdn.com/image/fetch/$s_!SP5j!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fec5ec1d6-4ac7-42ff-a29b-b4fe6c730056_1280x720.gif)Amazon Frugal Architecture Explained 💰[Neo Kim](https://substack.com/profile/135589200-neo-kim)·September 15, 2024[Read full story](https://newsletter.systemdesign.one/p/frugal-architecture)](https://newsletter.systemdesign.one/p/frugal-architecture)

* * *

### References

  * [Building A Billion-User Load Balancer](https://www.usenix.org/conference/srecon15europe/program/presentation/shuff)

  * [Connecting the World: A Look into Facebook’s Networking Infrastructure](https://www.youtube.com/watch?v=H5jyM0oD418)

  * [Open-sourcing Katran, a scalable network load balancer](https://engineering.fb.com/2018/05/22/open-source/open-sourcing-katran-a-scalable-network-load-balancer/)

  * [Moving fast with high-performance Hack](https://hhvm.com/)

  * [What is TLS (Transport Layer Security)?](https://www.cloudflare.com/en-gb/learning/ssl/transport-layer-security-tls/)

  * [Linux Virtual Server](https://en.wikipedia.org/wiki/Linux_Virtual_Server)

  * [What is SSL Termination?](https://www.f5.com/glossary/ssl-termination)

  * [What is Direct Server Return (DSR)?](https://www.haproxy.com/glossary/what-is-direct-server-return-dsr)

  * [What is the OSI Model?](https://www.cloudflare.com/en-gb/learning/ddos/glossary/open-systems-interconnection-model-osi/)

  * [Facebook Data Center Locations](https://datacenterlocations.com/facebook/)

119

7

10

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Orel's avatar](https://substackcdn.com/image/fetch/$s_!f8O0!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6e073cc8-6507-4def-8274-c14d2145a022_511x511.png)](https://substack.com/profile/51141391-orel?utm_source=comment)

[Orel](https://substack.com/profile/51141391-orel?utm_source=substack-feed-item)

[Dec 6, 2024](https://newsletter.systemdesign.one/p/facebook-load-balancer/comment/80305508 "Dec 6, 2024, 10:25 AM")Edited

Liked by Neo Kim

It's fascinating to see how to build a system, then they replicate that system with a load balancer, just to replicate that system with another form of load balancer and so on.

I wonder if they thought about changing something within the first system instead of replicating it.

Fantastic article Neo!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/facebook-load-balancer/comment/80305508)

[![Sudhanshu Shekhar's avatar](https://substackcdn.com/image/fetch/$s_!ErMC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F123a34d7-909d-4f11-8597-b6025cac2e86_800x800.jpeg)](https://substack.com/profile/45154813-sudhanshu-shekhar?utm_source=comment)

[Sudhanshu Shekhar](https://substack.com/profile/45154813-sudhanshu-shekhar?utm_source=substack-feed-item)

[Oct 21, 2024](https://newsletter.systemdesign.one/p/facebook-load-balancer/comment/73510567 "Oct 21, 2024, 5:17 PM")

Liked by Neo Kim

Interesting to see how different strategies are applied at layer 7 and 4 to achieve overall load balancing improving scalability by multiple times.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/facebook-load-balancer/comment/73510567)

[5 more comments...](https://newsletter.systemdesign.one/p/facebook-load-balancer/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture