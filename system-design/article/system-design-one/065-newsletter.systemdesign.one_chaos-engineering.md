[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Netflix Uses Chaos Engineering to Create Resilience Systems 🐒

### #53: Break Into Netflix Engineering (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Aug 05, 2024

120

7

10

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _Chaos engineering is used to build resilient[1](https://newsletter.systemdesign.one/p/chaos-engineering#footnote-1-146865156) distributed systems. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/chaos-engineering/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, Netflix offered DVD rentals via mail.

Yet their growth rate was limited.

So they pivoted to create a streaming service.

And set up a _monolith_ tech stack.

Although explosive growth is a good problem, scaling their infrastructure became difficult.

[![Chaos Engineering](https://substackcdn.com/image/fetch/$s_!Qjus!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43e8f751-e400-4972-8f4e-35b39590ac59_800x500.gif)](https://substackcdn.com/image/fetch/$s_!Qjus!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43e8f751-e400-4972-8f4e-35b39590ac59_800x500.gif)

So they set up _microservices_.

But it created newer problems.

Here are some of them:

#### 1\. Reliable Network

Computer networks suffer from:

  * Latency issues

  * Network failures

  * Bandwidth limitations

So there’s a risk of communication failure between services.

#### 2\. Resilience

The resilience of a distributed system depends on its weakest component. 

While weakest component is usually found after a failure - thus affecting users.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Chaos Engineering

They wanted to manage the chaos present in distributed systems.

So they created chaos engineering.

Here’s how:

### 1\. Implementation

A system with 0 downtime doesn’t exist.

But downtime can be minimized via automation. 

So they _proactively_ find potential failures during office hours & then _automate_ the fix. This means failure gets fixed quickly via automation if it occurs again. Observing a distributed system's behavior in a controlled experiment is called **chaos engineering**. 

It helps to find a problem before it causes a production outage.

[![Chaos Monkey Turning off Traffic to the Main Database](https://substackcdn.com/image/fetch/$s_!uThu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F39ec032b-56fa-4230-87ff-ae3e66b92a6d_800x500.gif)](https://substackcdn.com/image/fetch/$s_!uThu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F39ec032b-56fa-4230-87ff-ae3e66b92a6d_800x500.gif)Chaos Monkey Turning off Traffic to the Main Database

They created a tool to _randomly_ shut down servers and called it **[chaos monkey](https://netflix.github.io/chaosmonkey/)** :

  * It gets information about available servers via the continuous delivery platform

  * It interacts via the continuous delivery platform to shut down servers

Then they check if traffic gets routed to another server without affecting users.

[![A Systematic Approach to Chaos Engineering](https://substackcdn.com/image/fetch/$s_!ydum!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F755e30af-c439-4603-a902-f2326e20c4dd_1313x809.png)](https://substackcdn.com/image/fetch/$s_!ydum!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F755e30af-c439-4603-a902-f2326e20c4dd_1313x809.png)Systematic Approach to Chaos Engineering

Here’s how they do chaos engineering:

  * Create a hypothesis about how the system will behave during a failure

  * Run a small test to introduce failure - switch off the server or change the network configuration

  * Observe the system’s behavior & measure the failure impact

  * Automate fix for the problem

Then _rerun_ the test to check if the automated fix works as expected.

They run chaos engineering in _production_ traffic for accuracy. 

Yet blast radius[2](https://newsletter.systemdesign.one/p/chaos-engineering#footnote-2-146865156) must be controlled to avoid affecting users. Here’s how:

  * Prepare for the worst case with a backup plan

  * Use feature flags to roll back changes quickly if things go wrong

  * Run tests in pre-production before production

  * Run only small tests first & then scale

  * Introduce one chaos variable at a time - don’t break everything together 

Besides they measure test impact properly to prevent unnecessary damage.

### 2\. Principles

[![Principles of Chaos Engineering](https://substackcdn.com/image/fetch/$s_!n492!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa9a1bec2-256a-4299-8c2d-a41a44eec0cd_1313x809.png)](https://substackcdn.com/image/fetch/$s_!n492!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa9a1bec2-256a-4299-8c2d-a41a44eec0cd_1313x809.png)Principles of Chaos Engineering

They created chaos engineering around these [principles](https://principlesofchaos.org/):

  * Automate tests to save cost & time

  * Run tests in production for the same traffic pattern & reliable results

  * Run tests with events based on potential impact & frequency - server crash, wrong API response, traffic spike

  * Focus on measurable output to check if the system works - throughput, latency

  * Control & minimize blast radius

It gave them more confidence to experiment & get better results.

### 3\. Use Cases

[![Chaos Engineering Use Cases](https://substackcdn.com/image/fetch/$s_!PBbC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03fdbc9a-d691-4fe7-a940-73dbe5a8daa9_1313x809.png)](https://substackcdn.com/image/fetch/$s_!PBbC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03fdbc9a-d691-4fe7-a940-73dbe5a8daa9_1313x809.png)Chaos Engineering Use Cases

Here’s what they do with chaos engineering:

  * Reduce the number of failures

  * Improve system availability - 99.9%

  * Find potential failures & automate the fix

  * Check if failover mechanisms work as expected

  * Find system bottlenecks & [single points of failure](https://en.wikipedia.org/wiki/Single_point_of_failure#:~:text=A%20single%20point%20of%20failure,application%2C%20or%20other%20industrial%20system.)

  * Check if data backup & restoration work as expected

  * Check how the system responds to service dependency failures

  * Do better [capacity planning](https://systemdesign.one/back-of-the-envelope/) by studying the system’s response to various traffic

  * Check if the system recovers from failures quickly - mean time to resolution (**MTTR**)

Besides they created chaos monkey [variants](https://netflixtechblog.com/the-netflix-simian-army-16e57fbab116) to handle more use cases.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

They wrote chaos monkey in Go and open-sourced it.

While Netflix has only a few minutes of downtime per year.

And remains the world’s largest streaming service. 

This case study shows testing is important to build complex systems & move faster.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/chaos-engineering?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Meta Achieves 99.99999999% Cache Consistency 🎯](https://substackcdn.com/image/fetch/$s_!q3F9!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febed29c8-85c5-4113-8c84-641989c14fea_1280x720.gif)How Meta Achieves 99.99999999% Cache Consistency 🎯[Neo Kim](https://substack.com/profile/135589200-neo-kim)·July 18, 2024[Read full story](https://newsletter.systemdesign.one/p/cache-consistency)](https://newsletter.systemdesign.one/p/cache-consistency)

[![How Halo Scaled to 11.6 Million Users Using the Saga Design Pattern 🎮](https://substackcdn.com/image/fetch/$s_!mj67!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F67b350c1-3475-4192-a11d-45c685f7ea4f_1280x720.gif)How Halo Scaled to 11.6 Million Users Using the Saga Design Pattern 🎮[Neo Kim](https://substack.com/profile/135589200-neo-kim)·July 4, 2024[Read full story](https://newsletter.systemdesign.one/p/saga-design-pattern)](https://newsletter.systemdesign.one/p/saga-design-pattern)

* * *

### References

  * [The Netflix Simian Army](https://netflixtechblog.com/the-netflix-simian-army-16e57fbab116)

  * [Netflix Chaos Monkey Upgraded](https://netflixtechblog.com/netflix-chaos-monkey-upgraded-1d679429be5d)

  * [Chaos Engineering Upgraded](https://netflixtechblog.com/chaos-engineering-upgraded-878d341f15fa)

  * [Principles of Chaos Engineering](https://principlesofchaos.org/)

  * [Chaos Monkey Documentation](https://netflix.github.io/chaosmonkey/)

  * [AWS re: Invent 2020: Testing resiliency using chaos engineering](https://www.youtube.com/watch?v=OlobVYPkxgg)

  * [AWS re: Invent 2022 - The evolution of chaos engineering at Netflix](https://www.youtube.com/watch?v=Xbn65E-BQhA)

  * [Understanding Chaos Engineering](https://www.youtube.com/watch?v=mfEMXKSFtaQ)

  * [Kolton Andrus on Breaking Things at Netflix](https://www.infoq.com/interviews/kolton-andrus-on-breaking-things-at-netflix/)

  * [Chaos Engineering: the history, principles, and practice](https://www.gremlin.com/community/tutorials/chaos-engineering-the-history-principles-and-practice)

  * [What is Chaos Engineering?](https://www.dynatrace.com/news/blog/what-is-chaos-engineering/)

  * [NAB deploys Chaos Monkey to kill servers 24/7](https://www.itnews.com.au/news/nab-deploys-chaos-monkey-to-kill-servers-24-7-382285)

  * [Eight Fallacies Of Distributed Computing](https://wiki.c2.com/?EightFallaciesOfDistributedComputing)

  * [How to Get Started with Chaos Engineering](https://www.gremlin.com/blog/how-to-get-started-with-chaos-engineering)

  * [4 Chaos Experiments to Start With](https://www.gremlin.com/community/tutorials/4-chaos-experiments-to-start-with)

  * [Microservices Lessons From Netflix](https://newsletter.systemdesign.one/p/netflix-microservices)

  * [Mastering Chaos - A Netflix Guide to Microservices](https://www.infoq.com/presentations/netflix-chaos-microservices/)

  * [Diagram tracking chaos tools & engineers](https://coggle.it/diagram/WiKceGDAwgABrmyv/t/chaos-engineeringcompanies%2C-people%2C-tools-practices/0a2d4968c94723e48e1256e67df51d0f4217027143924b23517832f53c536e62)

  * [Images inspired by visualize value](https://visualizevalue.com/)

[1](https://newsletter.systemdesign.one/p/chaos-engineering#footnote-anchor-1-146865156)

**Resilience** is the ability of a system to recover from failures

[2](https://newsletter.systemdesign.one/p/chaos-engineering#footnote-anchor-2-146865156)

**Blast radius** is the area of a system affected during a test

120

7

10

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Salvador Lorca 📚's avatar](https://substackcdn.com/image/fetch/$s_!qAeh!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5a7be70f-d4c0-46ee-981b-3b682b184bc2_86x86.webp)](https://substack.com/profile/172879528-salvador-lorca?utm_source=comment)

[Salvador Lorca 📚](https://substack.com/profile/172879528-salvador-lorca?utm_source=substack-feed-item)

[Aug 6, 2024](https://newsletter.systemdesign.one/p/chaos-engineering/comment/64527841 "Aug 6, 2024, 6:30 AM")

Liked by Neo Kim

Good insight!!!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/chaos-engineering/comment/64527841)

[![Andrés Álvarez Iglesias's avatar](https://substackcdn.com/image/fetch/$s_!UQoJ!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F80c89686-cae1-4d0a-9c12-a3050af693e9_1715x1716.jpeg)](https://substack.com/profile/118619671-andres-alvarez-iglesias?utm_source=comment)

[Andrés Álvarez Iglesias](https://substack.com/profile/118619671-andres-alvarez-iglesias?utm_source=substack-feed-item)

[Aug 5, 2024](https://newsletter.systemdesign.one/p/chaos-engineering/comment/64450201 "Aug 5, 2024, 3:10 PM")

Liked by Neo Kim

Great article!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/chaos-engineering/comment/64450201)

[5 more comments...](https://newsletter.systemdesign.one/p/chaos-engineering/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture