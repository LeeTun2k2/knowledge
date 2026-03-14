[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# This Is How Quora Shards MySQL to Handle 13+ Terabytes

### #2: Must-Know Tips - Best Sharding Techniques for MySQL (8 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 03, 2023

34

4

6

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the extraordinary story of Quora co-founder Adam D'Angelo and the sharding techniques used to shard MySQL at Quora. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/mysql-sharding/?action=share) & I'll send you some rewards for the referrals._

Summer 2005 - California, United States.

Adam D'Angelo became a full-time employee at Facebook.

Later, he transitioned into the role of chief technology officer (CTO) at Facebook.

But he felt that his responsibilities no longer fit with his skills and interests.

That’s reasonable - _each person has a different definition of success in life._

> _I took the one less traveled by,. And that has made all the difference._
> 
> _\- Robert Frost, The Road Not Taken_

He wanted to build [Quora](https://quora.com/) \- a platform for asking and answering questions. So, he took the leap and left Facebook.

Quora has a great engineering team. They implemented state-of-the-art sharding techniques to scale the database layer.

Data storage requirements at Quora are in the order of tens of terabytes. The queries per second (**QPS**) are around 100 thousand.

They stored the critical data such as questions, answers, upvotes, and comments in MySQL.

They chose MySQL for improved read performance. The NoSQL data store, such as HBase, is implemented with a [Log-structured ](https://en.wikipedia.org/wiki/Log-structured_merge-tree)merge (**LSM**) tree. In general, LSM trees are not optimized for read-heavy workloads.

They wanted to further improve the read performance. So, they introduced a caching layer before the database.

But rapid data growth and high _write_ QPS remain a _problem_.

_Solution?_**Shard** **MySQL**.

* * *

## What Is MySQL Sharding?

A single server can store and handle only a limited amount of data.

Sharding is the technique of storing a large database across many servers. In other words, sharding spreads the workload of a single server across many servers.

## Does MySQL Support Sharding?

It is possible to shard MySQL. But there is _no support_ for _automatic_ sharding. So, sharding MySQL is an extra task performed by application engineers.

## How to Shard MySQL Database?

They used vertical sharding and horizontal sharding to shard MySQL.

## Vertical Sharding

The leader-follower is the most common replication topology in MySQL. The leader handles read-write requests. The followers handle read requests and replicate the leader.

**Vertical sharding** is the technique of moving a table to a separate server. Vertical sharding allows to keep different tables with different database leaders. This approach improves the write scalability.

A partition is a division of a logical database into distinct independent parts. But Quora has a different definition for partition. According to Quora, a _partition_ is a cluster with a leader and followers.

[![Vertical sharding](https://substackcdn.com/image/fetch/$s_!8-69!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd05332d6-c969-46c0-b3f9-939766b0387e_1298x766.png)](https://substackcdn.com/image/fetch/$s_!8-69!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd05332d6-c969-46c0-b3f9-939766b0387e_1298x766.png)Vertical sharding

They stored the following metadata in [Apache Zookeeper:](https://zookeeper.apache.org/)

  * mapping from a partition to the list of tables in that particular partition

  * mapping from a partition to the list of servers

They moved a large or high-traffic table to a new partition to scale out.

MySQL requires tables to be in the same partition to join them at the database level. But this is not possible on scaling out. So, they joined tables at the application level.

An outline of the drawbacks of vertical sharding is as follows:

  * replication lag problem on moving out large tables

  * reduced transactional functionality

  * degraded performance if the table becomes very large

A popular _variant_ of vertical sharding is splitting a table into many tables. With this approach, some columns are in one table and the rest of the columns are in another table.

## Horizontal Sharding

But the large tables in a database became problematic for them due to the following reasons:

  * Schema changes were difficult

  * Table movement became challenging

  * Unexpected errors might damage the entire table

Solution?**Horizontal sharding** is the technique of splitting a logical table into many physical tables.

The key**design decisions** taken for horizontal sharding at Quora were as follows:

### 1\. Buy or Make Decision

They decided _not_ to use a third-party solution ([Vitess)](https://vitess.io/). But to build their own sharding solution. The reasons behind their decision were the following:

  * It was expensive to gain expertise and modify a third-party sharding service. They had fewer than 10 large tables to shard

  * It was easy to reuse the existing vertical sharding logic

  * It was easy to support custom API at the database level

  * There was no need for database-level joins. They performed joins at the application level

  * There was no extra middleware introduced. This kept latency low

### 2\. Shard Level

There are 2 levels to sharding the database: logical database level and table level.

The shards will live either on the same server or on different servers.

Sharding at the logical database level is pretty straightforward. It creates shards with the same set of tables across each shard.

And sharding at the table level allows to shard only the large tables.

[![Sharding level](https://substackcdn.com/image/fetch/$s_!JUl2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa097c866-40f1-4cf3-bac5-c7d254cb786b_2020x781.png)](https://substackcdn.com/image/fetch/$s_!JUl2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa097c866-40f1-4cf3-bac5-c7d254cb786b_2020x781.png)Sharding level; Horizontal sharding

They preferred sharding at the table level. The reason was the extensive use of secondary indexes at Quora.

A secondary index is an index that is not based on the usual primary index and may contain duplicate values.

The secondary index is stored only within a shard. So, the queries relying on secondary indexes may have to query all the shards of the table. This pattern is called the [scatter-and-gather pattern](https://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html). And this makes sharding at the logical database level expensive.

There is a workaround. Convert the secondary index into dedicated tables and then shard them. But this process is not easy.

Sharding at the table level is workable. Because only the queries against sharded tables need to perform scatter and gather.

### 3\. Sharding Method

A logical table is split into many physical tables via sharding. The popular sharding methods are as follows:

  * range-based partitioning

  * hash-based partitioning

[![Sharding methods](https://substackcdn.com/image/fetch/$s_!snZa!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc7d75df1-2460-4741-8dcb-409324d316ed_1997x768.png)](https://substackcdn.com/image/fetch/$s_!snZa!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc7d75df1-2460-4741-8dcb-409324d316ed_1997x768.png)Sharding methods; Horizontal sharding

**Range-based sharding** splits database rows based on a range of values. A shard key is assigned to a particular range of values.

**Hash-based sharding** assigns the shard key to each database row via a hash function.

They _preferred_ range-based sharding because range queries were the most common at Quora.

### 4\. Metadata of Shards

They persisted metadata of the shards in Zookeeper.

### 5\. API to Query Sharded Tables

Their database APIs took in explicit arguments to generate an SQL statement. This was due to security reasons. For example, this approach eliminated queries that are vulnerable to SQL injections.

SQL injection is the placement of malicious code in SQL statements. The SQL statements might come through user input.

They _extended_ the database API to include the sharding column and the sharding key data.

### 6\. Sharding Column

They chose the sharding column based on 2 factors: latency sensitivity and QPS.

[![Scatter gather pattern](https://substackcdn.com/image/fetch/$s_!8Bry!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4b1b8c19-3bd8-40a1-a47a-47f848d34f2e_1080x964.png)](https://substackcdn.com/image/fetch/$s_!8Bry!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4b1b8c19-3bd8-40a1-a47a-47f848d34f2e_1080x964.png)Scatter-gather pattern

A query that uses a non-sharding column must interact with every shard. This results in scatter-and-gather pattern. The latency of the scatter and gather pattern depends on the latency of the slowest shard. This is a recipe for degraded performance.

A potential optimization to this problem is to use a cross-shard index.

A **cross-shard index** is a separate table that maps a non-sharding column to the sharding column. Give in value of a non-sharding column to a cross-shard index. It finds the corresponding values in the sharding column.

A cross-shard index prevents the scatter and gather pattern in certain cases. For example, if there is only a single value in the sharding column for a given value of a non-sharding column. It queries only one shard.

But an overhead to querying the cross-shared index remains.

And it can also further degrade performance in certain cases. For example, if there are many values in the sharding column for a given value of the non-sharding column. It queries many shards.

There is also an increased risk of inconsistencies in the cross-shard index. This is because it lives in a separate table and atomic updates become difficult.

### 7\. Number of Shards

The number of shards depends on the shard size. The general rule of thumb is to keep the number of shards low.

_Why?_ Queries using a non-sharding column might need to query many shards. So, the latency will degrade if the number of shards increases.

* * *

Quora became a _unicorn_ startup.

According to Forbes, Adam D' Angelo is worth around 600 million USD in 2023. He is an excellent example of a person who chased dreams to achieve success.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/mysql-sharding?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![Amazon Prime Video Microservices Top Failure](https://substackcdn.com/image/fetch/$s_!HhbG!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5c2e3e8c-d0c1-4fcd-a738-0641ea583e3b_1280x720.png)Amazon Prime Video Microservices Top Failure[NK](https://substack.com/profile/135589200-nk)·September 14, 2023[Read full story](https://newsletter.systemdesign.one/p/prime-video-microservices)](https://newsletter.systemdesign.one/p/prime-video-microservices)

[![8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers](https://substackcdn.com/image/fetch/$s_!88vE!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94e067cc-6ade-44bf-9818-5dc20a260541_1280x720.png)8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers[NK](https://substack.com/profile/135589200-nk)·August 27, 2023[Read full story](https://newsletter.systemdesign.one/p/whatsapp-engineering)](https://newsletter.systemdesign.one/p/whatsapp-engineering)

* * *

## References

  * Quora Engineering. (2020). [MySQL sharding at Quora](https://quoraengineering.quora.com/MySQL-sharding-at-Quora?ch=10&oid=7663168&share=778c8f87&srid=hPvFUo&target_type=post). Quora.

  * Amazon Web Services. (n.d.). [What is Database Sharding?](https://aws.amazon.com/what-is/database-sharding/)

  * Manik Chhabra. (2023). [Understanding MySQL Sharding Simplified 101](https://hevodata.com/learn/understanding-mysql-sharding-simplified/). Hevo Data Blog

  * Wikipedia Contributors (2022). _Adam D’Angelo_. [online] Wikipedia. Available at: https://en.wikipedia.org/wiki/Adam_D%27Angelo.

  * [What is the difference between the primary index and the secondary index, exactly?](https://stackoverflow.com/questions/20824686/what-is-difference-between-primary-index-and-secondary-index-exactly). Stack Overflow

34

4

6

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Garrett's avatar](https://substackcdn.com/image/fetch/$s_!OS7q!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febfc06d5-d0fa-4da1-b0ec-5875c7532f25_144x144.png)](https://substack.com/profile/94326932-garrett?utm_source=comment)

[Garrett](https://substack.com/profile/94326932-garrett?utm_source=substack-feed-item)

[Sep 3, 2023](https://newsletter.systemdesign.one/p/mysql-sharding/comment/39509988 "Sep 3, 2023, 5:49 PM")

Liked by Neo Kim

This is a good article but the constant *bold* around words is driving me nuts. 

ReplyShare

[2 replies by Neo Kim](https://newsletter.systemdesign.one/p/mysql-sharding/comment/39509988)

[![Duke's avatar](https://substackcdn.com/image/fetch/$s_!F1W-!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3a6b7f0f-3175-4a41-a44a-f270db3255ef_96x96.jpeg)](https://substack.com/profile/109338847-duke?utm_source=comment)

[Duke](https://substack.com/profile/109338847-duke?utm_source=substack-feed-item)

[Mar 7, 2024](https://newsletter.systemdesign.one/p/mysql-sharding/comment/51103099 "Mar 7, 2024, 5:03 AM")

So SQL with scalability solution for write like partitioning/sharding/replication are the solutions for high-read with somewhat high write QPS system?

Out of context question, then what do you think is the use case that we want for a NoSQL database ? Only extensive-write systems with low read?

ReplyShare

[2 more comments...](https://newsletter.systemdesign.one/p/mysql-sharding/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture