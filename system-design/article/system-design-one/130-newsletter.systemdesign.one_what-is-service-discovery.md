[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# What Is Service Discovery?

### Feed: Service Discovery In Microservices

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jun 30, 2023

6

3

2

Share

Thanks for reading [systemdesign.one](https://systemdesign.one/) newsletter. If you're not yet subscribed, let me help you with that:

Subscribe

## Service Discovery Write Workflow

[![](https://substackcdn.com/image/fetch/$s_!8Af-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6e1292f-b284-4d2c-a5ab-0a8b342ff113_691x409.webp)](https://substackcdn.com/image/fetch/$s_!8Af-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6e1292f-b284-4d2c-a5ab-0a8b342ff113_691x409.webp)

The service provider shares its IP address and port number for registration with the service registry. A periodic heartbeat signal is utilized to indicate the current status of a service instance. Typically, well-behaving service instances will voluntarily deregister upon shutdown. However, if a service abruptly shuts down and no heartbeat signal is detected for an extended duration, the system will automatically classify the instance as inactive.

### Service Discovery Read Workflow

[![](https://substackcdn.com/image/fetch/$s_!l4aB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F271ace38-2810-436f-bb1c-50200a6715c8_691x409.webp)](https://substackcdn.com/image/fetch/$s_!l4aB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F271ace38-2810-436f-bb1c-50200a6715c8_691x409.webp)

By employing the publish-subscribe pattern, reliable service discovery can be implemented. In this approach, service consumers are advised to subscribe to the desired service providers listed in the service registry. As any modifications occur in the service providers, the service registry proactively notifies the subscribed service consumers of these changes.

[Read the detailed article](https://systemdesign.one/what-is-service-discovery/)

* * *

Thank you for reading System Design Newsletter. This post is public so feel free to share it.

[Share](https://newsletter.systemdesign.one/p/what-is-service-discovery?utm_source=substack&utm_medium=email&utm_content=share&action=share)

6

3

2

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raviraj Achar's avatar](https://substackcdn.com/image/fetch/$s_!WJ1D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbc74ad57-dfb8-4073-9fb9-b47354598c8b_1528x1567.jpeg)](https://substack.com/profile/167123667-raviraj-achar?utm_source=comment)

[Raviraj Achar](https://substack.com/profile/167123667-raviraj-achar?utm_source=substack-feed-item)

[Oct 3, 2023](https://newsletter.systemdesign.one/p/what-is-service-discovery/comment/41222805 "Oct 3, 2023, 5:46 PM")

Liked by Neo Kim

This domain is what my team is responsible for in Meta's infrastructure. You described the essence of the space pretty well. Though there are many ways to do this and you have described one possible way here.

Also something to consider is the scale of reads >> scale of writes which makes this an extremely read heavy system.

ReplyShare

[2 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/what-is-service-discovery/comment/41222805)

[2 more comments...](https://newsletter.systemdesign.one/p/what-is-service-discovery/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture