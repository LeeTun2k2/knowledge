[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# This Is How Airbnb Adopted HTTP Streaming to Save 84 Million USD in Costs

### #7: Read Now - Awesome Web Optimisation Technique (6 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 21, 2023

33

2

3

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

> A 100 ms delay in website load time decreases sales by 1%. That’s an 84 million USD loss per year for Airbnb.[1](https://newsletter.systemdesign.one/p/what-is-critical-rendering-path#footnote-1-136403116)

Airbnb switched to HTTP streaming to improve its page load performance. It reduced the [first contentful paint](https://web.dev/fcp/) (**FCP**) metric by _100 milliseconds_ on every page.

The FCP measures the time from the start of the page load to the screen render.

_This post outlines how Airbnb reduced latency by 100 ms with HTTP streaming. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/what-is-critical-rendering-path/?action=share)_ _& I'll send you some rewards for the referrals._

* * *

## What Is Critical Rendering Path?

I will give you an overview of the critical rendering path.

A browser performs a sequence of events before the screen render. This sequence of events is called the _critical rendering path_.

[![What is critical rendering path?](https://substackcdn.com/image/fetch/$s_!sldG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6f919f9d-48ff-4115-8fe6-a44b6acfd998_2931x637.png)](https://substackcdn.com/image/fetch/$s_!sldG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6f919f9d-48ff-4115-8fe6-a44b6acfd998_2931x637.png)Critical rendering path

The browser receives HTML and CSS from the server. Then it translates HTML and CSS into DOM and CSSOM. This is called _parsing_.

Document Object Model (**DOM**) is the browser's internal representation of markup. CSS Object Model (**CSSOM**) is the browser's internal representation of styles. And they are independent tree data structures.

The browser parses HTML _incrementally_. So, DOM construction is incremental.

But the browser _blocks_ the screen until the server sends CSS files. Also parsing CSS is _not_ incremental.

The browser then combines the DOM and CSSOM to create the _render tree_.

The determination of the dimensions and node locations by browser is called _layout_.

The browser finally renders the individual nodes to the screen. This is called _painting_.

## What Is HTTP Streaming?

I will teach you HTTP streaming with an analogy. Imagine there is a water tap and a tube. And you want to fill the tube with water. There are 2 options:

_Buffering_ : Take a cup and fill it with water from the tap. And then pour it down the tube. This is called buffering. Everything happens in sequence. In computing, the server writes the entire response into a buffer. And then sends it to the browser.

_Streaming_ : Connect the tap to the tube. And fill it with water. This is called streaming. Many steps happen in parallel. In computing, the server breaks the response into chunks. And sends it out as soon as they are ready. The browser handles the received chunks. This speeds up things. A popular method for HTTP streaming is [Chunked transfer encoding](https://en.wikipedia.org/wiki/Chunked_transfer_encoding).

## How Airbnb Optimised Critical Rendering Path?

According to the _performance golden rule_ , 90% load time of most websites is on the front end. Yet many websites today rely on buffering. Because they want to _avoid_ _extra engineering effort_ to enable streaming.

A switch to streaming might not be worth it if the page content depends on a slow backend query. Because nothing gets rendered until that query finishes. But Airbnb found a _universal_ _use case_ for HTTP streaming to improve performance.

And they reduced their network waterfall by switching to streaming. A waterfall happens when a network request triggers another request.

[![What is critical rendering path? Buffering](https://substackcdn.com/image/fetch/$s_!Tr5Q!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4ba0403f-c5cb-4a4d-a3c8-f486d76733b0_3559x652.png)](https://substackcdn.com/image/fetch/$s_!Tr5Q!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4ba0403f-c5cb-4a4d-a3c8-f486d76733b0_3559x652.png)Buffering; How a browser fetches resources

 _Here is what they found_. The browser sat idle until the server created the entire HTML. Also their website needed to load external files (fonts, CSS, JavaScript) to render. But the browser downloaded the external files only after it received the entire HTML. So, this resulted in a cascade of requests ([waterfall](https://developer.chrome.com/docs/devtools/network/reference/#waterfall)). And poor performance.

[![What is HTTP streaming?](https://substackcdn.com/image/fetch/$s_!20IP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F34592e96-f219-4dde-b734-6ed0301409f7_2342x684.png)](https://substackcdn.com/image/fetch/$s_!20IP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F34592e96-f219-4dde-b734-6ed0301409f7_2342x684.png)Streaming; How a browser fetches resources

The browser parsed HTML incrementally. So, they used [early flush](https://www.stevesouders.com/blog/2009/05/18/flushing-the-document-early/) to _stream HTML_ to the browser. 

_Here's how they did it_. Split the HTML document into 2 chunks and send them separately. They sent the HTML <header> in the first chunk. And it allowed the browser to download external files as soon as it parsed the first chunk.

In the meantime, the server generated the remaining HTML. And this reduced the network waterfall.
    
    
    <html>
        <head>
            <title>Your Page</title>
            <link rel="stylesheet" href="your.css" />
            <script src="script.js" defer></script>
        </head>
        
        **Flush**
    
        <body>
        ...
        </body>
    </html>

But a problem remained - the user saw a blank page until the HTML <body> tag arrived. So, they rendered a loading state if there was no data. And the browser fetched data.

But this created a new problem - they sent an extra network request to fetch data. So, they _streamed_ a third chunk of data instead. It goes only after all the visible content and doesn't block render. This eliminated an extra network request.

They used JavaScript to detect the data chunk arrival. And [MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver) to observe DOM changes. After that insert data into the application's network data store.

Node.js ([Express](https://expressjs.com/) framework) ran on the server. They rewrote their [React](https://react.dev/) component to enable streaming. And rendered the entire HTML document as 3 separate React components.

But they ran into a few problems with enabling HTTP streaming. And resolved the problems by:

  * Disabling response buffering in [nginx](https://www.nginx.com/resources/wiki/start/topics/examples/x-accel/#x-accel-buffering)

  * Disabling [Nagle's algorithm](https://en.wikipedia.org/wiki/Nagle%27s_algorithm) in the [haproxy](https://www.haproxy.com/documentation/hapee/latest/onepage/#4.2-option%20http-no-delay) load balancer. This allowed chunks to reach the browser unaltered

## Takeaways

The takeaways from this case study are:

  * Stream HTML. It allows incremental construction of the page. And enables the early discovery of external files

  * Code split CSS files. And inline critical styles. This improves performance

  * Move non-critical JavaScript out from the critical rendering path. And download JavaScript after the initial render. Because JavaScript parsing is blocking

  * Stream HTML. And request-response approach for the remaining content. This keeps it simple

Google Search uses HTTP streaming to load the headers even before the user types in a full query. This speeds up their render times.

Do you know any websites that use HTTP streaming? Leave a comment.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/khan-academy-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjoxMzU1ODkyMDAsInBvc3RfaWQiOjE0MjMwMjE5MSwiaWF0IjoxNzEwNDE4NzMwLCJleHAiOjE3MTMwMTA3MzAsImlzcyI6InB1Yi0xNTExODQ1Iiwic3ViIjoicG9zdC1yZWFjdGlvbiJ9.ZM-qUdpawpou5aOFMfQEZ2eDPzcZC1Gwhff42xJak98)

* * *

[![How Disney+ Hotstar Scaled to 25 Million Concurrent Users](https://substackcdn.com/image/fetch/$s_!_0xq!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F47227723-e12a-47f3-b67a-16c287afc238_1280x720.png)How Disney+ Hotstar Scaled to 25 Million Concurrent Users[NK](https://substack.com/profile/135589200-nk)·September 17, 2023[Read full story](https://newsletter.systemdesign.one/p/hotstar-scaling)](https://newsletter.systemdesign.one/p/hotstar-scaling)

[![11 Reasons Why YouTube Was Able to Support 100 Million Video Views a Day With Only 9 Engineers](https://substackcdn.com/image/fetch/$s_!oJPb!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F15e36395-797c-40f8-9057-4ac4d97ab4c1_1280x720.png)11 Reasons Why YouTube Was Able to Support 100 Million Video Views a Day With Only 9 Engineers[NK](https://substack.com/profile/135589200-nk)·September 16, 2023[Read full story](https://newsletter.systemdesign.one/p/youtube-scalability)](https://newsletter.systemdesign.one/p/youtube-scalability)

* * *

## References

  * Victor (2023). _Improving Performance with HTTP Streaming_. [online] The Airbnb Tech Blog. Available at: https://medium.com/airbnb-engineering/improving-performance-with-http-streaming-ba9e72c66408.

  * www.stevesouders.com. (n.d.). _Flushing the Document Early | High-Performance Web Sites_. [online] Available at: https://www.stevesouders.com/blog/2009/05/18/flushing-the-document-early/ [Accessed 9 Sep. 2023].

  * Optimization, W. (2013). _Flush HTML Early and Often - flushing HTML to speed up start render times and page rendering_. [online] WebSiteOptimization.com. Available at: https://www.websiteoptimization.com/speed/tweak/flush/ [Accessed 9 Sep. 2023].

  * developer.mozilla.org. (n.d.). _Populating the page: how browsers work - Web Performance | MDN_. [online] Available at: https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work.

  * www.youtube.com. (n.d.). _Critical rendering path - Crash course on web performance (Fluent 2013)_. [online] Available at: [youtube.com](https://www.youtube.com/watch?v=PkOBnYxqj3k) [Accessed 9 Sep. 2023].

[1](https://newsletter.systemdesign.one/p/what-is-critical-rendering-path#footnote-anchor-1-136403116)

[Tests done at Amazon](https://pearanalytics.com/how-webpage-load-time-related-to-visitor-loss/) in 2007 revealed that for every 100ms increase in load time, sales would decrease by 1%. [Airbnb's annual revenue](https://www.zippia.com/airbnb-careers-13934/revenue/) is $8.4B. So, a 100ms delay would cost Airbnb 84 million USD per year. The user behavior might be different from the studies and the numbers are subject to vary.

33

2

3

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Jordan Cutler's avatar](https://substackcdn.com/image/fetch/$s_!tLUQ!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F670bb162-5a63-4fd2-8253-f98c28d446a7_1168x1168.jpeg)](https://substack.com/profile/58854493-jordan-cutler?utm_source=comment)

[Jordan Cutler](https://substack.com/profile/58854493-jordan-cutler?utm_source=substack-feed-item)

[Oct 3, 2023](https://newsletter.systemdesign.one/p/what-is-critical-rendering-path/comment/41236675 "Oct 3, 2023, 9:25 PM")

Liked by Neo Kim

Loved this short but informative read! Very helpful and also appreciated the code example.

Thanks NK!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/what-is-critical-rendering-path/comment/41236675)

[1 more comment...](https://newsletter.systemdesign.one/p/what-is-critical-rendering-path/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture