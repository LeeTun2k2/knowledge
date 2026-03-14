[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How API Gateway Actually Work ⭐

### #73: Break Into API Gateway (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 29, 2025

167

6

11

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how API Gateway_ _works. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-api-gateway-works/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there lived a software engineering student named Maya.

She worked as a freelancer part-time.

Although she had many customers, the platform fee was extremely high.

So she got paid less.

[![How api gateway works](https://substackcdn.com/image/fetch/$s_!KVXb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7c9bc2e9-c5b7-4774-808c-0fc92cece4a5_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!KVXb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7c9bc2e9-c5b7-4774-808c-0fc92cece4a5_1200x630.gif)

One day, she decided to build a freelancer site with fair pricing.

And her tiny site became popular in a short time.

So she set up a microservices architecture for scalability.

Yet she didn’t know much about architectural design patterns.

And set up separate public URLs for each microservice.

The client talked directly to different microservices based on the task.

This means tight coupling and increased client complexity.

[![Different Client Types Receive the Same Payload](https://substackcdn.com/image/fetch/$s_!aFuU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa73dde7f-58b6-4182-9a5b-ac27f11e303c_773x448.png)](https://substackcdn.com/image/fetch/$s_!aFuU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa73dde7f-58b6-4182-9a5b-ac27f11e303c_773x448.png)Different Clients Receive the Same Payload

Also many users accessed her site on their mobile. 

Yet she sent the same amount of information to the desktop and mobile users.

And this worsened the latency and bandwidth usage.

So she set up an API Gateway.

Imagine the **API Gateway** as a hotel receptionist who checks a user’s reservation and gives them room keys.

It let her move the non-business logic, such as authorization, into a separate service.

Onward.

* * *

### [Cut Code Review Time & Bugs in Half - Sponsor](https://coderabbit.link/neo-kim)

Code reviews are critical but time-consuming. CodeRabbit acts as your AI co-pilot, giving you instant code review comments and the potential impact of each pull request.

Besides, CodeRabbit provides one-click fix suggestions. It also lets you define custom code quality rules using AST Grep patterns and catch subtle issues that traditional static analysis tools might miss.

CodeRabbit has reviewed over 10 million PRs; it's installed on 1 million repositories, and 70k+ open-source projects use it. CodeRabbit is free for all open-source repos.

[![CodeRabbit](https://substackcdn.com/image/fetch/$s_!G7Th!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4dba5da7-30fb-4c08-9e4a-0ae57be1eb6c_1600x800.png)](https://coderabbit.link/neo-kim)

**Instantly spot:**

  * Syntax & functional bugs

  * Logical errors (incorrect conditions, miscalculations)

  * Common pitfalls (off-by-one, infinite loops)

  * Concurrency issues (data races, deadlocks)

  * Security vulnerabilities (SQL injection, XSS, CSRF)

  * Code smells (duplication, lengthy methods)

  * Best practices violations (SOLID, DRY, KISS)

  * Poor unit test coverage

  * Complexity issues (time & space inefficiencies)

  * Weak error handling (especially external calls)

  * Maintainability & readability concerns

Writing clean, secure, and performant code is tough. CodeRabbit makes it easy.

[Get Started Today](https://coderabbit.link/neo-kim)

* * *

## How API Gateway Works

Let’s dive in:

### 1\. Workflow

The API Gateway acts as a single entry point for the site.

[![SSL Termination on API Gateway](https://substackcdn.com/image/fetch/$s_!4SEP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb185fce2-0275-47a9-91ca-6f3be3fff61c_796x171.png)](https://substackcdn.com/image/fetch/$s_!4SEP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb185fce2-0275-47a9-91ca-6f3be3fff61c_796x171.png)SSL Termination on API Gateway

The client sends the request over [HTTPS](https://www.cloudflare.com/en-gb/learning/ssl/what-is-https/) for security.

Yet it has to be decrypted, and this takes extra processing power on each server.

So the API Gateway does **[SSL termination](https://www.vmware.com/topics/ssl-termination)**. This means decrypting traffic before forwarding it to microservices, thus reducing server load.

[![Routing Requests Using API Gateway](https://substackcdn.com/image/fetch/$s_!jzIQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2c20d45a-30bb-4ea3-94c1-5d8480e373b1_822x404.png)](https://substackcdn.com/image/fetch/$s_!jzIQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2c20d45a-30bb-4ea3-94c1-5d8480e373b1_822x404.png)Routing Requests Using API Gateway

Here’s how the API Gateway routes the request:

  1. The client sends the request to the API Gateway

  2. The API Gateway does [rate limiting](https://www.cloudflare.com/en-gb/learning/bots/what-is-rate-limiting/) to prevent server overload

  3. It then checks if the client is allowed to make the request

  4. The API Gateway validates the request’s header and body against the schema. Also transform the request if necessary

  5. It routes the request to the correct microservices. It handles routing based on the request’s URL path, HTTP headers, method, or query parameters

  6. The API Gateway then combines the responses from different microservices

  7. It responds to the client and caches the response for future requests if needed

Also it finds the device type, such as desktop or mobile, from HTTP headers to route the request accordingly. This approach simplifies the client logic and improves latency.

Besides the API Gateway prevents overloading of unhealthy servers by pausing repeated failing requests. This technique is called the **[circuit breaker pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker)**.

Let’s keep going!

### 2\. Tradeoffs

Although the API Gateway simplifies client interactions, it introduces a set of problems.

Here are some of them:

  * It increases latency as there’s an extra network hop

  * It increases costs and operational complexity because of maintenance efforts

  * It might become a performance bottleneck when there’s high traffic

Also it could become a _single point of failure_ if set up incorrectly. So it’s necessary to install more instances of the API Gateway for high availability.

[![Installing More Than a Single Instance of API Gateway for High Availability](https://substackcdn.com/image/fetch/$s_!xJ1F!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd18335ae-4900-4f37-8873-1a84152fefb8_791x431.png)](https://substackcdn.com/image/fetch/$s_!xJ1F!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd18335ae-4900-4f37-8873-1a84152fefb8_791x431.png)Running More Than a Single Instance of API Gateway for High Availability

* * *

Some popular ways to set up an API Gateway are using [Nginx](https://nginx.org/), [Kong](https://konghq.com/), or [Tyk](https://tyk.io/).

A popular variant of the API Gateway is the backend for frontend (**[BFF](https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends)**) pattern. It means a separate API Gateway for each device type—desktop and mobile.

While the API Gateway pattern offers many benefits, it’s important to use it carefully. Otherwise it’ll add more complexity than value.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://substackcdn.com/image/fetch/$s_!bEFk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)**👋 Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 150K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-api-gateway-works?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

**TL;DR** 🕰️

You can find a summary of the article [here](https://www.linkedin.com/posts/nk-systemdesign-one_give-me-2-minsill-teach-you-how-api-gateway-activity-7333816910720999428-mHbA). Consider a repost if you find it helpful.

* * *

[![How Load Balancing Algorithms Really Work ⭐](https://substackcdn.com/image/fetch/$s_!Agws!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdf63a17b-ea6d-4f14-9a1f-d571bf065f4c_1280x720.png)How Load Balancing Algorithms Really Work ⭐[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 19, 2025[Read full story](https://newsletter.systemdesign.one/p/load-balancing-algorithms)](https://newsletter.systemdesign.one/p/load-balancing-algorithms)

[![How JWT Works ✨](https://substackcdn.com/image/fetch/$s_!Xv0p!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5b48f3f4-fdda-4da8-9eb3-288289bb9475_1280x720.png)How JWT Works ✨[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 10, 2025[Read full story](https://newsletter.systemdesign.one/p/how-jwt-works)](https://newsletter.systemdesign.one/p/how-jwt-works)

* * *

### References

  * [API Gateway Pattern](https://microservices.io/patterns/apigateway.html)

  * [Use API gateways in microservices](https://learn.microsoft.com/en-us/azure/architecture/microservices/design/gateway)

  * [10 most common use cases of an API Gateway](https://apisix.apache.org/blog/2022/10/27/ten-use-cases-api-gateway/)

  * [My experiences with API gateways](https://mahesh-mahadevan.medium.com/my-experiences-with-api-gateways-8a93ad17c4c4)

  * [What does an API gateway do?](https://www.redhat.com/en/topics/api/what-does-an-api-gateway-do)

  * [What Is an API Gateway?](https://www.f5.com/glossary/api-gateway)

  * [Embracing the Differences: Inside the Netflix API Redesign](https://netflixtechblog.com/embracing-the-differences-inside-the-netflix-api-redesign-15fd8b3dc49d)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

167

6

11

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[May 29, 2025](https://newsletter.systemdesign.one/p/how-api-gateway-works/comment/121110920 "May 29, 2025, 12:08 PM")

Liked by Neo Kim

What a critical piece of modern architecture, Neo. 

API Gateways also help enforce security policies and centralize logging/monitoring, which is huge for debugging and compliance.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-api-gateway-works/comment/121110920)

[![Petar Ivanov's avatar](https://substackcdn.com/image/fetch/$s_!Hqxs!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb236a7ab-735e-49d2-bbe8-98b1f901b169_500x500.jpeg)](https://substack.com/profile/10269058-petar-ivanov?utm_source=comment)

[Petar Ivanov](https://substack.com/profile/10269058-petar-ivanov?utm_source=substack-feed-item)

[May 29, 2025](https://newsletter.systemdesign.one/p/how-api-gateway-works/comment/121151163 "May 29, 2025, 2:38 PM")

Liked by Neo Kim

API Gateways are a great addition to any backend since they come with many advantages, like rate limiting and throttling, as you mentioned.

Great article, Neo! 🙌 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-api-gateway-works/comment/121151163)

[4 more comments...](https://newsletter.systemdesign.one/p/how-api-gateway-works/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture