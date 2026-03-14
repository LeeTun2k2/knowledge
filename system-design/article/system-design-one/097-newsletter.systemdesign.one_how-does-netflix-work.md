[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Does Netflix Work?

### #22: Learn More - What Happens When You Press Play? (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Nov 16, 2023

73

7

7

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Netflix works. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-does-netflix-work/?action=share) & I'll send you some rewards for the referrals._

Netflix has 247 million subscribers.

But 99.99% of them don’t know what happens when they press play on Netflix.

This post outlines how Netflix works.

## Netflix Architecture

Netflix architecture consists of the client, backend, and content delivery network (**CDN**).

[![Netflix architecture](https://substackcdn.com/image/fetch/$s_!sqH2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7220d0e8-73b3-4a18-b14d-bad4c3a4e837_1200x630.png)](https://substackcdn.com/image/fetch/$s_!sqH2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7220d0e8-73b3-4a18-b14d-bad4c3a4e837_1200x630.png)Cartoon Architecture of Netflix

The user interface to browse and play Netflix videos is called the client. It could be a mobile app, a web browser, or a smart TV app.

The backend runs on the AWS cloud. It handles everything that happens _before the play button_ gets pressed. For example, content personalization and payment processing. 

While CDN handles everything that happens _after the play button_ gets pressed. Netflix uses a custom CDN called Open Connect Appliance (**OCA**). It’s used to store and stream videos to the client.

* * *

## [The .NET Weekly (Featured)](https://www.milanjovanovic.tech/?utm_source=neo)

[![The .NET Weekly](https://substackcdn.com/image/fetch/$s_!rekW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ad4ec7f-9613-4484-82bd-d8ac2d05bda9_2000x400.png)](https://substackcdn.com/image/fetch/$s_!rekW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ad4ec7f-9613-4484-82bd-d8ac2d05bda9_2000x400.png)

Want to become a better software engineer? Each week, Milan sends one piece of practical advice about .NET and software architecture. It's a 5-minute read (or less) and comes every Saturday morning. Join 32,000+ engineers here:

[Try it](https://www.milanjovanovic.tech/?utm_source=neo)

* * *

## Netflix on AWS

Netflix runs its backend on AWS because it’s cheaper. It allows Netflix to add servers when needed and return them when not needed anymore. So Netflix pays for only what is needed.

[![Netflix on AWS](https://substackcdn.com/image/fetch/$s_!9uh0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3e57cb3-0e22-4ae0-b59d-70886188520d_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!9uh0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3e57cb3-0e22-4ae0-b59d-70886188520d_1200x630.gif)AWS Elasticity

Netflix runs around 700 microservices and uses DynamoDB and Cassandra databases.

Also they run their backend on many availability zones and AWS regions for reliability.

* * *

## Netflix Open Connect Appliance

Netflix created its own CDN for storing and streaming videos. Because they wanted the highest network efficiency.

[![Netflix Open Connect Appliance](https://substackcdn.com/image/fetch/$s_!lv4n!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffb592fa2-ea61-4d9f-ac96-e340db91e299_2445x1400.png)](https://substackcdn.com/image/fetch/$s_!lv4n!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffb592fa2-ea61-4d9f-ac96-e340db91e299_2445x1400.png)[Open Connect Appliance](https://arstechnica.com/information-technology/2022/10/redditor-acquires-decommissioned-netflix-cache-server-with-262tb-of-storage/)

They built OCA with commodity hardware and optimized it for delivering large files. It runs the FreeBSD operation system and Nginx webserver.

When you press play, the Netflix client finds the nearest OCA and streams the video from it.

Besides Netflix client automatically switches to another OCA if the network gets overloaded or if the active OCA fails.

[![Netflix OCA Streaming](https://substackcdn.com/image/fetch/$s_!JzF5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffc2c76ff-67ee-4750-938f-076a4b662e25_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!JzF5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffc2c76ff-67ee-4750-938f-076a4b662e25_1200x630.gif)Netflix Streaming Content from OCA

Netflix keeps OCA closer to your house by installing it at your internet service provider (**ISP**). This allows Netflix to scale and offer the best streaming experience. The ISP is the one that provides you with home internet like Vodafone, Comcast, or Verizon.

Also Netflix would consume more bandwidth if ISPs didn’t install OCA. And cause network congestion. Put another way, ISPs would have to buy more network capacity without OCA. 

So it's a win-win situation for both ISPs and Netflix.

Besides Netflix predicts the videos you’ll want to watch and store them in OCA. The videos then get copied to the OCA during off-peak hours one day in advance to reduce bandwidth usage for ISPs. 

The benefits of Open Connect Appliance are:

  * High-quality streams by controlling the entire video path

  * Less expensive compared to cloud-based CDN

  * More scalable by owning the system

* * *

## Netflix Transcoding

Netflix supports 2200 different types of devices. But each device has a specific video format that works best on it. So they convert each video into many formats. 

When you press play, Netflix determines the client that you use and fetches the specific file for your device type.

[![Netflix transcoding](https://substackcdn.com/image/fetch/$s_!Ebml!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0f9634ab-b24e-45bd-ba8a-1a7f404b4c07_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!Ebml!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0f9634ab-b24e-45bd-ba8a-1a7f404b4c07_1200x630.gif)Transcoding Video

Production houses and studios send high-definition videos to Netflix. The original video gets stored on AWS S3 servers. Netflix then converts the video into many formats to support different devices. 

The process of converting a video into a different format is called transcoding. The videos get transcoded in parallel for fast delivery. 

Also each video gets split into many chunks to support adaptive bitrate streaming. Adaptive bitrate streaming allows switching video quality as network conditions change. 

A poor network condition results in a lower-resolution stream. While a stable network results in a higher-resolution stream. 

Besides a special code gets added to the files to prevent piracy. It is called digital rights management (**DRM**). It encrypts the video content and prevents unauthorized access.

* * *

## How Does Netflix Stream Video?

[![How Does Netflix Stream Video?](https://substackcdn.com/image/fetch/$s_!q25V!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda3df48b-bbe2-48f6-8a64-e87887dcf3f6_1200x630.png)](https://substackcdn.com/image/fetch/$s_!q25V!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda3df48b-bbe2-48f6-8a64-e87887dcf3f6_1200x630.png)What Happens When You Press Play?

Here is how Netflix streams a video when you press play:

  1. Netflix client sends a play request to the backend service on AWS

  2. The backend service returns the URLs to the 10 best OCA servers based on your IP address

  3. Netflix client intelligently selects the best OCA. It does that by testing the network connection quality

  4. Netflix client connects to the OCA and streams the video

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-does-netflix-work?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![Microservices Lessons From Netflix](https://substackcdn.com/image/fetch/$s_!ysX3!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F18aa57f7-e167-45f9-bb6f-186efb77a57a_1280x720.png)Microservices Lessons From Netflix[NK](https://substack.com/profile/135589200-nk)·October 31, 2023[Read full story](https://newsletter.systemdesign.one/p/netflix-microservices)](https://newsletter.systemdesign.one/p/netflix-microservices)

[![How Discord Boosts Performance With Code-Splitting](https://substackcdn.com/image/fetch/$s_!WoBE!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2586dc0c-2831-4bef-aa1a-b2e44326ad5a_1280x720.png)How Discord Boosts Performance With Code-Splitting[NK](https://substack.com/profile/135589200-nk)·November 8, 2023[Read full story](https://newsletter.systemdesign.one/p/what-is-code-splitting-in-react)](https://newsletter.systemdesign.one/p/what-is-code-splitting-in-react)

* * *

## References

  * https://openconnect.netflix.com/

  * https://netflixtechblog.com/

  * https://medium.com/refraction-tech-everything/how-netflix-works-the-hugely-simplified-complex-stuff-that-happens-every-time-you-hit-play-3a40c9be254b

  * http://highscalability.com/blog/2017/12/11/netflix-what-happens-when-you-press-play.html 

  * https://arstechnica.com/information-technology/2022/10/redditor-acquires-decommissioned-netflix-cache-server-with-262tb-of-storage/

73

7

7

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![ECecillo's avatar](https://substackcdn.com/image/fetch/$s_!IqtW!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1bfd98d1-d937-447e-8202-eb00d1074449_1024x1024.webp)](https://substack.com/profile/186131076-ececillo?utm_source=comment)

[ECecillo](https://substack.com/profile/186131076-ececillo?utm_source=substack-feed-item)

[Nov 29, 2023](https://newsletter.systemdesign.one/p/how-does-netflix-work/comment/44485015 "Nov 29, 2023, 8:42 PM")

Liked by Neo Kim

Straight to the point while giving you some information to dig deeper.

Good job!

However, if I understand correctly once the EC2 instance transcodes the video it gets sent to the OCA that stores it and provides the chunk of video as fast as possible?

Between, do you know how these videos are transferred to the OCA and if there is a lot more process and optimization that goes into it?

ReplyShare

[3 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/how-does-netflix-work/comment/44485015)

[![Jordan Cutler's avatar](https://substackcdn.com/image/fetch/$s_!tLUQ!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F670bb162-5a63-4fd2-8253-f98c28d446a7_1168x1168.jpeg)](https://substack.com/profile/58854493-jordan-cutler?utm_source=comment)

[Jordan Cutler](https://substack.com/profile/58854493-jordan-cutler?utm_source=substack-feed-item)

[Nov 21, 2023](https://newsletter.systemdesign.one/p/how-does-netflix-work/comment/44031272 "Nov 21, 2023, 12:45 PM")

Liked by Neo Kim

One of the best explanations to Netflix architecture. Easy to understand and technically deep as always. Thank you, NK!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-does-netflix-work/comment/44031272)

[5 more comments...](https://newsletter.systemdesign.one/p/how-does-netflix-work/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture