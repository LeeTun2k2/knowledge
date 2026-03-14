[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Amazon Frugal Architecture Explained 💰

### #56: And How to Properly Scale Your Business (5 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 15, 2024

108

2

13

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

I’m writing you this letter because cost is often ignored during system design.

Yet I think good architectural decisions should always consider costs.

Otherwise even a business with a solid tech stack would fail.

  * _[Share this post](https://newsletter.systemdesign.one/p/frugal-architecture/?action=share) & I'll send you some rewards for the referrals._

_This post outlines Amazon’s frugal architecture principles via a**hypothetical case study**. You will find references at the bottom of this page if you want to go deeper._

* * *

Once upon a time, there lived 2 software engineers.

They worked for a tech company - Hooli.

Although they were smart, they never got promoted.

So they were sad and frustrated.

[![Frugal Architecture](https://substackcdn.com/image/fetch/$s_!aL-W!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F218fa179-f527-40de-9e84-64f97cf70e3b_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!aL-W!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F218fa179-f527-40de-9e84-64f97cf70e3b_1200x630.gif)

Until one day when they had a wild idea to build a startup.

And posted about it on social media to find potential users - positive results. 

They created the app prototype based on their assumptions from intense brainstorming sessions. 

Yet there was a business risk due to high costs.

So they used Amazon’s [frugal architecture](https://thefrugalarchitect.com/) as guiding principles.

* * *

### [This Post Summary - Instagram](https://www.instagram.com/systemdesignone/)

I wrote a summary of this post:

(Save it for later.)

[systemdesignone](https://instagram.com/systemdesignone)

[![](https://substackcdn.com/image/fetch/$s_!_VEc!,w_640,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F__ss-rehost__IG-meta-C_74EtENKwh.jpg)](https://instagram.com/p/C_74EtENKwh)

A post shared by [@systemdesignone](https://instagram.com/systemdesignone)

And I’d love to connect if you’re on Instagram:

[Follow Instagram](https://www.instagram.com/systemdesignone/)

* * *

## Frugal Architecture

Here’s how:

### 1\. The 80-20 Rule

Their minimum viable product (**MVP**) became expensive from feature creep.

**Feature creep** occurs when extra features are added to the product than necessary.

[![Deliver Only Essential Features to Reduce Build + Operational Costs](https://substackcdn.com/image/fetch/$s_!iNYD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fafd052af-a639-41e9-93f0-3b4b2aabbaa7_1930x915.png)](https://substackcdn.com/image/fetch/$s_!iNYD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fafd052af-a639-41e9-93f0-3b4b2aabbaa7_1930x915.png)Deliver Only Essential Features to Reduce Build + Operational Costs

So they did user surveys to find the essential features. (Also asked the right questions using [the mom test](https://www.momtestbook.com/).)

And delivered only the necessary features to reduce costs.

**The bottom line** : consider cost as a non-functional requirement in system design. (It’ll improve system efficiency.)

### 2\. Scaling Without Breaking

They added servers as the app became popular.

[![Profit = Revenue - Cost](https://substackcdn.com/image/fetch/$s_!ZLf_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6eeb868-406d-4d97-a2f1-12ffd9363272_937x730.png)](https://substackcdn.com/image/fetch/$s_!ZLf_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa6eeb868-406d-4d97-a2f1-12ffd9363272_937x730.png)Profit = Revenue - Cost

But infrastructure costs skyrocketed as more users joined.

And profit per user went down.

This could destroy their business.

So they set up autoscaling. It automatically scales up and down the infrastructure based on traffic. Thus reducing infrastructure and operational costs.

Imagine **autoscaling** as getting extra chairs when more people want to sit. And putting those chairs away when they leave.

Also they archived inactive data to save storage costs.

And set up rate-limiting to avoid unnecessary traffic.

**The bottom line** : architect the system so costs align with the business model.

### 3\. You Can’t Control What You Don’t Measure

Their operational costs were still high.

[![Cost Distribution in a Typical App Lifecycle](https://substackcdn.com/image/fetch/$s_!N7Wn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa0f8e874-5400-4c17-af58-c5a106dc67c5_751x748.png)](https://substackcdn.com/image/fetch/$s_!N7Wn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa0f8e874-5400-4c17-af58-c5a106dc67c5_751x748.png)Cost Distribution in a Typical App Lifecycle

So they set up monitoring for important metrics.

And tracked resource usage + errors.

It let them:

  * Find wasteful practices

  * Assign resources based on priorities

  * Avoid unnecessary costs quickly

Besides they found bottlenecks in the workflows and automated the solution.

**The bottom line** : set up monitoring to avoid unknown costs.

### 4\. Every Decision Is a Tradeoff

They set up the database index to improve read performance. 

Think of the **database index** as the table of contents for a database table.

[![Tradeoff Triangle](https://substackcdn.com/image/fetch/$s_!3OmQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F81d4cc28-1b67-4c2d-b22d-b22ec57cb954_841x764.png)](https://substackcdn.com/image/fetch/$s_!3OmQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F81d4cc28-1b67-4c2d-b22d-b22ec57cb954_841x764.png)Tradeoff Triangle

Yet their growth rate was mind-boggling.

And their data layer wasn't stable.

So they added database read replicas to route the read traffic there.

Imagine the **database read replica** as the copy of a book so more people can read it at once without waiting.

**The bottom line** : know the tradeoffs of each architectural decision.

### 5\. Divide and Conquer

They added features as time passed.

[![The Frugal Software Matrix](https://substackcdn.com/image/fetch/$s_!5lIk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5e33a15d-62f9-4057-88d5-b3a2bf6675f4_1028x800.png)](https://substackcdn.com/image/fetch/$s_!5lIk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5e33a15d-62f9-4057-88d5-b3a2bf6675f4_1028x800.png)The Frugal Software Matrix

But managing many features at a high scale became expensive.

So they categorized services based on importance.

And implemented granular controls for different components.

  * The non-critical features got scaled down.

  * While unused features were removed.

It lets them reduce costs.

**The bottom line** : (1) build the app with tunable building blocks. (2) rank the services based on importance. (3) optimize important services.

### 6\. It’s a Marathon, Not a Sprint

The media content on their app received more traffic than before. (videos and so on.)

[![Serving Media Content via CDN for Low Latency](https://substackcdn.com/image/fetch/$s_!Ohds!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F73e28402-13ae-4214-9086-1c6565dddf50_1080x276.png)](https://substackcdn.com/image/fetch/$s_!Ohds!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F73e28402-13ae-4214-9086-1c6565dddf50_1080x276.png)Serving Media Content via CDN for Low Latency

Yet the storage and streaming costs were high.

So they stored _compressed_ media files in an object storage. 

And set up the content delivery network (**CDN**) for low latency.

Imagine **CDN** as the McDonald's fast food chain, you don't go to the main office to buy a burger. Instead nearest branch to save time.

**The bottom line** : (1) observe patterns. (2) optimize the system _periodically_.

### 7\. We’ve Always Done It This Way

Their core microservices received extreme requests during peak traffic.

[![](https://substackcdn.com/image/fetch/$s_!Ytsi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F230a5d6a-2319-46cc-9a8b-ee579dbaf810_1174x788.png)](https://substackcdn.com/image/fetch/$s_!Ytsi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F230a5d6a-2319-46cc-9a8b-ee579dbaf810_1174x788.png)[Programming Languages: Normalized Results For Energy, Time, and Memory](https://haslab.github.io/SAFER/scp21.pdf)

Yet most of their services were written in Ruby.

And the result? Bad performance.

So they built prototypes in different programming languages & profiled the performance.

Then rewrote code in Rust for 50x better performance. (Proof: [Ranking programming languages by energy efficiency white paper](https://haslab.github.io/SAFER/scp21.pdf).)

**The bottom line** : (1) question your assumptions. (2) pick the right tool for the job.

While they lived happily ever after.

* * *

Apollo 11 - mission to the moon used a guidance computer with only 4 KB RAM and 32 KB storage.

While mobile phones these days offer 8 GB RAM and 64 GB storage.

[![Apollo 11 vs Mobile Phone](https://substackcdn.com/image/fetch/$s_!bMnh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b2a39a8-4a90-49d1-9abb-5beb4355c9e5_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!bMnh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b2a39a8-4a90-49d1-9abb-5beb4355c9e5_1200x630.gif)

So it’s not just about minimizing costs but maximizing value. Put simply, frugality leads to innovation.

**The bottom line** : (1) don’t ignore costs during system design. (2) set limitations on resources - time, money, CPU, and so on.

* * *

Join to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/frugal-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Amazon Lambda Works 🔥](https://substackcdn.com/image/fetch/$s_!UT0O!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F531b744f-385b-4ae9-a70d-3d2e79ffbfed_1280x720.gif)How Amazon Lambda Works 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·September 3, 2024[Read full story](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work)](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work)

[![How to Scale an App to 100 Million Users on GCP 🚀](https://substackcdn.com/image/fetch/$s_!s5_i!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F93dca13a-c620-4e88-9fcf-ef4eece70b41_1280x720.gif)How to Scale an App to 100 Million Users on GCP 🚀[Neo Kim](https://substack.com/profile/135589200-neo-kim)·August 20, 2024[Read full story](https://newsletter.systemdesign.one/p/google-cloud-scalability)](https://newsletter.systemdesign.one/p/google-cloud-scalability)

* * *

### References

  * [AWS re: Invent 2023 - Keynote with Dr. Werner Vogels](https://www.youtube.com/watch?v=UTRBVPvzt9w)

  * [The Frugal Architect](https://thefrugalarchitect.com/)

  * [Achieving Frugal Architecture using the AWS Well-Architected Framework](https://aws.amazon.com/blogs/architecture/achieving-frugal-architecture-using-the-aws-well-architected-framework-guidance/)

  * [AWS Well-Architected](https://aws.amazon.com/architecture/well-architected/)

  * [Frugal innovation – a competitive advantage when resources are limited](https://www.rndtoday.co.uk/tool/frugal-innovation-creates-competitive-advantage-when-resources-are-limited/)

  * [Ranking Programming Languages by Energy Efficiency](https://haslab.github.io/SAFER/scp21.pdf)

  * [10 best practices to reduce your AWS RDS costs](https://www.stream.security/post/10-best-practices-to-reduce-your-aws-rds-costs)

  * [What tech would the Apollo 11 mission have today?](https://www.sciencefocus.com/space/what-tech-would-the-apollo-11-mission-have-today)

  * [How much RAM does your Android phone need in 2024?](https://www.androidauthority.com/how-much-ram-do-i-need-phone-3086661/)

  * [Apollo Guidance Computer](https://en.wikipedia.org/wiki/Apollo_Guidance_Computer)

  * [How does database indexing work?](https://stackoverflow.com/questions/1108/how-does-database-indexing-work)

108

2

13

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Sep 16, 2024](https://newsletter.systemdesign.one/p/frugal-architecture/comment/69143069 "Sep 16, 2024, 12:21 AM")

Liked by Neo Kim

Good System Design also saves and makes money because it allows you to understand the cost/value trade-offs.

Excellent article, Neo! 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/frugal-architecture/comment/69143069)

[1 more comment...](https://newsletter.systemdesign.one/p/frugal-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture