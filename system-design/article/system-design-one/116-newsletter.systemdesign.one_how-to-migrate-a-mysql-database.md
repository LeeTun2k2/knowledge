[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Tumblr Shares Database Migration Strategy With 60+ Billion Rows

### #3: Read Now - Amazing Database Migration Technique (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 10, 2023

17

2

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

> Database migration gone wrong could result in data loss, downtime, and compatibility issues.

_This post outlines the database migration techniques at Tumblr. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-to-migrate-a-mysql-database/?action=share) & I'll send you some rewards for the referrals._

Tumblr is a microblogging platform that allows users to publish short blog posts.

* * *

## How to Migrate a MySQL Database?

The fact that Tumblr uses MySQL database to store its critical data is wild.

The MySQL database at Tumblr consumes 21 terabytes with 60+ billion relational rows. And there are over 200 database servers.

Spreading the database leaders across many data centers provides scalability and high availability. So, they decided to migrate the database leader between data centers.

_A brute force approach to database migration._

The database leader ran in the remote data center. So, switch off the traffic toward the remote data center. Migrate the database leader from the remote to the local data center. And Route the traffic toward the local data center.

But this is a recipe for poor availability.

Tumblr wanted the _user_ _impact_ to be very _minimal_ on database migration.

[![CQRS pattern](https://substackcdn.com/image/fetch/$s_!46zv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6db6e05-36e4-468a-ab00-1ea190223d36_746x1262.png)](https://substackcdn.com/image/fetch/$s_!46zv!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6db6e05-36e4-468a-ab00-1ea190223d36_746x1262.png)CQRS pattern

 _An optimal approach to database migration._

The command and query responsibility segregation (**CQRS**) pattern simplified their database migration. The CQRS is a pattern that separates read and write operations to a database.

The most common replication topology in MySQL is leader-follower. The database leader accepts read-write requests. The database followers accept read requests and replicate the leader.

[![Leader-follower replication across data centers](https://substackcdn.com/image/fetch/$s_!PWsK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa83730b1-1fe4-457e-938b-678237b292f4_1289x1129.png)](https://substackcdn.com/image/fetch/$s_!PWsK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa83730b1-1fe4-457e-938b-678237b292f4_1289x1129.png)Leader-follower replication across data centers

The database leader ran in the _remote_ data center. And they provisioned database followers in the _local_ data center. The persistent connections routed the read-write requests to the database leader. The connection reuse over a large number of queries allowed to reduce latency.

But each application service had a different usage pattern. This resulted in frequent disconnections to the database leader. The connection establishment with the database was expensive. So, they ran into a latency problem.

[![Database proxy](https://substackcdn.com/image/fetch/$s_!CTYi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4122d24b-9cce-4bc1-ba85-14aedd043988_1564x801.png)](https://substackcdn.com/image/fetch/$s_!CTYi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4122d24b-9cce-4bc1-ba85-14aedd043988_1564x801.png)Database proxy

As a workaround, they provisioned a database proxy ([ProxySQL)](https://www.proxysql.com/) in the local data center. The database proxy held _persistent_ connections to the remote database leader.

The **database proxy** allowed database connection pooling. Database connection pooling is the method of holding database connections open for reuse by others. This approach prevented frequent disconnections and improved performance.

[![How to migrate a MySQL database](https://substackcdn.com/image/fetch/$s_!fh7i!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F90412460-413a-43f0-86fd-71aefd6d4589_1200x426.gif)](https://substackcdn.com/image/fetch/$s_!fh7i!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F90412460-413a-43f0-86fd-71aefd6d4589_1200x426.gif)How Tumblr migrates their MySQL database

They wanted to migrate the database leader from remote data center A to B. The workflow of the database migration at Tumblr is as follows:

  1. Each data center stored the metadata of database followers, the leader, and the database proxy

  2. They migrated the database leader from remote data center A to B

  3. An automation tool pointed all followers to the new leader

  4. An automation tool pointed the database proxy to the new leader

The database followers accepted read requests during the database migration. This approach resulted in minimal user impact with seconds of _read-only_ state. The write requests were either rejected or buffered for a few seconds.

_How to further improve write availability during database migration?_

The _leader-leader_ replication topology in MySQL could improve write availability during database migration. But this approach introduces the risk of data conflicts. This might be the reason why Tumblr doesn’t use it.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/aws-scale?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjoxMzU1ODkyMDAsInBvc3RfaWQiOjEzOTMyMjQyNiwiaWF0IjoxNzAzMzI2ODk3LCJleHAiOjE3MDU5MTg4OTcsImlzcyI6InB1Yi0xNTExODQ1Iiwic3ViIjoicG9zdC1yZWFjdGlvbiJ9.aYFg-1FPkgY0imOgrzdyWrDBerKQqecJw4HhAWZQm6I)

* * *

[![This Is How Quora Shards MySQL to Handle 13+ Terabytes](https://substackcdn.com/image/fetch/$s_!oxfV!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fef236a61-e44d-4b75-929a-a99dce9d4e56_1280x720.png)This Is How Quora Shards MySQL to Handle 13+ Terabytes[NK](https://substack.com/profile/135589200-nk)·September 3, 2023[Read full story](https://newsletter.systemdesign.one/p/mysql-sharding)](https://newsletter.systemdesign.one/p/mysql-sharding)

[![8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers](https://substackcdn.com/image/fetch/$s_!88vE!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94e067cc-6ade-44bf-9818-5dc20a260541_1280x720.png)8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers[NK](https://substack.com/profile/135589200-nk)·August 27, 2023[Read full story](https://newsletter.systemdesign.one/p/whatsapp-engineering)](https://newsletter.systemdesign.one/p/whatsapp-engineering)

* * *

## References

  * Tumblr Engineering Team. (2016, October 13). [Juggling Databases Between Datacenters](https://engineering.tumblr.com/post/151350779044/juggling-databases-between-datacenters). Tumblr Engineering Blog.

  * Engineering Team. (2012, June 8). [Jetpants: A Toolkit for Huge MySQL Topologies](https://engineering.tumblr.com/post/24612921290/jetpants-a-toolkit-for-huge-mysql-topologies?is_related_post=1). Tumblr Engineering Blog.

  * Atchison, L. (2019, November 26). [Avoiding Downtime When Migrating Data to the Cloud](https://www.linkedin.com/pulse/avoiding-downtime-when-migrating-data-cloud-lee-atchison/). LinkedIn.

  * Microsoft. [CQRS pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs). Microsoft Azure Architecture Patterns.

  * [What is database pooling?](https://stackoverflow.com/questions/4041114/what-is-database-pooling) (n.d.). In Stack Overflow.

17

2

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