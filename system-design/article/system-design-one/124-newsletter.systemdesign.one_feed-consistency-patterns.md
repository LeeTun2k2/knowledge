[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Consistency Patterns

### Feed: Consistency Models in Distributed Systems

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jul 25, 2023

8

Share

Thanks for reading [systemdesign.one](https://systemdesign.one/) newsletter. If you're not yet subscribed, let me help you with that:

Subscribe

* * *

### **Strong Consistency**

[![](https://substackcdn.com/image/fetch/$s_!54WW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcc9e8fe4-4f9a-4a58-beab-e639a35366c6_711x499.webp)](https://substackcdn.com/image/fetch/$s_!54WW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcc9e8fe4-4f9a-4a58-beab-e639a35366c6_711x499.webp)

In the context of the strong consistency pattern, when read operations are executed on a server, the client must consistently retrieve the data from the most recent write operation.

### **Eventual Consistency**

[![](https://substackcdn.com/image/fetch/$s_!qocD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4853d341-ba4f-4e8f-8234-2223913dbe74_711x499.webp)](https://substackcdn.com/image/fetch/$s_!qocD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4853d341-ba4f-4e8f-8234-2223913dbe74_711x499.webp)

In the eventual consistency pattern, following a write operation on a server, the subsequent read operations on other servers may not immediately return the latest written data.

### **Weak Consistency**

[![](https://substackcdn.com/image/fetch/$s_!KFr_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fead0a2c3-daf6-4ecc-af47-10f3d9c9037d_836x590.webp)](https://substackcdn.com/image/fetch/$s_!KFr_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fead0a2c3-daf6-4ecc-af47-10f3d9c9037d_836x590.webp) Write-behind cache

In the context of the weak consistency pattern, after a write operation is performed on a server, the subsequent read operations on other servers can return either the latest written data or not.

The **tradeoffs of the consistency patterns** can be summarized into the following:

[![](https://substackcdn.com/image/fetch/$s_!0VHn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb88286ab-7c28-4bd8-84ae-c9586dc35cb9_1378x524.png)](https://substackcdn.com/image/fetch/$s_!0VHn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb88286ab-7c28-4bd8-84ae-c9586dc35cb9_1378x524.png)

[Read the detailed article](https://systemdesign.one/consistency-patterns/)

* * *

Thank you for reading System Design Newsletter. This post is public so feel free to share it.

[Share](https://newsletter.systemdesign.one/p/feed-consistency-patterns?utm_source=substack&utm_medium=email&utm_content=share&action=share)

8

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