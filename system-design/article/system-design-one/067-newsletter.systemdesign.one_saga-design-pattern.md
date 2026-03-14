[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Halo Scaled to 11.6 Million Users Using the Saga Design Pattern 🎮

### #51: Break Into Saga Design Pattern (4 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jul 04, 2024

∙ Paid

191

24

14

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the Saga design pattern. You will find references at the bottom of this page if you want to go deeper._

  * [Refer just 3 people](https://newsletter.systemdesign.one/p/saga-design-pattern/?action=share) & I'll send you some rewards as a thank you.

Saga design**** pattern is often used to achieve data consistency and reliability in distributed systems. Microsoft now owns the Halo game series.

Once upon a time, a game development company named Bungie made a strategy game.

Yet they didn’t have success with it.

So they pivoted to create a shooting game and called it Halo.

They used a _single_ SQL database to store the entire game data.

But their growth rate was incredible.

[![saga design pattern](https://substackcdn.com/image/fetch/$s_!SSLK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F21f223f3-7250-43cf-ba89-25fc687acd6e_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!SSLK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F21f223f3-7250-43cf-ba89-25fc687acd6e_1200x630.gif)

And it became difficult to store all data in a single database.

So they set up a [NoSQL](https://learn.microsoft.com/en-us/azure/storage/tables/table-storage-overview) database and _partitioned_ it.

While each game can have up to 32 players.

And each player’s data gets stored in a different database partition.

[![Storing Game Data Across Database Partitions](https://substackcdn.com/image/fetch/$s_!SMjd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9d7599aa-b376-4d23-be83-49e25c5e5d01_1353x902.png)](https://substackcdn.com/image/fetch/$s_!SMjd!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9d7599aa-b376-4d23-be83-49e25c5e5d01_1353x902.png)Storing Data Across Database Partitions

Although it temporarily solved their scalability issue, it created new problems.

Here are some of them:

### 1\. Atomicity:

Each player in the same game must see the correct game points. That means atomic writes.

Atomicity is the idea that the writes to every partition succeed or no writes happen at all.

Yet there’s a risk of database partition failure.

This means a failed partition will have wrong data due to missing writes.

So it’s hard to achieve atomicity with a partitioned database.

### 2\. Consistency:

Consistency means changing data from one valid state to another valid state.

Yet there’s a risk of network latency and network failures.

That means some partitions will contain outdated data.

So it’s hard to achieve consistency with a partitioned database.

* * *

## Saga Design Pattern

They wanted a simple & scalable failure management pattern.

So they set up Saga.

Here’s how Saga works:

### 1\. Divide & Conquer:

It splits a [transaction](https://en.wikipedia.org/wiki/Database_transaction) into sub-transactions.

And assigns a separate sub-transaction to each database partition.

That means it still looks like a single transaction.

[![Saga Applying Compensating Transactions on a Failed Sub-Transaction](https://substackcdn.com/image/fetch/$s_!rtFY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F77a20954-9238-4744-8502-95b3ac0853e7_944x607.png)](https://substackcdn.com/image/fetch/$s_!rtFY!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F77a20954-9238-4744-8502-95b3ac0853e7_944x607.png)Saga Applying Compensating Transactions after a Failed Sub-Transaction

Besides a revert action is available for each sub-transaction - **compensating** **transaction**. It’s like a correction and not an undo. For example, canceling a hotel booking instead of removing it.

  * The compensating transactions get executed only if a sub-transaction fails.

  * The compensating transactions must be [idempotent](https://newsletter.systemdesign.one/p/idempotent-api), so retries are possible without side effects.

### 2\. Interacting with Database Partitions:

They set up a separate service to manage sub-transactions and called it **Orchestrator**.

[![Controlling Sub-Transactions Using Saga Orchestrator](https://substackcdn.com/image/fetch/$s_!NeJs!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F08de4584-6450-468e-9054-c1cfe1420bc1_844x515.png)](https://substackcdn.com/image/fetch/$s_!NeJs!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F08de4584-6450-468e-9054-c1cfe1420bc1_844x515.png)Saga Orchestrator Controlling Sub-Transactions

It let them:

  * Control sub-transactions

  * Apply compensating transactions if needed

### 3\. State Information:

It’s necessary to store each sub-transaction's state (start & end) outside Orchestrator.

Otherwise it will become a single point of failure.

So they use a durable and distributed **log**.

[![Distributed Log Storing State of Saga Sub-Transactions](https://substackcdn.com/image/fetch/$s_!eCyb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F909b42aa-1be5-4a5d-910e-cffe945682e0_2143x626.png)](https://substackcdn.com/image/fetch/$s_!eCyb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F909b42aa-1be5-4a5d-910e-cffe945682e0_2143x626.png)Storing State of Sub-Transactions in Log

It let them:

  * Track if a sub-transaction failed

  * Find compensating transactions that must be executed

  * Track the state of compensating transactions

  * Keep the orchestrator stateless

  * Recover from failures

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fsaga-design-pattern&utm_source=paywall&utm_medium=web&utm_content=145900943)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fsaga-design-pattern&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture