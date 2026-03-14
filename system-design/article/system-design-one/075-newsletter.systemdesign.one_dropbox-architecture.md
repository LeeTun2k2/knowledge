[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Dropbox Scaled to 100 Thousand Users in a Year After Launch

### #43: Break Into Early Architecture of Dropbox (6 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Apr 23, 2024

82

2

8

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the architecture of Dropbox from its early days. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/dropbox-architecture/?action=share) & I'll send you some rewards for the referrals._

_Disclaimer: This post is based on my research and might differ from real-world implementation._

March 2009.

Sunil bought a new iPhone.

[![dropbox system design](https://substackcdn.com/image/fetch/$s_!03Cp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3e43ff9-d398-40a3-b691-db3e7f8adb47_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!03Cp!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3e43ff9-d398-40a3-b691-db3e7f8adb47_1200x630.gif)

But he found it frustrating to copy individual files from desktop to iPhone each time.

This was when he heard about a file-sharing service called Dropbox from a friend.

So he installs it immediately.

And was dazzled by its simplicity in automatically synchronizing files across devices.

* * *

### [MindStream (Featured)](https://www.mindstream.news/subscribe?utm_source=CP&utm_medium=CP&utm_campaign=SystemDesign)

Struggling to keep up with AI? Then you need Mindstream! With daily news, polls, tool recommendations and so much more, join 120,000+ others reading the best AI newsletter around!

Did I mention it’s completely free?

Not only that, but you get access to their complete collection of 25+ AI resources when you join!

[Join now and master AI!](https://www.mindstream.news/subscribe?utm_source=CP&utm_medium=CP&utm_campaign=SystemDesign)

* * *

Dropbox is a cloud storage service with ACID requirements:

  * Atomicity: A large file uploaded to Dropbox must be available as a whole

  * Consistency: A file must be updated for different clients at the same time. While a deleted file in a shared folder shouldn’t be visible to any client

  * Isolation: Offline operations on files should be allowed

  * Durability: Data stored in Dropbox shouldn’t get damaged

They scaled Dropbox to 100 thousand users with 9 engineers. And was able to handle millions of file synchronizations each day. Yet synchronizing files across many devices efficiently is hard.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Dropbox Architecture

Here’s how Dropbox scaled their architecture:

### 1\. Uploading and Downloading Files

Dropbox application running on the desktop and mobile is called the **client**. 

They use the client to watch for file changes and handle upload-download logic.

[![Sending File Data in Chunks; Dropbox Architecture](https://substackcdn.com/image/fetch/$s_!OdZW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa418344c-f937-4423-a928-d4da0c00e522_1378x273.png)](https://substackcdn.com/image/fetch/$s_!OdZW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa418344c-f937-4423-a928-d4da0c00e522_1378x273.png)Sending File Data in Chunks

They get about the same amount of reads and writes to the server. That means the number of file downloads and uploads is the same.

Yet network bandwidth is expensive and its usage increases with many clients. So they don’t transfer the entire file on every file change. Instead the client breaks down files into chunks of 4 MB size and uploads them to the server. That means only chunks that got modified are transferred. It reduces the network bandwidth usage and makes it easier to resume upload-download after a network interruption. Think of each **chunk** as a data block. 

While they use rsync to upload data if the data diff is small for simplicity. **Rsync** is an efficient file synchronization and transfer protocol.

[![Assigning a Unique ID to Each Chunk by Hashing Them](https://substackcdn.com/image/fetch/$s_!XeLy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3881f24d-ab2a-4c90-a754-f46c3b57fa96_1241x515.png)](https://substackcdn.com/image/fetch/$s_!XeLy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3881f24d-ab2a-4c90-a754-f46c3b57fa96_1241x515.png)Hashing Each Chunk to Assign a Unique ID

Besides they give each chunk a unique ID by hashing it with SHA-256 to prevent duplicate data storage. They check whether two chunks have the same hash, and if so it gets mapped to the same data object in S3. This means if 2 people upload the same file, they store it only once on the backend to reduce storage costs. **SHA-256** is a secure hashing algorithm with 256 bits.

[![Storing File Data in Amazon S3](https://substackcdn.com/image/fetch/$s_!ebKh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F19478ae6-6e16-484b-96b8-f2f1838e63b5_1154x247.png)](https://substackcdn.com/image/fetch/$s_!ebKh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F19478ae6-6e16-484b-96b8-f2f1838e63b5_1154x247.png)Storing File Data in Amazon S3

They store file data in Amazon S3 and replicate it for durability. While they run the block service to handle the file upload-download from the client. Amazon Simple Storage Service (**S3**) is an object storage.

### 2\. Synchronizing Files Between Devices

They run a metadata database on the client side. It stores information about files, file chunks, devices, and file edits.

And they avoid extra network calls and make files available offline using the client metadata database. Also it helps to reconstruct a downloaded file from its chunks.

[![High-Level Architecture of Dropbox](https://substackcdn.com/image/fetch/$s_!s4vy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd8c7160a-bbb5-4b25-8025-6bb441367207_1741x634.png)](https://substackcdn.com/image/fetch/$s_!s4vy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd8c7160a-bbb5-4b25-8025-6bb441367207_1741x634.png)High-Level Architecture of Dropbox

Besides they run a version of the metadata database on the server using MySQL. 

Yet querying the metadata database often could get expensive. So they set up a caching layer with [Memcached](https://memcached.org/) in front of the database for scalability. It reduced the database load by caching frequently queried data.

Put simply, S3 stores the file data while MySQL stores file metadata. They separate upload-download functionality from file synchronizing functionality for better performance.

[![Synchronizing Metadata Between Client and Server](https://substackcdn.com/image/fetch/$s_!I-Lu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa12617c9-558a-4584-aa6a-bf578892ffe7_2377x534.png)](https://substackcdn.com/image/fetch/$s_!I-Lu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa12617c9-558a-4584-aa6a-bf578892ffe7_2377x534.png)Synchronizing Metadata Between Client and Server

They run the meta service to synchronize the file metadata between client and server. The meta service queries the metadata database to find if the client that just came online has the latest version of a file. It then broadcasts file changes to the client via the notification service.

They store all file edits in a log on the metadata database. While both the client and server keep a version of the log.

[![Updating Metadata Database When File Data Gets Transferred](https://substackcdn.com/image/fetch/$s_!NbJe!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fea56fd59-75fd-432b-aa20-30ba542d467c_1961x441.png)](https://substackcdn.com/image/fetch/$s_!NbJe!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fea56fd59-75fd-432b-aa20-30ba542d467c_1961x441.png)Updating Metadata Database After Transfer of File Data

The block service should update the metadata after a client uploads-downloads any file changes. But the block service doesn't directly interact with the metadata database. Instead it calls the meta service via RPC to update the metadata database. And they encapsulate the database queries in the meta service. Remote Procedure Call (**RPC**) communicates by executing procedures on a remote server.

A file change must be shown to every client watching that file. So they use the notification service to broadcast file changes to the client. They set up the notification service with a 2-level hierarchy for scalability. While the client keeps an open connection with the server using Server-Sent Events (**SSE**). And the server sends file changes to the client.

### 3\. Scalability

They run many instances of the services for high availability and throughput.

And installed a load balancer in front of each service to evenly distribute the load. Also they run a hot backup of the load balancer. And switch to the hot backup if the primary load balancer fails.

They do exponential backoff while reconnecting clients to the server. Otherwise there will be spikes in server load causing degraded performance. This means the client adds extra delay to reconnect after each failed attempt.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

Now they replaced Amazon S3 with an in-house storage system called Magic Pocket. It offers better performance and customization.

And Dropbox remains one of the main market players in the cloud storage industry.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/dropbox-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Lyft Support Rides to 21 Million Users](https://substackcdn.com/image/fetch/$s_!fR28!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9a93f378-9e8d-4e6b-9286-26e3dd628d99_1280x720.gif)How Lyft Support Rides to 21 Million Users[Neo Kim](https://substack.com/profile/135589200-neo-kim)·April 12, 2024[Read full story](https://newsletter.systemdesign.one/p/lyft-engineering)](https://newsletter.systemdesign.one/p/lyft-engineering)

[![How Disney+ Scaled to 11 Million Users on Launch Day](https://substackcdn.com/image/fetch/$s_!4Otw!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb1482536-dd97-46f0-915d-9bc735982731_1280x720.gif)How Disney+ Scaled to 11 Million Users on Launch Day[Neo Kim](https://substack.com/profile/135589200-neo-kim)·April 2, 2024[Read full story](https://newsletter.systemdesign.one/p/disney-architecture)](https://newsletter.systemdesign.one/p/disney-architecture)

* * *

### References

  * [How We've Scaled Dropbox](https://www.youtube.com/watch?v=PE4gwstWhmc)

  * [After four years of SMR storage, here's what we love—and what comes next](https://dropbox.tech/infrastructure/four-years-of-smr-storage-what-we-love-and-whats-next)

  * [Under the hood: Architecture overview](https://www.dropbox.com/business/trust/security/architecture)

  * [Inside the Magic Pocket](https://dropbox.tech/infrastructure/inside-the-magic-pocket)

  * [Streaming File Synchronization](https://dropbox.tech/infrastructure/streaming-file-synchronization)

  * [Scaling to exabytes and beyond](https://dropbox.tech/infrastructure/magic-pocket-infrastructure)

  * [Leading vendors' share in the file-sharing software market worldwide in 2023](https://www.statista.com/statistics/1328893/global-file-sharing-market-share-by-vendor/)

  * [rsync - Linux man page](https://linux.die.net/man/1/rsync)

82

2

8

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Eric Roby's avatar](https://substackcdn.com/image/fetch/$s_!eHwq!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd814203-f8c7-4690-a5ee-4e1bd2e950a5_1252x1252.png)](https://substack.com/profile/159529127-eric-roby?utm_source=comment)

[Eric Roby](https://substack.com/profile/159529127-eric-roby?utm_source=substack-feed-item)

[Apr 27, 2024](https://newsletter.systemdesign.one/p/dropbox-architecture/comment/54923476 "Apr 27, 2024, 2:39 AM")

Liked by Neo Kim

Pretty crazy when you realize that their main “thing” is simply storing objects on a S3 bucket. 

ReplyShare

[1 more comment...](https://newsletter.systemdesign.one/p/dropbox-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture