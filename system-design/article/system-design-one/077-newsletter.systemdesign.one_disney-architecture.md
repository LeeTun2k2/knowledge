[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Disney+ Scaled to 11 Million Users on Launch Day

### #42: A Simple Introduction to Disney+ Architecture (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Apr 02, 2024

91

8

11

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Disney+ scaled to 11 million users on launch day. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/disney-architecture/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there lived a teenager named Olivia.

[![Disney Architecture](https://substackcdn.com/image/fetch/$s_!t1ZW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4aa1e6fd-7521-420d-a64b-0b0ee6bd3183_800x500.gif)](https://substackcdn.com/image/fetch/$s_!t1ZW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4aa1e6fd-7521-420d-a64b-0b0ee6bd3183_800x500.gif)

She’s a big fan of Marvel and Pixar movies.

But she couldn’t find all the movies she wanted to watch on her streaming service. 

So she was unhappy.

She hears about the launch of a new video streaming service called Disney+.

And got hooked as it had everything she ever wished for.

She buys a Disney+ subscription on the very first day.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

### [Programming Digest (Featured)](https://programmingdigest.net/?utm_source=systemdesignnewsletter)

Programming Digest is a free carefully curated weekly newsletter for software engineers. Read 5 handpicked articles with short summaries. And learn something new every week.

[Try it](https://programmingdigest.net/?utm_source=systemdesignnewsletter)

* * *

## Disney Architecture

Here’s a simplified version of Disney+ architecture:

### 1\. A Tuesday Evening

Olivia streams a movie named Dark Phoenix on Android TV.

[![Infrastructure Running in Many Regions for Low Latency and High Availability](https://substackcdn.com/image/fetch/$s_!gb0V!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F61905d14-e84f-4580-808b-1773e4bf42af_1728x384.png)](https://substackcdn.com/image/fetch/$s_!gb0V!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F61905d14-e84f-4580-808b-1773e4bf42af_1728x384.png)Infrastructure Running in Many Regions for Low Latency and High Availability

They run infrastructure across many regions. Because it helps with failover when infrastructure in a region fails. And routes the user to the nearest region. 

Besides they serve videos from CDN for low latency. The content delivery network (**CDN**) improves performance by caching videos closer to users. While videos get stored in an object storage to lower costs.

### 2\. An Hour Later

Olivia takes a dinner break before finishing the movie.

[![Storing the Last Viewed Timestamp of a Video](https://substackcdn.com/image/fetch/$s_!rNCV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc3b0762e-aa54-48a9-9b6f-229c921c37ae_1902x322.png)](https://substackcdn.com/image/fetch/$s_!rNCV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc3b0762e-aa54-48a9-9b6f-229c921c37ae_1902x322.png)Storing the Last Viewed Timestamp of a Video

They send the video timestamp as a data stream while the user watches the movie. And the server stores it in a key-value database.

They use the Kinesis to stream data. Think of **Kinesis stream** as a data streaming service for processing large amounts of data. 

While a global table in DynamoDB stores the video timestamp for flexibility. A **global table** is a DynamoDB feature that replicates data across many regions automatically. It offers high availability.

[![Disney+ Architecture](https://substackcdn.com/image/fetch/$s_!Ojkb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6614a62a-2d57-4349-b8c4-ec25e90622e8_800x500.gif)](https://substackcdn.com/image/fetch/$s_!Ojkb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6614a62a-2d57-4349-b8c4-ec25e90622e8_800x500.gif)

Olivia resumes the movie on her mobile phone.

So they find the video timestamp where she left off. And streams it from there.

### 3\. Fifty Minutes Later

Olivia finishes watching the movie.

But was a little disappointed with it.

So she searches the movie catalog for something more interesting.

[![Caching Video Metadata Using Cache-Aside Pattern](https://substackcdn.com/image/fetch/$s_!IXYy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F409a3852-edcc-4aa1-b3aa-7e50aec93df9_1281x438.png)](https://substackcdn.com/image/fetch/$s_!IXYy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F409a3852-edcc-4aa1-b3aa-7e50aec93df9_1281x438.png)Caching Video Metadata Using Cache-Aside Pattern

They store the movie catalog in a document data store. Because it offers a flexible schema for movie metadata and user reviews.

Also they cache the popular queries in a key-value database. It buffers requests, thus reducing the load on the document data store.

Although DynamoDB isn't the best choice, they use it for caching. Because they couldn't predict the user traffic during launch. And scaling an in-memory cache would be extra effort and difficult.

Besides DynamoDB keeps things simple as they already used it in their architecture. That means reduced operational complexity.

Yet Olivia couldn’t find any interesting movies to watch.

### 4\. Some Minutes Later

So she starts exploring the Disney+ home page.

And sees the recommendations list.

[![High-Level Architecture of Disney+ Recommendations](https://substackcdn.com/image/fetch/$s_!U4od!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F23feabef-8540-449a-a6bf-3cde397e7b45_2296x322.png)](https://substackcdn.com/image/fetch/$s_!U4od!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F23feabef-8540-449a-a6bf-3cde397e7b45_2296x322.png)High-Level Architecture of Disney+ Recommendations

They use machine learning to recommend movies. It's based on various factors like location and watch history. Imagine **machine learning** as studying data patterns to make better predictions.

They send this list as a data stream and do some extra processing on it for correctness. After that, they store the result in a key-value database.

Olivia finds an interesting movie named Lilo & Stitch.

But has little time in the day to watch it.

### 5\. Time for Bed

So she adds that movie to her watchlist.

[![Storing the User Watchlist in a Key-Value Database](https://substackcdn.com/image/fetch/$s_!gZwZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0dd16742-69af-45c2-a95e-560e9927a316_1142x322.png)](https://substackcdn.com/image/fetch/$s_!gZwZ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0dd16742-69af-45c2-a95e-560e9927a316_1142x322.png)Storing the User Watchlist in a Key-Value Database

They store the movies put on the watchlist in a key-value database. 

And uses the global table in DynamoDB to keep it simple. This means the watchlist is automatically synchronized across many regions. So a failover wouldn’t show an outdated watchlist to the user.

[![Disney Plus Architecture](https://substackcdn.com/image/fetch/$s_!LhBX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F46d7ea60-aa8f-4cde-969e-0ebe63821676_800x500.gif)](https://substackcdn.com/image/fetch/$s_!LhBX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F46d7ea60-aa8f-4cde-969e-0ebe63821676_800x500.gif)

While Olivia was excited about the movie days to come. 

And went happily to bed.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

DynamoDB automatically partitions a table as traffic grows. Yet partitioning takes time. And the traffic gets throttled if it exceeds a specific limit before partitioning occurs. So they _pre-partitioned_ the tables before launch to avoid throttling. And autoscaled the database to handle growing traffic.

Besides the metadata of a popular movie gets more read traffic than others. So they replicate the metadata of a video across many database partitions. And randomly route the reads to a partition to prevent the hot shard problem.

Disney+ has grown to around 149 million users. And remains one of the biggest video streaming services in the market.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/disney-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Amazon Scaled E-commerce Shopping Cart Data Infrastructure](https://substackcdn.com/image/fetch/$s_!CMY6!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbd4f25c3-3e0c-42fa-bac0-446904284738_1280x720.gif)How Amazon Scaled E-commerce Shopping Cart Data Infrastructure[Neo Kim](https://substack.com/profile/135589200-neo-kim)·March 26, 2024[Read full story](https://newsletter.systemdesign.one/p/amazon-dynamo-architecture)](https://newsletter.systemdesign.one/p/amazon-dynamo-architecture)

[![How Tinder Scaled to 1.6 Billion Swipes per Day](https://substackcdn.com/image/fetch/$s_!gLsW!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fad4f699a-dc60-4921-a6c2-e40d861617c0_1280x720.gif)How Tinder Scaled to 1.6 Billion Swipes per Day[Neo Kim](https://substack.com/profile/135589200-neo-kim)·March 19, 2024[Read full story](https://newsletter.systemdesign.one/p/tinder-architecture)](https://newsletter.systemdesign.one/p/tinder-architecture)

* * *

### References

  * [AWS re: Invent 2020: How Disney+ scales globally on Amazon DynamoDB](https://www.youtube.com/watch?v=TCnmtSY2dFM&t=134s)

  * [A Disney Technology Blog](https://medium.com/disney-streaming)

  * [DynamoDB: Read/write capacity mode](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html#HowItWorks.OnDemand)

  * [Amazon DynamoDB global tables](https://aws.amazon.com/dynamodb/global-tables/)

  * [How Disney+ is Changing the Streaming Industry through Industrial Convergence](https://www.diggitmagazine.com/papers/disney-changing-streaming-industry)

  * [Wikipedia: Disney+](https://en.wikipedia.org/wiki/Disney%2B)

  * [What is Amazon CloudFront?](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

  * [Document Database versus Relational Database: how to choose?](https://softwareengineering.stackexchange.com/questions/90877/document-database-versus-relational-database-how-to-choose)

  * [Disney+ surpasses 10 million subscribers on the first day](https://www.theverge.com/2019/11/13/20963172/disney-plus-subscribers-10-million-star-wars-marvel-pixar-launch)

91

8

11

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Daniel's avatar](https://substackcdn.com/image/fetch/$s_!i9GT!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fpurple.png)](https://substack.com/profile/17199370-daniel?utm_source=comment)

[Daniel](https://substack.com/profile/17199370-daniel?utm_source=substack-feed-item)

[Apr 2, 2024](https://newsletter.systemdesign.one/p/disney-architecture/comment/53007496 "Apr 2, 2024, 1:46 PM")

Liked by Neo Kim

Love your posts but imho, you use too many short sentences, it really takes me as a reader out of the narrative thought sometimes. 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/disney-architecture/comment/53007496)

[![Basma Taha's avatar](https://substackcdn.com/image/fetch/$s_!Gs6J!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F64b2da40-0961-485e-8e00-5880b7a17702_1920x1920.jpeg)](https://substack.com/profile/170622182-basma-taha?utm_source=comment)

[Basma Taha](https://substack.com/profile/170622182-basma-taha?utm_source=substack-feed-item)

[Apr 2, 2024](https://newsletter.systemdesign.one/p/disney-architecture/comment/53011057 "Apr 2, 2024, 2:27 PM")

Liked by Neo Kim

Light yet informative read, Neo! Thank you for putting the effort.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/disney-architecture/comment/53011057)

[6 more comments...](https://newsletter.systemdesign.one/p/disney-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture