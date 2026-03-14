[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Tech Stack Evolution at Levels.fyi

### #8: Read Now - No Backend to Serving 2.5 Million Unique Users a Month (4 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 24, 2023

21

2

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

[Levels.fyi](https://levels.fyi/) lets you compare career levels and compensation packages across different companies. It started as a side project and now serves 2.5 million unique users a month.

  * _[Share this post](https://newsletter.systemdesign.one/p/levels-fyi-google-sheets/?action=share) & I'll send you some rewards for the referrals._

_This post outlines the startup lessons from Levels.fyi. If you want to learn more, scroll to the bottom and find the references._

_[Dushyant](https://www.linkedin.com/in/dushyantsabharwal/) from Levels.fyi reviewed this post before publishing - thanks._

## Levels.fyi Tech Stack Evolution

Their tech stack went from _no backend_ to a proper backend. Here is an overview of their tech stack evolution:

### Levels.fyi V0 🟩

They used Google _Forms_ and Google _Sheets_. 

Google Forms collected data. While Google Sheets stored data.

They relied on Sheets API to query data. And backed up Google Sheets for durability.

[![Levels fyi Google Sheets; Tech stack V0](https://substackcdn.com/image/fetch/$s_!MMH8!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9eb9af8d-d86a-4178-8f0e-1932c7e45766_1918x1444.png)](https://substackcdn.com/image/fetch/$s_!MMH8!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9eb9af8d-d86a-4178-8f0e-1932c7e45766_1918x1444.png)Levels.fyi V0 tech stack

They used Google Forms and Google Sheets because they wanted:

  * Avoid having to build their own user interface only to collect data.

  * Avoid extra effort in processing and maintaining data.

  * Avoid extra tools to analyze, clean, and validate data.

  * Avoid the operational and maintenance effort.

  * Avoid the need for data schema management, data migration, and ETL.

  * Familiarity with Google Sheets.

  * Easy to use.

  * Save money.

  * In-built support for data access control.

### Levels.fyi V1 🟩🟨

They built a web UI to serve read requests. 

Besides they used lambda functions to read data from Sheets and store it in S3 as JSON files.

[![Levels fyi Google Sheets; Tech stack V1](https://substackcdn.com/image/fetch/$s_!9EiC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff9497310-62c1-4ee2-ab4d-e785bafcda32_1918x1444.png)](https://substackcdn.com/image/fetch/$s_!9EiC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff9497310-62c1-4ee2-ab4d-e785bafcda32_1918x1444.png)Levels.fyi V1 tech stack

They didn’t use a front-end framework. Instead plain JavaScript, jQuery, CSS, and HTML.

Yet their architecture had these drawbacks:

  * Lack of support for SQL-based data analytics. And it limited their ability to make data-driven decisions.

  * Sheets API rate limited their queries.

  * Lambda functions timed out on processing large amounts of data.

### Levels.fyi V2 🟩🟨🟧

They wrote the core _backend services_ in Node.js ([Nestjs](https://nestjs.com/)). And used a static site generator.

They didn’t process graphs or statistics shown on the server side. Instead offloaded it to the browser. And it allowed them to scale with minimal resources.

[![Levels fyi Google Sheets; Tech stack V2](https://substackcdn.com/image/fetch/$s_!Rx1l!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f70cff0-9ad5-491b-84cc-a3401b07b33d_2154x1640.png)](https://substackcdn.com/image/fetch/$s_!Rx1l!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f70cff0-9ad5-491b-84cc-a3401b07b33d_2154x1640.png)Levels.fyi V2 tech stack

Also they built the API server using API Gateway and EC2. It helped to reduce latency and improve performance compared to V1.

And lambda functions aggregated data and analytics.

They used the backend service for:

  * Spam detection.

  * Rate limiting.

  * Preventing duplicate values.

### Levels.fyi V3 🟩🟨🟧🟥

They moved the salary data out of Google Sheets. And built the infrastructure on the _AWS_ stack.

[![Levels fyi Google Sheets; Tech stack](https://substackcdn.com/image/fetch/$s_!r3AX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8d5d5176-5bdd-4cc7-b3ea-e4e006ed6fa6_1760x1332.png)](https://substackcdn.com/image/fetch/$s_!r3AX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8d5d5176-5bdd-4cc7-b3ea-e4e006ed6fa6_1760x1332.png)Levels.fyi V3 tech stack

AWS RDS (Postgres) stored critical data. While Metabase generated charts and visuals for internal use by analysts. The lambda functions aggregated the data (for example, percentile) by querying RDS.

They run Node.js (TypeScript) on the server. And orchestrate 5 microservices using a service mesh. The database gets replicated across many regions.

And use [TinyStacks](https://www.linkedin.com/company/tinystacks/) for continuous deployments.

But Google Sheets is still used in production for other use cases. There is a 10 million cell limit on Sheets. So they _sharded_ the Sheets as a workaround. Creating many folders and workspaces allowed sharding.

[![Levels fyi Google Sheets; Team](https://substackcdn.com/image/fetch/$s_!9LqZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F61baec84-757a-488c-b524-2006a4ce1d52_2967x1004.png)](https://substackcdn.com/image/fetch/$s_!9LqZ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F61baec84-757a-488c-b524-2006a4ce1d52_2967x1004.png)Levels.fyi team has now grown to 14 persons including product and engineering.

## Takeaways

Here are the important lessons from this case study:

### 1\. Keep It Simple

They wanted to focus on what's important - the product idea. So, they used no-code tools and avoided buzzwords. This allowed faster iteration.

### 2\. Avoid Premature Optimization

They wanted to test their product idea. And not invest lots of time and money in doing it.

They used the AWS free tier with credits for the first 2 years. And moved to the paid tier after taking off.

### 3\. Nothing Is Sacred

They outsourced their development and operational burden to Google and AWS. And changed the tech stack to meet the needs.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/cell-based-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjoxMzU1ODkyMDAsInBvc3RfaWQiOjE0MDg5MDQ4MSwiaWF0IjoxNzA2ODYzOTE2LCJleHAiOjE3MDk0NTU5MTYsImlzcyI6InB1Yi0xNTExODQ1Iiwic3ViIjoicG9zdC1yZWFjdGlvbiJ9.pXqxRuxJ7ZNn_8IqAajn4xn_qD45UTUIcRsp_IkbMXc)

* * *

[![How Disney+ Hotstar Scaled to 25 Million Concurrent Users](https://substackcdn.com/image/fetch/$s_!_0xq!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47227723-e12a-47f3-b67a-16c287afc238_1280x720.png)How Disney+ Hotstar Scaled to 25 Million Concurrent Users[NK](https://substack.com/profile/135589200-nk)·September 17, 2023[Read full story](https://newsletter.systemdesign.one/p/hotstar-scaling)](https://newsletter.systemdesign.one/p/hotstar-scaling)

[![8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers](https://substackcdn.com/image/fetch/$s_!88vE!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94e067cc-6ade-44bf-9818-5dc20a260541_1280x720.png)8 Reasons Why WhatsApp Was Able to Support 50 Billion Messages a Day With Only 32 Engineers[NK](https://substack.com/profile/135589200-nk)·August 27, 2023[Read full story](https://newsletter.systemdesign.one/p/whatsapp-engineering)](https://newsletter.systemdesign.one/p/whatsapp-engineering)

* * *

## References

  * Dushyant Sabharwal (February 2023). _How Levels.fyi scaled to millions of users with Google Sheets as a backend_. [online] www.levels.fyi. Available at: https://www.levels.fyi/blog/scaling-to-millions-with-google-sheets.html [Accessed 19 Sep. 2023].

  * www.linkedin.com. (n.d.). _Dushyant S. on LinkedIn: #engineering #startups | 16 comments_. [online] Available at: https://www.linkedin.com/posts/dushyantsabharwal_engineering-startups-activity-6978282092782616576-VAIT/ [Accessed 19 Sep. 2023].

  * www.linkedin.com. (n.d.). _We hit 10M cell limit on Google Sheets. Thanks Tanishq Singh! | Zuhayeer Musa posted on the topic | LinkedIn_. [online] Available at: https://www.linkedin.com/posts/zuhayeer_googlesheets-salaries-salarytransparency-activity-7102715304924905472-fFH0 [Accessed 19 Sep. 2023].

  * www.youtube.com. (n.d.). _Building a new website using Google Sheets and Google Forms_. [online] Available at [YouTube](https://www.youtube.com/watch?v=86XilcZ65J4) [Accessed 19 Sep. 2023].

  * www.youtube.com. (n.d.). _How Levels.fyi Scaled to Millions of Users with Google Sheets_. [online] Available at [YouTube](https://www.youtube.com/watch?v=71rN2nJUAbo) [Accessed 19 Sep. 2023].

  * [Google Forms](https://icons8.com/icon/E4VmOrv6BZqd/google-forms) icon by [Icons8](https://icons8.com/)

  * [Google Sheets](https://icons8.com/icon/30461/google-sheets) icon by [Icons8](https://icons8.com/)

21

2

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture