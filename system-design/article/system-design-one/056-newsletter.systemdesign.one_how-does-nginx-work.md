[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Nginx Was Able to Support 1 Million Concurrent Connections on a Single Server ✨

### #62: Break Into Nginx Architecture (4 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Dec 04, 2024

208

5

16

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines Nginx architecture. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-does-nginx-work/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, there was a tech startup.

They offered invoice generation services.

Yet they had only a few users.

So they ran their site on a single web server - Apache HTTP.

Life was good.

[![How Does Nginx Work](https://substackcdn.com/image/fetch/$s_!SaFm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3b7b30a-4ead-4213-aa97-7d7eb248543c_800x500.gif)](https://substackcdn.com/image/fetch/$s_!SaFm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3b7b30a-4ead-4213-aa97-7d7eb248543c_800x500.gif)

But one day they started to receive massive traffic.

Yet their server had only limited capacity.

So they set up more servers.

Although it temporarily solved their scalability issues, there were newer problems. 

Here are some of them:

#### 1\. Resource Usage

A connection to the web server needs a separate [thread or process](https://stackoverflow.com/questions/200469/what-is-the-difference-between-a-process-and-a-thread).

Also consumes memory and CPU time until it’s released.

So computing resources get wasted.

#### 2\. Performance

A thread must wait until a _blocking task_ is complete.

Besides context switch increases with many threads.

Think of the **[context switch](https://en.wikipedia.org/wiki/Context_switch)** as switching a CPU between different threads.

So performance is bad.

Onward.

* * *

### [Eraser - Sponsor](https://app.eraser.io/auth/sign-up?utm_campaign=neo)

[![Eraser AI co-pilot](https://substackcdn.com/image/fetch/$s_!91Re!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F82e82808-dc13-4189-baeb-e8c1936af9d7_1278x300.png)](https://app.eraser.io/auth/sign-up?utm_campaign=neo)

[Eraser](https://www.eraser.io/) is an AI co-pilot for technical design that can help your team deliver accurate, consistent designs faster. Create beautiful technical diagrams in seconds from natural language prompts or code prompts (Terraform or SQL).

[Sign up for free](https://app.eraser.io/auth/sign-up?utm_campaign=neo)

* * *

## How Does Nginx Work

They wanted to reduce costs and ditch the complexity of many servers.

So they moved to Nginx.

And here’s how it offers extreme scalability:

### 1\. Parallelism

Running many tasks at the same time is called **parallelism**.

[![Master-Worker Model in Nginx](https://substackcdn.com/image/fetch/$s_!BsOq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3718afd-092e-48a8-ac24-a662ce0a20d0_818x501.png)](https://substackcdn.com/image/fetch/$s_!BsOq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3718afd-092e-48a8-ac24-a662ce0a20d0_818x501.png)Master-Worker Model in Nginx

They built Nginx with the master-worker model:

  * The master reads the configuration file and creates new workers.

  * While the worker handles client connections.

A simple approach is to give each connection a separate thread or process.

Yet it will result in context switches, thread trashing, and memory usage.

Imagine **[thread trashing](https://www.netdata.cloud/blog/understanding-context-switching-and-its-impact-on-system-performance/#:~:text=Thrashing%20is%20a%20phenomenon%20where,that%20can%20indicate%20its%20presence.)** as simply switching between threads instead of doing actual work.

So they run _each worker as a separate process_. And assign a separate CPU to each worker. Put simply, the number of workers equals the number of CPUs.

It gives better performance.

Ready for the best part?

### 2\. Concurrency

Managing many tasks at the same time is called **concurrency**. (Yet all tasks don't have to run at the same time.)

[![A Worker Processing Requests Using the Event Loop](https://substackcdn.com/image/fetch/$s_!On0v!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9a7acaf0-3b4b-4c07-bfdd-c3d1dcb2d196_1010x327.png)](https://substackcdn.com/image/fetch/$s_!On0v!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9a7acaf0-3b4b-4c07-bfdd-c3d1dcb2d196_1010x327.png)A Worker Processing Requests Using the Event Loop

They run the worker as a single-threaded process:

  * The client sends a request to the worker.

  * The request gets added to its event queue.

  * The worker processes the request using the event loop.

While event loop is asynchronous, event-driven, and non-blocking.

Think of the **[event loop](https://en.wikipedia.org/wiki/Event_loop)** as the chef in a busy kitchen - they prepare many orders by doing 1 task at a time. And switch between orders when ready.

[![A Worker Delegating Blocking Tasks to the Thread Pool](https://substackcdn.com/image/fetch/$s_!g4Ev!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff3069af1-1180-425c-85fc-eb9131078a68_988x268.png)](https://substackcdn.com/image/fetch/$s_!g4Ev!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff3069af1-1180-425c-85fc-eb9131078a68_988x268.png)A Worker Delegating a Blocking Task to the Thread Pool

A CPU-intensive task will keep the event loop busy. (Also block the worker from accepting new requests.)

So they _offload blocking tasks to the thread pool_.

Think of the **[thread pool](https://en.wikipedia.org/wiki/Thread_pool)** as a group of threads created at Nginx startup. And they're shared between workers.

[![Workflow of Processing a Blocking Request](https://substackcdn.com/image/fetch/$s_!IVSG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe685a66c-6133-4aa6-bd76-d35960c476df_984x247.png)](https://substackcdn.com/image/fetch/$s_!IVSG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe685a66c-6133-4aa6-bd76-d35960c476df_984x247.png)Workflow of Processing a Blocking Request

Here’s how it works:

  * A worker delegates a blocking task to the thread pool.

  * The worker then handles other requests using the event loop.

  * The thread pool tells the worker once the blocking task is complete.

  * The worker responds to the client.

This approach keeps the event loop responsive.

Ready for the next technique?

### 3\. Scalability

A simple way to scale is by caching responses.

Yet it’s inefficient to cache the same data across different workers.

So they set up **shared memory**.

[![Workers Accessing the Same Data via Shared Memory](https://substackcdn.com/image/fetch/$s_!f20J!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F48a3de8e-2d59-48de-a0f5-57ec1f2cc03a_792x464.png)](https://substackcdn.com/image/fetch/$s_!f20J!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F48a3de8e-2d59-48de-a0f5-57ec1f2cc03a_792x464.png)Workers Accessing the Same Data via Shared Memory

It let them:

  * Share cache data across workers.

  * Store session data and route requests to a specific backend server.

  * Rate limit by tracking requests.

Besides it reduces the memory usage of Nginx.

While workers use a lock (**[mutex](https://stackoverflow.com/questions/34524/what-is-a-mutex)**) to access shared memory.

* * *

A single Nginx server can handle 1 million concurrent connections.

Yet it isn’t a silver bullet to scalability.

So choose the right tool for your needs.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

#### Neo’s Recommendations

  * [Product for Engineers](https://newsletter.posthog.com?r=28q5ao): Actionable advice to improve your product skills as a software engineer.

  * [The Developing Dev](https://www.developing.dev/?r=28q5ao): Mentorship from a staff engineer at Instagram.

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a tech audience, you may want to [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-does-nginx-work?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Databases Keep Passwords Securely 🔒](https://substackcdn.com/image/fetch/$s_!o2OG!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0e082a6a-cc19-44d9-a90c-d18a5aa28d59_1280x720.gif)How Databases Keep Passwords Securely 🔒[Neo Kim](https://substack.com/profile/135589200-neo-kim)·November 18, 2024[Read full story](https://newsletter.systemdesign.one/p/how-to-store-passwords-in-database)](https://newsletter.systemdesign.one/p/how-to-store-passwords-in-database)

[![How Google Ads Was Able to Support 4.77 Billion Users With a SQL Database 🔥](https://substackcdn.com/image/fetch/$s_!xO_f!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc65080a9-b897-48b0-b638-f3951ae85ebb_1280x720.gif)How Google Ads Was Able to Support 4.77 Billion Users With a SQL Database 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·November 9, 2024[Read full story](https://newsletter.systemdesign.one/p/cloud-spanner-database)](https://newsletter.systemdesign.one/p/cloud-spanner-database)

* * *

### References

  * [Inside NGINX: How We Designed for Performance & Scale](https://blog.nginx.org/blog/inside-nginx-how-we-designed-for-performance-scale)

  * [The Powerful & Efficient NGINX Architecture](https://www.youtube.com/watch?v=i-8AISuZtN8)

  * [The Architecture of Open Source Applications (Volume 2) - nginx](https://aosabook.org/en/v2/nginx.html)

  * [Thread Pools in NGINX Boost Performance 9x!](https://www.f5.com/company/blog/nginx/thread-pools-boost-performance-9x)

  * [Tuning NGINX for Performance](https://www.f5.com/company/blog/nginx/tuning-nginx)

  * [Socket Sharding in NGINX Release](https://www.f5.com/company/blog/nginx/socket-sharding-nginx-release-1-9-1)

  * [Controlling nginx](https://nginx.org/en/docs/control.html)

  * [The C10K problem](http://www.kegel.com/c10k.html)

  * [Why does one NGINX worker take all the load?](https://blog.cloudflare.com/the-sad-state-of-linux-socket-balancing/)

  * [Does Nginx block on file IO?](https://forum.nginx.org/read.php?2,194884,194884)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

208

5

16

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Marcos F. Lobo 🗻🧭's avatar](https://substackcdn.com/image/fetch/$s_!7roK!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fff9211d7-f06d-4d11-b17c-4f1af3d2df5a_3264x1836.jpeg)](https://substack.com/profile/40136239-marcos-f-lobo?utm_source=comment)

[Marcos F. Lobo 🗻🧭](https://substack.com/profile/40136239-marcos-f-lobo?utm_source=substack-feed-item)

[Dec 4, 2024](https://newsletter.systemdesign.one/p/how-does-nginx-work/comment/80018938 "Dec 4, 2024, 1:50 PM")

Liked by Neo Kim

I didn't see the code of NGINX but, as a user, NGINX is pretty impressive.

Another one I love is HAProxy. I've used this for my services and the performance was amazing as well.

Thanks for this analysis Neo!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-does-nginx-work/comment/80018938)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Dec 4, 2024](https://newsletter.systemdesign.one/p/how-does-nginx-work/comment/80016725 "Dec 4, 2024, 1:34 PM")

Liked by Neo Kim

Nginx’s architecture rock—simple yet effective. A must-read for anyone scaling web servers.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-does-nginx-work/comment/80016725)

[3 more comments...](https://newsletter.systemdesign.one/p/how-does-nginx-work/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture