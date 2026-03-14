[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Amazon Prime Video Microservices Top Failure

### #4: Read Now - Awful Microservices Architecture (7 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 14, 2023

48

1

6

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

>  _They put serverless and microservices in their architecture to scale. But they faced high costs and scalability issues. A short story on microservices over-engineering._

Prime Video is a streaming service from Amazon. It offers a collection of movies and TV shows.

_This post outlines the re-architecture of the Prime Video stream quality monitoring tool with a monolith. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/prime-video-microservices/?action=share) & I'll send you some rewards for the referrals._

They used the stream quality monitoring tool to inspect the quality of every Prime Video stream.

Their reasons for monitoring stream quality were as follows:

  * Identify quality issues, such as block corruption or audio-video synchronization problems

  * Trigger an automatic fix if there is a problem with the stream

[![Prime video microservices](https://substackcdn.com/image/fetch/$s_!xy1g!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe156a3ad-e55c-4b4d-b168-4ea509a64d64_1184x1180.png)](https://substackcdn.com/image/fetch/$s_!xy1g!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe156a3ad-e55c-4b4d-b168-4ea509a64d64_1184x1180.png)Prime video failure; Source: [Amazon Help on Twitter](https://twitter.com/AmazonHelp/status/1570608296557703168)

They kept the initial release of the stream quality monitoring tool simple. Because they wanted to check only the streams with the highest number of viewers.

But the requirements changed as the video content and subscribers grew. They wanted to check thousands of concurrent streams.

The high-level workflow of the stream quality monitoring tool is as follows:

[![Prime video; Stream quality monitoring tool](https://substackcdn.com/image/fetch/$s_!wRD0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd996a4ec-5fa6-454c-8efa-9c944fe18acf_1175x1766.png)](https://substackcdn.com/image/fetch/$s_!wRD0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd996a4ec-5fa6-454c-8efa-9c944fe18acf_1175x1766.png)Stream quality monitoring tool workflow; Flow chart

## Amazon Prime Video Microservices

They built the stream quality monitoring tool with microservices and serverless components. This was their initial release.

Microservices and serverless components are poster children of scalability. But it turned out to be a wrong architectural decision in this specific use case.

### Workflow

[![Prime video microservices](https://substackcdn.com/image/fetch/$s_!5MPh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa78ccfd8-ecdb-44a3-88a5-f78805254db5_1618x1277.png)](https://substackcdn.com/image/fetch/$s_!5MPh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa78ccfd8-ecdb-44a3-88a5-f78805254db5_1618x1277.png)Stream quality monitoring tool microservices; Prime Video

An outline of the stream quality monitoring tool workflow is as follows:

  1. Conversion service translates streams to frames or decrypted audio buffers

  2. A temporary Amazon Simple Storage Service (**S3**) bucket stores the frames

  3. The defect detector queries the temporary S3 bucket to download the frames

  4. The defect detector executes algorithms to inspect frames in real time. This will identify defects: video freeze, block corruption, or audio-video synchronization problems

  5. The defect detector sends out real-time notifications if there is a defect

  6. The notifications trigger a corrective action on the stream

  7. The S3 bucket stores the defect detection results. They used this data to generate analytics

Amazon S3 is a cloud object storage. They used AWS Step Functions to build the conversion service and the defect detector. AWS Step Function is a workflow automation tool for coordinating AWS services.

They scaled the defect detector by plugging in new instances of Step Function and AWS Lambda. AWS Lambda is a computing service that runs code without provisioning dedicated servers.

But they ran into 2 problems with this architecture:

  * It became expensive on a high-scale workload

  * There were scalability issues

### What Went Wrong?

There were 2 factors that determined the pricing of AWS Step Functions. The number of requests (state transitions) for the workflow and its duration. But there was a state transition for every second of the stream. So, this resulted in high costs.

Also there is an AWS account threshold limit on the number of transitions. This became a scalability bottleneck in orchestration management with AWS Step Functions.

3 factors determine the pricing of AWS S3: data storage amount, amount of outbound data transfer, and number of requests.

The defect detector downloaded the frames from the temporary S3 bucket. The high number of requests to the temporary S3 bucket caused high costs.

## Amazon Prime Video Monolith

> Software architecture is the set of design decisions that, if made incorrectly, may cause your project to be canceled.
> 
> \- Eoin Woods, Endava (CTO)

They realized that microservices architecture would not fix their scalability problems. So, they re-architectured the _stream quality monitoring tool_. And re-implemented it with monolith architecture.

Yet the high-level architecture of Prime Video remains unaffected. Put another way, _Prime Video_ is __ still based on _Microservices_ architecture _._

Drawing the microservice boundaries incorrectly can significantly diminish the benefits of using microservices, or in some cases even derail the entire effort.

\- [Microservices: Up and Running](https://learning.oreilly.com/library/view/microservices-up-and/9781492075448/), Ronnie Mitra, Irakli Nadareishvili

They decided to change the boundary of microservices. And set new boundaries based on the business capabilities. So, they _consolidated_ only the microservices for the stream quality monitoring tool - and created a single monolith.

### Workflow

[![Amazon prime video monolith](https://substackcdn.com/image/fetch/$s_!hRSr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7cf02e5b-5415-4b0b-ae7e-0be5c196917c_1782x978.png)](https://substackcdn.com/image/fetch/$s_!hRSr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7cf02e5b-5415-4b0b-ae7e-0be5c196917c_1782x978.png)Steam quality monitoring tool monolith; Prime Video

An outline of the new stream quality monitoring tool workflow is as follows:

  1. Conversion service translates streams to frames or decrypted audio buffers

  2. The defect detector queries the conversion service to download the frames

  3. The defect detector executes algorithms to inspect frames in real time. This will identify defects: video freeze, block corruption, or audio-video synchronization problems

  4. The defect detector sends out real-time notifications if there is a defect

  5. The notifications trigger a corrective action on the stream

  6. The S3 bucket stores the defect detection results. They used this data to generate analytics

They combined all the components into a single process to create a monolith. This enabled buffer transfer through _memory_ instead of a _temporary S3 bucket_. Thus, they eliminated the need for a temporary S3 bucket.

They built a lightweight orchestration layer with ECS. It replaced the expensive AWS Step Functions.

ECS is the Amazon Elastic Container Service. It simplifies the deployment, management, and scaling of containerized applications.

The scaled defect detector diagonally. They ran many instances of the defect detector in a single EC2 instance. And replicated the EC2 instance.

They preallocated the resources on AWS. This allowed them to get the services at a discount price through the [compute savings plan](https://aws.amazon.com/savingsplans/compute-pricing/). This approach further reduced the costs by the ton.

## Microservice Monolith Architecture

The key takeaways from this case study are as follows:

  * Keep the architecture simple

  * Define microservice boundaries using domain-driven design principles

  * [Back-of-the-envelope](https://systemdesign.one/back-of-the-envelope/) analysis is important

  * Don't follow the buzzwords

  * Microservices come with an overhead cost

They _reduced_ the infrastructure _costs_ by over 90% with the monolith rewrite. Also, they resolved the scalability issues.

As a result, they were able to offer a better customer experience with improved stream quality.

* * *

👋 PS - Are you unhappy at your current job?

While preparing for system design interviews to get your dream job can be stressful.

Don't worry, I'm working on content to help you pass the system design interview. I'll make it easier - you spend only a few minutes each week to go from 0 to 1. Yet paid subscription fees will be higher than current pledge fees.

So pledge now to get access at a lower price.

_“An excellent newsletter to learn system design through practical case studies.”_ Franco

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/actor-model?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjoxMzU1ODkyMDAsInBvc3RfaWQiOjEzOTg1ODA4OSwiaWF0IjoxNzA0MjkxMjMwLCJleHAiOjE3MDY4ODMyMzAsImlzcyI6InB1Yi0xNTExODQ1Iiwic3ViIjoicG9zdC1yZWFjdGlvbiJ9.pkojHISnDAvh6-SUIAKJozx5JOmLI4EBF8NpB8CtIcs)

* * *

[![Tumblr Shares Database Migration Strategy With 60+ Billion Rows](https://substackcdn.com/image/fetch/$s_!JUzq!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7d7a33d0-f8dd-4b24-828c-66d166d90723_1280x720.png)Tumblr Shares Database Migration Strategy With 60+ Billion Rows[NK](https://substack.com/profile/135589200-nk)·September 10, 2023[Read full story](https://newsletter.systemdesign.one/p/how-to-migrate-a-mysql-database)](https://newsletter.systemdesign.one/p/how-to-migrate-a-mysql-database)

[![This Is How Quora Shards MySQL to Handle 13+ Terabytes](https://substackcdn.com/image/fetch/$s_!oxfV!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fef236a61-e44d-4b75-929a-a99dce9d4e56_1280x720.png)This Is How Quora Shards MySQL to Handle 13+ Terabytes[NK](https://substack.com/profile/135589200-nk)·September 3, 2023[Read full story](https://newsletter.systemdesign.one/p/mysql-sharding)](https://newsletter.systemdesign.one/p/mysql-sharding)

* * *

## References

  * Marcin Kolny. (2023, March 22). [Scaling up the Prime Video audio/video monitoring service and reducing costs by 90%](https://www.primevideotech.com/video-streaming/scaling-up-the-prime-video-audio-video-monitoring-service-and-reducing-costs-by-90). primevideotech

  * Sathya Balakrishnan, Ihsan Ozcelik. (2023, February 01). [How Prime Video Uses Machine Learning to Ensure Video Quality](https://www.primevideotech.com/computer-vision/how-prime-video-uses-machine-learning-to-ensure-video-quality). Prime Video Tech Blog.

  * Richard Jones. (2023, February 22). [How Prime Video Troubleshoots Quickly and Cost-Effectively at Scale](https://www.primevideotech.com/cloud-and-scale/how-prime-video-troubleshoots-quickly-and-cost-effectively-at-scale). Prime Video Tech Blog

  * Sathya Balakrishnan, Ihsan Ozcelik. (2022, March 04). [How Prime Video Uses Machine Learning to Ensure Video Quality](https://www.amazon.science/blog/how-prime-video-uses-machine-learning-to-ensure-video-quality). Amazon Science Blog.

  * Cloudflare. (n.d.). [Why Use Serverless?](https://www.cloudflare.com/en-gb/learning/serverless/why-use-serverless/) Cloudflare Learning. 

  * Ryan Frankel. (2023, March 24). [AWS S3 Pricing: How to Save Big on Cloud Storage Costs](https://www.hostingadvice.com/how-to/aws-s3-pricing/). HostingAdvice. 

  * Amazon Web Services, Inc. (n.d.). [AWS Step Functions Pricing](https://aws.amazon.com/step-functions/pricing/). [Amazon Web Services]. 

  * Irakli Nadareishvili, Ronnie Mitra. (2020, December 8), [Microservices: Up and Running](https://www.oreilly.com/library/view/microservices-up-and/9781492075448/ch04.html) (pp. Chapter 4). O'Reilly Media.

  * [What is the difference between Amazon ECS and Amazon EC2?](https://stackoverflow.com/questions/40575584/what-is-the-difference-between-amazon-ecs-and-amazon-ec2) (2016, October 31). In Stack Overflow.

  * [Allocate Memory for Amazon ECS Tasks](https://repost.aws/knowledge-center/allocate-ecs-memory-tasks). (n.d.). In AWS Knowledge Center.

  * Photo by [Thibault Penin](https://unsplash.com/@thibaultpenin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/GgOitQkoioo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

48

1

6

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Craig M's avatar](https://substackcdn.com/image/fetch/$s_!3-NW!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F744fc655-0b9e-48d8-bb5a-12d89857d50d_144x144.png)](https://substack.com/profile/31060314-craig-m?utm_source=comment)

[Craig M](https://substack.com/profile/31060314-craig-m?utm_source=substack-feed-item)

[Sep 17, 2023](https://newsletter.systemdesign.one/p/prime-video-microservices/comment/40237640 "Sep 17, 2023, 6:22 PM")

Liked by Neo Kim

I'm still not sure why everyone gets so concerned with "monolith" and "microservice" when actually all that matters is the service boundary. The largest monoliths of our day still have contracts between modules that have to be conformed to.

There is a race to the bottom when people think making your services ever more granular is the way to achieve scale and cost effectiveness. Where does this end? Do we eventually process each and every command or statement on a new "micro command as a service"?

ReplyShare

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture