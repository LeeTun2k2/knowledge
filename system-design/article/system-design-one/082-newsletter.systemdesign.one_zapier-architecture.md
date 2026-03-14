[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Zapier Automates Billions of Tasks

### #37: Learn More - Zapier Architecture Overview (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Feb 25, 2024

79

12

9

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines Zapier's architecture. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/zapier-architecture/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there lived an office assistant named Sophie.

She was bright and smart.

But she got exhausted from _repetitive_ office tasks.

[![Zapier Architecture](https://substackcdn.com/image/fetch/$s_!MLZz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc9101a30-99c0-4424-a049-29109994ac3e_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!MLZz!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc9101a30-99c0-4424-a049-29109994ac3e_1200x630.gif)

Until one day when she hears about Zapier from a coworker.

[![Automation Workflow](https://substackcdn.com/image/fetch/$s_!6Eeb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdc75f0e5-1d88-4946-9a7c-827e5db1b305_1658x454.png)](https://substackcdn.com/image/fetch/$s_!6Eeb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdc75f0e5-1d88-4946-9a7c-827e5db1b305_1658x454.png)Automation Workflow

She _automated_ the workflow that occurs frequently when an event gets created in her office Google Calendar.

It sends Slack notifications and adds rows to Google Sheets automatically.

And she was stunned by its ease of automation.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Zapier Architecture

Here's how Zapier automates billions of tasks:

### 1\. Tech Stack

They run the Nginx web server and Python [Django](https://www.djangoproject.com/) framework on the backend.

And stores data in MySQL and Redis. Put another way, Zaps gets stored in MySQL.

**Zap** is an automated workflow that connects different tasks or services.

While**MySQL** is a relational database management system.

[![Zapier Tech Stack Overview](https://substackcdn.com/image/fetch/$s_!NbKd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6dbc608d-e4f3-4fba-8f8a-35ad8ba60708_794x322.png)](https://substackcdn.com/image/fetch/$s_!NbKd!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6dbc608d-e4f3-4fba-8f8a-35ad8ba60708_794x322.png)Tech Stack Overview

They store the number of _in-flight tasks_ in Redis. It allows them to throttle.

**Redis** is an in-memory key-value database.

While AWS Lambda runs _custom scripts_ provided by the user.

**Lambda** is a serverless computing platform.

### 2\. Zap Implementation

They use the _directed rooted tree_ to create a workflow.

While each tree node represents a task.

And directed rooted tree is implemented in MySQL for simplicity.

A **directed rooted tree** is a directed graph with all edges pointing away from the root node.

[![Directed Rooted Tree Representing Tasks](https://substackcdn.com/image/fetch/$s_!TlAf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3b72f496-3405-41fe-a675-732f55b4e2d5_1114x404.png)](https://substackcdn.com/image/fetch/$s_!TlAf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3b72f496-3405-41fe-a675-732f55b4e2d5_1114x404.png)Directed Rooted Tree Representing Tasks

Also tasks are kept independent of each other. Put another way, a task consumes data from the file system, performs API calls, and returns results.

So they’re unaware of their positioning in the workflow.

And a _workflow engine_ orchestrates tasks. In other words, it decides the task execution order based on the directed rooted tree.

They store the _session data_ of task execution in a dedicated MySQL. It’s used as a key-value store with softer consistency requirements and offers low operational complexity.

Besides they use MySQL _read-only_ replicas to handle long-running background tasks. Because these tasks wouldn’t change often and a replication lag is tolerable.

### 3\. Asynchronous Processing

A long-living but idle connection to the web server consumes resources. Thus it’s expensive.

So they use a _message queue_ to avoid waiting for a request to finish. It prevents problems due to timeouts and resource bottlenecks.

They use [RabbitMQ](https://www.rabbitmq.com/) and [Celery](https://docs.celeryq.dev/en/stable/) to create a distributed workflow engine. Put another way, it’s used to schedule background tasks.

**RabbitMQ** is a lightweight message queue.

While**Celery** is an asynchronous task queue based on distributed message passing. And it supports real-time operations and scheduling.

Celery sends messages to workers using RabbitMQ.

Put another way, Celery is a _task management framework_. It provides a _high-level_ API to schedule and trigger tasks.

While RabbitMQ provides a _low-level_ API to do the same things. And RabbitMQ is one of the many backends for Celery.

[![Asynchronous Processing of Tasks](https://substackcdn.com/image/fetch/$s_!tgZk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbf937e33-8ad1-416d-be56-56046c8022ee_1218x316.png)](https://substackcdn.com/image/fetch/$s_!tgZk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbf937e33-8ad1-416d-be56-56046c8022ee_1218x316.png)Asynchronous Processing of Tasks

They send the _task ID_ to the message queue and the worker gets the _task data_ from the database.

And the worker executes the task.

### 4\. Zap History

Zap history shows a user the list of tasks that ran in their account.

They use GraphQL and [Next.js](https://nextjs.org/) API routes to get the Zap history.

While Python Django runs on the backend.

**GraphQL** is a data query language for APIs and a query runtime engine.

[![Storing the Results of Zap Execution](https://substackcdn.com/image/fetch/$s_!VYLV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffae92983-848c-484a-b4d9-c08b77f1cec0_956x319.png)](https://substackcdn.com/image/fetch/$s_!VYLV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffae92983-848c-484a-b4d9-c08b77f1cec0_956x319.png)Storing the Results of Zap Execution

They store the results of a Zap execution in _AWS S3_ and emit an event to _Kafka_.

The emitted event contains enough information to process the execution result.

AWS **S3** is an object storage.

While **Kafka** is a distributed event store and stream-processing platform.

[![Indexing the Results of Zap Execution](https://substackcdn.com/image/fetch/$s_!Y0jO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1e716e6-f843-44e5-92a1-2bde38bd49a2_1231x582.png)](https://substackcdn.com/image/fetch/$s_!Y0jO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1e716e6-f843-44e5-92a1-2bde38bd49a2_1231x582.png)Indexing the Results of Zap Execution

They use the _indexer service_ to consume the events from Kafka. Also it downloads the relevant data from S3.

And they _index_ the processed Zap execution data in the _Elasticsearch_ cluster.

Put another way, ElasticSearch stores the _historical activity_ of Zaps.

**Elasticsearch** is a search engine based on the Lucene library. It offers a text search functionality with an HTTP web interface.

### 5\. Scalability

They use a combination of auto-scaling and auto-replacement for _resilience_.

Besides they scale horizontally and replicate infrastructure for _high availability_.

They use _jitter_ to handle spikes in workload when many tasks get scheduled for the same time. 

Put another way, they don’t guarantee that every task will run at the exact time.

[![Scaling Workers for Task Execution](https://substackcdn.com/image/fetch/$s_!ITjd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbc7e50f8-0ce3-4500-8d73-748a84617360_957x789.png)](https://substackcdn.com/image/fetch/$s_!ITjd!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbc7e50f8-0ce3-4500-8d73-748a84617360_957x789.png)Scaling Workers for Task Execution

They _enqueue_ tasks on RabbitMQ. And the tasks get consumed by _workers_ running on Kubernetes.

**Kubernetes** is a container orchestration system for automating software deployment, scaling, and management.

They scale the workers based on _CPU_ usage and the number of _ready tasks_ in RabbitMQ. Thus allowing them to handle the varying load.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

Zapier remains one of the leading automation tools in the market.

And this case study indicates that a simple tech stack with proven technologies is enough for high scalability.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

#### NK’s Recommendations

  * [Leading Developers](https://zaidesanton.substack.com/): If you are a Development Team Leader, Engineering Manager, or considering that career path - try this newsletter.

Author: [Anton Zaides](https://open.substack.com/users/121956618-anton-zaides?utm_source=mentions)

  * [High Growth Engineer](https://careercutler.substack.com/): Get actionable tips to grow faster in your software engineering career.

Author: [Jordan Cutler](https://open.substack.com/users/58854493-jordan-cutler?utm_source=mentions)

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/zapier-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Disney+ Hotstar Delivered 5 Billion Emojis in Real Time](https://substackcdn.com/image/fetch/$s_!0053!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1c8de39-6df0-4eed-a42b-227cb449519d_1280x720.gif)How Disney+ Hotstar Delivered 5 Billion Emojis in Real Time[Neo Kim](https://substack.com/profile/135589200-neo-kim)·February 10, 2024[Read full story](https://newsletter.systemdesign.one/p/hotstar-architecture)](https://newsletter.systemdesign.one/p/hotstar-architecture)

[![How Canva Supports Real-Time Collaboration for 135 Million Monthly Users](https://substackcdn.com/image/fetch/$s_!IALn!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F44d95ef4-5647-4ae5-b868-3647265167c0_1280x720.gif)How Canva Supports Real-Time Collaboration for 135 Million Monthly Users[Neo Kim](https://substack.com/profile/135589200-neo-kim)·February 18, 2024[Read full story](https://newsletter.systemdesign.one/p/rsocket)](https://newsletter.systemdesign.one/p/rsocket)

* * *

### References

  * [Scaling Zapier to Automate Billions of Tasks](https://stackshare.io/zapier/scaling-zapier-to-automate-billions-of-tasks)

  * [The architecture behind Zapier's Zap History pages](https://zapier.com/blog/the-architecture-behind-zap-history-pages/)

  * [The Zapier Tech Stack](https://zapier.com/blog/zapier-tech-stack/)

  * [How Zapier uses KEDA](https://zapier.com/blog/keda-at-zapier/)

  * [KEDA at Zapier](https://www.cncf.io/blog/2022/01/21/keda-at-zapier/)

  * [Async Celery by Example: Why and How](https://zapier.com/blog/async-celery-example-why-and-how/)

  * [Scaling Zapier to Automate Billions of Tasks on Hacker News](https://news.ycombinator.com/item?id=11082359)

  * [What is the relationship between Celery and RabbitMQ?](https://stackoverflow.com/questions/43379554/what-is-the-relationship-between-celery-and-rabbitmq)

79

12

9

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Anton Zaides's avatar](https://substackcdn.com/image/fetch/$s_!m9AG!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe37a1acd-c9a1-4968-b60d-907005004d84_1728x1728.jpeg)](https://substack.com/profile/121956618-anton-zaides?utm_source=comment)

[Anton Zaides](https://substack.com/profile/121956618-anton-zaides?utm_source=substack-feed-item)

[Feb 25, 2024](https://newsletter.systemdesign.one/p/zapier-architecture/comment/50336285 "Feb 25, 2024, 6:55 PM")

Liked by Neo Kim

I was curious to see that Django is still used in modern software 😅

ReplyShare

[3 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/zapier-architecture/comment/50336285)

[![Sanskar Shrivastava's avatar](https://substackcdn.com/image/fetch/$s_!YE0F!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8455e192-cad0-46e2-975f-84d22560993c_96x96.jpeg)](https://substack.com/profile/153998439-sanskar-shrivastava?utm_source=comment)

[Sanskar Shrivastava](https://substack.com/profile/153998439-sanskar-shrivastava?utm_source=substack-feed-item)

[Feb 27, 2024](https://newsletter.systemdesign.one/p/zapier-architecture/comment/50488157 "Feb 27, 2024, 8:17 PM")

Liked by Neo Kim

Why did they choose to store the workflow DAGs in MySQL?

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/zapier-architecture/comment/50488157)

[10 more comments...](https://newsletter.systemdesign.one/p/zapier-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture