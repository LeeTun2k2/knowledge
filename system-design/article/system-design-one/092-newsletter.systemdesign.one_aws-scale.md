[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How to Scale an App to 10 Million Users on AWS

### #27: And High Scalability Explained Like You're Twelve (6 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Dec 06, 2023

332

29

44

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how to scale an app to millions of users on the cloud. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/aws-scale/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there lived 2 software engineers named James and Robert.

They worked for a tech company named Hooli.

Although they were bright engineers, they never got promoted.

So they were sad and frustrated.

[![AWS Scale](https://substackcdn.com/image/fetch/$s_!mDgf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5c55996-9355-40c1-8f65-22b617c51ff5_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!mDgf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5c55996-9355-40c1-8f65-22b617c51ff5_1200x630.gif)

Until one day when they had a smart idea to build a startup.

Their growth rate was mind-boggling.

Yet they wanted to keep it simple. So they hosted the app on AWS.

* * *

## AWS Scale

Here’s their scalability journey from 0 to 10 million users:

### 1\. Prelaunch

Their minimum viable product (**MVP**) was not production-ready.

So they used a static framework to build the launch page. And served static pages at no operational costs via AWS Amplify hosting.

_AWS Amplify_ is a web app hosting service.

[![AWS Scale; Static hosting](https://substackcdn.com/image/fetch/$s_!_sUk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F281ad72a-a6ce-4dea-85e1-800704f3160e_1585x468.png)](https://substackcdn.com/image/fetch/$s_!_sUk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F281ad72a-a6ce-4dea-85e1-800704f3160e_1585x468.png)Launch Page on Static Hosting

Also they added simple backend code in AWS Lambda.

_AWS Lambda_ is a serverless platform. It allowed them to run code without managing servers.

### 2\. Launch

They launched the MVP.

But they had only a handful of users.

So they used a single EC2 instance to run the entire web stack: database and backend.

_Amazon Elastic Compute Cloud_ (**EC2**) is a virtual server.

They attached an elastic IP address to the EC2 instance and then pointed Amazon Route 53 _DNS_ toward it.

An **Elastic IP address** is a static IP address.

[![AWS scale; web stack on EC2](https://substackcdn.com/image/fetch/$s_!KNOg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F83090fa5-61a4-4a42-a87b-97270009b52a_1727x462.png)](https://substackcdn.com/image/fetch/$s_!KNOg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F83090fa5-61a4-4a42-a87b-97270009b52a_1727x462.png)Single EC2 Instance Running the Entire Web Stack

Yet they hit the storage limits because some users had more data.

So they installed a larger instance type to scale vertically.

Increasing the capacity of a single server for performance is called **Vertical Scaling**.

The configuration template for virtual servers is called **Instance Type**. For example, _nano_ and _micro_ instance types offer varying CPU, memory, and storage.

But there is a risk of a single point of failure with vertical scaling. Also a service cannot be scaled vertically beyond a limit.

### 3\. Ten Users

But one day.

They hit the memory limit with the single EC2 instance.

So they moved the backend and database into separate EC2 instances. 

And run the database on a larger instance type.

[![AWS scale; EC2 instances running app services](https://substackcdn.com/image/fetch/$s_!DoZr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff801d5aa-ef35-4462-8544-880d65eb01f2_726x462.png)](https://substackcdn.com/image/fetch/$s_!DoZr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff801d5aa-ef35-4462-8544-880d65eb01f2_726x462.png)Backend and Database Running on Separate EC2 Instances

They used an SQL database because it was enough to handle the first 10 million users. Also it offered them strong community support and tools.

And life was good.

### 4\. Thousand Users

Until one day when they received a phone call.

There was a power outage in the availability zone hosting their servers.

So they installed an extra server in another availability zone. 

And set up the database in the leader-follower replication topology. 

The database follower ran in another availability zone.

The database leader served the write operations. While the database follower served the reads.

Also they set up automatic failover for high availability.

[![AWS Scale; High availability with 2 AZ](https://substackcdn.com/image/fetch/$s_!CcY-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8547e73e-c809-492e-bcc9-e143b673477a_1173x1174.png)](https://substackcdn.com/image/fetch/$s_!CcY-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8547e73e-c809-492e-bcc9-e143b673477a_1173x1174.png)Running the App Across Two Availability Zones

They load-balanced the traffic between 2 availability zones.

And pointed DNS toward the load balancer.

They used an Elastic Load Balancer (**ELB**). And it did server health checks to prevent traffic to failed instances.

### 5\. Ten Thousand Users

But one day.

They hit the memory limit again. And servers ran at full capacity.

They wanted to scale out.

So they made the backend stateless by moving out the session state. And storing it in ElasticCache.

Besides they added more servers behind the load balancer to scale horizontally. And installed extra database followers.

[![AWS Scale; stateless app](https://substackcdn.com/image/fetch/$s_!WbzH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F800e265f-1472-47fe-be30-b4546d78654c_1016x217.png)](https://substackcdn.com/image/fetch/$s_!WbzH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F800e265f-1472-47fe-be30-b4546d78654c_1016x217.png)Making App Server Stateless by Storing State in Cache

Yet the database reads were becoming a bottleneck.

So they cached the popular database reads in ElasticCache.

_ElasticCache_ is a managed Memcached or Redis.

[![AWS scale; caching reads](https://substackcdn.com/image/fetch/$s_!Sa6W!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0ab3c2f8-f968-4c26-970f-1daaecd70618_1021x491.png)](https://substackcdn.com/image/fetch/$s_!Sa6W!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0ab3c2f8-f968-4c26-970f-1daaecd70618_1021x491.png)Caching Database Reads to Reduce Load

Also they moved static content to Amazon S3 and CloudFront to reduce the server load. The static content is CSS, images, and JavaScript files.

_Amazon S3_ is an object store. While _CloudFront_ is a Content Delivery Network (**CDN**).

[![AWS scale; CDN with static content](https://substackcdn.com/image/fetch/$s_!T2_B!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4bbbc36d-47a3-4876-a944-b13a3bdfbee2_1241x315.png)](https://substackcdn.com/image/fetch/$s_!T2_B!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4bbbc36d-47a3-4876-a944-b13a3bdfbee2_1241x315.png)CDN Hosting Static Content

They reduced latency by caching data in CDN at edge locations. 

A content delivery endpoint in a CDN network is called an **Edge Location.**

[![AWS scale; Autoscaling](https://substackcdn.com/image/fetch/$s_!XdiC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fed271fac-fefb-48ad-afef-d23f939ff53b_1195x898.png)](https://substackcdn.com/image/fetch/$s_!XdiC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fed271fac-fefb-48ad-afef-d23f939ff53b_1195x898.png) Dynamically Scaling Servers With Autoscaling

Yet one day. 

They noticed their peak traffic was much higher than average traffic.

So they over-provisioned servers to handle peak traffic. 

But it cost them a lot of money.

So they set up autoscaling with CloudWatch.

Scaling the computing resources based on load is called **Autoscaling**. It reduced their costs and operational complexity.

_CloudWatch_ is Amazon's monitoring and management service. It embeds an agent in each service to collect metrics.

### 6\. Half a Million Users

Their growth was inevitable.

And they wanted to offer the users a service level agreement (**SLA**) of 99.99% high availability.

Put another way, they couldn’t tolerate downtime of more than 52 minutes per year.

So they replicated the servers across many availability zones. And offloaded session data to DynamoDB.

Besides they ran each server at full capacity for performance.

_DynamoDB_ is a managed NoSQL database.

[![AWS scale; microservices architecture](https://substackcdn.com/image/fetch/$s_!cx7I!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd98b7829-005f-4ffb-9e04-ea7c4aa6fa04_1505x686.png)](https://substackcdn.com/image/fetch/$s_!cx7I!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd98b7829-005f-4ffb-9e04-ea7c4aa6fa04_1505x686.png)Monolith vs Microservices

They moved to a microservices architecture because wanted to scale services independently.

[![AWS scale; load balancer](https://substackcdn.com/image/fetch/$s_!hVRD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6b25db77-c37d-4d9d-94e9-27013e86202c_1356x222.png)](https://substackcdn.com/image/fetch/$s_!hVRD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6b25db77-c37d-4d9d-94e9-27013e86202c_1356x222.png)Load Balancing Different Layers

Also they added load balancers between each layer to scale out.

[![AWS Scale; CloudFormation](https://substackcdn.com/image/fetch/$s_!m-B3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F44b92709-fc18-4dee-ace8-9c7679a96f78_1991x285.png)](https://substackcdn.com/image/fetch/$s_!m-B3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F44b92709-fc18-4dee-ace8-9c7679a96f78_1991x285.png)CloudFormation Workflow

They used AWS CloudFormation to create a templatized view of the web stack. Because it reduced the operational complexity.

_CloudFormation_ is an infrastructure as a code deployment service.

And life was good again.

### 7\. Ten Million Users

But one day. 

They noticed database writes being slow due to data contention.

So they federated the database and then sharded it.

[![AWS Scale; Federation](https://substackcdn.com/image/fetch/$s_!AoIB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F650cb4b6-e955-4816-8c31-c7bd21aaa311_853x244.png)](https://substackcdn.com/image/fetch/$s_!AoIB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F650cb4b6-e955-4816-8c31-c7bd21aaa311_853x244.png)Database Federation

Splitting a database into many databases based on the business domain is called **Federation**. For example, storing product and user information in separate databases. 

Federation allowed them to scale easily.

Yet it became difficult to query cross-function data due to many databases.

[![AWS Scale; Sharding](https://substackcdn.com/image/fetch/$s_!vq_9!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa76d1b62-88b5-4a2f-ad91-d6f18dc6f9c4_1180x295.png)](https://substackcdn.com/image/fetch/$s_!vq_9!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa76d1b62-88b5-4a2f-ad91-d6f18dc6f9c4_1180x295.png)Replication vs Sharding

Splitting a single dataset across many servers is called **Sharding**.

They used sharding to scale out.

But it made the application layer more complex because the data needed to be joined at the application level.

Also they moved the data that doesn't need complex joins to a NoSQL database.

[![AWS Scale; multi region scalability](https://substackcdn.com/image/fetch/$s_!H4dg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F292d0562-b78f-4fe1-936b-1b6df7a69c78_800x500.gif)](https://substackcdn.com/image/fetch/$s_!H4dg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F292d0562-b78f-4fe1-936b-1b6df7a69c78_800x500.gif)

Besides they extended the system across many AWS regions. Because it offered high availability and scalability.

And lived happily ever after.

* * *

AWS has 32 regions and 102 availability zones around the world.

[![AWS Scale; regions and availability zones](https://substackcdn.com/image/fetch/$s_!IqdB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb37f946-22c9-41f3-b7fd-45ea80f21dd2_1418x438.png)](https://substackcdn.com/image/fetch/$s_!IqdB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb37f946-22c9-41f3-b7fd-45ea80f21dd2_1418x438.png)AWS Regions and Availability Zones

A Geographical area with data centers is called an **AWS Region**. Each region has more than a single availability zone.

A single data center or a combination of many data centers within an AWS region is called an **Availability Zone**.

Each AZ is connected to a separate network and power for high availability.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/aws-scale?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Uber Computes ETA at Half a Million Requests per Second](https://substackcdn.com/image/fetch/$s_!mMKk!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F24c00d06-7158-43d8-9815-067e3b9c725d_1280x720.gif)How Uber Computes ETA at Half a Million Requests per Second[NK](https://substack.com/profile/135589200-nk)·December 3, 2023[Read full story](https://newsletter.systemdesign.one/p/uber-eta)](https://newsletter.systemdesign.one/p/uber-eta)

[![Everything You Need to Know About Micro Frontends](https://substackcdn.com/image/fetch/$s_!tijZ!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fedb1b927-4c3a-4b8d-a9e2-650da7093495_1280x720.png)Everything You Need to Know About Micro Frontends[NK](https://substack.com/profile/135589200-nk)·November 14, 2023[Read full story](https://newsletter.systemdesign.one/p/micro-frontends)](https://newsletter.systemdesign.one/p/micro-frontends)

* * *

## References

  * [Scaling on AWS for your first 10 million users](https://www.youtube.com/watch?v=yrP3M4_13QM)

  * [AWS Documentation Overview](https://aws.amazon.com/documentation-overview/)

  * [SRE fundamentals](https://cloud.google.com/blog/products/devops-sre/sre-fundamentals-slis-slas-and-slos)

332

29

44

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Fran Soto's avatar](https://substackcdn.com/image/fetch/$s_!XWMk!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10f90fdb-11ac-48b4-8f51-6a59e07763d2_1149x1149.png)](https://substack.com/profile/170998285-fran-soto?utm_source=comment)

[Fran Soto](https://substack.com/profile/170998285-fran-soto?utm_source=substack-feed-item)

[Dec 8, 2023](https://newsletter.systemdesign.one/p/aws-scale/comment/45035575 "Dec 8, 2023, 6:00 PM")

Liked by Neo Kim

Nice article!

I'm curious on the decision from our two engineers to handle the database on their own for many of the initial phases instead of offloading to a fully managed database service in AWS (DDB or RDS depending on needs).

What could be the reasons they decided to host them in EC2 instances? Costs? A particular DB technology not offered as a service in AWS?

ReplyShare

[4 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/aws-scale/comment/45035575)

[![Anton Zaides's avatar](https://substackcdn.com/image/fetch/$s_!m9AG!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe37a1acd-c9a1-4968-b60d-907005004d84_1728x1728.jpeg)](https://substack.com/profile/121956618-anton-zaides?utm_source=comment)

[Anton Zaides](https://substack.com/profile/121956618-anton-zaides?utm_source=substack-feed-item)

[Dec 8, 2023](https://newsletter.systemdesign.one/p/aws-scale/comment/45032492 "Dec 8, 2023, 5:20 PM")

Liked by Neo Kim

Loved the story format! It keeps the article engaging :) 

A question though - won't the pre-launch stack be enough for steps 1-3? I'm doing a small project myself, and we indeed chose Amplify, Lambda and RDS. What's the advantages of an EC2 instance versus Lambdas? Cost savings?

ReplyShare

[3 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/aws-scale/comment/45032492)

[27 more comments...](https://newsletter.systemdesign.one/p/aws-scale/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture