[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Live Comment System Design

### Feed: Real-Time Live Commenting Platform

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 31, 2023

7

Share

Thanks for reading [systemdesign.one](https://systemdesign.one/) newsletter. If you're not yet subscribed, let me help you with that:

Subscribe

## High-Level Design

The Live Comment service carries out the following operations at a high level:

[![](https://substackcdn.com/image/fetch/$s_!LWse!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd02bb0bb-579a-4768-a5d6-d6846eff57a9_2687x1772.webp)](https://substackcdn.com/image/fetch/$s_!LWse!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd02bb0bb-579a-4768-a5d6-d6846eff57a9_2687x1772.webp)

  * The gateway server distributes live comments to the clients via server-sent events (SSE)

  * Clients subscribe to a live video on the gateway server using Hypertext Transfer Protocol (HTTP)

  * The gateway server maintains an in-memory subscription store to keep track of viewership associations

  * The gateway server subscribes to a live video on the endpoint store

  * The endpoint store manages the list of gateway servers subscribed to a specific live video

  * Heartbeat signals or time to live (TTL) on keys in the subscription store can be used to handle inactive SSE connections

  * When a live video ends, it triggers an update in the endpoint store

  * The dispatcher broadcasts live comments to the dispatcher in peer data centers

[Read the full article](https://systemdesign.one/live-comment-system-design/)

* * *

Thank you for reading System Design Newsletter. This post is public so feel free to share it.

[Share](https://newsletter.systemdesign.one/p/live-comment-system-design?utm_source=substack&utm_medium=email&utm_content=share&action=share)

7

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