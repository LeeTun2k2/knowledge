[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Reddit Works 🔥

### #76: Break Into Reddit Architecture for 100M Daily Users (5 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jul 03, 2025

∙ Paid

131

5

18

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines Reddit's architecture. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/reddit-architecture/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

June 2005.

Two university students decide to build a mobile based food ordering service.

And pitched their startup idea for seed capital.

Yet their idea got rejected as smartphones didn’t exist back then.

[![Reddit Architecture](https://substackcdn.com/image/fetch/$s_!8pyy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5fe1da3d-49e5-4f2c-bc7c-a8816940721c_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!8pyy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5fe1da3d-49e5-4f2c-bc7c-a8816940721c_1200x630.gif)

So they pivoted to create a social network and called it Reddit.

They ran the entire site on a single machine.

[![Database Schema of Reddit](https://substackcdn.com/image/fetch/$s_!7kS7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fabe3aec4-c728-4b72-b254-e992ff1e44bb_1147x1051.png)](https://substackcdn.com/image/fetch/$s_!7kS7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fabe3aec4-c728-4b72-b254-e992ff1e44bb_1147x1051.png)Database Schema of Reddit

And stored accounts, posts, subreddits, and comments information in Postgres.

As more users joined, they installed separate machines for app server and database.

Although it temporarily solved their performance issues, there were new problems.

Here are some of them:

#### 1\. Scalability

Their growth rate was explosive.

Yet a single database and monolithic codebase became a bottleneck.

#### 2\. Latency

Popular posts received many votes and comments.

But there was a massive delay in showing the latest data.

Onward.

* * *

### [CodeRabbit: Free AI Code Reviews in VS Code - Sponsor](https://coderabbit.link/bM7fHiE)

[![CodeRabbit: AI Code Reviews in VS Code](https://substackcdn.com/image/fetch/$s_!qzSH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feeef9a7c-ed97-40e9-b48b-18e8d1b9d98d_1600x814.png)](https://coderabbit.link/bM7fHiE)

[CodeRabbit](https://coderabbit.link/bM7fHiE) brings real-time, AI-powered code reviews straight into VS Code, Cursor, and Windsurf. It lets you:

  * Get contextual feedback on every commit, not just at the PR stage

  * Catch bugs, security flaws, and performance issues as __ you code

  * Apply AI-driven suggestions instantly to implement code changes

  * Do code reviews in your IDE for free and in your PR for a paid subscription

[Install in VS Code for FREE](https://coderabbit.link/bM7fHiE)

* * *

## Reddit Architecture

Here’s how Reddit works:

### 1\. Post Submission

They added more app servers and set up a load balancer in front for scalability.

[![Database Partitions to Scale Writes](https://substackcdn.com/image/fetch/$s_!NHoE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffd85f234-e72a-44fc-9471-b1a0ca0424d3_1394x824.png)](https://substackcdn.com/image/fetch/$s_!NHoE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffd85f234-e72a-44fc-9471-b1a0ca0424d3_1394x824.png)Database Partitions to Scale Writes

Also they scaled the database vertically to handle extra I/O operations. Yet there’s a hardware limit with vertical scaling. So they [partitioned](https://www.cockroachlabs.com/blog/what-is-data-partitioning-and-how-to-do-it-right/) the database to scale writes.

But processing new posts is an expensive operation. Because it must update different tables, such as posts, subreddits, and accounts. Besides each submission goes through rate limit checks and spam filters.

So they use a job queue.

[![Processing New Posts Asynchronously Using Job Queue](https://substackcdn.com/image/fetch/$s_!2DK-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3dba4bd4-c546-4c3c-af00-3d621d425648_1731x259.png)](https://substackcdn.com/image/fetch/$s_!2DK-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3dba4bd4-c546-4c3c-af00-3d621d425648_1731x259.png)Processing a New Post Asynchronously Using Job Queue

Here’s how it works:

  1. They add a message to the job queue for each new post

  2. The processor then pulls the job from the queue asynchronously

  3. It handles the post and updates the databases

Ready for the best part?

### 2\. Lists

They show each subreddit and front page as an ordered list of posts.

Here’s the query to fetch a list:
    
    
    SELECT * FROM posts ORDER BY hot (upvotes, downvotes);

  * It selects all columns from the posts table

  * It then orders the posts by a custom function named ‘hot’, which takes upvotes and downvotes as inputs

[![Storing Post IDs on the Cache Server](https://substackcdn.com/image/fetch/$s_!GS3m!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6f1edf27-bc01-409e-935e-eccefa9fe550_1633x660.png)](https://substackcdn.com/image/fetch/$s_!GS3m!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6f1edf27-bc01-409e-935e-eccefa9fe550_1633x660.png)Storing Post IDs on the Cache Server

Yet querying the database often for an ordered list is expensive and adds extra latency. 

So they store the list of post IDs along with their rank on the cache server. Think of the **cache** as a denormalized index of posts. This makes it easy to look up the posts by primary key. 

Also they installed replica databases for read scalability.

[![Pre-computing Lists Using a Job Queue](https://substackcdn.com/image/fetch/$s_!qKbS!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fccdbdf8e-6331-4fcb-8e86-1e8d5c304fe5_2240x383.png)](https://substackcdn.com/image/fetch/$s_!qKbS!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fccdbdf8e-6331-4fcb-8e86-1e8d5c304fe5_2240x383.png)Pre-computing Lists Using a Job Queue

A single Reddit post is part of many lists.

So they pre-compute each list using a job queue and store the results on the cache server for performance. Besides they store the popular lists in [Cassandra](https://cassandra.apache.org/_/index.html) for durability.

New post submissions and voting will change the list order. But voting is an expensive operation. Because it must invalidate the cache and update different tables, such as posts, comments, and accounts. So they use a job queue.

Here’s how voting works:

  1. They add a message to the job queue

  2. The processor pulls the job from the queue asynchronously

  3. It then invalidates the cache and updates the databases

They update the cache through an atomic 'read-mutate-write' operation. 

Yet there’s a risk of a race condition when many processors handle votes on the same post at the same time. So they use locks. It ensures correctness when many processors access the same queue. 

They use [Zookeeper](https://zookeeper.apache.org/) for locking as it offers high availability.

[![Lock Contention With Many Processors](https://substackcdn.com/image/fetch/$s_!AEUu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fddf525e7-2f74-47d5-8ef2-94c0e679ed7f_1796x986.png)](https://substackcdn.com/image/fetch/$s_!AEUu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fddf525e7-2f74-47d5-8ef2-94c0e679ed7f_1796x986.png)Lock Contention With Many Processors

But the time spent waiting for locks skyrocketed during peak traffic. It means a user’s vote waits in the queue for hours before processing, thus affecting the user experience.

While more processors mean extra lock contention. It happens because different processors try to acquire the lock for a popular post. And it further slows down the queue.

So they partitioned the queue. And put votes into different queues based on the subreddit the voted post is in.

[![Partitioning the Queue for Scalability](https://substackcdn.com/image/fetch/$s_!tAyT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F985b822a-7164-400d-89a1-5cc7061a0f49_1328x662.png)](https://substackcdn.com/image/fetch/$s_!tAyT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F985b822a-7164-400d-89a1-5cc7061a0f49_1328x662.png)Partitioning the Queue for Scalability

They do a modulo N operation on the subreddit ID to find the correct queue. For example, if there are 3 queues, they do a modulo 3 on the subreddit ID.

The processors then handle different queues. Thus fewer processors ask for the same lock at once. It allowed them to scale well with the same number of processors without waiting for locks.

Let’s keep going!

### 3\. Comments

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Freddit-architecture&utm_source=paywall&utm_medium=web&utm_content=166487951)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Freddit-architecture&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture