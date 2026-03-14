[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Amazon S3 Works ✨

### #59: Break Into Amazon Engineering (4 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 25, 2024

∙ Paid

202

6

18

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the internal architecture of AWS S3. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/s3-architecture/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, there was an analytics startup.

They collect data from customer websites and store it in log files.

Yet they had only a few customers.

So a tiny storage server was enough.

But one morning they got a new customer with an extremely popular website.

And number of log files started to skyrocket.

Yet their storage server had only limited capacity.

[![S3 Architecture](https://substackcdn.com/image/fetch/$s_!sUlw!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdf574dc8-f7cd-41f3-9210-2ec0500d00ac_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!sUlw!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdf574dc8-f7cd-41f3-9210-2ec0500d00ac_1200x630.gif)

So they bought new hardware.

Although it temporarily solved their storage issues, there were newer problems.

Here are some of them:

#### 1\. Scalability

The storage server might become a capacity bottleneck over time.

While installation and maintenance of a larger storage server is expensive.

#### 2\. Performance

The storage server must be optimized for performance.

But they didn’t have the time and knowledge for it.

Onward.

* * *

### [Product for Engineers - Sponsor](https://dub.sh/sysdesign-oct)

[![Product for engineers - PostHog](https://substackcdn.com/image/fetch/$s_!gYY2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff4bcc904-6da6-4422-b5dc-ec198176dc2c_1280x300.png)](https://dub.sh/sysdesign-oct)

Product for Engineers is PostHog’s newsletter dedicated to helping engineers improve their product skills. It features curated advice on building great products, lessons (and mistakes) from building PostHog, and research into the practices of top startups.

[Subscribe for free](https://dub.sh/sysdesign-oct)

* * *

They wanted to ditch the storage management problem.

And focus only on product development.

So they moved to Amazon Simple Storage Service (**S3**) - an object storage. 

It stores [unstructured data](https://en.wikipedia.org/wiki/Unstructured_data) without hierarchy. 

And handle 100 million requests per second.

Yet having performance at scale is a hard problem.

So smart engineers at Amazon used simple ideas to solve it.

* * *

## S3 Architecture

Here’s how S3 works:

### 1\. Scalability

[![High-Level Architecture of S3](https://substackcdn.com/image/fetch/$s_!1WpL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4307608a-20b5-495b-a585-22cb1a3e8418_1234x384.png)](https://substackcdn.com/image/fetch/$s_!1WpL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4307608a-20b5-495b-a585-22cb1a3e8418_1234x384.png)High-Level Architecture of S3

They provide REST API via the web server.

While metadata & file content are stored separately - it lets them scale easily.

They store the metadata of uploaded data objects in a key-value database. And cache it for high availability.

Each component in the above diagram consists of many microservices. While services interact with each other via [API contracts](https://www.adobe.com/acrobat/business/hub/what-s-included-in-an-api-contract.html).

Ready for the best part?

### 2\. Performance

They store uploaded data in mechanical hard disks to reduce costs.

And organize data on the hard disk using [ShardStore](https://assets.amazon.science/77/5e/4a7c238f4ce890efdc325df83263/using-lightweight-formal-methods-to-validate-a-key-value-storage-node-in-amazon-s3-2.pdf) \- it gives better performance. Think of **ShardStore** as a variant of log-structured merge (**[LSM](https://en.wikipedia.org/wiki/Log-structured_merge-tree)**) tree data structure.

[![Moving Parts of the Hard Disk](https://substackcdn.com/image/fetch/$s_!ScHK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdcdadbee-43d9-478c-ab78-fcf2f420154a_1200x630.png)](https://substackcdn.com/image/fetch/$s_!ScHK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdcdadbee-43d9-478c-ab78-fcf2f420154a_1200x630.png)Moving Parts of the Hard Disk

A larger hard disk can store more data.

But seek & rotation times remain constant due to moving parts. So its throughput is the same as a small disk.

Put simply, a larger disk performs poorly in retrieving data.

  * **Throughput** means the amount of data transferred over time - measured in MB/s.

  * Imagine the **seek time** as time needed to move the head to a specific track on the disk. 

  * Think of **rotation time** as the time needed for the head to reach a specific piece of data.

Also a single disk might become a hot spot if the data isn’t distributed uniformly across disks.

[![Reading Data in Parallel for High Throughput](https://substackcdn.com/image/fetch/$s_!pUR3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F52a41b7f-f16c-4940-88a2-ba5fb0f4b76f_1090x518.png)](https://substackcdn.com/image/fetch/$s_!pUR3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F52a41b7f-f16c-4940-88a2-ba5fb0f4b76f_1090x518.png)Reading Data in Parallel for High Throughput

So they _replicate_ _data_ across many disks and do _parallel_ _reads_ \- it gives higher throughput.

Besides the load on a specific disk is lower because data can be read from any disk. Thus preventing hot spots.

Yet full data replication is expensive from a storage perspective.

[![Replicating Data With Erasure Coding](https://substackcdn.com/image/fetch/$s_!ixK4!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe445d626-3c3d-4a64-894b-00e7a8cca179_835x611.png)](https://substackcdn.com/image/fetch/$s_!ixK4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe445d626-3c3d-4a64-894b-00e7a8cca179_835x611.png)Replicating Data With Erasure Coding

So they use [erasure coding](https://en.wikipedia.org/wiki/Erasure_code) to replicate data.

Think of **erasure coding** as a technique to replicate data with a smaller storage overhead.

Here’s how it works:

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fs3-architecture&utm_source=paywall&utm_medium=web&utm_content=150109909)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fs3-architecture&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture