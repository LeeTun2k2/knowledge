[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Distributed Counter

### Feed: Sharded Counter

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 01, 2023

23

2

Share

Thanks for reading [systemdesign.one](https://systemdesign.one/) newsletter. If you're not yet subscribed, let me help you with that:

Subscribe

## High-Level Design

The distributed counter executes the following operations at a macro level: 

[![](https://substackcdn.com/image/fetch/$s_!C8Iu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6950fd5d-e753-4b34-8c42-a714d14c024f_1553x1116.webp)](https://substackcdn.com/image/fetch/$s_!C8Iu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6950fd5d-e753-4b34-8c42-a714d14c024f_1553x1116.webp)

  * the WebSocket exposes the counter to users via a gateway server

  * the heartbeat signal is utilized to terminate inactive WebSocket connections

  * the counter data type in a database based on Conflict-free Replicated Data Types (CRDT) is leveraged to realize the distributed counter

* * *

**Find out more by reading the full[article](https://systemdesign.one/distributed-counter-system-design/) on my website [systemdesign.one](https://systemdesign.one/)!**

Thank you for reading System Design Newsletter. This post is public so feel free to share it.

[Share](https://newsletter.systemdesign.one/p/distributed-counter?utm_source=substack&utm_medium=email&utm_content=share&action=share)

23

2

Share

Next

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Gerard Beaubrun's avatar](https://substackcdn.com/image/fetch/$s_!00hO!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9a4bf3ee-838e-4378-8929-032bd6ac4c02_1080x1080.jpeg)](https://substack.com/profile/38163621-gerard-beaubrun?utm_source=comment)

[Gerard Beaubrun](https://substack.com/profile/38163621-gerard-beaubrun?utm_source=substack-feed-item)

[Dec 4, 2023](https://newsletter.systemdesign.one/p/distributed-counter/comment/44744493 "Dec 4, 2023, 3:59 AM")

Liked by Neo Kim

Can you give us a commercial application of this system? 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/distributed-counter/comment/44744493)

[1 more comment...](https://newsletter.systemdesign.one/p/distributed-counter/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture