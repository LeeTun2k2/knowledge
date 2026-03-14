[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How to Scale an App to 100 Million Users on GCP 🚀

### #54: A Simple Guide to Scalability (7 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Aug 20, 2024

∙ Paid

155

16

18

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This isn’t a sponsored post. I wrote it for someone getting started with the Google Cloud Platform_ (_**GCP**)._

_There are various ways to scale an app on the cloud, and this is just one of them. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/google-cloud-scalability/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there lived 2 software engineers named Dominik and James.

They worked for a tech company named Hooli.

Although they had extremely smart ideas, their manager never listened to them.

So they were sad and frustrated.

[![Google Cloud Scalability](https://substackcdn.com/image/fetch/$s_!mDgf!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5c55996-9355-40c1-8f65-22b617c51ff5_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!mDgf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5c55996-9355-40c1-8f65-22b617c51ff5_1200x630.gif)

Until one afternoon when they had a wild idea to launch a startup.

And their growth rate was mind-boggling.

Yet they wanted to keep it simple. So they hosted the app on GCP.

Onward.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### [System Design Frontpage (Featured)](https://github.com/systemdesign42/system-design)

[![System Design Case Study](https://substackcdn.com/image/fetch/$s_!JGKM!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F05bccffc-c742-4e95-ada6-80a54aa75fb5_883x371.gif)](https://substackcdn.com/image/fetch/$s_!JGKM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F05bccffc-c742-4e95-ada6-80a54aa75fb5_883x371.gif)

I built a GitHub repository to help you learn system design months ago.

I want to make it the front page for system design. And help you pass system design interviews + become good at work. Consider putting a star if you find it valuable:

[Star it](https://github.com/systemdesign42/system-design)

* * *

## Google Cloud Scalability

Here’s their scalability journey from 0 to 100 million users:

### 1\. Thousand Users:

This is how they served the first 1000 users.

  * They created a minimum viable product[1](https://newsletter.systemdesign.one/p/google-cloud-scalability#footnote-1-147517391) (**MVP**) using monolith architecture - a single web server and a MySQL database.

  * Then ran the app on a single virtual machine.

[![Minimum Viable Product Using Monolith Architecture](https://substackcdn.com/image/fetch/$s_!Ee_t!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F65c96b0c-945f-470b-8b2a-989ddced5993_1847x925.png)](https://substackcdn.com/image/fetch/$s_!Ee_t!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F65c96b0c-945f-470b-8b2a-989ddced5993_1847x925.png)Minimum Viable Product Using Monolith Architecture

A _virtual machine_ provides workload isolation and extra security.

They used [Google Cloud Shell](https://cloud.google.com/shell) to deploy the app on GCP. It’s a command-line tool for the management and deployment of apps on GCP.

While [Cloud DNS](https://cloud.google.com/dns?hl=en) routed user traffic to the virtual machine. Domain Name System (**DNS**) is a service for translating human-readable domain names into IP addresses.

Yet they had users only from North America.

So they deployed the app only in that region to keep data closer to users and save costs.

Life Was Good.

### 2\. Five Thousand Users:

Until one day when they received a phone call as users couldn’t access the app.

They faced performance issues due to many concurrent users. And the single virtual machine running their app crashed. (This is a [single point of failure](https://en.wikipedia.org/wiki/Single_point_of_failure).)

While they needed a scalable[2](https://newsletter.systemdesign.one/p/google-cloud-scalability#footnote-2-147517391) and resilient[3](https://newsletter.systemdesign.one/p/google-cloud-scalability#footnote-3-147517391) app.

[![Scaling System Capacity to Meet Demand](https://substackcdn.com/image/fetch/$s_!bFd2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff49e2661-5623-4ab1-aeb7-d24efc3dea38_1847x925.png)](https://substackcdn.com/image/fetch/$s_!bFd2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff49e2661-5623-4ab1-aeb7-d24efc3dea38_1847x925.png)Scaling System Capacity to Meet Demand

So they set up autoscaling[4](https://newsletter.systemdesign.one/p/google-cloud-scalability#footnote-4-147517391) using the [Managed Instance Group](https://cloud.google.com/compute/docs/instance-groups) (**MIG**). It uses a base instance template to create new virtual machine instances.

[![Creating a New Virtual Machine From the Instance Template](https://substackcdn.com/image/fetch/$s_!jXVM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbe1707d8-c1f8-43f2-a1cb-e7a0a8679949_1847x925.png)](https://substackcdn.com/image/fetch/$s_!jXVM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbe1707d8-c1f8-43f2-a1cb-e7a0a8679949_1847x925.png)Creating a New Virtual Machine From the Instance Template

This is how they used MIG:

  * Auto scaler: create and destroy virtual machines based on load (CPU & memory)

  * Auto healer: recreate a virtual machine if it becomes unhealthy

And automate virtual machine installation across different zones for high availability.

[![Routing Traffic Using a Load Balancer](https://substackcdn.com/image/fetch/$s_!5cnX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F15421fd7-fb87-4fcd-aeba-a10a019a74c6_1847x925.png)](https://substackcdn.com/image/fetch/$s_!5cnX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F15421fd7-fb87-4fcd-aeba-a10a019a74c6_1847x925.png)Routing Traffic Using a Load Balancer

Yet they must distribute traffic across various virtual machines for performance.

So they set up a load balancer. And route the user traffic to the single IP address of the load balancer. It then distributes the traffic across virtual machines.

Also they set up monitoring and logging to troubleshoot system failures.

[![CI/CD for Reliable App Releases](https://substackcdn.com/image/fetch/$s_!MZLo!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16d7792a-bde3-456b-99f3-38b8cbe03aac_1847x925.png)](https://substackcdn.com/image/fetch/$s_!MZLo!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16d7792a-bde3-456b-99f3-38b8cbe03aac_1847x925.png)CI/CD for Reliable App Releases

Besides they set up continuous integration and continuing delivery (**[CI/CD](https://www.synopsys.com/glossary/what-is-cicd.html)**). It allowed them to automate:

  * Testing

  * Development

  * Deployment

Thus get faster feedback, improve code quality, and do reliable releases.

And Life Was Good Again.

### 3\. Ten Thousand Users:

Virtual machines failed at times.

While they ran the web server and MySQL database in the same virtual machine instance. Put simply, all functionality runs within a virtual machine. Thus limiting availability for some users on failures.

[![Three-Tier Architecture](https://substackcdn.com/image/fetch/$s_!77WB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9118e795-bd9b-4589-aa51-1616f33dd5eb_1847x925.png)](https://substackcdn.com/image/fetch/$s_!77WB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9118e795-bd9b-4589-aa51-1616f33dd5eb_1847x925.png)Three-Tier Architecture

So they decoupled the app into a 3-tier architecture:

  * Frontend

  * Backend

  * Database

And ran each layer on separate virtual machines.

They deploy virtual machines across different [zones](https://cloud.google.com/compute/docs/regions-zones) to prevent single-zone failures.

And set up auto-scaling for each layer. They use a load balancer to route traffic to the frontend layer. While another load balancer distributes traffic across the backend layer.

[![Leader-Follower Replication Topology in MySQL](https://substackcdn.com/image/fetch/$s_!wDjI!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F201c2a05-ae69-483e-be55-cdb84c0d5747_1847x925.png)](https://substackcdn.com/image/fetch/$s_!wDjI!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F201c2a05-ae69-483e-be55-cdb84c0d5747_1847x925.png)Leader-Follower Replication Topology in MySQL

Besides they ran MySQL in [leader-follower replication](https://docs.vmware.com/en/VMware-SQL-with-MySQL-for-Tanzu-Application-Service/3.3/mysql-for-tas/about-leader-follower.html) topology for high availability.

Many users from Europe and Asia signed up in the meantime.

And their growth was inevitable. Yet new users faced latency issues as they were located far from the servers. So they deployed the servers across many regions on GCP.

Very Neat.

### 4\. Hundred Thousand Users:

But one day, they noticed database performance issues.

They got extreme concurrent reads and writes. And adding more disks for increased input-output operations per second (**[IOPS](https://en.wikipedia.org/wiki/IOPS)**) needs manual operational overhead. So they moved to Google’s [managed relational database service](https://cloud.google.com/sql?hl=en).

It automatically extends the disk without downtime.

[![Caching Frequently Accessed Data for Fast Access](https://substackcdn.com/image/fetch/$s_!7Azw!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0a6bf42f-da80-451f-82a1-fb76539d1ffa_1847x925.png)](https://substackcdn.com/image/fetch/$s_!7Azw!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0a6bf42f-da80-451f-82a1-fb76539d1ffa_1847x925.png)Caching Frequently Accessed Data for Fast Access

Yet the database often got queried for the same data.

So they installed an in-memory database (Redis) between the backend and database. It cached frequently accessed data for fast access. And reduced database load.

This stabilized their data layer.

Very Clean.

### 5\. One Million Users:

Their growth skyrocketed.

Yet one morning, they noticed user traffic routed to failed virtual machines.

Here's what happened. They use [DNS Geo routing](https://community.cloudflare.com/t/how-does-geo-routing-work/76591) for user traffic. But the client’s DNS cache got outdated and pointed to the failed virtual machines. A simple fix is to reduce cache time to live (**TTL**). But frequent DNS policy updates are needed for better results.

So they set up the Google Cloud [global load balancer](https://cloud.google.com/load-balancing?hl=en).

[![Routing Traffic via the Global Load Balancer](https://substackcdn.com/image/fetch/$s_!aNdK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb482e8f3-e8c5-4832-a695-21523ad3faef_1847x925.png)](https://substackcdn.com/image/fetch/$s_!aNdK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb482e8f3-e8c5-4832-a695-21523ad3faef_1847x925.png)Routing Traffic via the Global Load Balancer

These are the benefits of using the global load balancer:

  * A single IP address gets configured on the global load balancer, so there’s no need to update the DNS policy

  * It knows failed virtual machines using periodic health checks

And routes the traffic to another region if a region fails.

[![Serving Static Content Through CDN](https://substackcdn.com/image/fetch/$s_!KyW7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F407abf6c-538e-46ea-b8d8-f5e0c79d941d_1847x925.png)](https://substackcdn.com/image/fetch/$s_!KyW7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F407abf6c-538e-46ea-b8d8-f5e0c79d941d_1847x925.png)Serving Static Content Through CDN

Yet web pages with images were slow for some users.

So they set up the content delivery network (**CDN**) for fast delivery. And used [cloud storage](https://cloud.google.com/storage?hl=en) for storing static content. It’s an object storage.

While CDN is cheaper to serve images due to low bandwidth usage.

They wanted to scale the data layer again.

But didn’t want to invest time and effort into it. (Scalability without sacrificing data consistency is difficult.)

So they use Google Cloud [Spanner](https://cloud.google.com/spanner?hl=en). It’s a scalable relational database with strong consistency.

Very Straight.

### 6\. Ten Million Users:

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fgoogle-cloud-scalability&utm_source=paywall&utm_medium=web&utm_content=147517391)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fgoogle-cloud-scalability&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture