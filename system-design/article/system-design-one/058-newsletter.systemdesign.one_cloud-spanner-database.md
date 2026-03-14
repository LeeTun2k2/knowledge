[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Google Ads Was Able to Support 4.77 Billion Users With a SQL Database 🔥

### #60: Break Into Google Spanner Architecture (5 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Nov 09, 2024

∙ Paid

192

13

21

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines Google Spanner architecture. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/cloud-spanner-database/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, 2 students decided to sell their university project for a million dollars.

Yet they failed to make the sale.

[![cloud spanner database](https://substackcdn.com/image/fetch/$s_!haoh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F60c8ad86-b4d8-4bbd-8246-23a4ffd99e8a_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!haoh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F60c8ad86-b4d8-4bbd-8246-23a4ffd99e8a_1200x630.gif)

So they decided to develop it further and named it Google.

The main source of their income was from advertisement slots (**Ads**).

And their growth rate was explosive.

Yet they stored Ads data in MySQL for simplicity and reliability.

As more users joined, they partitioned MySQL for scale.

[![Key-Range Partitioning of Mysql to Scale](https://substackcdn.com/image/fetch/$s_!g8go!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3edbe088-2805-4eb4-96f7-ac3003638d90_861x666.png)](https://substackcdn.com/image/fetch/$s_!g8go!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3edbe088-2805-4eb4-96f7-ac3003638d90_861x666.png)Key-Range Partitioning of MySQL to Scale

Although partitioning temporarily solved their scalability issues, there were newer problems.

Here are some of them:

#### 1\. Scalability

Their storage needs skyrocketed with growth.

And re-partitioning MySQL takes time as data must be moved between servers.

Yet they had extremely minimal downtime requirements.

So it became hard.

#### 2\. Transactions

They need [ACID](https://mariadb.com/resources/blog/acid-compliance-what-it-means-and-why-you-should-care/) compliance for Ads data.

But transactions became difficult after partitioning.

Think of a **transaction** as a series of writes and reads.

* * *

### [System Design Guided Practice - Sponsor](https://www.hellointerview.com/practice?utm_source=newsletter&utm_campaign=neo1)

[![Hello Interview](https://substackcdn.com/image/fetch/$s_!V_GA!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09918175-19a8-4ab9-8bac-293b4a6c1b1a_480x270.gif)](https://www.hellointerview.com/practice?utm_source=newsletter&utm_campaign=neo1)

Preparing for system design interviews? Work through common questions with personalized feedback to help you improve and own your next interview. Start practicing with [System Design Guided Practice](https://www.hellointerview.com/practice?utm_source=newsletter&utm_campaign=neo1) by Hello Interview today.

[Try Now](https://www.hellointerview.com/practice?utm_source=newsletter&utm_campaign=neo1)

* * *

## Cloud Spanner Database

They need massive scalability of NoSQL.

And ACID properties of MySQL.

So they created Spanner - _a distributed SQL database_.

Here’s how:

### 1\. Atomicity

Atomicity means a transaction is all or nothing. Put simply, a transaction must update data in different partitions at once.

[![Two-Phase Commit for Atomic Transactions](https://substackcdn.com/image/fetch/$s_!GG-G!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5f72e18a-6458-4617-8d51-ed74e1c25790_1480x805.png)](https://substackcdn.com/image/fetch/$s_!GG-G!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5f72e18a-6458-4617-8d51-ed74e1c25790_1480x805.png)Two-Phase Commit for Atomic Transactions

Yet it’s difficult to ensure every partition will commit.

So they use the two-phase commit (**[2PC](https://en.wikipedia.org/wiki/Two-phase_commit_protocol)**) protocol.

Here’s how it works:

  * Prepare phase: coordinator asks relevant partitions if they’re ready to commit

  * Commit phase: coordinator tells relevant partitions to commit if everyone agreed

The transaction gets aborted if any partition isn't ready. Thus achieving atomicity.

Ready for the best part?

### 2\. Consistency

They provide _strong_ _consistency_ at a global level.

This means if data gets updated in Europe, the same data would be shown in Asia. Put simply, a read will always return the latest write.

[![Deploying Partitions Across Zones](https://substackcdn.com/image/fetch/$s_!DPzX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7acbebd5-1d2e-45f0-b556-3ae6e412dffb_1221x625.png)](https://substackcdn.com/image/fetch/$s_!DPzX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7acbebd5-1d2e-45f0-b556-3ae6e412dffb_1221x625.png)Deploying Partitions Across Zones

A database partition gets replicated across different zones for scalability. Think of a **zone** as a geographical location.

Yet it’s important to coordinate writes among replicas to avoid data conflicts.

So they use the [Paxos algorithm](https://www.scylladb.com/glossary/paxos-consensus-algorithm/) to find a partition leader. A partition **leader** is responsible for managing writes, while followers handle reads. Also different partition leaders might get deployed in separate zones.

Imagine **Paxos** as a technique to ensure consensus across distributed systems.

[![TrueTime Architecture](https://substackcdn.com/image/fetch/$s_!VcMr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0f55728d-f593-45af-91af-ae338ad32165_1123x389.png)](https://substackcdn.com/image/fetch/$s_!VcMr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0f55728d-f593-45af-91af-ae338ad32165_1123x389.png)TrueTime Architecture

A simple way to achieve strong consistency is by ordering writes with timestamps. It allows a consistent view of data across servers.

Yet it’s difficult to maintain the same time across every server in the world.

So they use [TrueTime](https://cloud.google.com/spanner/docs/true-time-external-consistency). Think of **TrueTime** as a combination of [GPS receivers](https://timetoolsltd.com/gps/what-is-the-gps-clock/) and [atomic clocks](https://en.wikipedia.org/wiki/Atomic_clock). It finds the current time in each data center with high accuracy.

And each server synchronizes its quartz clock with TrueTime every 30 seconds.

[![Comparing Timestamps for Strong Consistency During Reads](https://substackcdn.com/image/fetch/$s_!AhiY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8456adfa-86c1-46ce-90f4-cd1315667737_1589x598.png)](https://substackcdn.com/image/fetch/$s_!AhiY!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8456adfa-86c1-46ce-90f4-cd1315667737_1589x598.png)Comparing Timestamps for Strong Consistency During Reads

Here’s how they perform reads:

  1. The request gets routed to the nearest zone even if it has a follower

  2. The follower asks the leader for the latest timestamp of the requested data

  3. The follower compares the leader’s response with its timestamp

  4. The follower responds to the client

Also the follower will wait for the leader to synchronize in case its data is outdated.

Ready for the next technique?

### 3\. Isolation

Isolation means transactions don’t interfere with each other.

[![Two-Phase Locking for Data Isolation During Writes](https://substackcdn.com/image/fetch/$s_!6xqV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F72265bbc-9de3-4d3c-9f75-53e528ea311c_648x514.png)](https://substackcdn.com/image/fetch/$s_!6xqV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F72265bbc-9de3-4d3c-9f75-53e528ea311c_648x514.png)Two-Phase Locking for Data Isolation During Writes

Yet it’s difficult to avoid data conflicts due to concurrent transactions.

So they use two-phase locking (**[2PL](https://en.wikipedia.org/wiki/Two-phase_locking)**) for data isolation during writes.

Here’s how it works:

  * Growing phase: transaction performs reads or writes once it acquires locks on data

  * Shrinking phase: transaction releases locks one at a time after it’s complete

Thus avoiding data conflicts on writes.

[![Snapshot Isolation During Reads](https://substackcdn.com/image/fetch/$s_!pZrj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa83f150c-6324-462a-969a-fe8d6cc324e7_634x430.png)](https://substackcdn.com/image/fetch/$s_!pZrj!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa83f150c-6324-462a-969a-fe8d6cc324e7_634x430.png)Snapshot Isolation During Reads

Besides they use snapshot isolation.

It’s a multi-version concurrency control (**[MVCC](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)**) technique for _lock-free reads_. Think of **snapshot** **isolation** as a way to look at the database from a point in time.

It returns a specific version of data during reads and doesn't affect ongoing writes. Thus providing data isolation.

Here’s how it works:

  * The previous data isn’t overwritten

  * Instead a new value gets written along with a TrueTime timestamp

Also older versions get automatically removed after a specific period to save storage.

Onward.

### 4\. Durability

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fcloud-spanner-database&utm_source=paywall&utm_medium=web&utm_content=150836306)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fcloud-spanner-database&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture