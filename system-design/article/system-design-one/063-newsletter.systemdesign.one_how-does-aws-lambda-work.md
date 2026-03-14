[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Amazon Lambda Works 🔥

### #55: Break Into Amazon Engineering (6 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 03, 2024

192

9

14

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

How to scale an app on the cloud without worrying about server management?

Simply use a serverless computing service such as AWS Lambda.

  * _[Share this post](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work/?action=share) & I'll send you some rewards for the referrals._

_This post outlines the internal architecture of AWS Lambda. You will find references at the bottom of this page if you want to go deeper._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, there was a 2-person startup.

They had a customer with a massive following on social networks.

[![How Does AWS Lambda Work](https://substackcdn.com/image/fetch/$s_!67Uu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9c081b90-0dc7-471a-9aa0-589fe2361c23_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!67Uu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9c081b90-0dc7-471a-9aa0-589fe2361c23_1200x630.gif)

And they were given an impossible task - build an extremely scalable app in 23 days.

Yet they were only good at mobile app development and business skills.

So they set up the infrastructure on the cloud.

Although it temporarily solved their scalability issues, there were newer problems.

Here are some of them:

#### 1\. Cost Efficiency

They should pay for each provisioned virtual machine (**VM**) even if unused. 

And scaling down VM instances needs extra effort with the setup of autoscaling groups.

#### 2\. Server Management

The operating system should be updated monthly and server configuration must be managed.

But they didn’t have the time for it.

#### 3\. Infrastructure Scalability

The network traffic management becomes complex as the app scales.

And proper capacity planning is needed for performance.

But they didn’t know how to do it.

* * *

They wanted to ditch distributed systems problems.

And focus only on mobile app development. So they moved to AWS Lambda (**serverless**). It lets them scale the backend quickly based on the traffic.

And there’s no need to provision or manage servers.

Instead provide the code as a zip file or a [container](https://circleci.com/blog/docker-image-vs-container/) image and it’ll run as an event when things happen.

Yet abstracting server management in Lambda is a difficult problem.

So the brilliant engineers at Amazon used simple ideas to solve it.

* * *

## How Does AWS Lambda Work

Here’s how:

### 1\. Scalability

They run Lambda functions on a server called the **worker**.

[![How a Lambda Function Code Is Executed](https://substackcdn.com/image/fetch/$s_!hJCk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc0638ae1-2a22-4867-91f6-6fa8b25e488b_1008x226.png)](https://substackcdn.com/image/fetch/$s_!hJCk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc0638ae1-2a22-4867-91f6-6fa8b25e488b_1008x226.png)How a Lambda Function Is Executed

  * And use **microservices** **architecture** to**** create the Lambda service.

  * While the **invoke service** forwards the**** Lambda request to the worker. And returns the result to the caller.

They run Lambda functions across many workers for scalability. (Also across different [availability zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) for high availability.)

[![Tracking the Status of a Worker](https://substackcdn.com/image/fetch/$s_!xE5p!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe42cd644-dd9c-402e-ac86-4091a5fddc24_1194x381.png)](https://substackcdn.com/image/fetch/$s_!xE5p!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe42cd644-dd9c-402e-ac86-4091a5fddc24_1194x381.png)Tracking the Status of a Worker

While the **assignment service** tracks workers executing a specific Lambda function. (It runs in the leader-follower pattern.)

Also it lets them:

  * Check if a worker is ready for another Lambda invocation

  * Mark a worker as unavailable if an error occurs during the server startup

Yet it’s important to track the workers running a specific Lambda function in a _fault-tolerant_ way.

Otherwise the workers will get orphaned during failures. And waste resources by creating new workers. So they store the worker metadata in an external**journal log**.

It lets them:

  * Perform a quick failover if the assignment service (leader) fails

  * Become resilient against host and network failures

  * Track workers running a specific Lambda function

In simple words, the journal log stores worker metadata durably.

And the assignment service (leader) writes to the journal log while followers read from it.

### 2\. Performance

They use AWS [EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) as the worker.

[![A Microvm Running on a Worker](https://substackcdn.com/image/fetch/$s_!pMOA!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F71b7fc8b-517c-47da-8828-7651c84d965a_628x702.png)](https://substackcdn.com/image/fetch/$s_!pMOA!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F71b7fc8b-517c-47da-8828-7651c84d965a_628x702.png)A microVM Running on a Worker

And it's necessary to isolate each customer's Lambda functions.

Yet having separate workers for each customer will result in resource wastage.

  * So they use lightweight virtual machines called **microVMs**. 

  * And a virtual machine manager called **[Firecracker](https://firecracker-microvm.github.io/)**. 

  * It lets them scale down while providing tenant-level isolation. 

Put simply, Firecracker runs many microVMs on a single worker.

[![How a Worker Is Set Up](https://substackcdn.com/image/fetch/$s_!vZKb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9274c7aa-2e34-49b6-af0c-1ff691f431c8_1242x226.png)](https://substackcdn.com/image/fetch/$s_!vZKb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9274c7aa-2e34-49b6-af0c-1ff691f431c8_1242x226.png)How a Worker Is Set Up

Besides they use the **placement service** to**** set up a new worker.

Also it lets them:

  * Lease workers for a specific period

  * Monitor the health of workers

  * Scale workers based on traffic

The worker then downloads the Lambda function code and initializes it.

### 3\. Latency

99% of requests get handled by an existing microVM.

[![Warm Start vs Cold Start](https://substackcdn.com/image/fetch/$s_!Niw1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1f9c9fe2-c7ed-462e-bdae-c84fadf0fa49_777x519.png)](https://substackcdn.com/image/fetch/$s_!Niw1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1f9c9fe2-c7ed-462e-bdae-c84fadf0fa49_777x519.png)Warm Start vs Cold Start

And the reuse of an existing microVM by a Lambda function is called **warm start**. It allows quick processing of a request.

But occasionally microVMs run out of capacity and new microVMs must be set up.

The creation of a new microVM to handle a request is called **cold start**.

[![Steps Involved in a Cold Start](https://substackcdn.com/image/fetch/$s_!8qko!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0786213c-5438-4eaf-951d-80f445bc76e2_1045x288.png)](https://substackcdn.com/image/fetch/$s_!8qko!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0786213c-5438-4eaf-951d-80f445bc76e2_1045x288.png)Steps Involved in a Cold Start

Yet a cold start takes extra time before it can serve requests because:

  * microVM must be launched

  * code should be initialized

[![A microVM Snapshot](https://substackcdn.com/image/fetch/$s_!0DmI!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5a026789-2a6e-40cb-8c68-0479af909871_755x206.png)](https://substackcdn.com/image/fetch/$s_!0DmI!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5a026789-2a6e-40cb-8c68-0479af909871_755x206.png)A microVM Snapshot

So they create a microVM **snapshot** and restore it when a cold start occurs. 

[![Steps Involved in Resuming a Microvm Snapshot](https://substackcdn.com/image/fetch/$s_!0bko!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F424b2bec-bcf4-457b-a6cc-0d6d4d2d8dfc_1978x296.png)](https://substackcdn.com/image/fetch/$s_!0bko!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F424b2bec-bcf4-457b-a6cc-0d6d4d2d8dfc_1978x296.png)Steps Involved in Resuming a microVM Snapshot

In other words, an actual running microVM gets delivered to the worker instead of the code.

It reduces the cold start latency by 90%.

[![When Does a Container Image Become Ready to Serve Requests?](https://substackcdn.com/image/fetch/$s_!4lBB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F36d8c0b4-c713-4926-bb6e-c5e86a7969d2_731x516.png)](https://substackcdn.com/image/fetch/$s_!4lBB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F36d8c0b4-c713-4926-bb6e-c5e86a7969d2_731x516.png)When Does a Container Image Become Ready to Serve Requests?

They allow container images (with code) for Lambda.

Yet only a subset of the container's image is needed to start serving requests. (Proof: [slacker white paper](https://www.usenix.org/system/files/conference/fast16/fast16-papers-harter.pdf).)

So they create chunks from the container's image, and it could be stored & accessed at low granularity. Then download only the subset of the container image needed to serve requests. Put simply, the container is **lazy-loaded**.

Thus reducing latency and bandwidth usage.

Besides a container image consists of layers. So they find the shared data between the layers for optimal delivery.

* * *

### TL;DR

[![High-Level Architecture of AWS Lambda](https://substackcdn.com/image/fetch/$s_!JdlY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9f89248d-c280-402f-9b5e-3e4cb7323c1d_1882x326.png)](https://substackcdn.com/image/fetch/$s_!JdlY!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9f89248d-c280-402f-9b5e-3e4cb7323c1d_1882x326.png)High-Level Architecture of AWS Lambda

  * Invoke service: route requests

  * Assignment service: tracks workers running a specific Lambda function

  * Worker: servers running Lambda function

This case study shows fundamentals haven’t changed much over time.

While AWS Lambda handles more than _10_ _trillion_ requests a month.

* * *

If you find this newsletter valuable, share it with a friend, and subscribe if you haven’t already. There are [group discounts](http://newsletter.systemdesign.one/subscribe?group=true), [gift options](http://newsletter.systemdesign.one/subscribe?gift=true), and [referral rewards](https://newsletter.systemdesign.one/leaderboard) available.

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How to Scale an App to 100 Million Users on GCP 🚀](https://substackcdn.com/image/fetch/$s_!s5_i!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F93dca13a-c620-4e88-9fcf-ef4eece70b41_1280x720.gif)How to Scale an App to 100 Million Users on GCP 🚀[Neo Kim](https://substack.com/profile/135589200-neo-kim)·August 20, 2024[Read full story](https://newsletter.systemdesign.one/p/google-cloud-scalability)](https://newsletter.systemdesign.one/p/google-cloud-scalability)

[![How Netflix Uses Chaos Engineering to Create Resilience Systems 🐒](https://substackcdn.com/image/fetch/$s_!YVPE!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10523690-010c-4085-baa5-6ae76f1e5848_1280x720.gif)How Netflix Uses Chaos Engineering to Create Resilience Systems 🐒[Neo Kim](https://substack.com/profile/135589200-neo-kim)·August 5, 2024[Read full story](https://newsletter.systemdesign.one/p/chaos-engineering)](https://newsletter.systemdesign.one/p/chaos-engineering)

* * *

### References

  * [AWS re:Invent 2022 - A closer look at AWS Lambda (SVS404-R)](https://www.youtube.com/watch?v=0_jfH6qijVY)

  * [AWS Lambda Under the Hood](https://www.infoq.com/presentations/aws-lambda-arch/)

  * [AWS Lambda: Resilience under-the-hood](https://aws.amazon.com/blogs/compute/aws-lambda-resilience-under-the-hood/)

  * [Slacker: Fast Distribution with Lazy Docker Containers](https://www.usenix.org/system/files/conference/fast16/fast16-papers-harter.pdf)

  * [Firecracker: Lightweight Virtualization for Serverless Applications](https://www.usenix.org/conference/nsdi20/presentation/agache)

  * [What is a microVM?](https://www.koyeb.com/blog/what-is-a-microvm)

  * [What is KVM?](https://www.redhat.com/en/topics/virtualization/what-is-KVM)

  * [Improving startup performance with Lambda SnapStart](https://docs.aws.amazon.com/lambda/latest/dg/snapstart.html)

192

9

14

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![André Luís Ferreira's avatar](https://substackcdn.com/image/fetch/$s_!X0bT!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F65ff4da6-f601-4ff3-892a-3b7f6bb9b96d_1288x1148.jpeg)](https://substack.com/profile/9900428-andre-luis-ferreira?utm_source=comment)

[André Luís Ferreira](https://substack.com/profile/9900428-andre-luis-ferreira?utm_source=substack-feed-item)

[Sep 3, 2024](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work/comment/67670482 "Sep 3, 2024, 3:18 PM")

Liked by Neo Kim

Awesome content Neo!

It got me thinking how the lazy loading of images would actually make it have smaller latency, and now I know how the snapshots make it work!

Thanks a lot!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work/comment/67670482)

[![Jogesh Rajiyan's avatar](https://substackcdn.com/image/fetch/$s_!rYAE!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbeb38789-59ee-4103-ae2d-14f5d896dc68_96x96.jpeg)](https://substack.com/profile/95152067-jogesh-rajiyan?utm_source=comment)

[Jogesh Rajiyan](https://substack.com/profile/95152067-jogesh-rajiyan?utm_source=substack-feed-item)

[Sep 3, 2024](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work/comment/67669139 "Sep 3, 2024, 3:07 PM")

Liked by Neo Kim

This was much needed!!! 🙏

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work/comment/67669139)

[7 more comments...](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture