[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# 6 Proven Guidelines on Open Sourcing From Tumblr

### #10: Check This Out - Incredible Open Sourcing Techniques (4 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 01, 2023

11

6

1

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

Open-source software offers its source code to the public to study or modify.

According to Wikipedia, there are more than 180 thousand open source projects.

_This post outlines the open-sourcing guidelines from Tumblr. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/open-source-guidelines/?action=share) & I'll send you some rewards for the referrals._

You are not interested in open-sourcing a project. But want to design a reliable system using an open-source project? I think this post will help you to choose the right open-source project for system design.

* * *

## Open Source Guidelines

6 proven guidelines on open sourcing from Tumblr are:

[![Open Source Guidelines](https://substackcdn.com/image/fetch/$s_!DVO-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F064720de-aa91-420d-9182-2d7f737a7954_1024x768.png)](https://substackcdn.com/image/fetch/$s_!DVO-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F064720de-aa91-420d-9182-2d7f737a7954_1024x768.png)Open Source Guidelines

### 1\. Reduce Dependencies

A common reason to create an open-source project is the need for a reusable library.

An open-source project must contain _minimal dependencies_. Because it reduces bloat and improves portability.

So a best practice to build an open-source project is by creating a separate repository or a module. Because it prevents accidental dependencies.

### 2\. Focus On Quality

The number of _bugs must be low_. The common techniques to reduce bugs are:

  * Write tests

  * Cover all code paths

  * User testing

Also it is important to gather feedback from architecture and code reviews.

### 3\. Keep a Minimalistic API

> The single most important factor that distinguishes a well-designed module from a poorly designed one is the degree to which the module hides its internal data and other implementation details from other modules.
> 
> \- Joshua Bloch, [Effective Java](https://www.amazon.com/Effective-Java-Joshua-Bloch/dp/0134685997)

An existing functionality of the open-source project shouldn't break with newer releases. Put another way, _backward compatibility_ is important. Otherwise it will break projects that rely on it.

A common approach to maintain backward compatibility is through creating a minimalistic API. Because it allows to change the inner implementation without affecting the external interface.

### 4\. Keep the MVP Simple

MVP ([Minimum Viable Product)](https://en.wikipedia.org/wiki/Minimum_viable_product) is a product version with enough features. And usable to early customers.

[![Open Source Guidelines MVP](https://substackcdn.com/image/fetch/$s_!-ccn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a95df24-1689-40c5-857f-5765c335e78c_816x608.png)](https://substackcdn.com/image/fetch/$s_!-ccn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a95df24-1689-40c5-857f-5765c335e78c_816x608.png)Development iteration

Keep the MVP simple - _a low number of features_ while building the open-source project. But not compromise on the quality.

And release the MVP and build further based on feedback.

### 5\. Grow the Community

An open-source project without a community is like a light bulb without electricity _._ It will not succeed. And the types of users participating in an open-source project are:

  * Users who expect things to work out of the box. Because they have no time to fiddle with code

  * Users who want to build on top of the open-source project

Each user is important. So it is crucial to listen to the _community_.

### 6\. Must Haves

Other must-haves for an open-source project are:

  * Good documentation

  * Demo application

  * Legal compliance

  * Security approval

Do you think there are further important guidelines not covered in this post? Leave a comment.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/caching-patterns?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjoxMzU1ODkyMDAsInBvc3RfaWQiOjEzNzM3NDkwMCwiaWF0IjoxNzA2MzYzMjk2LCJleHAiOjE3MDg5NTUyOTYsImlzcyI6InB1Yi0xNTExODQ1Iiwic3ViIjoicG9zdC1yZWFjdGlvbiJ9.hEJ68nvx8SpLGj3esjZh8PhYO4YjI7CD0hQWNMYnxOw)

* * *

[![11 Reasons Why YouTube Was Able to Support 100 Million Video Views a Day With Only 9 Engineers](https://substackcdn.com/image/fetch/$s_!oJPb!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F15e36395-797c-40f8-9057-4ac4d97ab4c1_1280x720.png)11 Reasons Why YouTube Was Able to Support 100 Million Video Views a Day With Only 9 Engineers[NK](https://substack.com/profile/135589200-nk)·September 16, 2023[Read full story](https://newsletter.systemdesign.one/p/youtube-scalability)](https://newsletter.systemdesign.one/p/youtube-scalability)

[![Tech Stack Evolution at Levels.fyi](https://substackcdn.com/image/fetch/$s_!UC3j!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5b607b54-0dd2-40a2-a252-4b393737908b_1280x720.png)Tech Stack Evolution at Levels.fyi[NK](https://substack.com/profile/135589200-nk)·September 24, 2023[Read full story](https://newsletter.systemdesign.one/p/levels-fyi-google-sheets)](https://newsletter.systemdesign.one/p/levels-fyi-google-sheets)

Do you want to be a **rockstar engineer**? Or are you looking for a mentor to level up your career? [Techlead Mentor Newsletter](https://ravirajachar.substack.com/) by [Raviraj Achar](https://open.substack.com/users/167123667-raviraj-achar?utm_source=mentions) might be helpful to you. He is a Staff Software Engineer at Meta. And offers great career advice for engineers. Consider subscribing to his newsletter.

* * *

## References

  * Tumblr Engineering. (2016). [The Art of Open Sourcing](https://engineering.tumblr.com/post/152294842005/the-art-of-open-sourcing). Tumblr Engineering Blog.

  * Alvi, S. (2016, October 20). [The Art of Open Sourcing](https://medium.com/@sumbulalvi/the-art-of-open-sourcing-c9b87e5905ee). Medium.

  * Tumblr. (n.d). [PermissMe](https://github.com/tumblr/PermissMe). GitHub.

  * https://en.wikipedia.org/wiki/Open-source_software

11

6

1

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Anton Zaides's avatar](https://substackcdn.com/image/fetch/$s_!m9AG!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe37a1acd-c9a1-4968-b60d-907005004d84_1728x1728.jpeg)](https://substack.com/profile/121956618-anton-zaides?utm_source=comment)

[Anton Zaides](https://substack.com/profile/121956618-anton-zaides?utm_source=substack-feed-item)

[Oct 1, 2023](https://newsletter.systemdesign.one/p/open-source-guidelines/comment/41088630 "Oct 1, 2023, 3:32 PM")

Liked by Neo Kim

Thanks! The people who are doing that work... You are our angles! People often complain about Open Source libraries, how they are not maintained properly or having lacking documentation. I say that unless you maintain one library on your own, you have no right to complain :) (and I don't)

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/open-source-guidelines/comment/41088630)

[![Fran Soto's avatar](https://substackcdn.com/image/fetch/$s_!XWMk!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10f90fdb-11ac-48b4-8f51-6a59e07763d2_1149x1149.png)](https://substack.com/profile/170998285-fran-soto?utm_source=comment)

[Fran Soto](https://substack.com/profile/170998285-fran-soto?utm_source=substack-feed-item)

[Oct 2, 2023](https://newsletter.systemdesign.one/p/open-source-guidelines/comment/41128850 "Oct 2, 2023, 6:54 AM")

Liked by Neo Kim

Very interesting to start with an "open-source first" mindset instead of trying to move code around later to release to the public something tightly coupled

Definitively this is a checklist for any library that *may* become open-source later.

Thanks for sharing NK!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/open-source-guidelines/comment/41128850)

[4 more comments...](https://newsletter.systemdesign.one/p/open-source-guidelines/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture