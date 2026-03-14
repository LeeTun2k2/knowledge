[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How PayPal Was Able to Support a Billion Transactions per Day With Only 8 Virtual Machines

### #30: Learn More - Awesome PayPal Engineering (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Dec 26, 2023

306

14

29

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how PayPal scaled to a billion daily transactions with only 8 virtual machines. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/actor-model/?action=share) & I'll send you some rewards for the referrals._

December 1998 - California, United States.

A team of engineers creates security software for hand-held devices.

Yet their business model failed.

So they pivoted to create an online payment service and called it PayPal.

As the number of users grew in the early days, they bought newer hardware to scale.

[![Moore’s Law](https://substackcdn.com/image/fetch/$s_!N7k7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2f636c33-4770-4eb9-bda3-8d460c4e6c83_1330x821.png)](https://substackcdn.com/image/fetch/$s_!N7k7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2f636c33-4770-4eb9-bda3-8d460c4e6c83_1330x821.png)Moore’s Law

But transistors in an integrated circuit (**IC**) stopped doubling every other year.

Put another way, the single-threaded performance gain of _Moore’s law_ slowed down. So buying newer hardware couldn't solve their scalability problems.

Yet their growth rate was explosive. 

And hit 1 million transactions a day in the next 2 years.

So they scaled out by running services in more than 1000 virtual machines.

[![Horizontal Scaling](https://substackcdn.com/image/fetch/$s_!8Yrf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcf0fea95-300c-4a3e-bf43-152746c09204_1282x871.png)](https://substackcdn.com/image/fetch/$s_!8Yrf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcf0fea95-300c-4a3e-bf43-152746c09204_1282x871.png)Horizontal Scaling

Although they solved the scalability issue, it created new problems.

Here are some of them:

### 1\. Network Infrastructure

A request took more network hops to finish and it worsened latency.

Also it became expensive to maintain the network infrastructure.

### 2\. Maintainance Costs

Adding more servers increased their infrastructure complexity.

Besides the service deployment across every machine took more time.

And setting up autoscaling needed extra effort.

Also infrastructure management like monitoring became difficult.

### 3\. Resource Usage

They didn’t fully use the CPU of each server.

Put another way, the server throughput was low.

It resulted in resource wastage and extra costs.

* * *

## Actor Model

The code doesn’t take full advantage of the hardware unless it’s run concurrently.

Also they wanted to keep it simple and scalable.

So they moved to the actor model based on the Akka framework (Java).

_It allowed them to support a billion daily transactions with only 8 virtual machines._

The**actor model** is a conceptual concurrent computation model.

And an actor is the fundamental unit of computation.

Here’s how the actor model offers extreme scalability:

### 1\. Resource Usage

An actor is an extremely lightweight object. It takes fewer resources than threads.

So it’s easy to create millions of actors if necessary.

The actor does an action when a message is received.

Yet the actor is decoupled from the source of the message.

[![Threads Assigned to Actors With a Message to Process](https://substackcdn.com/image/fetch/$s_!zpR7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F53cdccc0-6796-453a-9ade-13fac31815d0_1342x695.png)](https://substackcdn.com/image/fetch/$s_!zpR7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F53cdccc0-6796-453a-9ade-13fac31815d0_1342x695.png)Actors with Messages Are Assigned a Thread

A thread gets assigned to an actor when it must process a message.

While the thread is released after the message is processed and gets assigned to another actor.

The number of threads is proportional to the number of CPU cores.

Yet a small number of threads can handle a large number of concurrent actors.

Because a thread gets assigned to an actor only during its runtime.

### 2\. State Information

Actors don’t share memory and are isolated from each other.

Put another way, the state of an actor is private.

They communicate with each other through messages.

Messages are simple and immutable data structures that get sent over the network.

[![Actors Sending Messages](https://substackcdn.com/image/fetch/$s_!eqTJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9ad3195c-73be-47ec-a14f-6c42ab6a646a_800x500.png)](https://substackcdn.com/image/fetch/$s_!eqTJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9ad3195c-73be-47ec-a14f-6c42ab6a646a_800x500.png)Actors Communicating Through Messages

Each actor has a mailbox. It’s like a message queue. 

Actors store messages in the mailbox until they get processed in a First-in First-out (**FIFO**) order.

Also actors allow the system to avoid extra network calls to a distributed cache or a database. 

Because they store the local state in the application server.

[![App Server Handling Requests Without Querying Database](https://substackcdn.com/image/fetch/$s_!4V0p!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5c35b985-b25f-4c69-8b80-827c0c518f57_966x605.png)](https://substackcdn.com/image/fetch/$s_!4V0p!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5c35b985-b25f-4c69-8b80-827c0c518f57_966x605.png)Application Server Handling Requests Without Querying Database

Put another way, a stateful application server improves performance. Because it caches data locally.

Besides PayPal uses [consistent hashing](https://newsletter.systemdesign.one/p/what-is-consistent-hashing) to route a customer to the same server.

### 3\. Concurrency

Many actors can run at the same time but each actor process messages sequentially.

[![An Actor Processing Messages in Sequential Order](https://substackcdn.com/image/fetch/$s_!8o_1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09e53157-83a3-4029-b006-67f8e13c7c56_800x500.gif)](https://substackcdn.com/image/fetch/$s_!8o_1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09e53157-83a3-4029-b006-67f8e13c7c56_800x500.gif)An Actor Process Messages in Sequential Order

Put another way, an actor can process only a single message at a time.

So they need 3 actors to process 3 messages in parallel.

Also actors work asynchronously. In other words, they don’t wait for another actor's response.

So the actor model makes concurrency easier.

Besides PayPal uses the functional programming style of Akka for scalability.

It prevents side effects and makes testing easier.

Also they use pluggable code pieces with functional programming for easy scalability.

The actors could run locally or remotely on another machine. 

Yet it’s transparent to the system.

### 4\. Fault Tolerance

An actor can create more actors and also supervise them.

[![Fault Tolerance in Actor Model](https://substackcdn.com/image/fetch/$s_!muTY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd12b797-6b82-4ab4-91bd-b5b61da530ee_2058x1327.png)](https://substackcdn.com/image/fetch/$s_!muTY!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd12b797-6b82-4ab4-91bd-b5b61da530ee_2058x1327.png)Fault Tolerance in Actor Model

The supervisor actor restarts the supervised actor if it fails. Also the message can be routed to another actor.

Besides errors propagate to the supervisor actor. 

So graceful error handling can be done without code clutter.

* * *

The actor model is not a silver bullet to scalability.

It introduces a learning curve for the developers.

Also extra care should be taken to prevent race conditions and deadlocks.

The actor model allowed PayPal to handle extreme scale with fewer resources.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/actor-model?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![5 Reasons Why Zoom Was Able to Support 300 Million Video Calls a Day](https://substackcdn.com/image/fetch/$s_!bbxV!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb7b8cf4e-f008-4efa-93d9-05143a420d33_1280x720.png)5 Reasons Why Zoom Was Able to Support 300 Million Video Calls a Day[NK](https://substack.com/profile/135589200-nk)·December 11, 2023[Read full story](https://newsletter.systemdesign.one/p/zoom-architecture)](https://newsletter.systemdesign.one/p/zoom-architecture)

[![Virtual Waiting Room Architecture That Handles High-Demand Ticket Sales at SeatGeek](https://substackcdn.com/image/fetch/$s_!bpKU!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc6e0617f-6cb7-43d7-a784-bd0bef4d0256_1280x720.gif)Virtual Waiting Room Architecture That Handles High-Demand Ticket Sales at SeatGeek[NK](https://substack.com/profile/135589200-nk)·December 19, 2023[Read full story](https://newsletter.systemdesign.one/p/virtual-waiting-room)](https://newsletter.systemdesign.one/p/virtual-waiting-room)

* * *

## References

  * https://medium.com/paypal-tech/squbs-a-new-reactive-way-for-paypal-to-build-applications-127126bf684b

  * https://en.wikipedia.org/wiki/PayPal

  * https://akka.io/

  * https://finematics.com/actor-model-explained/

  * https://www.brianstorti.com/the-actor-model/

306

14

29

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Kent Bull's avatar](https://substackcdn.com/image/fetch/$s_!dRZL!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe889edfb-2e1a-4864-92bc-1767c31ae82a_1284x1282.jpeg)](https://substack.com/profile/91919402-kent-bull?utm_source=comment)

[Kent Bull](https://substack.com/profile/91919402-kent-bull?utm_source=substack-feed-item)

[Dec 26, 2023](https://newsletter.systemdesign.one/p/actor-model/comment/46057189 "Dec 26, 2023, 2:42 PM")

Liked by Neo Kim

Looks like Erlang/Elixir’s OTP model.

ReplyShare

[2 replies](https://newsletter.systemdesign.one/p/actor-model/comment/46057189)

[![Junaid Effendi's avatar](https://substackcdn.com/image/fetch/$s_!4vgA!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb06559f3-ee33-46f8-bfa0-50964179f235_1200x1200.png)](https://substack.com/profile/21393641-junaid-effendi?utm_source=comment)

[Junaid Effendi](https://substack.com/profile/21393641-junaid-effendi?utm_source=substack-feed-item)

[Mar 4, 2024](https://newsletter.systemdesign.one/p/actor-model/comment/50892131 "Mar 4, 2024, 2:05 PM")

Liked by Neo Kim

Actor model is rarely used, we use it in Scala in some of our projects. I think its underrated.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/actor-model/comment/50892131)

[12 more comments...](https://newsletter.systemdesign.one/p/actor-model/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture