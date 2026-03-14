[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Leaderboard System Design

### Feed: Real-Time Gaming Leaderboard

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 31, 2023

13

2

Share

Thanks for reading [systemdesign.one](https://systemdesign.one/) newsletter. If you're not yet subscribed, let me help you with that:

Subscribe

## High-Level Design

When a player updates the score, the following tasks are executed:

[![](https://substackcdn.com/image/fetch/$s_!0-cF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd7f470bb-405b-4781-b931-ea09001dbeac_2306x1565.webp)](https://substackcdn.com/image/fetch/$s_!0-cF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd7f470bb-405b-4781-b931-ea09001dbeac_2306x1565.webp)

  1. The client establishes a WebSocket connection to the load balancer for instantaneous communication

  2. The load balancer redirects the client's request to the nearest data center

  3. The server modifies the score in the sorted set data structure within Redis

  4. The serverless function updates the entries in the relational database by utilizing the write-behind cache pattern

[Read the full article](https://systemdesign.one/leaderboard-system-design/)

* * *

Thank you for reading System Design Newsletter. This post is public so feel free to share it.

[Share](https://newsletter.systemdesign.one/p/leaderboard-system-design?utm_source=substack&utm_medium=email&utm_content=share&action=share)

13

2

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