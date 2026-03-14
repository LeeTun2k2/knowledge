[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# 7 Best Practices for API Design 🔥

### #86: Break into API design (8 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 07, 2025

∙ Paid

137

7

18

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines best practices for API design. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/best-practices-for-api-design/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there was a tiny startup.

They had only a few customers.

So they ran their site using a simple monolith architecture.

[![Best Practices for API Design](https://substackcdn.com/image/fetch/$s_!SaFm!,w_1456,c_limit,f_auto,q_auto:good,fl_lossy/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3b7b30a-4ead-4213-aa97-7d7eb248543c_800x500.gif)](https://substackcdn.com/image/fetch/$s_!SaFm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3b7b30a-4ead-4213-aa97-7d7eb248543c_800x500.gif)

But one day, their site became extremely popular.

So they set up a microservices architecture for scalability.

Yet they didn’t know much about API design.

And their public APIs often failed from spiky traffic.

Also they sent the entire dataset when users asked for just the latest data.

Besides it became difficult to manage APIs as they added more features.

So they decided to study the best practices to design APIs.

Onward.

* * *

### [Design, Animate, Publish—All in Framer (Sponsor)](https://framer.link/systemdesign)

[![Framer](https://substackcdn.com/image/fetch/$s_!ICL1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd747ef60-ae8b-4a60-905c-e3632ea99910_1200x800.png)](https://framer.link/systemdesign)

Still using a copy-paste website? **[Framer](https://framer.link/systemdesign)** is the design-first, no-code website builder that lets anyone ship a production-ready site in minutes. Whether you’re starting with a template or a blank canvas, Framer gives you total creative control—no coding required. Add animations, localize with one click, and collaborate in real-time with your whole team. You can even A/B test and track clicks with built-in analytics.

Ready to build a site that looks hand-coded—without hiring a developer? Launch your site for free at Framer dot com, and use code **SYSTEMDESIGN** for a free month on Framer Pro.

[Try Framer](https://framer.link/systemdesign)

* * *

## Best Practices for API Design

Here’s what they learned:

### 1\. REST Fundamentals

REST stands for Representational State Transfer.

It's an architectural pattern for systems to talk with each other over the Internet. It helps to organize the data simply and clearly.

[![Rest Explained With an Analogy](https://substackcdn.com/image/fetch/$s_!PtCj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff77630e8-2550-4ad0-842b-198739110178_1536x1024.png)](https://substackcdn.com/image/fetch/$s_!PtCj!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff77630e8-2550-4ad0-842b-198739110178_1536x1024.png)REST Explained With an Analogy

Think of REST like a book library.

There is just one entrance to borrow or return books. While books get organized into different sections by subject.

REST APIs organize data into resources similarly.

[![How REST API Works](https://substackcdn.com/image/fetch/$s_!gH7r!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F68a2d439-a56a-4179-a3db-e05347d02691_713x144.png)](https://substackcdn.com/image/fetch/$s_!gH7r!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F68a2d439-a56a-4179-a3db-e05347d02691_713x144.png)How REST API Works

Here’s how it works:

  * Each data resource gets represented by an API endpoint. 

    * For example, `/items` represents a collection of items.

    * While `/items/<item>` represents a specific item.

  * The standard HTTP methods allow interaction with the data.

    * `GET`: lets you read data from the resource.

    * `POST`: lets you add a new item to the resource.

    * `PUT`: lets you replace an item with newer data.

    * `DELETE`: lets you remove an item from the resource.

This approach makes the APIs predictable and easy to interact with.

Yet it might be limiting for some real-world actions, such as publishing a draft of a document. So it’s necessary to have a balance between REST principles and a pragmatic approach.

Let’s keep going!

### 2\. Error Handling

A status code tells the client whether a request succeeded or failed.

API Error handling means returning clear, consistent error messages if something goes wrong. Thus making it easy for the client to handle the error properly.

Imagine you’re at an airport.

A bad API error is like the departure board displaying random errors when flight time changes. It doesn’t tell you which flight, why, or what to do next.

While a good API error is like displaying the flight name and its reason behind time changes.

[![HTTP Status Codes](https://substackcdn.com/image/fetch/$s_!Ficm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F97b9b211-4570-4511-9f43-f56111cb07a3_1080x1350.png)](https://substackcdn.com/image/fetch/$s_!Ficm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F97b9b211-4570-4511-9f43-f56111cb07a3_1080x1350.png)HTTP Status Codes

Here’s how it works:

  * The response status code tells whether a client error or a server error occurred.

  * And response body includes extra details about what went wrong.

Proper error handling in APIs makes a site reliable.

Yet it needs extra effort and discipline to handle each failure scenario. Besides error message must include only enough information to avoid security risks.

Ready for the next technique?

### 3\. API Versioning

It’s necessary to version an API so new changes don’t break existing clients.

Think of API versioning like publishing a new book edition.

The old book copies that have already been sold don’t get updated. But the new edition gets released with improvements. So readers who need the updates can get the latest edition. And those using the old one can read it without issues.

[![How API Versioning Works](https://substackcdn.com/image/fetch/$s_!9jyZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9acd973a-049e-409e-8823-88b01cb00ed2_540x269.png)](https://substackcdn.com/image/fetch/$s_!9jyZ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9acd973a-049e-409e-8823-88b01cb00ed2_540x269.png)How API Versioning Works

Here’s how it works:

  * The API URL path includes the version number.

    * First version → `https://app.com/v1/subpath`

    * Second version → `https://app.com/v2/subpath`

  * The old version doesn't get updated, while new changes happen on the latest version.

Thus ensuring backward compatibility.

Here are 2 alternative approaches to implement API versioning:

  * HTTP headers

    * Client includes the version in the Accept header.

    * `Accept: application/vnd.api.v1+json`

    * It means API version 1 in JSON format.

  * Query parameters

    * Client includes the version parameter in the URL.

    * `https://app.com/subpath?version=1`

    * It means API version 1.

But query parameters are designed for filtering or searching data. So it's better to avoid them for versioning. 

API versioning makes a site reliable. Yet it increases the complexity and maintenance efforts. So use it only for breaking changes.

### 4\. Rate Limiting

Rate limiting means controlling the number of requests a client can make to an API within a period. It prevents server overload and protects the API from abuse.

Imagine rate limiting as telling a person how many things they can have during a giveaway.

[![How Rate Limiter Works](https://substackcdn.com/image/fetch/$s_!Svcw!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcc3332fa-0b61-4703-86ac-08e7848f82d9_745x249.png)](https://substackcdn.com/image/fetch/$s_!Svcw!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcc3332fa-0b61-4703-86ac-08e7848f82d9_745x249.png)How Rate Limiter Works

Here’s how it works:

  1. The client sends requests to the server through the rate limiter.

  2. The rate limiter tracks the number of requests from each client using a [counter](https://en.wikipedia.org/wiki/Token_bucket).

  3. The rate limiter rejects a request in case it exceeds the limit.

The API includes special HTTP headers in the response about the rate-limiting rules.

Here are some of them:

  * `X-RateLimit-Limit` → total requests allowed.

  * `X-RateLimit-Remaining` → requests left before being rate-limited.

  * `X-RateLimit-Reset` → time when the rate-limiting counter resets.

While the API returns a `429 Too Many Requests` status code after the client exceeds the limit.

Rate limits ensure the high availability of a site. But it’s necessary to set a reasonable rate limit for a better user experience.

Besides many people might share the same IP address. So it’s better to rate limit a person using an API key instead of their IP address for accuracy and fairness.

Ready for the best part?

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fbest-practices-for-api-design&utm_source=paywall&utm_medium=web&utm_content=172694619)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fbest-practices-for-api-design&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture