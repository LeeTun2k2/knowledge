[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# 5 Reasons Why Zoom Was Able to Support 300 Million Video Calls a Day

### #28: Learn More - Awesome Zoom Architecture (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Dec 11, 2023

113

10

10

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Zoom architecture supports 300 million video calls per day. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/zoom-architecture/?action=share) & I'll send you some rewards for the referrals._

March 2020 - Berlin, Germany.

Annika moved to a new apartment during lockdown.

She has to attend video calls for work.

Yet she has only mobile internet.

So she was sad.

She receives a Zoom meeting invitation from a coworker the next day.

[![Zoom Scalability](https://substackcdn.com/image/fetch/$s_!x2m2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7532e7fb-b23b-44d7-9aa0-31f4b2685922_800x500.gif)](https://substackcdn.com/image/fetch/$s_!x2m2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7532e7fb-b23b-44d7-9aa0-31f4b2685922_800x500.gif)

She installed the Zoom app and was mind-blown by its video call quality.

* * *

* * *

## Zoom Architecture

Here’s how Zoom supports 300 million video calls a day:

### 1\. Video Streaming

They do adaptive streaming because each device type needs a different video resolution.

Adjusting resolution based on device type and bandwidth is called **adaptive streaming**.

And the number of pixels in a video frame is called **resolution**.

But sending _many video streams for different resolutions isn’t scalable_.

So they use _Scalable Video Coding_ (**SVC**) to stream videos.

Imagine SVC as having a video in Lego blocks. The lower blocks contain the basic picture. While upper blocks contain extra details.

[![Scalable Video Coding; Zoom Architecture](https://substackcdn.com/image/fetch/$s_!9ZcC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0f812575-0618-4481-9d76-5787e5e85ed6_800x500.gif)](https://substackcdn.com/image/fetch/$s_!9ZcC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0f812575-0618-4481-9d76-5787e5e85ed6_800x500.gif)Scalable Video Coding

Put another way, SVC sends a _single video stream that is_ _divided into hierarchical layers_. Each layer holds a different resolution.

The lower layers contain basic information. While upper layers contain extra information for higher resolution.

The _receiving client decodes only specific layers_ that match its device type.

SVC reduces bandwidth usage because there is only a single video stream.

Also it reduces server CPU usage by avoiding the need to encode and decode many video streams.

So SVC video streaming scales well and provides low latency.

### 2\. Video Processing

They _separate video stream processing from routing_.

Also they don’t process video streams on the server because it isn’t scalable.

[![Stream Routing vs Processing; Zoom Architecture](https://substackcdn.com/image/fetch/$s_!l2Tg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F88b9e1fd-639b-4845-9aee-604945e2e436_800x500.png)](https://substackcdn.com/image/fetch/$s_!l2Tg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F88b9e1fd-639b-4845-9aee-604945e2e436_800x500.png)Stream Routing vs Processing

Instead the server only routes the video streams.

While the _client processes it_.

### 3\. Video Routing

They don’t combine video streams from a video call’s participants on the server.

Instead they _send separate video streams from each participant to the client_. The client then decodes them.

So it avoids the need for transcoding on the server.

Converting a video to a different format is called **transcoding**.

[![Separate Video Streams Zoom Architecture](https://substackcdn.com/image/fetch/$s_!L4yn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16e32cc1-9f40-484b-bb42-0abbb52d5951_800x500.png)](https://substackcdn.com/image/fetch/$s_!L4yn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16e32cc1-9f40-484b-bb42-0abbb52d5951_800x500.png)Each Participant Send Separate Video Streams

They do _multimedia routing_ to send video streams with low latency.

The _multimedia router_ finds the best network paths to send video between participants in a video call.

### 4\. Monitoring Quality of Service

The network can be unreliable especially if the user is on mobile internet.

[![Quality of Service Zoom Architecture](https://substackcdn.com/image/fetch/$s_!wO_c!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6dbb0550-b65e-429a-98e7-391c8c1ac205_800x500.png)](https://substackcdn.com/image/fetch/$s_!wO_c!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6dbb0550-b65e-429a-98e7-391c8c1ac205_800x500.png)Client Monitors Quality of Service

So Zoom _client monitors_ the Quality of Service (**QoS**). It does that by measuring data packet loss and latency.

The client then _optimizes the video stream using_ _proprietary algorithms_ to provide the best user experience.

### 5\. Network Awareness

Video calls need faster delivery of data.

So they use User Datagram Protocol (**UDP**).

Imagine UDP as a person sending postcards without a confirmation recipient.

It's a lightweight and connectionless protocol.

Also they set up the client to use TCP, HTTPS, and HTTP as a fallback for consistent user experience.

[![Peer-To-Peer Connection Between 2 Participants in Zoom](https://substackcdn.com/image/fetch/$s_!KgPh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F42aa4008-647a-4f40-9b6b-41597e5b1d68_800x500.png)](https://substackcdn.com/image/fetch/$s_!KgPh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F42aa4008-647a-4f40-9b6b-41597e5b1d68_800x500.png)Peer-to-Peer Connection Between Two Participants in a Video Call

Zoom uses a _peer-to-peer connection_ if there are only 2 participants in the video call. Because it reduces server load and provides low latency.

* * *

They use a _client-server architecture_.

And run microservices on Amazon Web Services (**AWS**).

Zoom client connects to the closest data center for low latency.

[![Zoom Architecture](https://substackcdn.com/image/fetch/$s_!Uc_a!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe8b7df18-2ff2-4763-a21f-c2514faa2b17_800x500.png)](https://substackcdn.com/image/fetch/$s_!Uc_a!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe8b7df18-2ff2-4763-a21f-c2514faa2b17_800x500.png)Zoom Architecture

They use _meeting zones_ to group servers.

While a _zone controller_ manages every activity that occurs within a meeting zone.

They engineered Zoom for video streaming by keeping its architecture simple.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/zoom-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How to Scale an App to 10 Million Users on AWS](https://substackcdn.com/image/fetch/$s_!yp2f!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F80856407-8ea4-49b0-8d0c-eb9d303356c9_1280x720.gif)How to Scale an App to 10 Million Users on AWS[NK](https://substack.com/profile/135589200-nk)·December 6, 2023[Read full story](https://newsletter.systemdesign.one/p/aws-scale)](https://newsletter.systemdesign.one/p/aws-scale)

[![How Uber Computes ETA at Half a Million Requests per Second](https://substackcdn.com/image/fetch/$s_!mMKk!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F24c00d06-7158-43d8-9815-067e3b9c725d_1280x720.gif)How Uber Computes ETA at Half a Million Requests per Second[NK](https://substack.com/profile/135589200-nk)·December 3, 2023[Read full story](https://newsletter.systemdesign.one/p/uber-eta)](https://newsletter.systemdesign.one/p/uber-eta)

* * *

## References

  * [Here’s How Zoom Provides Industry-Leading Video Capacity](https://blog.zoom.us/zoom-can-provide-increase-industry-leading-video-capacity/)

  * [How Zoom's Unique Architecture Powers Your Video First UC Future](https://www.youtube.com/watch?v=5BMbsFqtD0A)

  * [Zoom Expands with Equinix to Future-Proof and Scale Its Video-First, Cloud-Native Architecture](https://www.prnewswire.com/news-releases/zoom-expands-with-equinix-to-future-proof-and-scale-its-video-first-cloud-native-architecture-300962299.html)

  * [Scalable video coding (SVC)](https://trueconf.com/features/core/scalable-video-coding.html)

113

10

10

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Mindi Weik's avatar](https://substackcdn.com/image/fetch/$s_!2UBB!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6290d8e8-b312-4bd2-ad8f-34cf3d05b7e7_400x400.jpeg)](https://substack.com/profile/137458291-mindi-weik?utm_source=comment)

[Mindi Weik](https://substack.com/profile/137458291-mindi-weik?utm_source=substack-feed-item)

[Dec 23, 2023](https://newsletter.systemdesign.one/p/zoom-architecture/comment/45931870 "Dec 23, 2023, 2:56 PM")

Liked by Neo Kim

Excellent. It's clear, concise and informative! Thank you 🙌

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/zoom-architecture/comment/45931870)

[![Akhil's avatar](https://substackcdn.com/image/fetch/$s_!Fjjx!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fea123d69-0fb0-4096-9a1e-cad9af0e4e0a_144x144.png)](https://substack.com/profile/174971998-akhil?utm_source=comment)

[Akhil](https://substack.com/profile/174971998-akhil?utm_source=substack-feed-item)

[Dec 12, 2023](https://newsletter.systemdesign.one/p/zoom-architecture/comment/45230350 "Dec 12, 2023, 4:20 AM")

Liked by Neo Kim

A very useful article with valuable information. A lot is happening behind the scenes of our Zoom meetings. So, are meetings end-to-end encrypted? 😄

ReplyShare

[3 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/zoom-architecture/comment/45230350)

[8 more comments...](https://newsletter.systemdesign.one/p/zoom-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture