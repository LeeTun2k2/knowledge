[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# This Is How Stripe Does Rate Limiting to Build Scalable APIs

### #9: Read Now - Awesome Rate Limiter (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 28, 2023

38

4

2

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Stripe does rate limiting. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/rate-limiter/?action=share) & I'll send you some rewards for the referrals._

## Rate Limiter

A rate limiter is important to building a scalable API. Because it prevents bad users from abusing the API.

A rate limiter keeps a _counter_ on the number of requests received. And reject a request if the threshold exceeds. Requests are rate-limited at the user or IP address level.

And it is a good choice if a change in the pace of the requests _doesn't_ affect the user experience.

Other potential _reasons_ to rate limit are:

  * Prevent low-priority traffic from affecting high-priority traffic

  * Prevent service degradation

Rejecting low-priority requests under heavy load is called _load shedding_.

_This post outlines how Stripe scales their API with the rate limiter. Consider[sharing this post](https://newsletter.systemdesign.one/p/rate-limiter/?action=share) with someone who wants to study scalability patterns._

## Rate Limiter Workflow

The rules for rate limiting are predefined. And here is the rate limiter workflow:

[![Rate limiter workflow](https://substackcdn.com/image/fetch/$s_!H9MM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc7b98eef-38f3-43ad-85b9-827ddacf0b9c_470x1055.png)](https://substackcdn.com/image/fetch/$s_!H9MM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc7b98eef-38f3-43ad-85b9-827ddacf0b9c_470x1055.png)Rate limiter

  1. Check rate limiter rule

  2. Reject a request if the threshold exceeds

  3. Otherwise let the request pass-through

## Rate Limiter Implementation

Stripe uses the [token bucket algorithm](https://en.wikipedia.org/wiki/Token_bucket) to do rate limiting. Here is a quick overview of this algorithm:

[![Token bucket algorithm; Rate limiter](https://substackcdn.com/image/fetch/$s_!quNb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F857ea765-9cae-471b-a9e8-d525c900c6c2_1145x975.png)](https://substackcdn.com/image/fetch/$s_!quNb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F857ea765-9cae-471b-a9e8-d525c900c6c2_1145x975.png)Token bucket algorithm

Imagine there is a bucket filled with tokens. Every request must pick a token from the bucket to pass through. 

No requests get a token if the bucket is empty. So, further requests get rejected.

And tokens get refilled at a steady pace.

Other _popular_ rate-limiting algorithms are sliding windows and leaky buckets.

They used Redis to build the rate limiter. Because it is in-memory and provides low latency.

Things they considered when implementing the rate limiter are:

  * Quality check rate limiter logic and allow bypass on failures

  * Show a clear response to the user: status code 429 - too many requests or 503 - service unavailable

  * Enable panic mode on the rate limiter. This allowed switching it off on failures

  * Set up alerts and monitoring

  * Tune rate limiter to match traffic patterns

It's difficult to _rate-limit a distributed system_. Because each request from a single user might not hit the same server. I don't know how Stripe solved this problem. But here is a potential solution.

Redirect the traffic from an IP address to the same data center using DNS. And create an isolated rate limiter in each data center. 

Yet a new TCP connection might hit a different server within a data center.

So set up a caching proxy ([twemproxy](https://github.com/twitter/twemproxy)) in each data center. Because it allows to share state across many servers. Put another way, many servers share a single cache: rate limiter.

And use [consistent](https://systemdesign.one/consistent-hashing-explained/) [hashing](https://newsletter.systemdesign.one/p/what-is-consistent-hashing) to reduce key redistribution on changing load (cluster resize).

## Rate Limiting Types

Stripe categorizes rate limiting into 4 types:

### 1\. Request Rate Limiter

[![Request Rate Limiter](https://substackcdn.com/image/fetch/$s_!FbXR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc54880f5-bec7-46b5-b822-4a9e87593f23_1569x1094.png)](https://substackcdn.com/image/fetch/$s_!FbXR!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc54880f5-bec7-46b5-b822-4a9e87593f23_1569x1094.png)Request Rate Limiter

Each user gets _n_ requests per second. This rate limiter type acts as the first line of defense for an API. And it is the most popular type.

### 2\. Concurrent Requests Rate Limiter

[![Concurrent Requests Rate Limiter](https://substackcdn.com/image/fetch/$s_!1gE_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3f6b7fb9-1459-42e6-ab07-67e0c57ecd5c_1584x1345.png)](https://substackcdn.com/image/fetch/$s_!1gE_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3f6b7fb9-1459-42e6-ab07-67e0c57ecd5c_1584x1345.png)Concurrent Requests Rate Limiter

The number of concurrent requests that are in progress is rate-limited. This protects resource-intensive API. And prevents resource contention.

### 3\. Fleet Usage Load Shedder

[![Fleet Usage Load Shedder; Rate limiter](https://substackcdn.com/image/fetch/$s_!fQtd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa88813d6-3cfd-4e91-b3bc-2bb7dd7cb138_1580x1370.png)](https://substackcdn.com/image/fetch/$s_!fQtd!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa88813d6-3cfd-4e91-b3bc-2bb7dd7cb138_1580x1370.png)Fleet Usage Load Shedder

The critical APIs reserve 20% of computing capacity. And requests to _non-critical APIs get rejected_ if the critical API doesn’t get 20% of the resources.

### 4\. Worker Utilization Load Shedder

[![Worker Utilization Load Shedder; Rate limiter](https://substackcdn.com/image/fetch/$s_!1MBR!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa447d21b-5eca-4ab4-b495-63bb663d94db_1567x1338.png)](https://substackcdn.com/image/fetch/$s_!1MBR!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa447d21b-5eca-4ab4-b495-63bb663d94db_1567x1338.png)Worker Utilization Load Shedder

The non-critical traffic gets shed on server overload. And it gets re-enabled after a delay. This rate limiter type is the last line of defense for an API.

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

[![Tech Stack Evolution at Levels.fyi](https://substackcdn.com/image/fetch/$s_!UC3j!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5b607b54-0dd2-40a2-a252-4b393737908b_1280x720.png)Tech Stack Evolution at Levels.fyi[NK](https://substack.com/profile/135589200-nk)·September 24, 2023[Read full story](https://newsletter.systemdesign.one/p/levels-fyi-google-sheets)](https://newsletter.systemdesign.one/p/levels-fyi-google-sheets)

[![11 Reasons Why YouTube Was Able to Support 100 Million Video Views a Day With Only 9 Engineers](https://substackcdn.com/image/fetch/$s_!oJPb!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F15e36395-797c-40f8-9057-4ac4d97ab4c1_1280x720.png)11 Reasons Why YouTube Was Able to Support 100 Million Video Views a Day With Only 9 Engineers[NK](https://substack.com/profile/135589200-nk)·September 16, 2023[Read full story](https://newsletter.systemdesign.one/p/youtube-scalability)](https://newsletter.systemdesign.one/p/youtube-scalability)

* * *

## References

  * https://stripe.com/blog/rate-limiters

  * https://www.cloudflare.com/en-gb/learning/bots/what-is-rate-limiting/

  * https://blog.cloudflare.com/counting-things-a-lot-of-different-things/

38

4

2

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Anton Zaides's avatar](https://substackcdn.com/image/fetch/$s_!m9AG!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe37a1acd-c9a1-4968-b60d-907005004d84_1728x1728.jpeg)](https://substack.com/profile/121956618-anton-zaides?utm_source=comment)

[Anton Zaides](https://substack.com/profile/121956618-anton-zaides?utm_source=substack-feed-item)

[Sep 28, 2023](https://newsletter.systemdesign.one/p/rate-limiter/comment/40875644 "Sep 28, 2023, 4:34 PM")

Liked by Neo Kim

If you implement the simple request rate limiter - one important part is how to communicate it to the clients. I had 2 cases in the last year where we breached the limit:

1\. A free 3rd party api. We had a limit of 500 requests per day (which was not written down), which we didn't pass for a long time. Then we suddenly started to get 403 errors, which immediately led us to think our token was revoked or expired. Only after it started to work at 12:00, did we understand we breached the limit, and for some reason, they decided to throw unauthorized instead of the standard 429.

2\. A paid api, that we heavily use. Recently we reached their limit (100 calls/minute), and started to get 429 responses. The good part, is that in the header we got the time when our limit will be reset. This allowed us to implement internal queueing of the request, without noisy errors. We had the 429 ones, we stored the requests, and then we continued at the allowed time until we hit it again. As it's mainly in rare peaks, this solution is perfect for us.

ReplyShare

[2 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/rate-limiter/comment/40875644)

[![Owaise Imdad's avatar](https://substackcdn.com/image/fetch/$s_!72Q8!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F156d29aa-8259-48eb-983e-8aec498dbefe_144x144.png)](https://substack.com/profile/202080858-owaise-imdad?utm_source=comment)

[Owaise Imdad](https://substack.com/profile/202080858-owaise-imdad?utm_source=substack-feed-item)

[Aug 26](https://newsletter.systemdesign.one/p/rate-limiter/comment/149332364 "Aug 26, 2025, 4:24 PM")

Neo, correct me if I’m wrong, but from what I understand, Stripe’s strategy seems to be layered rather than type-based. It feels like all four types you mentioned are actually layers. I didn’t see this explicitly stated in the documentation, so just wanted to point it out for clarity. Appreciate the blog—great work!

ReplyShare

[2 more comments...](https://newsletter.systemdesign.one/p/rate-limiter/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture