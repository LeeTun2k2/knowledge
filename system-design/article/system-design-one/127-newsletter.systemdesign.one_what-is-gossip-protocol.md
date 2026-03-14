[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# What Is Gossip Protocol?

### Feed: Broadcast Algorithm in Distributed Systems

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jul 10, 2023

3

Share

Thanks for reading [systemdesign.one](https://systemdesign.one/) newsletter. If you're not yet subscribed, let me help you with that:

Subscribe

## **Gossip Protocol Explained**

The gossip protocol represents a decentralized method of peer-to-peer communication, employed for transmitting messages within a vast distributed system. At its core, this protocol operates by having each node regularly dispatch a message to a randomly selected subset of other nodes. As a result, there is a high likelihood that the entire system will eventually receive the specific message. To put it simply, the gossip protocol enables nodes to construct a global map by engaging in limited local interactions.

[![](https://substackcdn.com/image/fetch/$s_!CH2H!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1225e99-0358-47eb-b0a2-e0e5994916d0_685x377.webp)](https://substackcdn.com/image/fetch/$s_!CH2H!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1225e99-0358-47eb-b0a2-e0e5994916d0_685x377.webp)

The following tools offer a simulation of the gossip protocol:

  * [serf convergence simulator](https://www.serf.io/docs/internals/simulator.html)

  * [gossip simulator](https://flopezluis.github.io/gossip-simulator/)

[Read the detailed article](https://systemdesign.one/gossip-protocol/)

* * *

Thank you for reading System Design Newsletter. This post is public so feel free to share it.

[Share](https://newsletter.systemdesign.one/p/what-is-gossip-protocol?utm_source=substack&utm_medium=email&utm_content=share&action=share)

3

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