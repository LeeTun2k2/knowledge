[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# HTTP Headers to Build 10X APIs 🔥

### #83: Break Into HTTP Headers (15 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Aug 19, 2025

∙ Paid

83

12

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the must-know HTTP headers._

  * _[Share this post](https://newsletter.systemdesign.one/p/http-headers/?action=share) & I'll send you some rewards for the referrals._

_I created the block diagrams in this newsletter with_ _[Eraser](https://app.eraser.io/auth/sign-up?ref=neo)._

Once upon a time, there lived a junior software engineer.

He worked for a tech company named Hooli.

Although extremely bright, he never got promoted.

So he was sad and frustrated.

[![HTTP Headers](https://substackcdn.com/image/fetch/$s_!oKlN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7db66c6-291a-4c0c-939c-a8c2ebe7581f_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!oKlN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7db66c6-291a-4c0c-939c-a8c2ebe7581f_1200x630.gif)

Until one day, when he had the idea to apply for a job at a unicorn startup.

And worked hard on solving LeetCode.

But the interviewer asked him just about HTTP headers.

And he failed to answer, so the interview was over in 7 minutes.

So he studied [web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers) later to fill the knowledge gap.

Onward.

* * *

#### [“Building a scalable authorization system: A step-by-step blueprint” Ebook by Cerbos - Sponsor](https://solutions.cerbos.dev/building-a-scalable-authorization-system?utm_campaign=system_design_aug_2025&utm_source=newsletter&utm_medium=email&utm_content=&utm_term=)

[![](https://substackcdn.com/image/fetch/$s_!W1s4!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2b8fbe3-350d-4f25-8083-2717cc0da4a9_3216x1884.png)](https://solutions.cerbos.dev/building-a-scalable-authorization-system?utm_campaign=system_design_aug_2025&utm_source=newsletter&utm_medium=email&utm_content=&utm_term=)

Authorization can make or break your application’s security and scalability. From managing dynamic permissions to implementing fine-grained access controls, the challenges grow as your requirements and users scale.

This **[eBook](https://solutions.cerbos.dev/building-a-scalable-authorization-system?utm_campaign=system_design_aug_2025&utm_source=newsletter&utm_medium=email&utm_content=&utm_term=)** will guide you through the 6 key requirements all authorization layers should include to avoid technical debt:

  * Architectural and design considerations for building a scalable and secure authorization layer.

  * 20+ technologies, approaches, and standards to consider for your permission management systems.

  * Practical insights, based on 500+ interviews with engineers, architects, and IAM professionals.

Learn how to create an authorization solution that evolves with your business needs, while avoiding technical debt.

[Download FREE eBook](https://solutions.cerbos.dev/building-a-scalable-authorization-system?utm_campaign=system_design_aug_2025&utm_source=newsletter&utm_medium=email&utm_content=&utm_term=)

* * *

An HTTP request-response has 2 parts:

  * Header: tiny pieces of metadata

  * Body: actual content

A request means the client asks for something. While a response means the server sends back something.

An HTTP header consists of a name and a value. Although headers are invisible to the users, they’re important in transferring information. Yet extra headers could consume bandwidth and affect performance. So it’s necessary to use them correctly.

[![HTTP Status Codes](https://substackcdn.com/image/fetch/$s_!GFR0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03d785d5-58aa-42e9-97a6-9436d3c05763_1080x1350.png)](https://substackcdn.com/image/fetch/$s_!GFR0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03d785d5-58aa-42e9-97a6-9436d3c05763_1080x1350.png)HTTP Status Codes

And the server sends a status code with each response. It tells the client whether its request succeeded, failed, or needs extra action.

* * *

## **HTTP Headers**

He failed the interview, so you don’t have to.

Here’s a summary of what he learned:

### 1\. General Headers

Both requests and responses include these headers.

#### Cache-Control

It controls the caching behavior of browsers and intermediary caches. For example, caching in proxy servers and CDN. Put simply, it helps to determine if the browser should serve data from its cache when the user revisits a site.

[![Cache-Control HTTP Header](https://substackcdn.com/image/fetch/$s_!vaxn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22f0f43b-f9e2-420d-a6bf-203201f9fdfd_1017x250.png)](https://substackcdn.com/image/fetch/$s_!vaxn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22f0f43b-f9e2-420d-a6bf-203201f9fdfd_1017x250.png)Cache-Control HTTP Header

Here are some of its directives:
    
    
    Cache-Control: max-age=3600, public

  * It means cache the response in each infrastructure layer for 3600 seconds.

    
    
    Cache-Control: no-store

  * It means don’t cache the response anywhere.

    
    
    Cache-Control: no-cache

  * It means cache the response. But the browser must re-validate with the server before using it.

An incorrect configuration of this header might display stale data to the user. Also there’s a risk of storing private data. Besides forgetting to cache static resources, such as images or scripts, affects the performance badly.

#### Date

It indicates when the server sent the response. It’s used to calculate cache freshness and also debug clock issues.

[![Date HTTP Header](https://substackcdn.com/image/fetch/$s_!3d3l!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fccdef59c-856b-46dd-92c4-09a490b9b0cb_1015x248.png)](https://substackcdn.com/image/fetch/$s_!3d3l!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fccdef59c-856b-46dd-92c4-09a490b9b0cb_1015x248.png)Date HTTP Header

Here’s an example:
    
    
    Date: Sun, 22 Aug 2025 14:00:00 GMT

  * It means the server sent the response on 22 August 2025 at 14:00 GMT.

An incorrect configuration of this header will make the cache freshness calculation wrong. Also it'd make debugging harder.

#### Via

It’s added by proxy servers automatically. And helps to track and debug network routing paths. Put simply, it shows the intermediary servers such as proxies, load balancers, and CDN.

[![Via HTTP Header](https://substackcdn.com/image/fetch/$s_!h81K!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc12060b4-8103-4d54-9ecf-81e02e784296_1001x252.png)](https://substackcdn.com/image/fetch/$s_!h81K!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc12060b4-8103-4d54-9ecf-81e02e784296_1001x252.png)Via HTTP Header

Here’s an example:
    
    
    Via: 1.1 proxy1.example.com, 1.1 proxy2.example.com

  * It means the message passed through `proxy1` and `proxy2` using HTTP/1.1.

So there’s no impact on users if this header is missing. But its absence can make debugging difficult when there are many proxies.

Let’s keep going!

* * *

### 2\. Request Headers

The client sends these headers to the server to give information about the client or the request.

#### Host

It contains the hostname and port number of the server receiving the request. 

Yet it defaults to port 443 for HTTPS if the client specifies nothing. It’s helpful when the server handles different sites via [virtual hosting](https://en.wikipedia.org/wiki/Virtual_hosting) on the same IP address.

[![Host HTTP Header](https://substackcdn.com/image/fetch/$s_!eDm4!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febc46aa5-3879-4462-ab65-16883b35ef26_1015x248.png)](https://substackcdn.com/image/fetch/$s_!eDm4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febc46aa5-3879-4462-ab65-16883b35ef26_1015x248.png)Host HTTP Header

Here’s an example:
    
    
    Host: blog.example.com

  * It means the request is for the blog subdomain (virtual host) on the server.

The server might return the default site or a 400 error status code if this header is missing. Put simply, an incorrect header will prevent the request from reaching the correct site.

#### User-Agent

It informs the server about the client’s browser and operating system. It’s useful for analytics, compatibility workarounds, and content optimization (mobile vs desktop).

[![User-Agent HTTP Header](https://substackcdn.com/image/fetch/$s_!dbsC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcc274b9a-301f-4ae3-b054-0157da997353_1001x248.png)](https://substackcdn.com/image/fetch/$s_!dbsC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcc274b9a-301f-4ae3-b054-0157da997353_1001x248.png)User-Agent HTTP Header

Here’s an example:
    
    
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 Chrome/114.0.0.0 Safari/537.36

  * It means the request came from Google Chrome 114 on a 64-bit Windows 10 machine.

Some older browsers might receive incompatible content if this header is missing. Also a wrong header might serve incorrect data to the mobile client. Thus affecting performance badly.

#### Accept

It informs the server of the content format expected in the response. For example, JSON or HTML.

[![Accept HTTP Header](https://substackcdn.com/image/fetch/$s_!PPwE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8a183a4-d1c1-42b5-903b-f5ce8cd17e4e_1001x248.png)](https://substackcdn.com/image/fetch/$s_!PPwE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8a183a4-d1c1-42b5-903b-f5ce8cd17e4e_1001x248.png)Accept HTTP Header

Here’s an example:
    
    
    Accept: application/json

  * It means the client prefers a JSON response.

The server responds with its default format if this header is missing. And the client might fail to process the response. Besides the server responds with a 406 Not Acceptable error code if it doesn’t support the requested format.

#### Accept-Language

It informs the server of the human language expected in the response. Put simply, it tells the browser’s language preference. For example, English or German.

[![Accept-Language HTTP Header](https://substackcdn.com/image/fetch/$s_!pVz1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb1a74ef7-35e2-449c-bd33-6ed48d3f176c_1017x248.png)](https://substackcdn.com/image/fetch/$s_!pVz1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb1a74ef7-35e2-449c-bd33-6ed48d3f176c_1017x248.png)Accept-Language HTTP Header

Here’s an example:
    
    
    Accept-Language: en-US, de;q=0.9

  * It means the client prefers content in US English. And prefers German only if it’s unavailable.

The server falls back to its default language if this header is missing or wrongly set. Thus affecting user experience.

#### Accept-Encoding

It informs the server of the compression algorithm expected in the response. For example, gzip or deflate. It helps to reduce the response size by compressing it, thus saving bandwidth.

[![Accept-Encoding HTTP Header](https://substackcdn.com/image/fetch/$s_!76qW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9e7373bc-1352-4e71-b8a9-ffa0449d8efe_1017x248.png)](https://substackcdn.com/image/fetch/$s_!76qW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9e7373bc-1352-4e71-b8a9-ffa0449d8efe_1017x248.png)Accept-Encoding HTTP Header

Here’s an example:
    
    
    Accept-Encoding: gzip, deflate, br

  * It means the client can handle responses compressed with gzip, deflate, or brotli (br).

The server sends an uncompressed response if this header is missing. Thus affecting performance badly. Also an incorrect header might create a response in the wrong compression format. And the client might fail to decode it.

#### Cookie

It lets the client send its cookies to the server. Think of a **cookie** as a piece of data stored on the browser. It helps to remember things like logins, preferences, or sessions.

[![Cookie HTTP Header](https://substackcdn.com/image/fetch/$s_!2ciU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb84c8a6-09ca-4d00-bda5-6c0bca11d0fa_1017x248.png)](https://substackcdn.com/image/fetch/$s_!2ciU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb84c8a6-09ca-4d00-bda5-6c0bca11d0fa_1017x248.png)Cookie HTTP Header

Here’s an example:
    
    
    Cookie: session_id=abc567; theme=dark

  * It means the client is sending its stored data, such as session ID and theme preference, to the server.

The server might start a new session if this header is missing. Thus affecting the user experience badly.

#### Referer

It contains the site URL that sent the client to the current page. Put simply, it shows where the request came from. And it’s commonly used in analytics.

[![Referer HTTP Header](https://substackcdn.com/image/fetch/$s_!mo5b!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbe020fcf-850b-4256-b6d9-ec5a2bfe63b1_1017x248.png)](https://substackcdn.com/image/fetch/$s_!mo5b!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbe020fcf-850b-4256-b6d9-ec5a2bfe63b1_1017x248.png)Referer HTTP Header

Here’s an example:
    
    
    Referer: https://example.com/page1

  * It means the request came from page1 on example.com

The site traffic analytics might become inaccurate if this header is missing.

#### Authorization

It lets the client send authentication credentials, such as tokens or API keys. Imagine **authentication credentials** as a password or token to prove who the client is. So the server can allow it to access specific resources.

[![Authorization HTTP Header](https://substackcdn.com/image/fetch/$s_!FnnW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86f79e4e-3d68-4cf5-ae90-48d21a95c9f7_1017x248.png)](https://substackcdn.com/image/fetch/$s_!FnnW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F86f79e4e-3d68-4cf5-ae90-48d21a95c9f7_1017x248.png)Authorization HTTP Header

Here’s an example:
    
    
    Authorization: Bearer eyJhbKciOiJm

  * It means the client is using a Bearer token as proof of identity to access the server. Think of the **Bearer token** as a temporary digital key for access.

The server responds with a 401 Unauthorized error code if this header is missing or invalid. While server responds with a 403 Forbidden error code if the client has insufficient permission.

#### Range

It lets the client download specific byte ranges of a file. It’s useful for streaming media or resuming broken downloads.

[![Range HTTP Header](https://substackcdn.com/image/fetch/$s_!lqT2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3b08b174-2ea6-4ecf-ae17-9db45ed60d31_1017x248.png)](https://substackcdn.com/image/fetch/$s_!lqT2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3b08b174-2ea6-4ecf-ae17-9db45ed60d31_1017x248.png)Range HTTP Header

Here’s an example:
    
    
    Range: bytes=0-499

  * It means the client is telling the server to send only the first 500 bytes of a file.

The server sends the entire file on each request if this header is missing. Thus wasting bandwidth.

#### If-Modified-Since

It tells the server to send the resource only if an update occurred on it after a specific period. Put simply, it lets the client make conditional GET requests.

[![If-Modified-Since HTTP Header](https://substackcdn.com/image/fetch/$s_!qZqi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd948768c-7b68-4e68-96cb-c0ae2722b45c_1015x248.png)](https://substackcdn.com/image/fetch/$s_!qZqi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd948768c-7b68-4e68-96cb-c0ae2722b45c_1015x248.png)If-Modified-Since HTTP Header

Here’s an example:
    
    
    If-Modified-Since: Tue, 11 Jun 2024 10:00:00 GMT

  * It means the client is asking the server to send the resource only if it has changed since 11 June 2024, 10 hours GMT.

The server sends a fresh copy every time if this header is missing. Thus wasting bandwidth. While the server responds with 304 Not Modified without a body if the resource hasn’t changed.

#### If-None-Match

It tells the server to send the resource only if the client’s ETag doesn’t match the server’s current ETag. Think of **ETag** as a unique resource version identifier for a resource. Put simply, it lets the client make conditional GET requests, but much more precisely.

[![If-None-Match HTTP Header](https://substackcdn.com/image/fetch/$s_!heGM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F76698cd0-e8a7-404a-94d6-419a9d5ea73f_1015x248.png)](https://substackcdn.com/image/fetch/$s_!heGM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F76698cd0-e8a7-404a-94d6-419a9d5ea73f_1015x248.png)If-None-Match HTTP Header

Here’s an example:
    
    
    If-None-Match: "abc123"

  * It means the client is asking the server to send the resource only if its current ETag differs from `“abc123”.`

The server sends a fresh copy every time if this header is missing. Thus wasting bandwidth and affecting performance.

#### X-Forwarded-For

The client doesn't add this header, but the proxy does. 

It lets the server know the client's IP address even if the request passed through proxies or load balancers. It’s useful for analytics, logging, and rate limiting.

[![X-Forwarded-for HTTP Header](https://substackcdn.com/image/fetch/$s_!BzD5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F235c334d-7f16-451e-abe5-c1f5132ceab7_1085x284.png)](https://substackcdn.com/image/fetch/$s_!BzD5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F235c334d-7f16-451e-abe5-c1f5132ceab7_1085x284.png)X-Forwarded-for HTTP Header

Here’s an example:
    
    
    X-Forwarded-For: 203.0.113.21

  * It means the request originally came from the client with IP address 203.0.113.21

While the server only sees the proxy or load balancer’s IP address if the header is missing. Thus making rate limiting difficult and analytics less accurate.

#### X-Forwarded-Proto

The proxy or load balancer adds this header.

It tells the server the original protocol the client used before the request passed through proxies or load balancers. For example, HTTP or HTTPS.

It helps the server to generate correct absolute URLs that match the client’s protocol.

[![X-Forwarded-Proto HTTP Header](https://substackcdn.com/image/fetch/$s_!9Pxc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F54fce0d8-f96d-4543-b896-d42f576358c8_1081x325.png)](https://substackcdn.com/image/fetch/$s_!9Pxc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F54fce0d8-f96d-4543-b896-d42f576358c8_1081x325.png)X-Forwarded-Proto HTTP Header

Here’s an example:
    
    
    X-Forwarded-Proto: https

  * It means the original client used HTTPS.

The server might return insecure HTTP links to the client if this header is missing.

#### X-Forwarded-Port

The proxy or load balancer adds this header. 

It tells the server the original port the client used before the request passed through proxies or load balancers. For example, port 80 for HTTP or 443 for HTTPS.

It helps the server generate correct absolute URLs for redirects.

[![X-Forwarded-Port HTTP Header](https://substackcdn.com/image/fetch/$s_!cx_w!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe30ae7e1-6c27-4ab8-ad80-3e14dd6d2777_1081x323.png)](https://substackcdn.com/image/fetch/$s_!cx_w!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe30ae7e1-6c27-4ab8-ad80-3e14dd6d2777_1081x323.png)X-Forwarded-Port HTTP Header

Here’s an example:
    
    
    X-Forwarded-Port: 443

  * It means the original client used port 443 (default for HTTPS).

The server might return incorrect absolute links to the client if this header is missing.

Ready for the best part?

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

### 3\. Response Headers

The server sends these headers to give instructions to the client.

#### Location

It’s used by the server to redirect the client to a specific URL. For example, the server sends this header after a form submission to tell the browser where the new resource is.

[![Location HTTP Header](https://substackcdn.com/image/fetch/$s_!3jlL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F830e55ab-245b-4b87-9bf0-a32112357931_1015x248.png)](https://substackcdn.com/image/fetch/$s_!3jlL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F830e55ab-245b-4b87-9bf0-a32112357931_1015x248.png)Location HTTP Header

Here’s an example:
    
    
    Location: https://example.com/welcome

  * It means the server is telling the client to redirect to example.com/welcome

The client might get redirected to a broken URL if this header is wrong. While the client will stay on the same URL if this header is missing.

#### Set-Cookie

It lets the server store cookies on the client. The client then includes it in future requests. Imagine a **cookie** as a small piece of data stored on the client. It allows session management, personalization, and tracking.

[![Set-Cookie HTTP Header](https://substackcdn.com/image/fetch/$s_!B7NB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5e7cdc3b-200a-4326-813f-a2f2aabdeeb9_1015x248.png)](https://substackcdn.com/image/fetch/$s_!B7NB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5e7cdc3b-200a-4326-813f-a2f2aabdeeb9_1015x248.png)Set-Cookie HTTP Header

Here’s an example:
    
    
    Set-Cookie: session_id=abc123; Path=/; HttpOnly

  * It means the server is telling the browser to store a session cookie for the entire site and block JavaScript from accessing it.

Each request might get treated as a new session if this header is missing. Also there might be security risks or broken logins if this header is wrong.

#### Access-Control-Allow-Origin

It’s the main header for cross-origin requests (**CORS**). 

It tells the browser which sites are allowed access to a resource on a specific site. While the browser blocks the response if it's missing.

Imagine an e-commerce store that uses an external payment service. The store tries to fetch the payment confirmation from the payment service.

Yet browsers have the **same-origin policy.** It means JavaScript can only read responses from the same site unless the other site allows it. So without CORS, the browser blocks the response. (Even if the request went through successfully.)

[![Access-Control-Allow-Origin HTTP Header](https://substackcdn.com/image/fetch/$s_!1K7Z!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd5cbc557-ae4f-4792-9352-cf6093e43dff_1010x246.png)](https://substackcdn.com/image/fetch/$s_!1K7Z!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd5cbc557-ae4f-4792-9352-cf6093e43dff_1010x246.png)Access-Control-Allow-Origin HTTP Header

With CORS enabled, the payment service responds with this header:
    
    
    Access-Control-Allow-Origin: ecommerce-store.com

  * It means allow store’s JavaScript to read the payment confirmation.

And the e-commerce store displays the payment confirmation data. This approach prevents random sites from accessing sensitive data.

Think of this header with a real-world analogy:

  * Access-Control-Allow-Origin header = guest list for a party

  * Browser = security guard

  * Payment service = party host

  * E-commerce store = guest who wants to visit the party

If the payment service doesn’t have the e-commerce store on the guest list, the browser won’t let it in to see the data.

#### Age

This header is added by a cache, such as the CDN or proxy.

It tells how many seconds the response has been in the cache since it was fetched from the origin server. Thus helping the browser decide if the cache is still fresh or needs revalidation.

[![Age HTTP Header](https://substackcdn.com/image/fetch/$s_!BE7c!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd9cc77e-c5b3-4b33-a8cf-c9192b512014_1015x248.png)](https://substackcdn.com/image/fetch/$s_!BE7c!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd9cc77e-c5b3-4b33-a8cf-c9192b512014_1015x248.png)Age HTTP Header

Here’s an example:
    
    
    Age: 120

  * It means the cached copy is 120 seconds old.

Each time the cache serves a response, it calculates the age and includes it in the header. The cache freshness is then determined by comparing this header value with the cache-control header.

While the client might show stale data to the user if this header is missing.

#### Vary

The server sends different responses for the same URL based on request headers. For example, user-agent, accept-encoding, or accept-language. So there are different cache versions for the same URL.

This header tells the cache which request headers affect the server’s response. Thus ensuring cache to serve the right version of a resource to different clients.

[![Vary HTTP Header](https://substackcdn.com/image/fetch/$s_!rGdX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0eeb01f3-2cc8-43f5-acf5-57adb03202b1_1002x248.png)](https://substackcdn.com/image/fetch/$s_!rGdX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0eeb01f3-2cc8-43f5-acf5-57adb03202b1_1002x248.png)Vary HTTP Header

Here’s an example:
    
    
    Vary: User-Agent

  * It means the server might send different responses for desktop and mobile users.

The cache might serve the wrong version of a site if this header is missing. Thus breaking pages or showing wrong language.

#### Accept-Ranges

It tells the client whether the server supports partial requests. It’s useful for downloading parts of a large media file and for streaming media.

[![Accept-Ranges HTTP Header](https://substackcdn.com/image/fetch/$s_!5pZJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9f72088f-cbdb-4696-91a0-379952103bf5_1015x248.png)](https://substackcdn.com/image/fetch/$s_!5pZJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9f72088f-cbdb-4696-91a0-379952103bf5_1015x248.png)Accept-Ranges HTTP Header

Here’s an example:
    
    
    Accept-Ranges: bytes

  * It means the server supports partial requests in bytes.

The client assumes the server doesn’t support partial requests if this header is missing. Thus downloading the file in full.

#### Content-Range

It indicates which part of a resource is being sent in response to a partial request. This lets the client assemble the resource correctly from different parts.

[![Content-Range HTTP Header](https://substackcdn.com/image/fetch/$s_!-cDC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe58124a1-6b1f-481b-9abc-ebbc7ea1428b_1015x248.png)](https://substackcdn.com/image/fetch/$s_!-cDC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe58124a1-6b1f-481b-9abc-ebbc7ea1428b_1015x248.png)Content-Range HTTP Header

Here’s an example:
    
    
    Content-Range: bytes 0-499/1234

  * It means this response contains bytes 0 through 499 of a resource that is 1234 bytes in total.

The client won't know the part it received if this header is missing. Thus it might fail to assemble the resource correctly.

#### Content-Security-Policy

It tells the browser which sources are allowed for scripts, styles, and images. Thus preventing cross-site scripting (**XSS**) attacks. Think of XSS as a situation where a hacker injects malicious JavaScript into a site to steal data.

Here’s an example:

A hacker could inject malicious JavaScript into the site through a form or comment box. Without this header, the browser runs the script and lets the hacker steal user data.

[![Content-Security-Policy HTTP Header](https://substackcdn.com/image/fetch/$s_!VIOD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faea8a7fa-2323-4cc1-aeb4-d05a30a8df3a_1015x248.png)](https://substackcdn.com/image/fetch/$s_!VIOD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faea8a7fa-2323-4cc1-aeb4-d05a30a8df3a_1015x248.png)Content-Security-Policy HTTP Header

But with this header, the browser loads resources only from trusted sources. Thus blocking malicious scripts.
    
    
    Content-Security-Policy: default-src 'self'; script-src 'self'

  * It means the site only allows content and scripts to load from its own domain.

The browser checks every resource against these rules and blocks anything not on the allowed list.

#### Strict-Transport-Security

It tells the browser to always use HTTPS for this domain for a specific period.

[![Strict-Transport-Security HTTP Header](https://substackcdn.com/image/fetch/$s_!fIAh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F720c8126-2d71-4029-821c-3b02f5e6a959_1015x248.png)](https://substackcdn.com/image/fetch/$s_!fIAh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F720c8126-2d71-4029-821c-3b02f5e6a959_1015x248.png)Strict-Transport-Security HTTP Header

Here’s an example:
    
    
    Strict-Transport-Security: max-age=31536000; includeSubDomains

  * It means the browser uses HTTPS only for this site and its subdomains for the next 1 year.

First-time visitors to a site might get downgraded to insecure HTTP if this header is missing. Thus making them vulnerable to man-in-the-middle attacks. 

So include this header with the maximum age for security.

* * *

### 4\. Payload Headers

These headers describe the content body of a request or response.

Let’s dive in!

#### Content-Type

It indicates the format of the message body, so the receiver knows how to process it.

[![Content-Type HTTP Header](https://substackcdn.com/image/fetch/$s_!L4ND!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc6fd7b06-744d-407d-8997-dc37ac69d271_1015x248.png)](https://substackcdn.com/image/fetch/$s_!L4ND!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc6fd7b06-744d-407d-8997-dc37ac69d271_1015x248.png)Content-Type HTTP Header

Here’s an example:
    
    
    Content-Type: application/json

  * It means that the body of the message is in JSON format.

The client might fail to interpret the content if this header is wrong or missing.

#### Content-Disposition

It tells the browser how to handle the response body. For example, whether to display it inline or download it as a file.

[![Content-Disposition HTTP Header](https://substackcdn.com/image/fetch/$s_!LbuP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F58bc21a5-4568-4db4-b784-05d425a61786_1015x262.png)](https://substackcdn.com/image/fetch/$s_!LbuP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F58bc21a5-4568-4db4-b784-05d425a61786_1015x262.png)Content-Disposition HTTP Header
    
    
    Content-Disposition: attachment; filename="report.pdf"

  * It means the browser prompts the user to download the file.

The browser might just show the file inline if this header is missing. Thus affecting user experience.

#### Content-Length

It tells the receiver how big the body is. For example, when uploading or downloading a file.

[![Content-Length HTTP Header](https://substackcdn.com/image/fetch/$s_!KyJb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F71957f50-552b-49d4-960b-43ef9416f505_1015x248.png)](https://substackcdn.com/image/fetch/$s_!KyJb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F71957f50-552b-49d4-960b-43ef9416f505_1015x248.png)Content-Length HTTP Header

Here’s an example:
    
    
    Content-Length: 348

  * It means the message body is 348 bytes long.

This helps the client show accurate progress bars for uploads or downloads.

The receiver might fail to understand where the body ends if this header is missing. Thus misinterpreting the message.

#### Content-Encoding

It tells the client which compression algorithm was applied to the response body. So the client can decode it correctly.

[![Content-Encoding HTTP Header](https://substackcdn.com/image/fetch/$s_!WfS6!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F15395602-30e3-4ba7-a729-34b171ff2582_1015x248.png)](https://substackcdn.com/image/fetch/$s_!WfS6!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F15395602-30e3-4ba7-a729-34b171ff2582_1015x248.png)Content-Encoding HTTP Header

Here’s an example:
    
    
    Content-Encoding: gzip

  * It means the body got compressed with gzip.

The client might fail to decode the content and show an error if this header is wrong.

#### Content-Language

It tells the client the human language of the response body. It’s useful in multilingual sites to interpret the content correctly.

[![Content-Language HTTP Header](https://substackcdn.com/image/fetch/$s_!P0cq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F706f6ee2-ffb2-48cf-baad-e279f6dfb5a4_1015x248.png)](https://substackcdn.com/image/fetch/$s_!P0cq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F706f6ee2-ffb2-48cf-baad-e279f6dfb5a4_1015x248.png)Content-Language HTTP Header

Here’s an example:
    
    
    Content-Language: en-US

  * It means the response is for US English users.

The client might try to guess the language if this header is missing. Thus causing accessibility or localization issues.

#### Last-Modified

It tells the client the date and time when the resource was last changed on the server. This helps to determine if the cached copy is still fresh.

[![Last-Modified HTTP Header](https://substackcdn.com/image/fetch/$s_!Wf9G!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ca6ee71-a980-4b28-b21a-376b3e5605fc_1015x248.png)](https://substackcdn.com/image/fetch/$s_!Wf9G!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ca6ee71-a980-4b28-b21a-376b3e5605fc_1015x248.png)Last-Modified HTTP Header

Here’s an example:
    
    
    Last-Modified: Tue, 11 Jun 2024 10:00:00 GMT

  * It means the resource was last changed on 11 June 2024 at 10:00 GMT.

The client wouldn’t be able to make conditional requests if this header is missing. Thus wasting bandwidth.

#### ETag

Entity Tag (ETag) is a response header. Think of it as a unique version identifier, like a hash or fingerprint, for a resource.

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fhttp-headers&utm_source=paywall&utm_medium=web&utm_content=171153829)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fhttp-headers&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture