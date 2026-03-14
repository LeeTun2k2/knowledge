[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# 1 Simple Technique to Scale Microservices Architecture 🚀

### #66: How Great Software Engineers Build Microservices (2 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Feb 04, 2025

142

5

6

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-to-scale-microservices/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there was a tech startup.

They tracked food deliveries in real-time.

Yet they had only a few customers; so, a simple monolith architecture was enough.

[![How to Scale Microservices](https://substackcdn.com/image/fetch/$s_!cMDP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F58f4018e-efea-4a93-9f43-0b3d0c21058a_800x500.gif)](https://substackcdn.com/image/fetch/$s_!cMDP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F58f4018e-efea-4a93-9f43-0b3d0c21058a_800x500.gif)

Until one day, an influencer with a massive following shared their app, and traffic skyrocketed.

So they moved to microservices architecture for scale.

[![Microservices With Tight Coupling and Dependencies](https://substackcdn.com/image/fetch/$s_!j5NL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F85017f6e-4a01-40ba-a882-97d9d95929de_644x402.jpeg)](https://substackcdn.com/image/fetch/$s_!j5NL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F85017f6e-4a01-40ba-a882-97d9d95929de_644x402.jpeg)Microservices With Tight Coupling and Dependencies

Although it temporarily solved their scalability issues, there were newer problems.

Here are some of them:

  1. Implementing new features became difficult because of the dependencies between microservices.

  2. The operational complexity increased due to different programming languages across microservices.

Onward.

* * *

#### [“Building a Scalable Authorization System: A Step-By-Step Blueprint” Ebook by Cerbos - Sponsor](https://www.cerbos.dev/?utm_campaign=brand_cerbos&utm_source=system_design&utm_medium=email&utm_content=&utm_term=)

[![Cerbos](https://substackcdn.com/image/fetch/$s_!3qDQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7fa51149-17e5-41ab-85c3-51f89d52654f_9832x4813.png)](https://solutions.cerbos.dev/building-a-scalable-authorization-system?utm_campaign=authorization_ebook&utm_source=system_design&utm_medium=email&utm_content=&utm_term=)

Authorization can make or break your application’s security and scalability. From managing dynamic permissions to implementing fine-grained access controls, the challenges grow as your requirements and users scale.

This eBook will guide you through the 6 key requirements all authorization layers should include to avoid technical debt:

  * Architectural and design considerations for building a scalable and secure authorization layer.

  * 20+ technologies, approaches, and standards to consider for your permission management systems.

  * Practical insights, based on 500+ interviews with engineers, architects, and IAM professionals.

Learn how to create an authorization solution that evolves with your business needs, while avoiding technical debt.

[Download FREE eBook](https://solutions.cerbos.dev/building-a-scalable-authorization-system?utm_campaign=authorization_ebook&utm_source=system_design&utm_medium=email&utm_content=&utm_term=)

* * *

## How to Scale Microservices

They wanted to reduce the maintenance effort and scale microservices quickly.

So they relied on automation.

Here’s how:

### 1\. Server Management

A microservice must be scaled to handle changing traffic.

Yet scaling manually is slow and causes errors.

[![Infrastructure as Code for Service Management](https://substackcdn.com/image/fetch/$s_!2MAH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F901009f5-a056-4287-852f-975ede9a8a18_824x161.png)](https://substackcdn.com/image/fetch/$s_!2MAH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F901009f5-a056-4287-852f-975ede9a8a18_824x161.png)Infrastructure as Code for Service Management

So they use [infrastructure as code](https://en.wikipedia.org/wiki/Infrastructure_as_code) to keep things simple; it’s used to deploy new instances based on demand. **Infrastructure as code** means managing and provisioning infrastructure using code.

Ready for the next technique?

### 2\. API Definition

It’s difficult to understand and maintain APIs without a proper naming convention.

Yet maintaining consistent naming for APIs across microservices is hard. 

_So they generate APIs using code and store API definitions as JSON in a separate Git repository._

Here are the benefits of code-generated APIs:

  * API paths can be validated using [linters](https://www.sonarsource.com/learn/linter/).

  * API naming conventions and request-response structures are standardized.

  * API version changes can be tracked through tagging.

  * It ensures API operations are defined.

  * User-friendly API paths can be set up as default.

Also it standardizes APIs built using different programming languages. Put simply, code generation makes it look like a single person wrote APIs across the system.

[![Checking the Health of a Microservice](https://substackcdn.com/image/fetch/$s_!vxgh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fafdaa3be-5994-48a9-8fe8-39a2e6fb3aef_733x191.png)](https://substackcdn.com/image/fetch/$s_!vxgh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fafdaa3be-5994-48a9-8fe8-39a2e6fb3aef_733x191.png)Checking the Health of a Microservice

Besides code generation ensures health checks are included in each microservice.

Here’s how it works:

  * A health check URL is included in the API specification of the microservice.

  * It returns HTTP [status code 200](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200) if the microservice is healthy.

  * Otherwise, it returns HTTP [status code 422](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422).

An unhealthy service is removed from production to avoid service interruptions.

### 3\. Database Architecture

A change to database schema shouldn’t break microservices depending on it. 

Yet sharing a single database across microservices introduces tight coupling and failure risk.

[![Database per Service Pattern](https://substackcdn.com/image/fetch/$s_!5dU-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F882543b3-93e2-4dae-9db2-d3f9b25e3074_884x240.png)](https://substackcdn.com/image/fetch/$s_!5dU-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F882543b3-93e2-4dae-9db2-d3f9b25e3074_884x240.png)Database per Service Pattern

So they set up separate databases for each microservice.

But maintaining many databases is a huge effort; so, they create databases and tables using code.

Here are its main benefits:

  * Consistent database names.

  * Normalized data access.

  * Ensure proper database indexes exist from the start.

  * Easy to maintain and operate the database.

Besides a microservice shouldn't talk to a database owned by another microservice. Instead, communication happens only via the microservice’s API owning the database. Otherwise, the data changes aren't safe as they bypass business logic.

* * *

**The bottom line** : Use code generation and automation to scale microservices quickly. It reduces maintenance efforts and helps with version control.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a tech audience, you may want to [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

**TL;DR 🕰️**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-to-scale-microservices?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Bluesky Works 🦋](https://substackcdn.com/image/fetch/$s_!GyYg!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F449c6406-d812-4547-bc2a-8e55c325ce3c_1280x720.gif)How Bluesky Works 🦋[Neo Kim](https://substack.com/profile/135589200-neo-kim)·January 22, 2025[Read full story](https://newsletter.systemdesign.one/p/how-does-bluesky-work)](https://newsletter.systemdesign.one/p/how-does-bluesky-work)

[![How Do AirTags Work ](https://substackcdn.com/image/fetch/$s_!tsPl!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43c2ccc9-1f05-472d-ad8b-933c4c32d38e_1280x720.gif)How Do AirTags Work [Neo Kim](https://substack.com/profile/135589200-neo-kim)·January 8, 2025[Read full story](https://newsletter.systemdesign.one/p/how-do-airtags-work)](https://newsletter.systemdesign.one/p/how-do-airtags-work)

* * *

### References

  * [Design Microservice Architectures the Right Way](https://www.youtube.com/watch?v=j6ow-UemzBc)

  * [Mastering Chaos - A Netflix Guide to Microservices](https://www.youtube.com/watch?v=CZ3wIuvmHeM)

  * [Pattern: Database per service](https://microservices.io/patterns/data/database-per-service.html)

  * [What is Infrastructure as Code (IaC)?](https://www.redhat.com/en/topics/automation/what-is-infrastructure-as-code-iac)

  * [How to Perform an API Health Check](https://apitoolkit.io/blog/how-to-perform-an-api-health-check/)

  * Meme created with [Imgflip](https://imgflip.com/)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

142

5

6

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Petar Ivanov's avatar](https://substackcdn.com/image/fetch/$s_!Hqxs!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb236a7ab-735e-49d2-bbe8-98b1f901b169_500x500.jpeg)](https://substack.com/profile/10269058-petar-ivanov?utm_source=comment)

[Petar Ivanov](https://substack.com/profile/10269058-petar-ivanov?utm_source=substack-feed-item)

[Feb 5, 2025](https://newsletter.systemdesign.one/p/how-to-scale-microservices/comment/91163871 "Feb 5, 2025, 5:14 AM")

A bad microservice handles multiple domains.

A good microservice handles only one of the business’s domains.

Great article, Neo!

ReplyShare

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Feb 13, 2025](https://newsletter.systemdesign.one/p/how-to-scale-microservices/comment/93281931 "Feb 13, 2025, 7:44 PM")

Step #3 is where many teams drop the ball, failing to understand the complexity and scalability limitations that shared database produces.

Simply put, Neo!

ReplyShare

[3 more comments...](https://newsletter.systemdesign.one/p/how-to-scale-microservices/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture