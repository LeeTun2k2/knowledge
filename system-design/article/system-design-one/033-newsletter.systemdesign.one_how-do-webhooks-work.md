[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Do Webhooks Work ⭐

### #82: Break Into Webhooks Pattern (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Aug 14, 2025

106

6

10

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how webhooks work. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-do-webhooks-work/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there was an e-commerce store.

They used an external payment service to handle orders.

It means the payment service processes a payment first. 

And only after that, the store sends a confirmation email to the customer.

[![How Do Webhooks Work](https://substackcdn.com/image/fetch/$s_!cYqn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F163c6c55-4c04-423e-a743-d4c27ac4f243_1200x630.png)](https://substackcdn.com/image/fetch/$s_!cYqn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F163c6c55-4c04-423e-a743-d4c27ac4f243_1200x630.png)

Yet they had only a tiny number of customers.

So they checked for new payments every hour through HTTP polling.

Imagine **polling** as repeatedly asking a server for updates.

But one day, their store became extremely popular because of a limited flash sale.

And they received a massive number of orders in a short period.

Although explosive growth is a good problem to have, they sent email notifications to customers extremely late because of delays in polling.

It was frustrating.

[![Polling vs Websockets](https://substackcdn.com/image/fetch/$s_!t5xW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F600ce589-0a77-41d8-87ef-4c931c9627d4_2170x1373.png)](https://substackcdn.com/image/fetch/$s_!t5xW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F600ce589-0a77-41d8-87ef-4c931c9627d4_2170x1373.png)Polling vs Websockets

So they set up websockets.

Think of the **websockets** as a way for the client and server to communicate in real-time, in both directions.

But it needs extra work because of connection management, server scaling, and monitoring. Thus increasing the operational efforts and resource usage.

They wanted a simple way to solve this problem.

So they set up webhooks.

Imagine **webhooks** as a way of sending messages to an external service when specific events happen.

Onward.

* * *

### **[Ship faster, with context-aware AI that speaks your language - Sponsor](https://refactoring.link/aug-sd)**

**[Augment Code](https://refactoring.link/aug-sd)** is the only AI coding agent built for real engineering teams.

It understands your codebase—across 10M+ lines, 10k+ files, and every repo in your stack—so it can actually help: writing functions, fixing CI issues, triaging incidents, and reviewing PRs.  
All from your IDE or terminal. No vibes. Just progress.

[Learn more about Augment](https://refactoring.link/aug-sd)

* * *

## How Do Webhooks Work

Let’s dive in:

### 1\. Webhook Components

A webhook isn’t a protocol, but a communication pattern.

It has 3 parts:

  * Sender: The system where the event happens (external payment service)

  * Event: The action that occurred (payment completion)

  * Receiver: The system that needs to know about the event (e-commerce store)

[![Webhook Components](https://substackcdn.com/image/fetch/$s_!6V9R!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F266c12b6-da62-4731-b118-344282531c5b_1652x734.png)](https://substackcdn.com/image/fetch/$s_!6V9R!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F266c12b6-da62-4731-b118-344282531c5b_1652x734.png)Webhook Components

The external payment service sends an event to the e-commerce store when the customer makes a successful payment.

And the e-commerce store then sends a confirmation email to the customer.

Ready for the best part?

### 2\. Webhook Workflow

They set up a separate API endpoint on the e-commerce store to handle webhook events. And registered its URL with the payment service.

[![Webhook Workflow](https://substackcdn.com/image/fetch/$s_!xj25!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F915527bd-1a84-4d1d-bb70-2dee4fe53e85_1652x504.png)](https://substackcdn.com/image/fetch/$s_!xj25!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F915527bd-1a84-4d1d-bb70-2dee4fe53e85_1652x504.png)Webhook Workflow

Here’s the webhook workflow:

  1. The user tries to buy something from the store

  2. The user then gets sent to the payment service

  3. The payment service creates a webhook event upon successful payment

  4. The payment service sends an HTTP POST request to the store’s webhook URL

  5. The store validates the payment and emails the customer to confirm the purchase

Also the store notifies the customer in case the payment is invalid.

* * *

Webhooks let systems communicate in real time via HTTP.

Yet there’s a risk of fake data or spam reaching the webhook endpoint as it’s a public API. So it’s necessary to set up extra [security](https://cheatsheetseries.owasp.org/cheatsheets/Web_Service_Security_Cheat_Sheet.html) using signature verification for safety.

Besides an event delivery could fail because of network issues. Because of this, it’s necessary to retry with [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff) until it succeeds.

But a retry might cause duplicate action, such as emailing the customer twice. So it’s necessary to set up the webhook API endpoint with [idempotency](https://newsletter.systemdesign.one/p/idempotent-api).

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋 Say hi on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 165K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-do-webhooks-work?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Does HTTPS Work 🔥](https://substackcdn.com/image/fetch/$s_!aXfg!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6c94ae9-610c-45c0-845b-781f15c15243_1280x720.png)How Does HTTPS Work 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·August 7, 2025[Read full story](https://newsletter.systemdesign.one/p/how-does-https-work)](https://newsletter.systemdesign.one/p/how-does-https-work)

[![How to Improve Availability Using Deployment Patterns ★](https://substackcdn.com/image/fetch/$s_!QwFP!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3bc05d4c-2244-4261-9c2e-62440ace9957_1280x720.png)How to Improve Availability Using Deployment Patterns ★[Neo Kim](https://substack.com/profile/135589200-neo-kim)·August 5, 2025[Read full story](https://newsletter.systemdesign.one/p/deployment-patterns)](https://newsletter.systemdesign.one/p/deployment-patterns)

* * *

### References

  * [Red Hat - What is a webhook?](https://www.redhat.com/en/topics/automation/what-is-a-webhook)

  * [Github - Webhooks documentation](https://docs.github.com/en/webhooks/about-webhooks)

  * [Stripe - Webhooks documentation](https://stripe.com/docs/webhooks)

  * [Azure - Webhooks](https://learn.microsoft.com/en-us/azure/devops/service-hooks/services/webhooks?view=azure-devops)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

106

6

10

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Mohammad Noushad Siddiqi's avatar](https://substackcdn.com/image/fetch/$s_!zkQr!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4822b70d-558a-4f00-9381-a6df2618ef95_1536x1024.webp)](https://substack.com/profile/378917615-mohammad-noushad-siddiqi?utm_source=comment)

[Mohammad Noushad Siddiqi](https://substack.com/profile/378917615-mohammad-noushad-siddiqi?utm_source=substack-feed-item)

[Aug 27](https://newsletter.systemdesign.one/p/how-do-webhooks-work/comment/149550257 "Aug 27, 2025, 6:36 AM")

I was always being confused about it but now it's quite clear. Thanks for your article. 

ReplyShare

[![Subtain Malik's avatar](https://substackcdn.com/image/fetch/$s_!z7Xw!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4b23929-ca30-48f4-b4ef-611d7c349972_969x969.jpeg)](https://substack.com/profile/24140515-subtain-malik?utm_source=comment)

[Subtain Malik](https://substack.com/profile/24140515-subtain-malik?utm_source=substack-feed-item)

[Aug 15](https://newsletter.systemdesign.one/p/how-do-webhooks-work/comment/145661388 "Aug 15, 2025, 12:27 PM")

Explained with quite interesting example <3 

ReplyShare

[4 more comments...](https://newsletter.systemdesign.one/p/how-do-webhooks-work/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture