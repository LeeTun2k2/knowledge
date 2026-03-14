[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How to Improve Availability Using Deployment Patterns ★

### #80: Break Into Deployment Patterns (4 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Aug 05, 2025

87

2

8

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the important deployment patterns. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/deployment-patterns/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, there was a tiny startup. 

They offered live-streaming services.

Yet they had only a few customers.

So they deployed code overnight with a scheduled downtime.

**Deployment** means releasing new code to users.

[![Deployment Patterns](https://substackcdn.com/image/fetch/$s_!yaQ2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3d9c3735-a934-4e94-a1a6-e9bb244ae948_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!yaQ2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3d9c3735-a934-4e94-a1a6-e9bb244ae948_1200x630.gif)

But one day, a celebrity joined their platform for a livestream.

Because of this, their platform became popular among users around the world.

Although explosive growth is a good problem to have, deploying new code without downtime became difficult.

Also they were on a tight budget.

[![Big Bang Deployment Pattern](https://substackcdn.com/image/fetch/$s_!zebv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F18a785a4-fc2b-4ddb-a1fc-5bf2e9c01954_1368x1318.png)](https://substackcdn.com/image/fetch/$s_!zebv!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F18a785a4-fc2b-4ddb-a1fc-5bf2e9c01954_1368x1318.png)Big Bang Deployment Pattern

They used the **Big Bang** deployment pattern to keep things simple.

Put another way, they take the system offline for a short period. And replace the old version with the newer version on each server.

So the service remains unavailable during deployment. Besides any failure with the new version affected all users at once.

Until one day, when their lead engineer decided to evaluate different deployment patterns to solve this problem.

Onward.

* * *

### [Become an AI Generalist that makes $100K (in 16 hours) - Sponsor](https://link.outskill.com/SD1AG2)

One of the biggest IT giants, TCS, laid off 12,000 people this week. And this is just the beginning of the bloodbath.

In the coming days, you’ll see not thousands, but millions more layoffs & displacement of jobs. So what should you do right now to avoid getting affected?

Invest your time in learning about AI: the tools, the use cases, the workflows–as much as you can.

Join the world’s first 16-hour LIVE AI upskilling sprint for professionals, founders, consultants & business owners like you. [Register Now (Only 500 free seats left)](https://link.outskill.com/SD1AG2).

**Date: 9 August (Saturday) and 10 August (Sunday), 10 AM - 7 PM, EST time zone.**

[![](https://substackcdn.com/image/fetch/$s_!86PI!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdfaa9596-479a-4e8b-b33f-7dbf9e113708_1600x900.png)](https://link.outskill.com/SD1AG2)

_Rated 4.9/5 by global learners–this will truly make you an AI Generalist that can build, solve & work on anything with AI.  
  
_In just 16 hours & 5 sessions, you will:

✅ **Learn how AI really works** by learning 10+ AI tools, LLM models, and their practical use cases.

✅**Learn to build and ship products** faster, in days instead of months

✅ **Build AI Agents** that handle your repetitive work and free up 20+ hours weekly

✅ **Create professional images and videos** for your business, social media, and marketing campaigns.

✅ **Turn these AI skills into $10k income** by consulting or starting your own AI services business.

**All by global experts from companies like Amazon, Microsoft, SamurAI, and more. And it’s all for free. 🤯 🚀**

[$5100+ worth of AI tools across 2 days](https://link.outskill.com/SD1AG2) — Day 1: 3000+ Prompt Bible, Day 2: Roadmap to make $10K/month with AI, additional bonus: Your Personal AI Toolkit Builder.

[Register Now](https://link.outskill.com/SD1AG2)

* * *

## Deployment Patterns

Here’s what they found:

### 1\. Rolling

The rolling release offers zero-downtime deployment.

[![Rolling Deployment Pattern](https://substackcdn.com/image/fetch/$s_!rPiQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6b07b470-f950-4e15-af33-c4fc49811a3d_1359x1327.png)](https://substackcdn.com/image/fetch/$s_!rPiQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6b07b470-f950-4e15-af33-c4fc49811a3d_1359x1327.png)Rolling Deployment Pattern

Here’s how it works:

  * Some servers get taken offline and updated

  * Those updated servers then go online

  * This process repeats until all servers get the new version

Put simply, a new version is rolled out server by server until it’s fully deployed.

While the system remains available because the remaining servers handle requests during deployment. 

For example, Twitter and Etsy use this pattern for routine feature deployment with minimal user impact.

Only a subset of servers runs the code initially. Because of that, the failure impact is low during deployment. Yet it’s infrastructure-driven and doesn’t focus on specific user segments. Also the rollout is relatively slow as updating each server takes time.

So use this pattern only if you need zero downtime and to avoid extra infrastructure costs.

Ready for the next technique?

### 2\. Blue Green

The blue-green pattern offers zero-downtime deployment and instant rollback.

[![Blue Green Deployment Pattern](https://substackcdn.com/image/fetch/$s_!oWvj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7b1f327f-e533-48e5-98de-c77c3f24ee49_1359x1332.png)](https://substackcdn.com/image/fetch/$s_!oWvj!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7b1f327f-e533-48e5-98de-c77c3f24ee49_1359x1332.png)Blue Green Deployment Pattern

Here’s how it works:

  * Set up 2 identical environments called Blue & Green

  * The new version gets tested on Green

  * While Blue runs the old version and handles requests to avoid downtime

  * The entire traffic then gets routed to Green using a load balancer or DNS

Also Blue acts as a fallback until the new version on Green becomes stable. Because of that, an instant rollback is possible. Besides testing the new version in a production-like environment (Green) increases release confidence.

For example, Netflix uses this pattern for critical services to avoid service interruption.

Yet running 2 environments in parallel increases infrastructure costs and complexity. Also there’s an extra operational overhead.

So use this pattern only for critical services and high-risk deployments.

### 3\. Canary

The name comes from the use of the canary bird in coal mines to detect toxic gases quickly.

[![Canary Deployment Pattern](https://substackcdn.com/image/fetch/$s_!Is_N!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F344fb85c-05c9-4cfc-a713-8945d2078357_1359x1327.png)](https://substackcdn.com/image/fetch/$s_!Is_N!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F344fb85c-05c9-4cfc-a713-8945d2078357_1359x1327.png)Canary Deployment Pattern

Here’s how it works:

  * A tiny percentage of users get the new version

  * While the remaining traffic goes to the old version

  * The traffic to the new version then gets slowly increased (10% → 50% → 100%)

Put another way, real-world users validate the new version before rolling it out entirely. Thus increasing the release confidence.

Also a failure in the new version affects only some users, and a quick rollback is possible. 

For example, Amazon uses this pattern to fix problems before a global rollout.

Yet it increases the complexity with advanced monitoring and automated rollout control. Besides setting up routing rules needs extra effort.

So use this pattern for large-scale services and high-risk deployments. Or when a beta launch to a subset of users is necessary.

Let’s keep going!

### 4\. Feature Toggle

This pattern allows for code deployment without directly releasing it to users.

A **feature toggle** means conditional logic in the code.

[![Feature Toggle Deployment Pattern](https://substackcdn.com/image/fetch/$s_!S8j1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F41b7673f-d9b9-45dc-a1c7-f61a55f66084_1359x893.png)](https://substackcdn.com/image/fetch/$s_!S8j1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F41b7673f-d9b9-45dc-a1c7-f61a55f66084_1359x893.png)Feature Toggle Deployment Pattern

Here’s how it works:

  * The developer puts the new feature behind a feature toggle

  * And the feature remains hidden until someone turns on the toggle

  * One can turn a feature toggle on or off for specific users via a configuration dashboard

Thus separating code deployment from feature release.

For example, Google and Facebook use feature toggles for [A/B testing](https://en.wikipedia.org/wiki/A/B_testing) and safe releases.

This pattern makes it easy to enable or disable features for a specific set of users. Also it enables continuous integration by deploying code safely behind feature toggles.

Yet the code complexity increases because of conditional paths. Also testing becomes hard because one should test both on and off states. Besides feature toggles can become a technical debt if unremoved on time.

So use this pattern when you need frequent deployments without affecting users. Or when you need to do an A/B test or a beta launch.

* * *

Each deployment pattern solves a specific problem.

But there’s no silver bullet.

So choose the right pattern based on your needs: safety, speed, and cost.

While most companies use different deployment patterns within the same project. For example, a payment service might use blue-green for zero downtime. While frontend UI might use canary or feature flags for safe releases.

Ever since that day, our tiny startup installed a combination of deployment patterns and lived happily ever after.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋 Find me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Neo’s recommendation** 🚀

Everything you need to know to land your next Data Science job. Get expert tips straight to your inbox at **[Ask Data Dawn](https://www.askdatadawn.com/)**.

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 165K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/deployment-patterns?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

**TL;DR** 🕰️

You can find a summary of this article [here](https://www.linkedin.com/posts/nk-systemdesign-one_if-i-had-to-deploy-code-here-are-5-patterns-activity-7358460667185491968-tKqr). Consider a repost if you find it helpful.

* * *

[![How to Scale Code Reviews 🔥](https://substackcdn.com/image/fetch/$s_!E4hP!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F98f5d659-048f-4bb5-ac48-373406bc8c67_1280x720.png)How to Scale Code Reviews 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·July 25, 2025[Read full story](https://newsletter.systemdesign.one/p/how-to-do-code-review)](https://newsletter.systemdesign.one/p/how-to-do-code-review)

[![How Amazon S3 Achieves Strong Consistency Without Sacrificing 99.99% Availability 🌟](https://substackcdn.com/image/fetch/$s_!kEaw!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F95ebfdc1-79b4-41da-9bad-b0c0432ceeb4_1280x720.png)How Amazon S3 Achieves Strong Consistency Without Sacrificing 99.99% Availability 🌟[Neo Kim](https://substack.com/profile/135589200-neo-kim)·July 29, 2025[Read full story](https://newsletter.systemdesign.one/p/s3-strong-consistency)](https://newsletter.systemdesign.one/p/s3-strong-consistency)

* * *

### References

  * [The Risks of Big-Bang Deployments and Techniques for Step-wise Deployment](https://dzone.com/articles/risks-big-bang-deployments-and)

  * [Blue-green vs. Rolling deployments](https://launchdarkly.com/blog/blue-green-deployments-versus-rolling-deployments/)

  * [Rolling deployments](https://docs.aws.amazon.com/whitepapers/latest/overview-deployment-options/rolling-deployments.html)

  * [Feature Toggles (aka Feature Flags)](https://martinfowler.com/articles/feature-toggles.html)

  * [What is a canary deployment?](https://cloud.google.com/deploy/docs/deployment-strategies/canary)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

87

2

8

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Aug 5](https://newsletter.systemdesign.one/p/deployment-patterns/comment/142520486 "Aug 5, 2025, 9:23 PM")

Really liked this breakdown. 

I think deployment isn’t just about pushing code, it’s about managing risk at scale.

That's a good combination there; canary for frontend and blue-green for payments.

ReplyShare

[![Petar Ivanov's avatar](https://substackcdn.com/image/fetch/$s_!Hqxs!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb236a7ab-735e-49d2-bbe8-98b1f901b169_500x500.jpeg)](https://substack.com/profile/10269058-petar-ivanov?utm_source=comment)

[Petar Ivanov](https://substack.com/profile/10269058-petar-ivanov?utm_source=substack-feed-item)

[Aug 6](https://newsletter.systemdesign.one/p/deployment-patterns/comment/142615227 "Aug 6, 2025, 4:03 AM")

Canary Deployment is a great strategy for testing new features. I know many big companies are using it.

Great breakdown, Neo! 🙌 

ReplyShare

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture