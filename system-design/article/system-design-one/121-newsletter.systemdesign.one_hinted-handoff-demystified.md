[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Hinted Handoff Demystified

### Feed: High Availability Architecture in Distributed Systems

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Aug 12, 2023

9

1

Share

Thanks for reading this newsletter. If you're not yet subscribed, let me help you with that:

Subscribe

* * *

The workflow of the hinted handoff pattern involves the following steps:

[![Hinted handoff high-level workflow](https://substackcdn.com/image/fetch/$s_!qRDU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3107a4d-ef52-497d-88a6-e6e129589ed1_913x490.gif)](https://substackcdn.com/image/fetch/$s_!qRDU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3107a4d-ef52-497d-88a6-e6e129589ed1_913x490.gif)Hinted handoff high-level workflow

1\. Networking protocols like the gossip protocol is utilized to identify instances of node failures within the system.

2\. Nodes experiencing failures are designated as unavailable.

3\. Requests initially directed at the unavailable node are rerouted toward the backup node.

4\. The backup node is responsible for persistently storing data changes and metadata from the affected node in the form of hints.

5\. The health status of the formerly unavailable target node is established using the gossip protocol.

6\. Once the target node has fully recovered, having regained its healthy status.

7\. The hints accumulated by the backup node are then conveyed to the target node.

8\. The target node proceeds to replay the recorded data mutations.

9\. Subsequently, the backup node proceeds to eliminate the stored hints.

[Read the detailed article](https://systemdesign.one/hinted-handoff/)

* * *

If you know somebody who wants to upskill in system design, share this post with them so they can also grow.

[Share](https://newsletter.systemdesign.one/p/hinted-handoff-demystified?utm_source=substack&utm_medium=email&utm_content=share&action=share)

9

1

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