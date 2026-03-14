[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Khan Academy Scaled to 30 Million Users

### #39: Break Into Khan Academy Architecture (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Mar 12, 2024

65

8

4

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Khan Academy scaled to 30 million users. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/khan-academy-architecture/?action=share) & I'll send you some rewards for the referrals._

August 2004 - Boston, United States.

Sal Khan works for a hedge fund.

One day he receives a phone call from a cousin.

[![Khan Academy Architecture](https://substackcdn.com/image/fetch/$s_!hXVP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F494258c4-c224-4c80-b260-fdb88e6c91af_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!hXVP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F494258c4-c224-4c80-b260-fdb88e6c91af_1200x630.gif)

His cousin needs help to learn mathematics.

So he started tutoring her through phone and Yahoo Doodle after work.

And seasons passed.

Sal started tutoring many more of his cousins.

But he faced problems with time management.

So he started recording videos and posted them on YouTube.

The rest is history.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Khan Academy Architecture

Here’s how Khan Academy achieved extreme scalability:

### 1\. Keep It Simple and Stupid

[![YouTube Serving Videos to Students](https://substackcdn.com/image/fetch/$s_!h_d9!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7a07d6d7-d0da-4794-b46d-e022a00e4db0_1012x273.png)](https://substackcdn.com/image/fetch/$s_!h_d9!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7a07d6d7-d0da-4794-b46d-e022a00e4db0_1012x273.png)YouTube Serving Videos to Students

They used YouTube to serve videos to students. Because it offers reduced costs and performance.

[![Fallback Service Serving Videos From Amazon S3](https://substackcdn.com/image/fetch/$s_!qG78!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2f0f6c56-e0dc-4357-84fd-21f6fd336c5a_1056x251.png)](https://substackcdn.com/image/fetch/$s_!qG78!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2f0f6c56-e0dc-4357-84fd-21f6fd336c5a_1056x251.png)Serving Videos From Amazon S3

Also they set up a fallback to serve videos from Amazon S3 via Fastly CDN. It solved the problem with institutions where YouTube isn’t allowed.

In other words, S3 stores the videos while Fastly caches them for subsequent views.

Amazon Simple Storage Service (**S3**) is an object storage. It stores unstructured data without hierarchy. While Content Delivery Network (**CDN**) is a globally distributed server for low latency.

[![Scaling the Infrastructure Using Serverless](https://substackcdn.com/image/fetch/$s_!oV2A!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb65d5cb7-06b1-4dc2-bf4d-f8908c29a389_1450x251.png)](https://substackcdn.com/image/fetch/$s_!oV2A!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb65d5cb7-06b1-4dc2-bf4d-f8908c29a389_1450x251.png)Scaling the Infrastructure Using Serverless

Besides they use serverless architecture for scalability. And runs a content management system (**CMS**) with a versioned content store. It’s used to handle dynamic behavior like tracking the progress of students.

### 2\. Essential Complexity Is Unavoidable

They started with a monolithic architecture for simplicity.

But moved to a microservices architecture and runs around 20 services. 

[![High Level Architecture](https://substackcdn.com/image/fetch/$s_!nR0L!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe58f1a1c-1de4-476a-b2c9-9aa6967ff514_1158x616.png)](https://substackcdn.com/image/fetch/$s_!nR0L!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe58f1a1c-1de4-476a-b2c9-9aa6967ff514_1158x616.png)High-Level Architecture

Although microservices increase the system complexity, they offer the following benefits:

  * Faster deployments and test runs due to independent small services

  * Reduced blast radius on deployment failures

  * Easy to choose the instance type and hosting configuration

  * Easy to optimize the services for performance and cost

Each microservice is responsible for its data. While a service can only access other services' data via a GraphQL API.

### 3\. Don’t Reinvent the Wheel

They didn't buy more servers or scale the operations team. Instead run their infrastructure on Google Cloud. And used serverless on Google App Engine for scalability.

So they could focus more on the application logic and worry less about scalability. Thus avoiding reinventing the wheel.

[![Don’t Reinvent the Wheel](https://substackcdn.com/image/fetch/$s_!s239!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67c9df99-7049-428c-b7c9-65b2220d3259_1398x709.png)](https://substackcdn.com/image/fetch/$s_!s239!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67c9df99-7049-428c-b7c9-65b2220d3259_1398x709.png)Don’t Reinvent the Wheel

Also they used Fastly CDN for low latency and reduced server load.

And GraphQL for APIs because it provides efficiency and flexibility.

### 4\. Pick Your Battles Wisely

They used Python in the early days to stay productive.

But aimed at lowering costs, better performance, and high reliability.

So they _moved to the Go programming language_. It offers fast compile time and uses less memory. Also there's good support for concurrency.

[![Pick Your Battles Wisely](https://substackcdn.com/image/fetch/$s_!mSXl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F41e0fad4-9e8c-4c44-8e8b-703282bf7951_3531x935.png)](https://substackcdn.com/image/fetch/$s_!mSXl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F41e0fad4-9e8c-4c44-8e8b-703282bf7951_3531x935.png)Pick Your Battles Wisely

Besides they use Google Cloud Datastore as the database for performance and scalability. **Google Cloud Datastore** is a fully managed NoSQL database.

They use Google App Engine because it’s a fully managed environment. And it lets them scale easily without extra operational efforts.

In other words, they offloaded the scaling part to focus more on creating educational content.

### 5\. Don’t Repeat Yourself

They use Fastly CDN to cache static data for scalability and performance.

[![Don’t Repeat Yourself](https://substackcdn.com/image/fetch/$s_!yuqM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa73a3df8-c35c-476e-a088-2798fde0f0d2_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!yuqM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa73a3df8-c35c-476e-a088-2798fde0f0d2_1200x630.gif)Don’t Repeat Yourself

Also every request goes through Fastly CDN to prevent unwanted server traffic.

Besides they cache common queries, user preferences, and session data using Memcache. And it allowed them to reduce server costs.

### 6\. People Are the Most Important Asset

They embraced a common goal as a team - either succeed or fail together. And didn't try to find perfection in every change but did only what was enough.

Also they eliminated scope creep at every cost. And prevented adding new work while in the middle of something.

[![People Are the Most Important Asset](https://substackcdn.com/image/fetch/$s_!GSNF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43273ec4-f984-4c8a-aee7-6e7de92e15bb_900x280.jpeg)](https://substackcdn.com/image/fetch/$s_!GSNF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43273ec4-f984-4c8a-aee7-6e7de92e15bb_900x280.jpeg)

Each engineer worked on different product areas to increase the sense of ownership.

Besides they celebrated each milestone because humans grow on a sense of accomplishment.

### 7\. Obey Conway's Law

Conway's law is a software development principle. It says that the software structure will be similar to the communication structure of the organization.

[![Conway’s Law](https://substackcdn.com/image/fetch/$s_!i6qO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe49d84a0-4553-444a-8ebe-d8ef94a243e4_1378x818.png)](https://substackcdn.com/image/fetch/$s_!i6qO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe49d84a0-4553-444a-8ebe-d8ef94a243e4_1378x818.png)Conway’s Law

They kept an open communication channel between team members for better decision-making.

Also they used Architecture Decision Records (**[ADR](https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions)**) to communicate design decisions. Because it’s easier to provide details about design choices and change them later if needed.

ADR is a document that describes how a software or a system is designed and organized. Think of it like a blueprint for software.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

Khan Academy is a nonprofit organization.

They served 30 million students in April 2020 with a simple tech stack and architecture.

And became the most used online resource during the 2020 distance learning. They offer quality content and classroom experience for millions of students for free.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/khan-academy-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Amazon S3 Achieves 99.999999999% Durability](https://substackcdn.com/image/fetch/$s_!obVc!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3645f110-2311-45c9-8e01-e50ea90e94e0_1280x720.gif)How Amazon S3 Achieves 99.999999999% Durability[Neo Kim](https://substack.com/profile/135589200-neo-kim)·March 5, 2024[Read full story](https://newsletter.systemdesign.one/p/amazon-s3-durability)](https://newsletter.systemdesign.one/p/amazon-s3-durability)

[![How Zapier Automates Billions of Tasks](https://substackcdn.com/image/fetch/$s_!YaTI!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1fc4a18e-4392-483c-b3bd-945e6885e668_1280x720.gif)How Zapier Automates Billions of Tasks[Neo Kim](https://substack.com/profile/135589200-neo-kim)·February 25, 2024[Read full story](https://newsletter.systemdesign.one/p/zapier-architecture)](https://newsletter.systemdesign.one/p/zapier-architecture)

* * *

### References

  * [How Khan Academy Successfully Handled 2.5x Traffic in a Week](https://blog.khanacademy.org/how-khan-academy-successfully-handled-2-5x-traffic-in-a-week/)

  * [How Engineering Principles Can Help You Scale](https://blog.khanacademy.org/how-engineering-principles-can-help-you-scale/)

  * [Go + Services = One Goliath Project](https://blog.khanacademy.org/go-services-one-goliath-project/)

  * [Beating the odds: Khan Academy’s successful monolith→services rewrite](https://blog.khanacademy.org/beating-the-odds-khan-academys-successful-monolith%e2%86%92services-rewrite/)

  * [Incremental Rewrites with GraphQL](https://blog.khanacademy.org/incremental-rewrites-with-graphql/)

  * [Half a million lines of Go](https://blog.khanacademy.org/half-a-million-lines-of-go/)

  * [Technical choices behind a successful rewrite project](https://blog.khanacademy.org/technical-choices-behind-a-successful-rewrite-project/)

  * [The Original Serverless Architecture is Still Here](https://blog.khanacademy.org/the-original-serverless-architecture-is-still-here/)

  * [Implementation: A new content store for Khan Academy](https://www.arguingwithalgorithms.com/blog/14-01-03-content-implementation)

  * [How was Khan Academy started?](https://support.khanacademy.org/hc/en-us/articles/202483180-What-is-the-history-of-Khan-Academy#:~:text=How%20was%20Khan%20Academy%20started,the%20more%20advanced%20math%20track.)

  * [Memcached-Backed Content Infrastructure](https://blog.khanacademy.org/memcached-backed-content-infrastructure/)

  * [Conway’s Law: the little-known principle that influences your work more than you think](https://www.atlassian.com/blog/teamwork/what-is-conways-law-acmi)

  * Image taken from [dilbert.com](https://dilbert.com/)

65

8

4

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Norbi Takacs's avatar](https://substackcdn.com/image/fetch/$s_!la1M!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffc4f5e5f-4ad2-4983-abcb-7faa9dd22af1_1116x1116.jpeg)](https://substack.com/profile/18472497-norbi-takacs?utm_source=comment)

[Norbi Takacs](https://substack.com/profile/18472497-norbi-takacs?utm_source=substack-feed-item)

[Mar 17, 2024](https://newsletter.systemdesign.one/p/khan-academy-architecture/comment/51848731 "Mar 17, 2024, 4:25 PM")

Liked by Neo Kim

Awesome read, I would have never guessed it’s a non profit organisation. The legal paperwork around non profits scare off many. 

ReplyShare

[![Fran Soto's avatar](https://substackcdn.com/image/fetch/$s_!XWMk!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10f90fdb-11ac-48b4-8f51-6a59e07763d2_1149x1149.png)](https://substack.com/profile/170998285-fran-soto?utm_source=comment)

[Fran Soto](https://substack.com/profile/170998285-fran-soto?utm_source=substack-feed-item)

[Mar 17, 2024](https://newsletter.systemdesign.one/p/khan-academy-architecture/comment/51823948 "Mar 17, 2024, 5:16 AM")

Liked by Neo Kim

Thanks for sharing the case study, Neo.

We often think these big systems started being complex, but it's inspiring to see how they started with YouTube or [Levels.fyi](http://levels.fyi) with Google sheets

ReplyShare

[6 more comments...](https://newsletter.systemdesign.one/p/khan-academy-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture