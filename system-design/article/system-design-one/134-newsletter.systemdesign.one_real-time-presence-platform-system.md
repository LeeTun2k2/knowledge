[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Real Time Presence Platform System Design

### Feed: User Online Status Indicator

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 31, 2023

7

Share

Thanks for reading [systemdesign.one](https://systemdesign.one/) newsletter. If you're not yet subscribed, let me help you with that:

Subscribe

## High-Level Design

The real-time platform should display the online status of a client connected to it. The client needs to subscribe to the platform to receive notifications about the status of their connections (friends). At a high level, the presence platform 2 performs the following operations:

[![](https://substackcdn.com/image/fetch/$s_!BywO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe7a53cc0-12d2-4197-8355-c69d384e2b95_1864x1205.webp)](https://substackcdn.com/image/fetch/$s_!BywO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe7a53cc0-12d2-4197-8355-c69d384e2b95_1864x1205.webp)

  1. The subscriber (client) utilizes the HTTP GET method to query the presence service and retrieve the status of a publisher

  2. The presence service then queries the presence database to determine the presence status

  3. The client subscribes to the publisher's status through the real-time platform, establishing an SSE connection

  4. Once the publisher comes online, they establish an SSE connection with the real-time platform

  5. The real-time platform sends a UDP heartbeat signal to the presence service

  6. The presence service queries the presence database to verify if the publisher has recently come online

  7. If the publisher is confirmed as online, the presence service publishes an online event to the real-time platform using the HTTP PUT method

  8. The real-time platform broadcasts the change in the publisher's presence status to subscribers through SSE

The presence service can retrieve the last active timestamp of an offline publisher by querying the presence database. The existing architecture can be leveraged to implement a real-time presence platform.

[Read the full article](https://systemdesign.one/real-time-presence-platform-system-design/)

* * *

Thank you for reading System Design Newsletter. This post is public so feel free to share it.

[Share](https://newsletter.systemdesign.one/p/real-time-presence-platform-system?utm_source=substack&utm_medium=email&utm_content=share&action=share)

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