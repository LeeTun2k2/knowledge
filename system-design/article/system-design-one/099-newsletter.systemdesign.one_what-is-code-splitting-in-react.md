[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Discord Boosts Performance With Code-Splitting

### #20: Read Now - Fantastic Frontend Engineering (3 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Nov 08, 2023

32

5

2

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how code-splitting works. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/what-is-code-splitting-in-react/?action=share) & I'll send you some rewards for the referrals._

Discord is a voice and text chat app with 10 million+ daily active users. 

This post outlines how Discord code splits to improve performance.

## Software Bloat

A software bloat is a code that makes the app slower, consumes extra memory, or needs more CPU cycles. A new feature is a software bloat if it has similar problems.

[![Software Bloat](https://substackcdn.com/image/fetch/$s_!enJd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3be9282-a401-4220-803b-801d72e04f04_1024x768.png)](https://substackcdn.com/image/fetch/$s_!enJd!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3be9282-a401-4220-803b-801d72e04f04_1024x768.png)Oxymoron:**** What if the new feature is a bloat?

The app is susceptible to feature creep if it serves a diverse marketplace. Yet most users will likely need only a subset of the features. So unused features can be considered bloat.

A simple approach to reduce bloat is through running split tests to identify the most used features. And then removing the unused features.

But the essential complexity of the app is unavoidable. So code split is a potential technique to reduce bloat. The Discord app code splits to maintain performance while adding new features.

## Code Split

The technique of loading code only when needed is called _code split_.

The Discord app needs code and styles to run. But it takes memory and CPU cycles to parse them. So the server code splits and sends only minimal code and styles needed to run the Discord app. Put another way, code split makes the Discord app faster and consumes less memory.

[![What is code splitting in React](https://substackcdn.com/image/fetch/$s_!YWrK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdce29e52-62ca-42cb-8f3e-808c86ad8953_1649x873.png)](https://substackcdn.com/image/fetch/$s_!YWrK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdce29e52-62ca-42cb-8f3e-808c86ad8953_1649x873.png)Code Split in Discord

The Discord app gets only the necessary JavaScript, CSS, fonts, and images on start. And the server sends the remaining assets as the user navigates through the Discord app.

There are many ways to code split. A good place to start code split is at the app route level. The routes can be bundled into separate chunks using an automation tool like Webpack.

[![Code split assets](https://substackcdn.com/image/fetch/$s_!dB-H!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbddb1663-5c57-41f9-90e4-63294c025215_1154x361.png)](https://substackcdn.com/image/fetch/$s_!dB-H!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbddb1663-5c57-41f9-90e4-63294c025215_1154x361.png)Potential Assets for Code Split

Some web assets that can be code split are:

  * Stylesheets

  * Translations

  * Fonts

The code split chunks get _lazy-loaded_ for high performance. The Discord app receives only the relevant translation file through code split. And it allowed them to add many translation files without having a performance impact.

Some benefits of code split are:

  * Improved performance on mobile clients in an unreliable network

  * Reduced content delivery network (CDN) costs

  * Improved search engine optimization (SEO)

The CDN gets charged based on the bandwidth and number of HTTP requests. So it is important to keep a balance between the number of requests and bandwidth consumed.

Yet code split has the following drawbacks:

  * Overhead due to an increased number of requests

  * Degraded latency due to many requests

**Code splitting via HTTP streaming** is a potential solution to avoid overhead and improve latency.

The key takeaway is code splitting allows adding new features without bloating the app.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![system design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/what-is-code-splitting-in-react?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![Wechat Architecture That Powers 1.67 Billion Monthly Users](https://substackcdn.com/image/fetch/$s_!R02-!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5e56341f-d779-4e43-878b-da70b8f44e57_1280x720.png)Wechat Architecture That Powers 1.67 Billion Monthly Users[NK](https://substack.com/profile/135589200-nk)·October 24, 2023[Read full story](https://newsletter.systemdesign.one/p/chat-application-architecture)](https://newsletter.systemdesign.one/p/chat-application-architecture)

[![Slack Architecture That Powers Billions of Messages a Day](https://substackcdn.com/image/fetch/$s_!Xn1I!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5ff71b0-786e-411c-986d-456e6ea168a0_1280x720.png)Slack Architecture That Powers Billions of Messages a Day[NK](https://substack.com/profile/135589200-nk)·October 26, 2023[Read full story](https://newsletter.systemdesign.one/p/messaging-architecture)](https://newsletter.systemdesign.one/p/messaging-architecture)

* * *

## References

  * https://discord.com/blog/how-discord-maintains-performance-while-adding-features

  * https://userguiding.com/blog/feature-bloat

  * https://www.freecodecamp.org/news/code-splitting-with-higher-order-components-4ac8f094b059/

  * https://crystallize.com/blog/frontend-performance-optimization-react-code-splitting

32

5

2

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Fran Soto's avatar](https://substackcdn.com/image/fetch/$s_!XWMk!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10f90fdb-11ac-48b4-8f51-6a59e07763d2_1149x1149.png)](https://substack.com/profile/170998285-fran-soto?utm_source=comment)

[Fran Soto](https://substack.com/profile/170998285-fran-soto?utm_source=substack-feed-item)

[Nov 9, 2023](https://newsletter.systemdesign.one/p/what-is-code-splitting-in-react/comment/43338222 "Nov 9, 2023, 9:31 AM")

Liked by Neo Kim

Interesting read, NK! 

For small apps this may seem overkill. E.g. some back-office that a few internal people use

But for larger apps at scale, growing more means degrading user experience. I found that Amazon really takes it into account: I have had to create investigate increases of sub millisecond latency in the retail Search page to ask leadership support on launching a feature. I imagine Discord also found the same, people would leave if they can't access information fast

Some interesting read about it: <https://www.gigaspaces.com/blog/amazon-found-every-100ms-of-latency-cost-them-1-in-sales>

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/what-is-code-splitting-in-react/comment/43338222)

[![MICHAEL ODELL's avatar](https://substackcdn.com/image/fetch/$s_!DFX4!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fgreen.png)](https://substack.com/profile/64099103-michael-odell?utm_source=comment)

[MICHAEL ODELL](https://substack.com/profile/64099103-michael-odell?utm_source=substack-feed-item)

[Nov 13, 2023](https://newsletter.systemdesign.one/p/what-is-code-splitting-in-react/comment/43583810 "Nov 13, 2023, 6:38 PM")

Liked by Neo Kim

What every happened to modularization? If you spend half a day thinking about how a software project is going to grow, it should be pretty clear, pretty quickly that the only way to have a sliver of a chance to stay ahead of the impending success disaster is to PLAN for it. Spend a bit of of effort understanding where the ticking bombs lie. Finally, what ever happened to the understanding that Programming is not “coding” and it is not done with an IDE, or anything else with a keyboard. Rome was not built by “running fast to get away from things that are breaking.” But don’t mind me - I’m a Geezer, and I Geeze professionally.

ReplyShare

[3 more comments...](https://newsletter.systemdesign.one/p/what-is-code-splitting-in-react/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture