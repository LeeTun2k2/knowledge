[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Stripe Prevents Double Payment Using Idempotent API

### #45: A Simple Introduction to Idempotent API (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 09, 2024

∙ Paid

485

32

53

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Stripe implements idempotent API. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/idempotent-api/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

2010 - California, United States.

Two brothers want to run a business.

Yet they found it difficult to set up an online payment for it.

So they pivoted to create an online payment service and called it Stripe.

[![Idempotent API](https://substackcdn.com/image/fetch/$s_!ACaD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F385d1bc7-12f6-420b-a136-95fc6158edff_800x500.gif)](https://substackcdn.com/image/fetch/$s_!ACaD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F385d1bc7-12f6-420b-a136-95fc6158edff_800x500.gif)Double Payment Problem

As number of users grew, **double payment** became an issue for them.

That means, mistakenly charging a user twice for the same transaction.

Here are some reasons for this:

### 1\. Server Error

The request fails while the server processes it.

[![The Transaction Fails While the Server Processes It](https://substackcdn.com/image/fetch/$s_!ZL8F!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fffd78fe3-6aa6-43c7-a313-55dfd0b52eac_897x247.png)](https://substackcdn.com/image/fetch/$s_!ZL8F!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fffd78fe3-6aa6-43c7-a313-55dfd0b52eac_897x247.png)The Transaction Fails During Processing

But the client doesn’t know whether the request was successful or not.

So it isn’t safe to retry.

Otherwise, there might be a double payment.

### 2\. Network Error

The server processed the request successfully.

But the network connection failed before returning a response to the client.

[![The Connection Fails While Returning a Response to the Client](https://substackcdn.com/image/fetch/$s_!TNm8!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94d3ca71-6ba5-493b-9d95-48c8ddf15a34_897x247.png)](https://substackcdn.com/image/fetch/$s_!TNm8!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94d3ca71-6ba5-493b-9d95-48c8ddf15a34_897x247.png)The Connection Fails Before Returning a Response to the Client

So the client doesn’t know if the request was successful.

Hence it isn’t safe to retry.

Otherwise, there might be a double payment.

* * *

### [Hungry Minds (Featured)](https://hungrymindsdev.substack.com/)

Learning system design is about staying hungry. 

Hungry Minds scans 100+ sources for the best deep dives, trends, and tools for software engineers to stay ahead. 

Join 9,001+ engineers from big tech to startups for 1 free digest every Monday.

[Join Hungry Minds](https://hungrymindsdev.substack.com/)

* * *

## Idempotent API

They wanted to solve the double payment problem in the best possible way.

And as in most cases, the simplest solution is the best solution.

So they created an [idempotent API](https://datatracker.ietf.org/doc/html/rfc7231#section-4.2.2).

It guarantees that a specific request can be retried many times without side effects.

That means a specific request gets processed exactly once even after many retries.

Here’s how Stripe implements idempotent API:

### 1\. Idempotency Keys

They should process a request only if it wasn't processed earlier.

This means they must track requests processed by the server.

So they create a unique string ([UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)) to use as the idempotency key.

And send it with each request’s HTTP header.

Also they generate a new UUID whenever the request payload changes.

Imagine the**idempotency key** as a fingerprint to find whether a request has already been processed.

[![Using Idempotent Keys to Prevent Double Payments](https://substackcdn.com/image/fetch/$s_!AURS!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc0e49ae2-eb70-4eb1-b5bc-ceee581c3fd4_1648x421.png)](https://substackcdn.com/image/fetch/$s_!AURS!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc0e49ae2-eb70-4eb1-b5bc-ceee581c3fd4_1648x421.png)Using Idempotency Key to Prevent Double Payment

They store the idempotency keys with an in-memory database on the server side.

And cache the server response after a request gets processed successfully.

So the in-memory database gets queried to check whether a request has been processed.

They process a request only if it's new and then store its idempotency key in the database.

Otherwise, the cached response gets returned. This means the request was processed earlier.

Also they roll back a transaction using the ACID database when a server error occurs.

Besides they remove the idempotency keys from the in-memory database after 24 hours. 

It helps to reduce storage costs and gives enough time to retry failed requests.

Put another way, an idempotency key could be reused after that period.

### 2\. Retrying Failed Requests

Although it’s safe to retry using an idempotency key, there’s a risk of server overload with many requests.

[![Exponential Backoff With Jitter While Retrying Requests for Reliability](https://substackcdn.com/image/fetch/$s_!SBr0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffec6763f-0460-48bf-9eb4-6503f6cc5b72_1507x649.png)](https://substackcdn.com/image/fetch/$s_!SBr0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffec6763f-0460-48bf-9eb4-6503f6cc5b72_1507x649.png)Exponential Backoff With Jitter While Retrying Requests for Reliability

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fidempotent-api&utm_source=paywall&utm_medium=web&utm_content=144300470)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fidempotent-api&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture