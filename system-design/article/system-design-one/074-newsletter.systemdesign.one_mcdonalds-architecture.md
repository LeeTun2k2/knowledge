[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How McDonald’s Food Delivery Platform Handles 20,000 Orders per Second

### #44: Break Into McDonald’s Architecture (7 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 03, 2024

141

8

12

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the architecture of McDonald’s food delivery platform. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/mcdonalds-architecture/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

March 2024 - Berlin, Germany.

Maria is taking a lunch break at the end of her shift at the hospital.

But she forgot to pack lunch that day.

So she was hungry and sad.

[![McDonald’s Architecture](https://substackcdn.com/image/fetch/$s_!aMnu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F119f58a1-6ff2-4523-8db6-a753dd3d7b96_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!aMnu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F119f58a1-6ff2-4523-8db6-a753dd3d7b96_1200x630.gif)

She decides to order food from McDonald’s through the mobile app.

* * *

### [Refind – Brain food, delivered daily (Featured)](https://refind.com/n/c?put=MlHF&att=rfnd)

Loved by 450,000+ curious minds. Every day Refind analyzes thousands of articles and sends you only the best. Subscribe for free today.

[![Refind](https://substackcdn.com/image/fetch/$s_!pZgP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe07ccd1d-09a9-4b8a-a21e-cd9d71bf8e4a_800x800.jpeg)](https://substackcdn.com/image/fetch/$s_!pZgP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe07ccd1d-09a9-4b8a-a21e-cd9d71bf8e4a_800x800.jpeg)

[Try it](https://refind.com/n/c?put=MlHF&att=rfnd)

* * *

They use hexagonal architecture to keep complexity low.

Imagine **hexagonal architecture** as a pattern that separates the core application from external services like databases and user interfaces.

[![Connection to External Services With Hexagonal Architecture](https://substackcdn.com/image/fetch/$s_!PuJ7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4ba043d9-9d7c-4b65-a059-0cfbe2fdbfc5_1200x630.png)](https://substackcdn.com/image/fetch/$s_!PuJ7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4ba043d9-9d7c-4b65-a059-0cfbe2fdbfc5_1200x630.png)Connection to External Services Through Hexagonal Architecture

The application domain lives inside the hexagon. And business logic follows domain-driven design (**DDD**) principles, which means it shouldn’t leak outside the hexagon.

**Ports** are interfaces through which the application interacts with external services. While **Adapters** are implementations of the ports. This means ports define the contract or interface. While adapters provide implementation to fulfill the contract.

[![Sending Events via the Message Broker](https://substackcdn.com/image/fetch/$s_!KmqQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdfed64bf-829b-465e-b9e2-989480088b8b_1383x196.png)](https://substackcdn.com/image/fetch/$s_!KmqQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdfed64bf-829b-465e-b9e2-989480088b8b_1383x196.png)Sending Events via the Message Broker

Besides they use event-driven architecture alongside hexagonal architecture. It provides modularity and keeps the architecture loosely coupled.

Imagine **event-driven architecture** as a pattern that uses events for communication between services. The producer publishes events to a message broker. While the consumer subscribes to those events and reacts accordingly.

So they represent the domain logic using the hexagonal core. While adapters produce and consume events.

They publish an event to the message broker when a domain event occurs. And external services subscribe to the message broker for events.

[![Improving the Reliability of Event-Driven Architecture](https://substackcdn.com/image/fetch/$s_!62uF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66881426-b90e-40ef-b14d-b5fee1838d96_1383x953.png)](https://substackcdn.com/image/fetch/$s_!62uF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F66881426-b90e-40ef-b14d-b5fee1838d96_1383x953.png)Improving the Reliability of Event-Driven Architecture

They use a schema registry to maintain well-defined contracts for events. Also it helps with schema validation. Besides they cache the event schema in services for performance. A **schema** describes the expected data fields and types of an event. 

Also they run a standby database to prevent data loss if the message broker becomes unavailable. So the events get written to the standby database and get published back to the message broker after it's healthy again.

Besides they route the events to a dead-letter topic if schema validation fails. And then use a utility tool to fix those events.

They serve around 37 thousand locations and 64 million people each day. 

Scalability is a difficult problem. And scalability with a distributed network at large volumes is even more difficult.

Yet they scaled their platform to 20,000 orders per second with less than 100 millisecond latency.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## McDonald's Architecture

Here’s how McDonald’s scaled their food delivery platform:

### 1\. Choosing Food From the Menu

Maria opens the mobile app and sees a menu with food items.

[![Showing the Menu to the User](https://substackcdn.com/image/fetch/$s_!P-LO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F04e3f058-0e07-4375-ac58-12eb2919864d_1763x446.png)](https://substackcdn.com/image/fetch/$s_!P-LO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F04e3f058-0e07-4375-ac58-12eb2919864d_1763x446.png)Showing the Menu to the User

They use a reverse proxy server to host all API endpoints and use it to route requests to microservices. This means an **API gateway pattern** with REST APIs. 

They store the menus and restaurant's working hours in an SQL database. And query the SQL database using a serverless function to create menus in JSON data format.

They show the user the menu of available food items and prices via HTTP response.

### 2\. Getting a Discount With Loyalty Rewards

Maria selects a Big Mac burger from the menu.

She then remembers the points she earned through past purchases.

So she applies them and gets a discount.

[![Discounting the Price With Rewards](https://substackcdn.com/image/fetch/$s_!FuUo!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffa5dd4a7-16a5-49c9-a642-7cd198acca76_1481x260.png)](https://substackcdn.com/image/fetch/$s_!FuUo!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffa5dd4a7-16a5-49c9-a642-7cd198acca76_1481x260.png)Discounting the Price With Rewards

They publish user events into a message queue. It allows services to talk to each other via an asynchronous pattern. Also a first-in-first-out (**FIFO**) queue guarantees ordering and exactly-once processing of transactions.

They use a serverless function to poll the message queue and then process the messages in it. 

They use serverless to add new features without worrying about infrastructure. Also it helps them to reduce costs by avoiding server provisioning for maximum capacity.

### 3\. Ordering Food

Maria places the order.

[![Validating an Order](https://substackcdn.com/image/fetch/$s_!xb_Z!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F29d13087-6f67-48c8-88cd-fed3740950ee_1850x260.png)](https://substackcdn.com/image/fetch/$s_!xb_Z!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F29d13087-6f67-48c8-88cd-fed3740950ee_1850x260.png)Validating an Order

They create a WebSocket connection for bidirectional communication with the client. And the client sends an event to the API gateway when a new order gets created.

The API gateway then forwards that event to an event bus. It has a routing system that can identify new orders. Think of the **event bus** as a central hub for messages between services.

The event bus triggers a serverless function to check whether the ordered food is available at the restaurant. Put simply, it validates if a specific order could be completed.

Afterward they send an event to the client when the restaurant accepts the order.

They use an in-memory cache with Redis to process orders. It provides high performance and low latency. Also they back up Redis in a relational database for durability in case of an outage.

While the serverless function that validated the order waits there for a callback. It's used to notify the user when the food is ready to be picked up by the delivery driver. Besides they store the serverless function’s callback task token in a key-value database.

They use the estimated time of arrival (**[ETA](https://newsletter.systemdesign.one/p/uber-eta)**) and food preparation time to notify the driver of a pick-up. It helps to avoid extra waiting time.

[![Completing an Order](https://substackcdn.com/image/fetch/$s_!Pq9Q!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6b0db5e7-30e3-4a82-86b0-572672f02594_2076x436.png)](https://substackcdn.com/image/fetch/$s_!Pq9Q!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6b0db5e7-30e3-4a82-86b0-572672f02594_2076x436.png)Completing an Order

They use a separate serverless function to trigger an event as the driver gets near the restaurant. The event bus would identify if it’s an event for an existing order and if so query the key-value database for the callback task token. 

And then it forwards that event to the waiting serverless function. The waiting serverless function will then release the order. Also it notifies the user that the driver has picked up their food.

### 4\. Giving Feedback

Maria was happy with the burger she had.

While she receives a feedback survey notification from McDonald’s.

[![User Feedback Architecture](https://substackcdn.com/image/fetch/$s_!am4n!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45e0aac2-59be-47b7-ac0b-567460026f20_1941x243.png)](https://substackcdn.com/image/fetch/$s_!am4n!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45e0aac2-59be-47b7-ac0b-567460026f20_1941x243.png)User Feedback Architecture

They need data about user experience to improve their service.

So they run surveys and collect social media comments. And do extract-transform-load (**ETL**) to analyze information. The data then gets stored in S3 object storage.

Afterward they sent the data to natural language processing (**NLP**) service for sentiment analysis. Also the processed data is sent to in-house models to improve its accuracy. Finally the result data gets stored in a data warehouse to create operational reports.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

Their food delivery platform runs microservices to support more features. And uses a load balancer to distribute the traffic across microservices.

Yet each microservice has different scale and runtime profiles. That means customer-facing services are considered critical. While background processing services are tolerable to failures.

They moved services that change often into separate microservices. So they could deploy and iterate faster.

They do smoke testing to check the responsiveness of their API. Imagine **smoke testing** as a quick and basic test to check functionality.

Besides they use circuit breaker logic and exponential backoff for resilience. And focus on automation to reduce operational efforts.

While McDonald's remains one of the world's largest fast-food restaurant chains.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/mcdonalds-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

[![How Dropbox Scaled to 100 Thousand Users in a Year After Launch](https://substackcdn.com/image/fetch/$s_!tANH!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F93e043a4-6259-46fe-a292-f660d364e13f_1280x720.gif)How Dropbox Scaled to 100 Thousand Users in a Year After Launch[Neo Kim](https://substack.com/profile/135589200-neo-kim)·April 23, 2024[Read full story](https://newsletter.systemdesign.one/p/dropbox-architecture)](https://newsletter.systemdesign.one/p/dropbox-architecture)

[![How Lyft Support Rides to 21 Million Users](https://substackcdn.com/image/fetch/$s_!fR28!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9a93f378-9e8d-4e6b-9286-26e3dd628d99_1280x720.gif)How Lyft Support Rides to 21 Million Users[Neo Kim](https://substack.com/profile/135589200-neo-kim)·April 12, 2024[Read full story](https://newsletter.systemdesign.one/p/lyft-engineering)](https://newsletter.systemdesign.one/p/lyft-engineering)

* * *

### References

  * [McDonald's Home Delivery Case Study](https://aws.amazon.com/solutions/case-studies/mcdonalds-home-delivery/)

  * [McDonald’s path to secure operational excellence on AWS](https://www.youtube.com/watch?v=-0SlmCSNYCs)

  * [Discovering insights from customer surveys at McDonald’s](https://www.youtube.com/watch?v=la2dHBYJPoI)

  * [Delivering agility at McDonald's with microservices transformation](https://www.youtube.com/watch?v=0vF4TWluhAA)

  * [Enhancing Loyalty rewards: How McDonald’s leverages AWS Lambda for microservices](https://medium.com/mcdonalds-technical-blog/enhancing-loyalty-rewards-how-mcdonalds-leverages-aws-lambda-for-microservices-67e2a243275f)

  * [Behind the scenes: McDonald’s event-driven architecture](https://medium.com/mcdonalds-technical-blog/behind-the-scenes-mcdonalds-event-driven-architecture-51a6542c0d86)

  * [McDonald’s event-driven architecture: The data journey and how it works](https://medium.com/mcdonalds-technical-blog/mcdonalds-event-driven-architecture-the-data-journey-and-how-it-works-4591d108821f)

  * [Taco Bell: Order Middleware - Enabling Delivery Orders at Massive Scale](https://www.youtube.com/watch?v=sezX7CSbXTg)

  * [FoodHub: Enabling Massive Scale Order Processing with Serverless Architecture](https://www.youtube.com/watch?v=gpWR5JBC64A)

  * [Taco Bell: Aurora as The Heart of the Menu Middleware and Data Integration Platform for Taco Bell](https://www.youtube.com/watch?v=hf9nMAG9XoU)

  * [Hexagonal Architecture for Cloud Ecosystems](https://medium.com/mcdonalds-technical-blog/hexagonal-architecture-for-cloud-ecosystems-5a5ec6d4126b)

  * [Hexagonal Architectures - the sequel](https://medium.com/mcdonalds-technical-blog/hexagonal-architectures-the-sequel-073c9ee79385)

  * [Hexagonal Architecture - software](https://en.wikipedia.org/wiki/Hexagonal_architecture_\(software\))

  * [Event-driven architecture](https://en.wikipedia.org/wiki/Event-driven_architecture)

141

8

12

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Fran Soto's avatar](https://substackcdn.com/image/fetch/$s_!XWMk!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10f90fdb-11ac-48b4-8f51-6a59e07763d2_1149x1149.png)](https://substack.com/profile/170998285-fran-soto?utm_source=comment)

[Fran Soto](https://substack.com/profile/170998285-fran-soto?utm_source=substack-feed-item)

[May 5, 2024](https://newsletter.systemdesign.one/p/mcdonalds-architecture/comment/55577514 "May 5, 2024, 7:04 AM")

Liked by Neo Kim

It's interesting their use of events for things like discounts and placing orders.

My intuition was to treat async communication as something that can take more time. I bet they'll have tight alarm thresholds if the backlog of messages starts growing. In the end, a good customer experience requires low latency in those flows.

Good read, Neo!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/mcdonalds-architecture/comment/55577514)

[![Jaikumar Guwalani's avatar](https://substackcdn.com/image/fetch/$s_!Fvnc!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd39fcdce-1c52-4a28-aec4-f1a6f7b324c9_96x96.png)](https://substack.com/profile/18106826-jaikumar-guwalani?utm_source=comment)

[Jaikumar Guwalani](https://substack.com/profile/18106826-jaikumar-guwalani?utm_source=substack-feed-item)

[May 3, 2024](https://newsletter.systemdesign.one/p/mcdonalds-architecture/comment/55436704 "May 3, 2024, 1:13 PM")

Can someone simplify the point two ? How it works ? 

ReplyShare

[2 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/mcdonalds-architecture/comment/55436704)

[6 more comments...](https://newsletter.systemdesign.one/p/mcdonalds-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture