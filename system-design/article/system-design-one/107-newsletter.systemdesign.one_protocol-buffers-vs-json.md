[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How LinkedIn Adopted Protocol Buffers to Reduce Latency by 60%

### #12: You Need to Read This - Awful JSON Serialization (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 08, 2023

141

15

9

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how LinkedIn reduced latency using Protocol Buffers. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/protocol-buffers-vs-json/?action=share) & I'll send you some rewards for the referrals._

LinkedIn uses microservices architecture because the number of daily requests they receive is in the range of billions. Also they needed to scale.

But microservices architecture increased their network calls and degraded latency.

They used JavaScript Object Notation (JSON) as the data serialization format but it became a performance bottleneck. So they moved to Protocol Buffers.

This post outlines how LinkedIn adopted Protocol Buffers to reduce latency. I think it’s important to understand data serialization and Protocol Buffers first. So I’ll teach you the basics first.

* * *

## What Is Data Serialization?

[![What is data serialization?](https://substackcdn.com/image/fetch/$s_!htpK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fca685668-4324-45fe-bf45-47a85d57ca7d_1826x1668.png)](https://substackcdn.com/image/fetch/$s_!htpK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fca685668-4324-45fe-bf45-47a85d57ca7d_1826x1668.png)A get-together of people who speak different languages

Imagine a get-together of people who speak different languages. They have no choice but to speak in a language that everybody understands.

So each person must translate thoughts from their native to the common language. But it reduces communication efficiency. This is how data serialization works.

Now in computing terms. Translating an in-memory data structure to a format that can be stored or sent across the network. This is called data serialization.

## What Is Protocol Buffers?

Protocol buffer (**Protobuf**) is a data serialization format and a set of tools to exchange data.

Protobuf keeps data and the metadata separate. And serializes data into binary format.

Besides Protobuf messages are sent across network protocols such as REST or RPC. And supports many programming languages: Python, Java, Go, and C++.

[![What is protocol buffers?](https://substackcdn.com/image/fetch/$s_!nukX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fde96dec7-d0f8-426b-9a8e-06b7a329c262_2017x675.png)](https://substackcdn.com/image/fetch/$s_!nukX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fde96dec7-d0f8-426b-9a8e-06b7a329c262_2017x675.png)Protobuf data serialization workflow

Here is the Protobuf workflow:

  1. _Create a proto file_

And define payload schema: data fields and types.

  2. _Compile proto file to language-specific source files_

Compile the proto file using the Protobuf compiler. And create language-specific source files: One file for the client to serialize data. And other for the server to deserialize data.

  3. _Create an executable package_

Compile the generated Protobuf source file with the project code.

  4. _Serialize or deserialize data_

Serialize data at runtime.

## Why Use Protocol Buffers?

JSON serialization became a _performance bottleneck_ at LinkedIn. Because textual format needed extra network bandwidth. And more computing resources to compress data. It resulted in poor latency and throughput.

Also skipping unwanted data fields is not possible while parsing JSON. Because there is no separation between data and metadata.

But metadata in Protobuf allows parsing specific data fields. And makes it a lot more efficient for a big payload.

Their criteria to find a JSON data serialization alternative were:

  * Smaller payload size. Because it reduces bandwidth needs

  * Improved efficiency. Because it reduces latency

  * Support for many programming languages. Because their tech stack was diverse

  * Easy to plug into the existing setup. Because they wanted to reduce the engineering effort

Protobuf satisfied all the criteria. So they moved to Protobuf.

Protobuf reduced their P99 latency by 60% for big payloads. And improved average throughput by 6.25% for response payloads.

99 _th_ latency percentile is called P99 latency. Put another way, 99% of requests will be faster than the given latency number. Or only 1% of requests will be slower than P99 latency.

_Here is a summary of the Protobuf study by Auth0.com_

If services running JavaScript and Java communicate with each other. There is no big performance improvement with Protobuf. And the latency difference was only 4% compared to compressed JSON.

But if services running Java and Python or Java communicated with each other. Protobuf offered 6 times better latency compared to JSON.

In simple words, there is a big performance improvement for Protobuf with non-JavaScript environments.

## Protobuf Rollout at LinkedIn

They used [Rest.li](https://engineering.linkedin.com/architecture/restli-restful-service-architecture-scale) framework for calls between microservices. Because it abstracted many parts of data exchange: serialization and [service discovery](https://systemdesign.one/what-is-service-discovery/).

[![Protobuf Rest.li framework](https://substackcdn.com/image/fetch/$s_!5Pnf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0422b8bf-96ed-49c5-943b-ad21e0926365_523x1023.png)](https://substackcdn.com/image/fetch/$s_!5Pnf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0422b8bf-96ed-49c5-943b-ad21e0926365_523x1023.png)Rest.li framework

And this is how they rolled out Protobuf:

  1. Add Protobuf support to Rest.li framework

  2. Increment Rest.li framework version. And redeploy every microservice

  3. Release slowly using client configuration. And reduce service disruption

## Protocol Buffers vs JSON

I will outline the _top_ _3 benefits and limitations_ of Protobuf and JSON because it might help you to make better architectural decisions with your project.

Protobuf benefits:

  * Support for schema validation

  * Improved performance with big payloads. Because it uses the binary format

  * Support for backward compatibility

Protobuf limitations:

  * Hard to debug. And not human-readable

  * Extra effort to update the proto file needed

  * Limited language support compared to JSON

JSON benefits:

  * Easy to use and human-readable

  * Easy to change. Because it provides a flexible schema

  * Support for many programming languages

JSON limitations:

  * No support for schema validation

  * Poor performance for big payloads

  * Backward compatibility problems

### Takeaways

Use Protobuf when:

  * Payload is big

  * Communication between non-JavaScript environments needed

  * Frequent changes to the payload schema expected

Use JSON when:

  * Simplicity needed

  * High performance is not needed

  * Communication between JavaScript and Node.js or other environments needed

Protobuf gave big performance improvements for LinkedIn. But it is important to check if Protobuf is best for your use case to prevent over-engineering.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/protocol-buffers-vs-json?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Giphy Delivers 10 Billion GIFs a Day to 1 Billion Users](https://substackcdn.com/image/fetch/$s_!QOCr!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9ed955a6-ad0b-423f-90c3-c8f5a2fb1ff1_1280x720.gif)How Giphy Delivers 10 Billion GIFs a Day to 1 Billion Users[NK](https://substack.com/profile/135589200-nk)·October 12, 2023[Read full story](https://newsletter.systemdesign.one/p/cdn-explained)](https://newsletter.systemdesign.one/p/cdn-explained)

[![7 Simple Ways to Fail System Design Interview](https://substackcdn.com/image/fetch/$s_!-Sxa!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F541c2154-853f-4a25-b513-7cbe54c4fb47_1280x720.png)7 Simple Ways to Fail System Design Interview[NK](https://substack.com/profile/135589200-nk)·October 10, 2023[Read full story](https://newsletter.systemdesign.one/p/design-system-newsletter)](https://newsletter.systemdesign.one/p/design-system-newsletter)

* * *

## References

  * https://engineering.linkedin.com/blog/2023/linkedin-integrates-protocol-buffers-with-rest-li-for-improved-m

  * https://linkedin.github.io/rest.li/user_guide/server_architecture

  * https://auth0.com/blog/beating-json-performance-with-protobuf/

  * https://programmathically.com/protobuf-vs-json-which-data-serialization-format-is-best-for-your-engineering-project/

  * https://protobuf.dev/

141

15

9

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Sathish's avatar](https://substackcdn.com/image/fetch/$s_!Tfxb!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Forange.png)](https://substack.com/profile/132514842-sathish?utm_source=comment)

[Sathish](https://substack.com/profile/132514842-sathish?utm_source=substack-feed-item)

[Oct 14, 2023](https://newsletter.systemdesign.one/p/protocol-buffers-vs-json/comment/41854219 "Oct 14, 2023, 6:20 PM")

Liked by Neo Kim

Thanks NK for the detailed explanations. Why does JS doesn’t perform as good as Java or python using Protobuf?

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/protocol-buffers-vs-json/comment/41854219)

[![Raviraj Achar's avatar](https://substackcdn.com/image/fetch/$s_!WJ1D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbc74ad57-dfb8-4073-9fb9-b47354598c8b_1528x1567.jpeg)](https://substack.com/profile/167123667-raviraj-achar?utm_source=comment)

[Raviraj Achar](https://substack.com/profile/167123667-raviraj-achar?utm_source=substack-feed-item)

[Oct 8, 2023](https://newsletter.systemdesign.one/p/protocol-buffers-vs-json/comment/41501290 "Oct 8, 2023, 4:13 PM")

Liked by Neo Kim

JSON -> Binary, yea totally makes sense for perf wins.

ReplyShare

[13 more comments...](https://newsletter.systemdesign.one/p/protocol-buffers-vs-json/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture