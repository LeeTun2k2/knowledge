[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Back of the Envelope

### Feed: Capacity Planning System Design

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 07, 2023

11

6

Share

Thanks for reading [systemdesign.one](https://systemdesign.one/) newsletter. If you're not yet subscribed, let me help you with that:

Subscribe

The following is the back-of-the-envelope calculation for a [URL shortener](https://systemdesign.one/url-shortening-system-design/).

### **Traffic**

The [URL shortener](https://systemdesign.one/url-shortening-system-design/) is a read-heavy service. The Daily Active Users (**DAU**) for writes is 100 million. The Query Per Second (**QPS**) of reads is approximately 100 thousand.

[![](https://substackcdn.com/image/fetch/$s_!Uy-X!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1ae21052-2a33-404e-a0a0-0addd5190e8a_1606x434.png)](https://substackcdn.com/image/fetch/$s_!Uy-X!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1ae21052-2a33-404e-a0a0-0addd5190e8a_1606x434.png)

* * *

### **Storage**

The shortened URL persists by default for 5 years in the data store. Each character size is assumed to be 1 byte. A URL record is approximately 2.5 KB in size. Set the replication factor of the storage to a value of at least three for improved durability and disaster recovery.

[![](https://substackcdn.com/image/fetch/$s_!W4oV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10c1269c-8769-4d7f-849d-501fa19188dc_1288x528.webp)](https://substackcdn.com/image/fetch/$s_!W4oV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10c1269c-8769-4d7f-849d-501fa19188dc_1288x528.webp)

* * *

### **Bandwidth**

Ingress is the network traffic that enters the server (client requests).

[![](https://substackcdn.com/image/fetch/$s_!KDPq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9680af48-2e74-4309-b46c-f32281778b1c_1230x376.webp)](https://substackcdn.com/image/fetch/$s_!KDPq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9680af48-2e74-4309-b46c-f32281778b1c_1230x376.webp)

Egress is the network traffic that exits the servers (server responses).

[![](https://substackcdn.com/image/fetch/$s_!Gn41!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F54662454-5436-4a5a-8b3e-2ea705a788aa_1206x380.webp)](https://substackcdn.com/image/fetch/$s_!Gn41!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F54662454-5436-4a5a-8b3e-2ea705a788aa_1206x380.webp)

* * *

### **Memory**

The URL redirection traffic (egress) is cached to improve the latency. Following the [80/20 rule](https://en.wikipedia.org/wiki/Pareto_principle), 80% of egress is served by 20% of URL data stored on the cache servers. The remaining 20% of the egress is served by the data store to improve the latency. A Time-to-live ([TTL](https://en.wikipedia.org/wiki/Time_to_live)) of 1 day is reasonable.

[![](https://substackcdn.com/image/fetch/$s_!gACE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4304450b-6812-48a4-893a-ff05b1e6fdee_1317x471.webp)](https://substackcdn.com/image/fetch/$s_!gACE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4304450b-6812-48a4-893a-ff05b1e6fdee_1317x471.webp)

* * *

**Find out more by reading the full[article](https://systemdesign.one/back-of-the-envelope/) on my website [systemdesign.one](https://systemdesign.one/)!**

[Share](https://newsletter.systemdesign.one/p/back-of-the-envelope?utm_source=substack&utm_medium=email&utm_content=share&action=share)

11

6

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Vishal borana's avatar](https://substackcdn.com/image/fetch/$s_!Tfxb!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Forange.png)](https://substack.com/profile/5584042-vishal-borana?utm_source=comment)

[Vishal borana](https://substack.com/profile/5584042-vishal-borana?utm_source=substack-feed-item)

[May 16, 2023](https://newsletter.systemdesign.one/p/back-of-the-envelope/comment/16162666 "May 16, 2023, 2:57 AM")

Why do we consider 300 days instead of 365 days while calculating storage?

ReplyShare

[5 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/back-of-the-envelope/comment/16162666)

[5 more comments...](https://newsletter.systemdesign.one/p/back-of-the-envelope/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture