[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Microservices Lessons From Netflix

### #19: Check This Out - Awesome Microservices Guide (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 31, 2023

158

4

12

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines microservices architecture best practices from Netflix. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/netflix-microservices/?action=share) & I'll send you some rewards for the referrals._

## Microservices Architecture

Netflix runs on AWS. They started with a monolith and moved to microservices. Their reasons for migrating to microservices were the following:

  * It was difficult to find bugs with many changes to a single codebase

  * It became difficult to scale vertically

  * There were many single points of failures

[![Netflix microservices; Microservices benefits](https://substackcdn.com/image/fetch/$s_!f-1r!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03b285d3-2998-48d3-a78f-1c33688e7d55_1756x1756.png)](https://substackcdn.com/image/fetch/$s_!f-1r!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03b285d3-2998-48d3-a78f-1c33688e7d55_1756x1756.png)Microservices benefits

## Microservices Challenges and Solutions

3 main problems with microservices architecture are:

  1. Dependency

  2. Scale

  3. Variance

### 1\. Dependency

Here are 4 scenarios where the dependency problem occurs:

#### i) Intra-Service Requests

A client request results in a service calling another service. Put another way, service A needs to call service B to create a response.

[![Netflix microservices; Intra-service requests](https://substackcdn.com/image/fetch/$s_!h4DK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10da5fac-ce50-4265-a2fc-fe65f0a7babf_1515x324.png)](https://substackcdn.com/image/fetch/$s_!h4DK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10da5fac-ce50-4265-a2fc-fe65f0a7babf_1515x324.png)Intra-service requests

The problem with it is that a failure of a microservice results in cascading failures. The solutions are:

  * Use the circuit breaker pattern to prevent cascading failures. It avoids an operation that will probably fail

  * Do fault injection testing to check if the circuit breaker pattern works as expected. It does it by creating artificial traffic

  * Set up fallback to a static page to keep the system always responsive

  * Install exponential backoff to prevent thundering herd problem

Yet degraded availability and increased system test scope become a problem. Because availability reduces when the downtime of individual microservices gets combined. And the permutations of test scope grow by a ton as the number of microservices increases.

So it’s important to identify critical services and create a bypass path to avoid non-critical service failures.

#### ii) Client Libraries

An API gateway allows the reuse of business logic across many types of clients. 

An _API gateway_ is a central entry point that routes API requests. But it has the following limitations:

  * Heap consumption becomes difficult to manage

  * Potential logical defects or bugs

  * Potential transitive dependencies

So the solution is to keep the API gateway simple and avoid it from becoming the new monolith.

#### iii) Persistence

The choice of the storage layer depends on the CAP theorem. Put another way, it is a trade-off decision between availability and consistency level. 

So the solution is to study the data access patterns and choose the right storage.

#### iv) Infrastructure

The entire data center might fail. So the solution is to replicate the infrastructure across many data centers.

[![Netflix outage on Forbes](https://substackcdn.com/image/fetch/$s_!7UXR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc821b117-e70f-4118-9827-679f3be652a0_1587x2245.png)](https://substackcdn.com/image/fetch/$s_!7UXR!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc821b117-e70f-4118-9827-679f3be652a0_1587x2245.png)Netflix outage; [Forbes.com](https://www.forbes.com/sites/kellyclay/2012/12/24/amazon-aws-takes-down-netflix-on-christmas-eve/)

### 2\. Scale

The ability of the system to manage increased workload while maintaining performance is called scale. The 3 dimensions of horizontal scalability are:

  * Keep the service stateless if possible

  * Partition the service if it can't be stateless

  * Replicate the service

[![Netflix microservices; Scalability](https://substackcdn.com/image/fetch/$s_!iU9O!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F59315b81-d519-4ffb-87cd-f8c97847d36f_2697x1105.png)](https://substackcdn.com/image/fetch/$s_!iU9O!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F59315b81-d519-4ffb-87cd-f8c97847d36f_2697x1105.png)Stateful service and Stateless service

Here are 3 scenarios where the scale problem occurs:

#### i) Stateless Services

The 2 qualities of stateless service are:

  * There is no instance affinity (sticky sessions). Put another way, requests don't get routed to the same server

  * Failure of a stateless service is not notable

The stateless service needs to be replicated for high availability. And autoscaling must be set up for on-demand replication.

Also autoscaling reduces the impact of the following problems:

  * Reduced compute efficiency

  * Node failures

  * Traffic spikes

  * Performance bugs

The database and cache are not stateless services. 

Chaos engineering checks whether autoscaling works as expected. It tests system resilience through controlled disruptions to ensure improved reliability.

#### ii) Stateful Services

The database and cache are stateful services. Also a custom service that holds large amounts of data is a stateful service. The failure of a stateful service is a notable event.

An anti-pattern with the stateful service is having a sticky session without replication. Because it creates a single point of failure.

The solution is to replicate the writes across many servers in different data centers. And route the reads to the local data center.

#### iii) Hybrid Services

A cache is a hybrid service. A hybrid service expects an extreme load. For example, Netflix’s cache gets 30 million requests per second.

The best approach to building a hybrid service is the following:

  * Partition the workload using techniques like [consistent hashing](https://newsletter.systemdesign.one/p/what-is-consistent-hashing)

  * Enable request-level caching

  * Allow fallback to a database

And use Chaos engineering to check whether the hybrid service remains functional under extreme workloads.

### 3\. Variance

The variety in the software architecture is called variance. The system complexity grows as variance increases.

Here are 2 scenarios where the scale problem occurs:

#### i) Operational Drift

Operational drift is the unintentional variance that happens as time passes. It is usually a side-effect of new features added to the system. The examples of operational drift are:

  * Increased alert thresholds

  * Increased timeouts

  * Degraded throughput

The solution to this is continuous learning and automation.

[![Netflix microservices; Continuous Learning and Automation](https://substackcdn.com/image/fetch/$s_!wI_X!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d4378b1-4163-4219-9ea8-1aa63749b436_2705x1934.png)](https://substackcdn.com/image/fetch/$s_!wI_X!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4d4378b1-4163-4219-9ea8-1aa63749b436_2705x1934.png)Continuous Learning and Automation

Here is the workflow:

  1. Review an incident resolution

  2. Put remediation in place to prevent it from occurring again

  3. Analyze many incidents to identify patterns

  4. Derive best practices from incident resolutions

  5. Automate the best practices if possible

  6. Promote the adoption of best practices

  7. And repeat

#### ii) Polyglot

The variance introduced by engineers on purpose is called Polyglot. It happens when different programming languages are used to create different microservices.

It comes with the following drawbacks:

  * A large amount of work needed to get productive tooling

  * Extra operational complexity

  * Difficulty in server management

  * Business logic duplication across many technologies

  * Increased learning curve to become an expert

The solution to this problem is to use proven technologies. 

Besides polyglot forces the decomposition of the API gateway, which is a good thing. So the best ways to use Polyglot architecture are:

  * Raise team awareness of the costs of each technology

  * Limit centralized support to critical services

  * Prioritize based on the impact

  * Create reusable solutions by offering a pool of proven technologies

* * *

Here is a _checklist_ of best practices for microservices architecture from Netflix:

  * Automate tasks

  * Setup alerts

  * Autoscale to handle dynamic load

  * Chaos engineering for improved reliability

  * Consistent naming conventions

  * Health check services

  * Blue-green deployment to rollback quickly

  * Configure timeouts, retries, and fallbacks

Also change is inevitable and things always break with changes. So the best approach is to move faster but with fewer breaking changes.

Besides it's helpful to restructure teams to support the software architecture.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/netflix-microservices?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![Slack Architecture That Powers Billions of Messages a Day](https://substackcdn.com/image/fetch/$s_!Xn1I!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5ff71b0-786e-411c-986d-456e6ea168a0_1280x720.png)Slack Architecture That Powers Billions of Messages a Day[NK](https://substack.com/profile/135589200-nk)·October 26, 2023[Read full story](https://newsletter.systemdesign.one/p/messaging-architecture)](https://newsletter.systemdesign.one/p/messaging-architecture)

[![Wechat Architecture That Powers 1.67 Billion Monthly Users](https://substackcdn.com/image/fetch/$s_!R02-!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5e56341f-d779-4e43-878b-da70b8f44e57_1280x720.png)Wechat Architecture That Powers 1.67 Billion Monthly Users[NK](https://substack.com/profile/135589200-nk)·October 24, 2023[Read full story](https://newsletter.systemdesign.one/p/chat-application-architecture)](https://newsletter.systemdesign.one/p/chat-application-architecture)

* * *

## References

  * https://www.infoq.com/presentations/netflix-chaos-microservices/

  * https://netflixtechblog.com/tagged/microservices

  * https://www.atlassian.com/blog/teamwork/what-is-conways-law-acmi

  * https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker

158

4

12

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![MICHAEL ODELL's avatar](https://substackcdn.com/image/fetch/$s_!DFX4!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fgreen.png)](https://substack.com/profile/64099103-michael-odell?utm_source=comment)

[MICHAEL ODELL](https://substack.com/profile/64099103-michael-odell?utm_source=substack-feed-item)

[Nov 13, 2023](https://newsletter.systemdesign.one/p/netflix-microservices/comment/43588417 "Nov 13, 2023, 7:54 PM")

Liked by Neo Kim

Well done. Good advise without theology 😉. I

n the dim, dark past, the first half-dozen time “distributed systems” was the New New Thing, people discovered (the hard way) that what runs great on the white board isn’t so wonderful up against real silicon. Over the years, new systems based on decomposition (to varying degrees of “extreme”) explored how the decomposed parts “recomposed” dynamically to provide a service application programs had come to embrace. Some emphasized fast, lightweight mechanisms optimized for fine-grained concurrency, others took the conceptual virtual machine (not instruction set interpreters) to a very high degree of sophistication, and some explored extremely novel (some might say “baroque”) techniques of considerable imagination. Time and again, however, the Gold Standard for moving between protection domains was a procedure call plus fiddling with some protection registers. All the schemes require validating arguments to some degree.

And some required what today would be catastrophic cache flushing. The reality not yet avoided is that ring-crossings are expensive compared to function calls.

Transfers between complex protection domains are painfully slow. And putting network communication in the middle of any of these needed a REALLY good reason.

I’m a huge fan of system decomposition as long as it doesn’t start to smell bad. Given that RESTful stuff was not designed as an efficient RPC mechanism,

How does one decide when and what can be packaged as a Microservice without causing more trouble down the road as the scaling heats a up?

It seems one would want to be pretty confident in the architectural partitioning of the system, or at least the ability to bob-and-weave as 

Necessary to keep the wheels on the wanton while the jet engines can be changed in midair. 

Cap’n Fred’s Rule #1: Always keep the water on the outside of the boat.

Cap’n Fred’s Rule #2: When you hit something, do it going as slowly as possible.

Cap’n Fred’s Rule #3: There is no speed at which a bridge piling that won’t violate Rule #1.

Stay dry!

-mo

ReplyShare

[![Anton Zaides's avatar](https://substackcdn.com/image/fetch/$s_!m9AG!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe37a1acd-c9a1-4968-b60d-907005004d84_1728x1728.jpeg)](https://substack.com/profile/121956618-anton-zaides?utm_source=comment)

[Anton Zaides](https://substack.com/profile/121956618-anton-zaides?utm_source=substack-feed-item)

[Nov 2, 2023](https://newsletter.systemdesign.one/p/netflix-microservices/comment/42944530 "Nov 2, 2023, 4:27 PM")

Liked by Neo Kim

Love the real-world examples! Much more interesting to learn that way :)

I can relate to the api gateway problem... As we have different microservices responsible for seperate entitites, the logics to combine and process the information usually happens at the gateway, which makes it much heavier than it should be. 

The alternative is to have the logic in one of the microservices, which might break the 'domain ownership' (as it'll need to know more). 

Haven't found a satisfying guideline for that. 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/netflix-microservices/comment/42944530)

[2 more comments...](https://newsletter.systemdesign.one/p/netflix-microservices/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture