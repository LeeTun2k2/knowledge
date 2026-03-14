[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# 5 Rate Limiting Strategies Explained, Simply 🚦

### #90: Break Into Rate Limiting (5 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 19, 2025

68

3

9

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines important rate limiting strategies. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/rate-limiting/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there was a tiny bank.

They had only a few customers.

And offered services through a mobile app.

Yet some people attempted to log in to customers’ accounts by guessing passwords.

So they solved this problem by blocking specific IP addresses using firewall rules.

[![Rate Limiting](https://substackcdn.com/image/fetch/$s_!_56o!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2dff200a-3a5e-4e39-93a6-02d980124181_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!_56o!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2dff200a-3a5e-4e39-93a6-02d980124181_1200x630.gif)

But one day, their app became extremely popular.

And it became difficult to block many malicious IP addresses quickly.

So they set up **request** **throttling**.

It means slowing down requests instead of blocking them.

Yet it doesn’t stop abuse because someone could queue many requests and waste resources.

So they installed a rate limiter.

**Rate limiting** means controlling the number of requests a user can make within a time window.

For example, their app allows only 5 login attempts an hour by a user. While future attempts of the user get blocked until the time window passes.

This technique prevents abuse and server overload.

Imagine rate limiting as tickets to a movie theater. A show sells only 50 tickets. Once the tickets are sold, you’ve got to wait for the next showtime to get in.

[![How Rate Limiting Works](https://substackcdn.com/image/fetch/$s_!zO-a!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a075b1-50ef-4adf-864e-5f80a82fe59d_745x243.png)](https://substackcdn.com/image/fetch/$s_!zO-a!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a075b1-50ef-4adf-864e-5f80a82fe59d_745x243.png)How Rate Limiting Works

Here’s how it works:

  1. The user sends requests through a rate limiter

  2. The rate limiter tracks requests from a user by their IP address, user ID, or API key

  3. The extra requests get rejected if the limit exceeds (response status code:` 429 Too Many Requests`)

  4. The counter resets after the time window ends

Onward.

* * *

### [Vapi x MiniMax Free TTS API Week - Sponsor](https://vapi.ai/blog/minimax-speech-02-free-week)

[![Vapi MiniMax](https://substackcdn.com/image/fetch/$s_!pS3y!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2b92235f-5175-4541-974d-7b6de65e7541_1200x675.png)](https://vapi.ai/blog/minimax-speech-02-free-week)

**[Vapi](https://vapi.ai/blog/minimax-speech-02-free-week)** is partnering with MiniMax for a free TTS API week.

Developers & builders can get:

• Free & unlimited access to all MiniMax voices until Sept 22

• 20% off MiniMax API for a year if you try it this week

• $10 Vapi credit with code VAPIMINIMAX10

Explore next-gen TTS with the free API now.

[Explore Now](https://vapi.ai/blog/minimax-speech-02-free-week)

* * *

## Rate Limiting

There are different strategies to implement a rate limiter. 

Let’s dive in:

### 1\. Token Bucket

It’s one of the most popular rate limiting strategies.

[![Token Bucket Algorithm](https://substackcdn.com/image/fetch/$s_!Y9rx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F96cd227e-c30c-42ea-b996-c86d26829cc6_1100x784.png)](https://substackcdn.com/image/fetch/$s_!Y9rx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F96cd227e-c30c-42ea-b996-c86d26829cc6_1100x784.png)Token Bucket Algorithm

And here’s how it works:

  1. Each user gets a bucket of tokens

  2. And new tokens get added to the bucket at a fixed rate

  3. While each request from the user needs 1 token to go through

  4. The request gets blocked if the bucket is empty

This strategy is easy to implement and understand. 

Also it allows spiky traffic. For example, imagine the bucket has 5 tokens and refills at 1 token per hour. A user can make 5 requests in a few seconds, but then they’d have to wait 1 hour to make an extra request.

Yet this strategy affects the user experience by making them wait a long time if new tokens get added slowly. While refilling tokens quickly might affect the security.

So it’s necessary to control the speed at which tokens get added based on the domain and server capacity.

### 2\. **Leaky Bucket**

This strategy ensures fair usage of resources by smoothing traffic.

[![Leaky Bucket Algorithm](https://substackcdn.com/image/fetch/$s_!BLk7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1b79ac84-0b8e-4c15-bee3-ac9918cf37b3_507x873.png)](https://substackcdn.com/image/fetch/$s_!BLk7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1b79ac84-0b8e-4c15-bee3-ac9918cf37b3_507x873.png)Leaky Bucket Algorithm

Here’s how it works:

  1. Each user gets a bucket with a tiny hole at its bottom

  2. Incoming requests are like water poured into the bucket

  3. While water drips through the hole, which means a request got processed

  4. Also water drips at a constant rate, even without new requests, representing server capacity

  5. The bucket overflows if so much water is poured in quickly, which means rejected requests

Yet unused capacity gets lost even without processing requests with this approach.

Besides it’ll block requests during a traffic burst. So use this strategy when traffic is predictable, and when it’s okay to block some requests for users during a traffic burst.

Ready for the next technique?

### 3\. **Fixed Window**

This strategy works well with predictable traffic.

[![Fixed Window Algorithm](https://substackcdn.com/image/fetch/$s_!FDGz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F24967654-f17a-44ab-9156-2a159229886b_754x376.png)](https://substackcdn.com/image/fetch/$s_!FDGz!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F24967654-f17a-44ab-9156-2a159229886b_754x376.png)Fixed Window Algorithm

Here’s how:

  1. Time gets divided into equal blocks; for example, 1-hour windows

  2. Each block has a request limit: 5 requests per hour

  3. Requests get counted within the current block

  4. Extra requests get blocked if the count exceeds the limit

  5. The counter resets to 0 when a new block starts

Although it’s easy to implement, it allows traffic bursts on window boundaries. 

For example, a user could double their allowed requests by sending them at the end of one window and the start of the next. Thus overloading the server.

So use this strategy when the server can handle occasional traffic bursts.

Let’s keep going!

### 4\. **Sliding Window Counter**

This strategy offers fairness and works better with traffic bursts.

[![Sliding Window Counter Algorithm](https://substackcdn.com/image/fetch/$s_!SByn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ba0b5a5-f252-45b0-a9a3-e4a49e8629bb_754x315.png)](https://substackcdn.com/image/fetch/$s_!SByn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ba0b5a5-f252-45b0-a9a3-e4a49e8629bb_754x315.png)Sliding Window Counter Algorithm

Here’s how:

  1. Define a rolling time window, for example, a 1-hour window

  2. Each window has a request limit: 5 requests per hour

  3. A counter then keeps track of the current and previous windows

  4. The counter combines the current and previous windows to _approximate_ the number of requests in the last hour

  5. While a new request gets blocked if the count exceeds the limit

Put simply, it approximates the number of requests within the last hour from the current time.

Yet it’s more complex to implement than the fixed window, and the rate limiter count is approximate.

So use this strategy to rate limit efficiently in systems with moderate traffic.

### 5\. **Sliding Window Log**

It’s like the sliding window counter, except it keeps a log of all request timestamps. Thus offering accuracy.

[![Sliding Window Log Algorithm](https://substackcdn.com/image/fetch/$s_!E1rY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0ae7e7bf-7809-4aba-afbb-5c051ef0b869_728x280.png)](https://substackcdn.com/image/fetch/$s_!E1rY!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0ae7e7bf-7809-4aba-afbb-5c051ef0b869_728x280.png)Sliding Window Log Algorithm

Here’s how it works:

  1. A rolling time window gets defined, for example, a 1-hour window

  2. Each window has a request limit: 5 requests per hour

  3. Old requests outside the window get removed for each new request

  4. The remaining requests inside the window get counted

  5. A new request gets blocked if the count has already reached the limit

This strategy is precise and fair.

Yet it’s memory and CPU-intensive. So use it only when strict precision and fairness are necessary.

* * *

A rate limiter protects infrastructure from abuse and ensures high availability.

Yet it’s necessary to choose the right rate limiting strategy to avoid blocking users unnecessarily. 

Also users from a building might share the same IP address. So it’s better to rate limit a person using an API key instead of their IP address for accuracy and fairness.

The right strategy for rate limiting depends on simplicity and the fairness needs.

A banking app needs precision and fairness. So it’s better to use the sliding window log strategy for it.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋 Find me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 170K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter.

You are now 171,001+ readers strong, very close to 172k. Let’s try to get 172k readers by 20 September. Consider sharing this post with your friends and get rewards.

Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/rate-limiting?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

### References

  * [What is rate limiting, and how does it work? | Radware](https://www.radware.com/cyberpedia/bot-management/rate-limiting/)

  * [What is rate limiting? | Rate limiting and bots | CloudFlare](https://www.cloudflare.com/en-gb/learning/bots/what-is-rate-limiting/)

  * [What is API Rate Limiting and How to Implement It](https://datadome.co/bot-management-protection/what-is-api-rate-limiting/)

  * [What Is Rate Limiting? Benefits, Techniques & Tips | Solo.io](https://www.solo.io/topics/rate-limiting)

  * [API rate limiting explained: From basics to best practices](https://tyk.io/learning-center/api-rate-limiting/)

  * [Exploring API Rate Limiting and How to Test Limits Effectively](https://www.aptori.com/blog/what-is-api-rate-limiting-and-how-to-test-rate-limits)

  * [How to Design a Scalable Rate Limiting Algorithm with Kong API](https://konghq.com/blog/engineering/how-to-design-a-scalable-rate-limiting-algorithm)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

68

3

9

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Prasanna Reddy's avatar](https://substackcdn.com/image/fetch/$s_!Tfxb!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Forange.png)](https://substack.com/profile/307652412-prasanna-reddy?utm_source=comment)

[Prasanna Reddy](https://substack.com/profile/307652412-prasanna-reddy?utm_source=substack-feed-item)

[Sep 19](https://newsletter.systemdesign.one/p/rate-limiting/comment/157784094 "Sep 19, 2025, 6:49 PM")

Liked by Neo Kim

Some sample code examples would have made this article even better. Appreciate the effort, very helpful and informative 🫶

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/rate-limiting/comment/157784094)

[![Yellow Tail Tech's avatar](https://substackcdn.com/image/fetch/$s_!LXr2!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F07195f29-5a45-44b6-aee6-536bb49c654f_1080x1080.png)](https://substack.com/profile/335218664-yellow-tail-tech?utm_source=comment)

[Yellow Tail Tech](https://substack.com/profile/335218664-yellow-tail-tech?utm_source=substack-feed-item)

[Sep 23](https://newsletter.systemdesign.one/p/rate-limiting/comment/159046649 "Sep 23, 2025, 2:15 PM")

Loved the clarity here. Do you think most systems overdo rate limiting or don’t do it enough?

ReplyShare

[1 more comment...](https://newsletter.systemdesign.one/p/rate-limiting/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture