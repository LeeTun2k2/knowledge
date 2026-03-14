[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Forward Proxy vs Reverse Proxy ✨

### #77: Break Into Proxy Servers (4 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jul 11, 2025

108

3

4

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the differences between a forward proxy and a reverse proxy. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/forward-proxy-vs-reverse-proxy/?action=share) & I'll send you some rewards for the referrals._

Imagine a family with a small kid on a tourist visit to Italy.

They visit a local restaurant for lunch.

The kid then tells her father that she needs 9 ice creams.

But the father orders only a single ice cream when the waiter arrives.

[![Forward Proxy Analogy](https://substackcdn.com/image/fetch/$s_!lGrj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1bab744-70ed-44a1-950e-e0695ab7cafe_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!lGrj!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1bab744-70ed-44a1-950e-e0695ab7cafe_1200x630.gif)

(The forward proxy works similarly.)

The kid is like the client, while the father is like the forward proxy.

The father filters the kid's request and speaks to the waiter in Italian on her behalf.

Likewise, the forward proxy filter requests, provides compatibility layers, and reduces unnecessary requests. This approach offers performance.

The waiter then passes their order to the kitchen expeditor. 

And he informs the right chef to prepare the ice cream.

[![Reverse Proxy Analogy](https://substackcdn.com/image/fetch/$s_!U3-X!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1da805ab-71b9-4249-96fa-a6dabcc26eeb_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!U3-X!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1da805ab-71b9-4249-96fa-a6dabcc26eeb_1200x630.gif)

(The reverse proxy works similarly.)

Think of the waiter as the internet layer.

And the kitchen expeditor is like the reverse proxy, while the chef is like the server.

The waiter doesn’t have to enter the kitchen or talk directly to the chef.

Likewise, a reverse proxy protects the server by avoiding direct exposure to the internet.

Onward.

* * *

### [“How to Adopt Externalized Authorization: Step-By-Step Roadmap” Ebook by Cerbos - Sponsor](https://solutions.cerbos.dev/how-to-adopt-externalized-authorization?utm_campaign=system_design_july_2025&utm_source=system_design&utm_medium=email&utm_content=&utm_term=)

[![Cerbos Ebook](https://substackcdn.com/image/fetch/$s_!XTcm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2bd3484c-9a21-406e-b2b1-191ead527cad_3429x1881.png)](https://solutions.cerbos.dev/how-to-adopt-externalized-authorization?utm_campaign=system_design_july_2025&utm_source=system_design&utm_medium=email&utm_content=&utm_term=)

Hardcoded authorization logic doesn't scale. As your application, requirements, and users grow - it turns into a bottleneck, slowing you down, creating security gaps, and making compliance a mess.

If you’re thinking about moving from hardcoded permissions to externalized authorization, this [10-step playbook](https://solutions.cerbos.dev/how-to-adopt-externalized-authorization?utm_campaign=system_design_july_2025&utm_source=system_design&utm_medium=email&utm_content=&utm_term=) will guide you:

  * Step-by-step adoption strategy from planning to PoC rollout

  * Frameworks, policy examples, code samples + 80 pages of in-depth content

  * Lessons from teams who've made the transition

If you're dealing with authorization complexity, this might save you some trial and error.

[Download free eBook](https://solutions.cerbos.dev/how-to-adopt-externalized-authorization?utm_campaign=system_design_july_2025&utm_source=system_design&utm_medium=email&utm_content=&utm_term=)

* * *

## Forward Proxy

The forward proxy sits between the client and the Internet.

Think of the forward proxy as a funnel; all traffic goes through it. Yet it checks whether a request is allowed.

Also it’s necessary to configure the client to use the forward proxy.

[![How Forward Proxy Works](https://substackcdn.com/image/fetch/$s_!es9B!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe077e338-15e1-4180-8218-63982319e59e_1448x710.png)](https://substackcdn.com/image/fetch/$s_!es9B!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe077e338-15e1-4180-8218-63982319e59e_1448x710.png)How Forward Proxy Works

Here’s **how it works** :

  1. The client sends a request to the forward proxy

  2. The forward proxy passes the request to the Internet

  3. The forward proxy receives the response from the server

  4. The forward proxy then passes it to the client

Schools and corporate networks often install a forward proxy to control the sites people can visit. And one can set up a forward proxy using Nginx or [Squid](https://www.squid-cache.org/).

Here are some popular **use cases** of the forward proxy:

  * Caching: It stores frequently accessed sites to reduce network bandwidth and latency. This improves the user experience in a shared network.

  * Request filtering: It blocks access to specific sites based on predefined policies.

  * Anonymity: It masks the client’s IP address for privacy and makes it difficult to track user activity.

Put simply, a forward proxy acts as a gateway to the Internet.

But a forward proxy could add latency as it introduces an extra network hop. Also it increases administrative overhead as client setup is necessary. So use it only if necessary.

Ready for the best part?

* * *

## Reverse Proxy

Both forward proxy and reverse proxy send client requests to the server.

But a forward proxy belongs to the client side, while a reverse proxy belongs to the server side. It means the difference between them is in the direction from which you look at them.

The reverse proxy sits between the Internet and the server.

[![How Reverse Proxy Works](https://substackcdn.com/image/fetch/$s_!AT1m!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa5680c2e-60f6-4865-81e2-f0edbb1cc1e3_1404x710.png)](https://substackcdn.com/image/fetch/$s_!AT1m!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa5680c2e-60f6-4865-81e2-f0edbb1cc1e3_1404x710.png)How Reverse Proxy Works

Here’s **how it works** :

  1. The client sends the request to the reverse proxy

  2. It forwards the request to the server

  3. The server responds to the reverse proxy

  4. The reverse proxy then forwards it to the client

The client interacts with the reverse proxy as if it’s the origin server. And one can set up a reverse proxy using [Nginx](https://nginx.org/) or [HAProxy](https://www.haproxy.org/).

Here are some popular **use cases** of the reverse proxy:

  * TLS termination: It [decrypts](https://en.wikipedia.org/wiki/TLS_termination_proxy) the incoming traffic. Thus freeing up server resources for performance.

  * Load balancing: It routes requests uniformly across servers for scale and reliability.

  * Security: It avoids direct server exposure to the internet. And reduces the risk of [DDoS](https://en.wikipedia.org/wiki/Denial-of-service_attack) by hiding its IP address. Also it drops unnecessary incoming traffic.

  * Caching: It stores static content, such as images, to reduce server load and latency.

  * A/B testing: It allows for testing a newer app version with a subset of users by routing traffic only to specific servers.

  * Authentication & Authorization: It verifies the client ID and checks if the client is allowed to perform a specific action.

Put simply, the reverse proxy acts as a gateway from the Internet.

But a reverse proxy adds operational complexity. Besides it could become a single point of failure without redundancy. So use it based on your needs and scale.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋 Find me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 160K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

**Neo’s recommendation 🚀**

Want instant code feedback and catch bugs quickly? CodeRabbit helps you by spotting bugs, providing one-click fix suggestions, and reviewing as you write code. Try CodeRabbit's [VS Code extension](http://coderabbit.link/bM7fHiE) for free.

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/forward-proxy-vs-reverse-proxy?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

**TL;DR** 🕰️

You can find a summary of this article [here](https://www.linkedin.com/posts/nk-systemdesign-one_forward-proxy-vs-reverse-proxy-explained-activity-7349401095359610881-urY2). Consider a repost if you find it helpful.

* * *

[![How Reddit Works 🔥](https://substackcdn.com/image/fetch/$s_!NUAh!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5736f335-13e1-4f99-8a16-bcd184a43ddb_1280x720.png)How Reddit Works 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·July 3, 2025[Read full story](https://newsletter.systemdesign.one/p/reddit-architecture)](https://newsletter.systemdesign.one/p/reddit-architecture)

[![Concurrency Is Not Parallelism 🔥](https://substackcdn.com/image/fetch/$s_!nk1q!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0fcfed3a-4c04-4759-b872-33dbfa050c54_1280x720.png)Concurrency Is Not Parallelism 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·June 18, 2025[Read full story](https://newsletter.systemdesign.one/p/concurrency-is-not-parallelism)](https://newsletter.systemdesign.one/p/concurrency-is-not-parallelism)

* * *

### References

  * [What is a Reverse Proxy Server? Learn How they Protect You](https://www.upguard.com/blog/what-is-a-reverse-proxy)

  * [What is a reverse proxy? | Proxy servers explained](https://www.cloudflare.com/en-gb/learning/cdn/glossary/reverse-proxy/)

  * [Using Nginx as a Forward Proxy](https://www.baeldung.com/nginx-forward-proxy)

  * [Creating a Forward Proxy Using Application Request Routing](https://learn.microsoft.com/en-us/iis/extensions/configuring-application-request-routing-arr/creating-a-forward-proxy-using-application-request-routing)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

108

3

4

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Jul 11](https://newsletter.systemdesign.one/p/forward-proxy-vs-reverse-proxy/comment/134192160 "Jul 11, 2025, 11:51 AM")

Liked by Neo Kim

My rule of thumb is: forward proxy hides the client, reverse proxy hides the server.

Thanks for the clear breakdown, Neo.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/forward-proxy-vs-reverse-proxy/comment/134192160)

[![Mike S.'s avatar](https://substackcdn.com/image/fetch/$s_!QVcv!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F652c93e1-b0f9-422f-acb9-9571ab660e00_144x144.png)](https://substack.com/profile/304888953-mike-s?utm_source=comment)

[Mike S.](https://substack.com/profile/304888953-mike-s?utm_source=substack-feed-item)

[Jul 12](https://newsletter.systemdesign.one/p/forward-proxy-vs-reverse-proxy/comment/134412526 "Jul 12, 2025, 12:24 AM")

Liked by Neo Kim

Thanks, Neo! Awesome illustrations as well.

ReplyShare

[1 more comment...](https://newsletter.systemdesign.one/p/forward-proxy-vs-reverse-proxy/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture