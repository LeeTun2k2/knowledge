[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Sidecar Pattern Works ✨

### #89: Break Into Design Patterns (4 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 18, 2025

82

4

6

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the sidecar pattern. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/sidecar-pattern/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, a single server was enough to run an entire site.

The clients connected to it over the internet.

[![sidecar pattern](https://substackcdn.com/image/fetch/$s_!SaFm!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3b7b30a-4ead-4213-aa97-7d7eb248543c_800x500.gif)](https://substackcdn.com/image/fetch/$s_!SaFm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3b7b30a-4ead-4213-aa97-7d7eb248543c_800x500.gif)

But as the internet grew, traffic to some sites skyrocketed.

And it became hard to scale those sites reliably.

So they installed more servers and added logic for logging, monitoring, and security.

Yet it became difficult to update the app’s code without affecting its reliability.

So they set up a **sidecar**.

It’s a design pattern. And runs a small service alongside the app to help with tasks such as logging, monitoring, or security.

Imagine a motorcycle with a sidecar. The driver steers the vehicle, while the sidecar passenger carries the map, radio, or bag.

Similarly, the sidecar pattern decouples the operational features from the app’s logic.

Onward.

* * *

### **[CodeRabbit: Free AI Code Reviews in CLI (Sponsor)](https://coderabbit.link/neo)**

[![CodeRabbit CLI](https://substackcdn.com/image/fetch/$s_!5LSQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0df5bd04-5caf-4644-bf13-41d11dbf2cb3_1999x1208.png)](https://coderabbit.link/neo)

**[CodeRabbit CLI](https://coderabbit.link/neo)** is an AI code review tool that runs directly in your terminal. It provides intelligent code analysis, catches issues early, and integrates seamlessly with AI coding agents like Claude Code, Codex CLI, Cursor CLI, and Gemini to ensure your code is production-ready before it ships.

  * Enables pre-commit reviews of both staged and unstaged changes, creating a multi-layered review process.

  * Fits into existing Git workflows. Review uncommitted changes, staged files, specific commits, or entire branches without disrupting your current development process.

  * Reviews specific files, directories, uncommitted changes, staged changes, or entire commits based on your needs.

  * Supports programming languages including JavaScript, TypeScript, Python, Java, C#, C++, Ruby, Rust, Go, PHP, and more.

  * Offers free AI code reviews with rate limits so developers can experience senior-level reviews at no cost.

  * Flags hallucinations, code smells, security issues, and performance problems.

  * Supports guidelines for other AI generators, AST Grep rules, and path-based instructions.

[Install CodeRabbit CLI](https://coderabbit.link/neo)

* * *

## Sidecar Pattern

Let’s dive in:

### 1\. Architecture

[![Sidecar Pattern Architecture](https://substackcdn.com/image/fetch/$s_!AGf1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F713be558-93a0-4905-8a68-90f2f0bdce7d_672x135.png)](https://substackcdn.com/image/fetch/$s_!AGf1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F713be558-93a0-4905-8a68-90f2f0bdce7d_672x135.png)Sidecar Pattern Architecture

Here’s how it works:

  * The app runs the core logic and handles requests.

  * Although detached, the sidecar and app share the same storage and networking environment.

  * Sidecar runs alongside the app, handling tasks such as logging, monitoring, and security.

  * They communicate with each other through a local network or shared resources, such as a configuration file or shared memory.

  * The sidecar and app start, stop, and scale together for reliability.

Also it’s possible to update or replace the sidecar without affecting the app.

There are 2 ways to deploy a sidecar:

  * As a separate container alongside the app.

  * As a separate process on the same server.

Yet the way a sidecar gets deployed depends on the use case and infrastructure setup.

Let’s keep going!

### 2\. Use Cases

Here are three popular use cases of the sidecar pattern:

**Traffic Proxy**

The sidecar acts as a traffic manager. It controls incoming and outgoing requests for the app.

[![Sidecar Pattern for Traffic Proxy](https://substackcdn.com/image/fetch/$s_!LSCl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F54795afd-8d36-4d9b-bebc-29fe4790ea39_576x190.png)](https://substackcdn.com/image/fetch/$s_!LSCl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F54795afd-8d36-4d9b-bebc-29fe4790ea39_576x190.png)Sidecar Pattern for Traffic Proxy

Here’s how:

  1. Imagine the app wants to call another service, for example, the payments API.

  2. It sends a request to the sidecar instead of calling the API directly.

  3. The sidecar then forwards the request to the correct API.

  4. It also automatically retries if the request fails due to a timeout or other error.

Besides the sidecar [decrypts](https://en.wikipedia.org/wiki/TLS_termination_proxy) the incoming traffic before sending it to the app.

This avoids the need for retry logic or security logic inside the app itself.

It’s also used to add [HTTPS](https://newsletter.systemdesign.one/p/how-does-https-work) support to legacy services.

A popular implementation of the sidecar traffic proxy is [Envoy](https://www.envoyproxy.io/). It usually gets deployed as a separate container alongside the app.

**Logging and Monitoring**

Log management and monitoring increase the app complexity. 

A sidecar solves this problem by collecting logs and sending them to a central system.

[![Sidecar Pattern for Logging and Monitoring](https://substackcdn.com/image/fetch/$s_!9l10!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a8a1b64-f30b-4798-aeb1-8afbca086235_653x256.png)](https://substackcdn.com/image/fetch/$s_!9l10!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a8a1b64-f30b-4798-aeb1-8afbca086235_653x256.png)Sidecar Pattern for Logging and Monitoring

Here’s how:

  1. The app writes logs to its local file, standard output, or a stream.

  2. Sidecar collects these logs from the app and combines them if needed.

  3. It then sends those logs to a central system, such as [Elasticsearch](https://www.elastic.co/elasticsearch) or [Splunk](https://www.splunk.com/).

It lets the developer view all logs in a single place without changing the app’s code.

A popular implementation of the sidecar for logging and monitoring is [Fluentd](https://www.fluentd.org/). It’s possible to deploy it as a separate process or container alongside the app.

**Security Management**

It’s unsafe for an app to store sensitive data, such as passwords or API tokens. 

A sidecar solves this problem by managing sensitive data for the app.

[![Sidecar Pattern for Security Management](https://substackcdn.com/image/fetch/$s_!N5YA!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5f330325-f84a-4283-b22e-db7a4c00d648_653x256.png)](https://substackcdn.com/image/fetch/$s_!N5YA!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5f330325-f84a-4283-b22e-db7a4c00d648_653x256.png)Sidecar Pattern for Security Management

Here’s how:

  1. The sidecar gets secrets, such as passwords or certificates, from a secure system.

  2. It then provides them to the app at runtime through a file, environment variable, or shared memory.

This technique keeps the secrets separate from the app’s code for security.

A popular implementation of the sidecar for security management is [Vault Agent](https://developer.hashicorp.com/vault/tutorials/vault-agent).

### 3\. Tradeoffs

Here’s how the sidecar pattern helps with a microservices architecture:

  * Sidecar handles extra tasks, while the microservice contains only business logic. Thus keeping it simple.

  * Each microservice has the same sidecar setup. Thus ensuring consistent logging, monitoring, or security features.

  * A sidecar usually runs outside the app. So it doesn’t matter if one microservice runs java and another runs python. Thus making it language independent.

Yet everything comes with a tradeoff, and the sidecar too.

Here are some of them:

  * It consumes extra CPU, memory, and network capacity.

  * It adds extra latency to each request.

  * It introduces the risk of hidden failures because a functional app may look broken just from a sidecar failure.

  * It increases operational complexity as it’s necessary to configure, monitor, and update the sidecar.

  * It brings synchronization challenges as the app and sidecar should start, stop, and scale together.

* * *

The sidecar pattern became popular with microservices architecture.

Yet a monolithic app can also use it to handle operational tasks for reliability.

The sidecar pattern is useful if the app runs in different languages and frameworks. But avoid using it if the app has resource limitations and needs fast communication. 

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

[Share](https://newsletter.systemdesign.one/p/sidecar-pattern?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

### References

  * [Sidecar pattern - Azure](https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar)

  * [Sidecar pattern - MOSN](https://mosn.io/en/docs/concept/sidecar-pattern/)

  * [Sidecar pattern - DZone](https://dzone.com/articles/sidecar-design-pattern-in-your-microservices-ecosy-1)

  * [Kubernetes Sidecars - A comprehensive guide - Plural](https://www.plural.sh/blog/kubernetes-sidecar-guide/)

  * [What's so bad about sidecars, anyway? - Cerbos](https://www.cerbos.dev/blog/whats-so-bad-about-sidecars-anyway)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

82

4

6

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Daniil Shykhov's avatar](https://substackcdn.com/image/fetch/$s_!8sse!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0d1016c4-45c4-4359-b3fe-885ad366b4b5_947x947.png)](https://substack.com/profile/256275965-daniil-shykhov?utm_source=comment)

[Daniil Shykhov](https://substack.com/profile/256275965-daniil-shykhov?utm_source=substack-feed-item)

[Sep 18](https://newsletter.systemdesign.one/p/sidecar-pattern/comment/157258479 "Sep 18, 2025, 11:31 AM")

Liked by Neo Kim

Clear, structured, and well-paced, exactly what a 4-minute design pattern lesson should be.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/sidecar-pattern/comment/157258479)

[![Giga's avatar](https://substackcdn.com/image/fetch/$s_!n1pv!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3bb11f1a-e295-4047-a620-004cba899f2d_4672x4672.jpeg)](https://substack.com/profile/211260250-giga?utm_source=comment)

[Giga](https://substack.com/profile/211260250-giga?utm_source=substack-feed-item)

[Sep 18](https://newsletter.systemdesign.one/p/sidecar-pattern/comment/157249229 "Sep 18, 2025, 10:49 AM")

Liked by Neo Kim

Quite well written, thanks. 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/sidecar-pattern/comment/157249229)

[2 more comments...](https://newsletter.systemdesign.one/p/sidecar-pattern/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture