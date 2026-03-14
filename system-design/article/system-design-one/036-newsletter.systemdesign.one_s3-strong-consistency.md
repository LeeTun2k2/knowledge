[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Amazon S3 Achieves Strong Consistency Without Sacrificing 99.99% Availability 🌟

### #79: Break Into Amazon S3 Architecture (4 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jul 29, 2025

∙ Paid

78

8

5

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how AWS S3 achieves strong consistency. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/s3-strong-consistency/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from actual implementation._

Once upon a time, there was a data processing startup.

They convert raw data into a structured format.

Yet they had only a few customers.

So an on-premise server was enough.

[![S3 Strong Consistency](https://substackcdn.com/image/fetch/$s_!uDks!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F88353352-9f5f-4120-8071-0a0e4160ee33_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!uDks!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F88353352-9f5f-4120-8071-0a0e4160ee33_1200x630.gif)

But one morning, they got a new customer with an extremely popular site.

This means massive data storage needs.

Yet their storage server had only limited capacity.

So they moved to Amazon Simple Storage Service (**[S3](https://newsletter.systemdesign.one/p/s3-architecture)**), an object storage.

It stores unstructured data without hierarchy.

[![High-Level Architecture of S3](https://substackcdn.com/image/fetch/$s_!whRc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2546466-b471-42fa-8b73-5731f8c7878d_1851x576.png)](https://substackcdn.com/image/fetch/$s_!whRc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2546466-b471-42fa-8b73-5731f8c7878d_1851x576.png)High-Level Architecture of S3

S3 provides a REST API via the web server.

And stores metadata and file content separately for scale. It stores metadata of data objects in a key-value database. 

Also it caches the metadata for low latency and high availability.

[![Strong Consistency vs Eventual Consistency](https://substackcdn.com/image/fetch/$s_!9TvG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5e576701-c427-479c-9bf3-77d090290c70_1548x998.png)](https://substackcdn.com/image/fetch/$s_!9TvG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5e576701-c427-479c-9bf3-77d090290c70_1548x998.png)Strong Consistency vs Eventual Consistency

Although caching metadata offers performance, some requests might return an older version of metadata.

Because there could be network partitions in a distributed architecture. This means writes go to one cache partition, while reads go to another cache partition. 

Thus causing eventual consistency.

But they need strong consistency in data processing for ordering and correctness.

While setting up extra app logic for strong consistency increases infrastructure complexity.

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

## S3 Strong Consistency

It’s difficult to achieve strong consistency at scale without performance or availability tradeoffs.

So smart engineers at Amazon used simple ideas to solve this hard problem.

Here’s how:

### 1\. Write Path

They update the metadata cache using the [write-through pattern](https://newsletter.systemdesign.one/i/165613635/write-through-strategy).

It means the write coordinator updates the cache first. Then updates the metadata store synchronously.

Thus reducing the risk of a stale cache.

[![How S3 Metadata Gets Updated](https://substackcdn.com/image/fetch/$s_!phUi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2df94dd5-b0df-4636-a493-0d30a7cb5d42_1839x503.png)](https://substackcdn.com/image/fetch/$s_!phUi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2df94dd5-b0df-4636-a493-0d30a7cb5d42_1839x503.png)How S3 Metadata Gets Updated

Also they set up a separate service to track the cache freshness and called it **Witness**. It stores only the latest version of a data object and keeps it lightweight (in-memory) for low latency. While the write coordinator notifies the witness whenever there’s a metadata update.

Besides they introduced a transaction log in the metadata store. It tracks the order of operations and allows them to check if the cache is fresh.

Ready for the best part?

### 2\. Read Path

Here’s the read request workflow:

  1. The server queries the cache.

  2. It then asks the witness to understand if the cache has the latest data.

  3. The server queries the metadata store only if the cache is stale.

Thus achieving strong consistency.

[![How S3 Metadata Gets Queried](https://substackcdn.com/image/fetch/$s_!evw2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6ca7144c-30bf-47d0-8cd3-a851ec5dd4df_1813x665.png)](https://substackcdn.com/image/fetch/$s_!evw2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6ca7144c-30bf-47d0-8cd3-a851ec5dd4df_1813x665.png)How S3 Metadata Gets Queried

Put simply, they find out if the metadata cache is fresh using the witness. Think of the **witness** as a central observer of metadata changes and a checkpoint for reads.

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fs3-strong-consistency&utm_source=paywall&utm_medium=web&utm_content=168979009)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fs3-strong-consistency&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture