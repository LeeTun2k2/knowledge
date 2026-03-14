[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Giphy Delivers 10 Billion GIFs a Day to 1 Billion Users

### #14: And CDN Explained Like You’re Five (3 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Oct 12, 2023

58

15

3

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

Once upon a time, there lived a tiny server.

She had a single purpose in life: to give people dancing cat GIFs and make the internet a fun place.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!1NM9!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F986f251a-2c73-461d-86c2-257d7fe764b4_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!1NM9!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F986f251a-2c73-461d-86c2-257d7fe764b4_1200x630.gif)

People who asked for the dancing cat GIFs lived closer to her. 

So she was able to give it to them fast.

And she was living a Happy life.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!rB5y!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F05326a51-8d6a-43e9-936a-31b239373f8d_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!rB5y!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F05326a51-8d6a-43e9-936a-31b239373f8d_1200x630.gif)

But one day.

An Evil GIF maker, Bob entered the picture.

And ruined it all.

He puts a _famous_ cat GIF on the tiny server.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!KtN2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F145ef264-9ef9-4d1e-ab5b-ac456b93ddab_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!KtN2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F145ef264-9ef9-4d1e-ab5b-ac456b93ddab_1200x630.gif)

So people from many countries started asking her for it.

But most of them lived far away.

So it took her lots of time to give it to them.

And she was Sad.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!sGhz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6df6230-8266-46d9-ab66-86c2b1c19191_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!sGhz!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6df6230-8266-46d9-ab66-86c2b1c19191_1200x630.gif)

So she dials Giphy engineers for help. 

[![cdn explained](https://substackcdn.com/image/fetch/$s_!Fx85!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1a0ec18c-efd5-4467-9de4-ac446f61d39b_1200x630.png)](https://substackcdn.com/image/fetch/$s_!Fx85!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1a0ec18c-efd5-4467-9de4-ac446f61d39b_1200x630.png)

“What’s the big deal?”, said Giphy engineers, “Let’s get you some friends for help”.

And called her new friends _Edge servers_.

Their only job was to keep a GIF copy and give it to people nearby.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!Qp7g!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8bfbeb09-2c78-4ae3-88a5-6e5799bcb2c1_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!Qp7g!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8bfbeb09-2c78-4ae3-88a5-6e5799bcb2c1_1200x630.gif)

Also they had a superpower: they were fast because they stored GIFs in memory.

Edge servers lived in different countries closer to people.

So people who lived far away got the GIF fast from them.

And life was Good again.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!JMO9!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff4efff84-f036-40f2-82b6-e1e71af264dd_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!JMO9!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff4efff84-f036-40f2-82b6-e1e71af264dd_1200x630.gif)

Until one day when Bob returned. 

He showed no Mercy. 

And puts a new _viral_ cat GIF on the tiny server.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!MJmP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5a719d99-ecd1-47e0-8cf1-b3b9dde1c7e7_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!MJmP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5a719d99-ecd1-47e0-8cf1-b3b9dde1c7e7_1200x630.gif)

Phone calls for the tiny server skyrocketed.

Because no edge servers had a copy of the viral GIF.

Yet people all over the world asked them for it.

But the tiny server could attend only a few phone calls simultaneously.

So she was under pressure. And Heartbroken.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!OQfn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F186fcbd0-137b-4f7f-973a-c65923fa2d51_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!OQfn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F186fcbd0-137b-4f7f-973a-c65923fa2d51_1200x630.gif)

She picks up the phone and calls Giphy engineers for help.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!1pWY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff6a8eeff-8bac-4b4b-bd65-eb2b7eefafdd_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!1pWY!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff6a8eeff-8bac-4b4b-bd65-eb2b7eefafdd_1200x630.gif)

This time Giphy engineers thought hard. And came up with a simple idea.

“Did you make any best friends?”, asked Giphy engineers.

“Oh, I bonded with a few of the edge servers”, replied the tiny server.

Then they did something Magical.

“You talk only to your best friends from now on”, said Giphy engineers.

And called her best friends the _Shield servers_.

Their only job was to protect her.

From that day on, the tiny server had more time for herself. Because only _shield servers_ called her for the viral GIF.

While _edge servers_ rang the shield servers for the viral GIF.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!f0zT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feff6e39b-c98e-4bf1-be32-e1a0e13968a4_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!f0zT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feff6e39b-c98e-4bf1-be32-e1a0e13968a4_1200x630.gif)

And everyone lived Happily ever after.

[![cdn explained](https://substackcdn.com/image/fetch/$s_!1dHa!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe3b55824-c668-47cf-b0d5-bae3422f25ea_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!1dHa!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe3b55824-c668-47cf-b0d5-bae3422f25ea_1200x630.gif)

[![](https://substackcdn.com/image/fetch/$s_!q0gO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb57f4f8-d6a5-46bb-8af7-8e8a9fd39a43_1922x542.png)](https://substackcdn.com/image/fetch/$s_!q0gO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb57f4f8-d6a5-46bb-8af7-8e8a9fd39a43_1922x542.png)

Using edge servers to deliver GIFs faster is called the Content Delivery Network (**CDN**).

Giphy uses Fastly CDN.

It helped them to handle 100 thousand requests per second. And deliver 10 billion GIFs a day to more than 1 billion people.

Besides CDN reduced their latency and server costs.

Also AWS S3 stores GIFs. And DynamoDB stores GIF metadata.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/cdn-explained?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

* * *

Did you enjoy this post? Then don’t forget to hit the Like button ❤️

**Reference** : https://engineering.giphy.com/how-giphy-uses-fastly-to-achieve-global-scale/

* * *

[![How LinkedIn Adopted Protocol Buffers to Reduce Latency by 60%](https://substackcdn.com/image/fetch/$s_!_Bj-!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3cd2544e-d467-455c-82b3-6483ea5e55e6_1280x720.png)How LinkedIn Adopted Protocol Buffers to Reduce Latency by 60%[NK](https://substack.com/profile/135589200-nk)·October 8, 2023[Read full story](https://newsletter.systemdesign.one/p/protocol-buffers-vs-json)](https://newsletter.systemdesign.one/p/protocol-buffers-vs-json)

[![Top 5 Caching Patterns](https://substackcdn.com/image/fetch/$s_!H-0b!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3b798466-24c2-4c51-a4c1-42296355a55e_1280x720.png)Top 5 Caching Patterns[NK](https://substack.com/profile/135589200-nk)·October 5, 2023[Read full story](https://newsletter.systemdesign.one/p/caching-patterns)](https://newsletter.systemdesign.one/p/caching-patterns)

58

15

3

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Nakul Agrawal's avatar](https://substackcdn.com/image/fetch/$s_!ryS2!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fyellow.png)](https://substack.com/profile/128653236-nakul-agrawal?utm_source=comment)

[Nakul Agrawal](https://substack.com/profile/128653236-nakul-agrawal?utm_source=substack-feed-item)

[Oct 27, 2023](https://newsletter.systemdesign.one/p/cdn-explained/comment/42575537 "Oct 27, 2023, 5:30 AM")

Liked by Neo Kim

Amazing work 

Would be great if you could give a more detailed explantation of CDN 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/cdn-explained/comment/42575537)

[![José Enrique Estremadoyro fort's avatar](https://substackcdn.com/image/fetch/$s_!kYCA!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc43a586d-9944-45d2-b4d5-305064ce9d84_1080x1080.jpeg)](https://substack.com/profile/22739390-jose-enrique-estremadoyro-fort?utm_source=comment)

[José Enrique Estremadoyro fort](https://substack.com/profile/22739390-jose-enrique-estremadoyro-fort?utm_source=substack-feed-item)

[Nov 9, 2023](https://newsletter.systemdesign.one/p/cdn-explained/comment/43344746 "Nov 9, 2023, 1:12 PM")

Liked by Neo Kim

Fastly cdn is not available everywhere worldwide and a fallback mechanism must be implemented if used. 

Some áreas of rural Mexiico and full countries had no coverage when I used it last year

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/cdn-explained/comment/43344746)

[13 more comments...](https://newsletter.systemdesign.one/p/cdn-explained/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture