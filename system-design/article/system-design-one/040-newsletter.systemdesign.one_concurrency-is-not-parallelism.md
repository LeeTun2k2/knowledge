[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Concurrency Is Not Parallelism 🔥

### #75: Break Into Concurrency vs Parallelism (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jun 18, 2025

∙ Paid

142

3

13

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the differences between concurrency and parallelism. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/concurrency-is-not-parallelism/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, computers had a single CPU and could run only one task at a time.

Still it completed tasks faster than humans.

[![concurrency is not parallelism](https://substackcdn.com/image/fetch/$s_!QGoS!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6210089d-f8ce-4f01-bac7-019b0459dab9_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!QGoS!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6210089d-f8ce-4f01-bac7-019b0459dab9_1200x630.gif)

Yet the system wasted CPU time waiting for input-output (**I/O**) responses, such as keyboard input. 

And the system became slow and non-responsive as the number of tasks grew.

So they set up a scheduler and broke down each task into smaller parts.

[![A CPU Running Concurrent Tasks](https://substackcdn.com/image/fetch/$s_!2WjV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb3cc44b2-6ea6-4d12-ad2c-151c13108df4_1538x360.png)](https://substackcdn.com/image/fetch/$s_!2WjV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb3cc44b2-6ea6-4d12-ad2c-151c13108df4_1538x360.png)A CPU Running Concurrent Tasks

The scheduler assigns a specific time for processing each task and then moves to another. Put simply, the CPU runs those micro tasks in an interleaved order.

The technique of switching between different tasks at once is called **concurrency**.

But concurrency only creates an illusion of parallel execution. 

The performance worsens when the number of tasks increases. Because each task has to wait for its turn, causing an extra delay.

[![CPU Cores Running Tasks in Parallel](https://substackcdn.com/image/fetch/$s_!JWFF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b90a060-17d7-4198-a957-90a3e450d49e_1526x683.png)](https://substackcdn.com/image/fetch/$s_!JWFF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b90a060-17d7-4198-a957-90a3e450d49e_1526x683.png)CPU Cores Running Tasks in Parallel

So they added extra CPU cores. And each CPU core could run a task independently.

Think of the **CPU core** as a processing unit within the CPU; it handles instructions.

The technique of handling many tasks at once is called **parallelism.** It offers efficiency.

Onward.

* * *

### [Cut Code Review Time and Bugs in Half — Sponsor](https://coderabbit.link/neo-kim)

[![CodeRabbit](https://substackcdn.com/image/fetch/$s_!Fz-O!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F93e12584-238b-40d0-9cfa-eebf6569fc38_1600x800.png)](https://coderabbit.link/neo-kim)

[CodeRabbit](https://coderabbit.link/neo-kim) is your AI-powered code review co-pilot. 

It gives you instant, contextual feedback on every PR and one-click fix suggestions. It has reviewed over 10 million PRs and is used in 1 million repositories. 

Besides, it’s free to use for open-source repos; 70K+ open-source projects already use it.

CodeRabbit lets you instantly spot:

  * Logic & syntax bugs

  * Security issues (XSS, SQL injection, CSRF)

  * Concurrency problems (deadlocks, race conditions)

  * Code smells & readability concerns

  * Best practice violations (SOLID, DRY, KISS)

  * Weak test coverage & performance bottlenecks

Write clean, secure, and performant code with CodeRabbit.

[Try CodeRabbit Now](https://coderabbit.link/neo-kim)

* * *

## Concurrency Is Not Parallelism

Concurrency is about the design, while parallelism is about the execution model.

Let’s dive in:

### 1\. Concurrency vs Parallelism

[![Concurrency vs Parallelism](https://substackcdn.com/image/fetch/$s_!uzP5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3e474ff7-6fb5-443f-9e08-36cadaacf48b_1200x630.png)](https://substackcdn.com/image/fetch/$s_!uzP5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3e474ff7-6fb5-443f-9e08-36cadaacf48b_1200x630.png)Concurrency vs Parallelism

Imagine **concurrency** as a chef making doner kebab for 3 customers. He switches between fries, meat, and vegetables. 

And consider **parallelism** as 3 chefs making doner kebab for 3 customers; all done at the same time.[1](https://newsletter.systemdesign.one/p/concurrency-is-not-parallelism#footnote-1-164937181)

The CPU-intensive tasks run efficiently with parallelism.

Yet parallelism wastes CPU time when there are I/O heavy tasks.

Also running tasks on different CPUs doesn’t always speed them up. Because coordinating tasks for independent execution is necessary.

[![Concurrency + Parallelism = High Throughput](https://substackcdn.com/image/fetch/$s_!nB_9!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe558d281-51b6-4baa-8bfb-5903aeefc4c1_1543x683.png)](https://substackcdn.com/image/fetch/$s_!nB_9!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe558d281-51b6-4baa-8bfb-5903aeefc4c1_1543x683.png)Concurrency + Parallelism = High Throughput

So combine concurrency with parallelism to build efficient, scalable, and responsive systems.

Besides simultaneous changes to the same data might corrupt it.

Yet it’s difficult to determine when a thread will get scheduled or which instructions it’ll run.

So it’s necessary to use synchronization techniques, such as locking.

Think of a **thread** as the smallest processing unit scheduled by the operating system.

[![Synchronizing CPU Access Using Locks](https://substackcdn.com/image/fetch/$s_!spso!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa40fb162-a366-41b2-9e73-03d9ba4d7a6d_1451x451.png)](https://substackcdn.com/image/fetch/$s_!spso!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa40fb162-a366-41b2-9e73-03d9ba4d7a6d_1451x451.png)Synchronizing CPU Access Using Locks

A thread must acquire the lock to do a set of operations. While another thread can use the CPU only after the current thread releases the lock.

_The 2 popular synchronization primitives are mutex and semaphore_. They’re used to coordinate access to a shared resource.

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fconcurrency-is-not-parallelism&utm_source=paywall&utm_medium=web&utm_content=164937181)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fconcurrency-is-not-parallelism&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture