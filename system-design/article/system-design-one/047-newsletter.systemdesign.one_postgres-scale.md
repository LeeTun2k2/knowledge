[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Figma Scaled to 4M Users—Without Fancy Databases 🔥

### #69: Break Into Postgres Scalability (4 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Apr 16, 2025

119

9

11

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Figma scaled the Postgres database. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/postgres-scale/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, 2 university students decided to build a meme generator.

Yet their business idea failed.

So they pivoted to create a design tool for the browser and called it Figma.

They stored metadata such as file information and user comments in a **single** **Postgres** database for simplicity.

[![Postgres Scale](https://substackcdn.com/image/fetch/$s_!5ZNO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ec20f6d-47d4-4ee0-ad51-fc16d92ac555_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!5ZNO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ec20f6d-47d4-4ee0-ad51-fc16d92ac555_1200x630.gif)

Yet their traffic was massive.

And it affected latency and performance because of high CPU usage.

So they **scaled vertically** by installing Postgres on a larger machine. It means more CPU.

[![Vertical Scaling](https://substackcdn.com/image/fetch/$s_!0FA4!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F70fbb328-5bec-4718-9812-4c060dfe3031_662x361.png)](https://substackcdn.com/image/fetch/$s_!0FA4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F70fbb328-5bec-4718-9812-4c060dfe3031_662x361.png)Vertical Scaling

Although it temporarily solved their scalability issue, there were new problems.

Here are some of them:

#### 1\. Storage Capacity

Some tables grew extremely fast.

While smallest unit of partition is a table.

So a single database couldn’t handle their storage needs.

#### 2\. Performance

They received a ton of write operations.

It exceeded the input-output per second (**IOPS**) limit of a single database. 

So the performance became poor.

Onward.

* * *

### [GibsonAI: AI-Powered Cloud Database - Sponsor](https://app.gibsonai.com/?utm_medium=email&utm_source=system-design&utm_campaign=0425)

[![](https://substackcdn.com/image/fetch/$s_!9N3g!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F36c21abf-6c93-41fe-8508-89dee0169c4e_2400x785.jpeg)](https://app.gibsonai.com/?utm_medium=email&utm_source=system-design&utm_campaign=0425)

Ship and scale production grade apps at a blistering pace with GibsonAI, your AI-powered serverless database. With GibsonAI, you will:

  * Design your database with natural language and view it as a diagram (ERD) & code.

  * Deploy both dev and production environments in seconds with one click.

  * Access your database directly or through the secure REST API built for you.

  * Instruct GibsonAI to build, query, or update databases right from your favorite IDE via the GibsonAI MCP server.

  * Migrations, optimization and scaling handled for you.

[Get Started for Free](https://app.gibsonai.com/?utm_medium=email&utm_source=system-design&utm_campaign=0425)

* * *

## Postgres Scale

Here’s how they scaled Postgres step by step:

### 1\. Database Replication

They set up database replicas and _route read traffic_ towards it.

[![Database Replicas to Scale Read Traffic](https://substackcdn.com/image/fetch/$s_!vrcp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94f64e6d-40bf-4330-bed0-a4654cbaa404_997x559.png)](https://substackcdn.com/image/fetch/$s_!vrcp!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94f64e6d-40bf-4330-bed0-a4654cbaa404_997x559.png)Database Replicas to Scale Read Traffic

It let them:

  * Handle more read traffic

  * Reduce latency by keeping data closer to users around the world

  * Increase fault tolerance as replicas could take over when the leader database fails

Yet replicas couldn’t handle some read operations because of [replication lag](https://en.wikipedia.org/wiki/Wikipedia:Replication_lag#:~:text=Replication%20lag%20is%20what%20happens,is%20called%20the%20replication%20lag.) and strong consistency needs. So they route those operations to the leader database.

Besides they added a caching layer to store frequently accessed data, thus reducing the database load.

Let’s keep going.

### 2\. Database Federation

They moved tables into separate databases based on the domain.

[![Database Federation](https://substackcdn.com/image/fetch/$s_!vCL4!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F23132075-e2ab-448b-9e1e-0aed3fbea27b_553x442.png)](https://substackcdn.com/image/fetch/$s_!vCL4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F23132075-e2ab-448b-9e1e-0aed3fbea27b_553x442.png)Database Federation

It let them:

  * Scale horizontally by adding more databases

  * Spread workload across databases and reduce bottlenecks

For example, they store file information and user comments in separate databases because file information grows faster than comments. While the workload is different for each table.

Ready for the next technique?

### 3\. Connection Pooling

They set up [PgBouncer](https://www.pgbouncer.org/) as the connection pooler.

It acts as a TCP proxy holding a pool of connections to Postgres.

And the client connects directly to PgBouncer instead of Postgres.

[![Connection Pooling](https://substackcdn.com/image/fetch/$s_!YGrh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F834be2a7-c50b-4a3e-8331-4c230233a1d8_750x251.png)](https://substackcdn.com/image/fetch/$s_!YGrh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F834be2a7-c50b-4a3e-8331-4c230233a1d8_750x251.png)Connection Pooling

It let them:

  * Allow clients to reuse the same connection and improve throughput

  * Limit the number of database connections to avoid connection starvation

  * Keep persistent connections with the client to avoid expensive reconnection requests

### 4\. Vertical Partitioning

They moved _columns from high-traffic tables_ into separate tables. 

And then stored those tables in different databases.

Yet each table has its own database. Put simply, a single table doesn’t spread across many databases.

[![Vertical Partitioning](https://substackcdn.com/image/fetch/$s_!tnLN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc7522676-e80e-49e9-99b3-9507c6644e03_1200x630.png)](https://substackcdn.com/image/fetch/$s_!tnLN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc7522676-e80e-49e9-99b3-9507c6644e03_1200x630.png)Vertical Partitioning

It let them:

  * Isolate sensitive data for security

  * Reduce workload by routing traffic across many databases

  * Achieve high query performance by scanning only a subset of data

Yet vertical partitioning becomes a bottleneck with high write traffic. Because a single database wouldn’t be able to store a table with billions of rows.

Ready for the best part?

### 5\. Horizontal Partitioning

They _split tables at the row level_ and stored them across many databases for maximum scalability.

[![Horizontal Partitioning](https://substackcdn.com/image/fetch/$s_!BLmI!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc1b1654e-f60a-45c2-bf6a-ee060eb91e5d_1200x630.png)](https://substackcdn.com/image/fetch/$s_!BLmI!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc1b1654e-f60a-45c2-bf6a-ee060eb91e5d_1200x630.png)Horizontal Partitioning

Also they set up a **proxy server** to route queries to the correct database.

[![Proxy Server for Topology Management](https://substackcdn.com/image/fetch/$s_!0nLV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F99f62426-7bf7-4932-af0a-dc907d00e386_1093x251.png)](https://substackcdn.com/image/fetch/$s_!0nLV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F99f62426-7bf7-4932-af0a-dc907d00e386_1093x251.png)Proxy Server for Topology Management

It let them:

  * Rewrite expensive queries for performance

  * Manage database topology and support transactions

  * Drop extra requests when it exceed the threshold limit

They used the _hash of the partition key to route write requests._ Thus distributing data uniformly across databases.

Besides they co-located tables partitioned by the same key to make transactions and joins easier.

* * *

Figma became a popular design tool with over 40 million users.

This case study shows Postgres can easily handle internet-scale traffic.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Find me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 100K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/postgres-scale?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

**TL;DR** 🕰️

You can find a [summary of this article on Twitter](https://x.com/systemdesignone/status/1912466042615365743). Please consider a retweet if you find it helpful.

* * *

[![How Apple Pay Handles 41 Million Transactions a Day Securely 💸](https://substackcdn.com/image/fetch/$s_!6aLm!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff75ede44-6b6f-4e22-ae4d-dc762586955d_1280x720.gif)How Apple Pay Handles 41 Million Transactions a Day Securely 💸[Neo Kim](https://substack.com/profile/135589200-neo-kim)·March 27, 2025[Read full story](https://newsletter.systemdesign.one/p/how-does-apple-pay-work)](https://newsletter.systemdesign.one/p/how-does-apple-pay-work)

[![How Do Websockets Work ✨](https://substackcdn.com/image/fetch/$s_!7XyR!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcf1bf388-28af-4284-b0df-1f95b80f0df6_1280x720.gif)How Do Websockets Work ✨[Neo Kim](https://substack.com/profile/135589200-neo-kim)·February 28, 2025[Read full story](https://newsletter.systemdesign.one/p/how-do-websockets-work)](https://newsletter.systemdesign.one/p/how-do-websockets-work)

* * *

### References

  * [The growing pains of database architecture](https://www.figma.com/blog/how-figma-scaled-to-multiple-databases/)

  * [How Figma’s databases team lived to tell the scale](https://www.figma.com/blog/how-figmas-databases-team-lived-to-tell-the-scale/)

  * [Database Scaling at Figma with Sammy Steele](https://softwareengineeringdaily.com/2024/04/23/database-scaling-at-figma-with-sammy-steele/)

  * [In Conversation: Dylan Field and Garry Tan on design, AI, and the power of locking in](https://www.figma.com/blog/in-conversation-dylan-field-and-garry-tan/)

  * [How did Figma Succeed? A Brief History](https://dfeldman.medium.com/how-did-figma-succeed-a-brief-history-b816492da146)

  * [PostgreSQL official site](https://www.postgresql.org/)

  * [Figma Statistics: Valuation, Funding & More](https://www.auditshq.com/blog/figma-statistics)

  * [Figma on CNBC](https://www.cnbc.com/2024/05/14/figma-cnbc-disruptor-50.html)

  * [The Story of Figma: Collaboration, Community, and Creativity](https://www.indexventures.com/perspectives/the-story-of-figma-collaboration-community-and-creativity/)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

119

9

11

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Apr 16, 2025](https://newsletter.systemdesign.one/p/postgres-scale/comment/109258661 "Apr 16, 2025, 12:11 PM")

Liked by Neo Kim

Here is a lesson on how different it is to scale read than write.

Thanks for sharing, Neo.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/postgres-scale/comment/109258661)

[![Petar Ivanov's avatar](https://substackcdn.com/image/fetch/$s_!Hqxs!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb236a7ab-735e-49d2-bbe8-98b1f901b169_500x500.jpeg)](https://substack.com/profile/10269058-petar-ivanov?utm_source=comment)

[Petar Ivanov](https://substack.com/profile/10269058-petar-ivanov?utm_source=substack-feed-item)

[Apr 27, 2025](https://newsletter.systemdesign.one/p/postgres-scale/comment/112466752 "Apr 27, 2025, 12:26 PM")

Liked by Neo Kim

There are some very important tips like caching, separation of concerns, and dealing with heavy load.

Great breakdown, Neo! 🙌 

ReplyShare

[7 more comments...](https://newsletter.systemdesign.one/p/postgres-scale/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture