[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Does Google Docs Work 🔥

### #84: Break Into Google Docs Architecture (12 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Aug 22, 2025

∙ Paid

122

6

Share

Unlock access to every deep dive article by becoming a paid subscriber:

Subscribe

* * *

I spent hours studying how Google Docs works so you don't have to. And I wrote this newsletter to make the key concepts simple and easy for you.

  * _[Share this post](https://newsletter.systemdesign.one/p/how-does-google-docs-work/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, there lived a data analyst named Maria.

She emailed draft copies many times to different people to prepare monthly reports.

So she wasted a ton of time and was frustrated.

[![How Does Google Docs Work](https://substackcdn.com/image/fetch/$s_!g3YK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22157752-d417-4aa8-8033-108a4230cfd9_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!g3YK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22157752-d417-4aa8-8033-108a4230cfd9_1200x630.gif)

Until one day, when she decides to use Google Docs for it.

Google Docs allows collaborative editing over the internet. It means many users can work on the same document in real-time.

Yet it’s difficult to implement Google Docs correctly for 3 reasons:

  * Concurrent changes to the same document should converge to the same version.

  * Concurrent changes to the same document must avoid conflicts.

  * Any changes should be visible in real-time to each user.

Also a user should be able to make changes while they’re offline.

[![Handling Concurrency Using a Lock](https://substackcdn.com/image/fetch/$s_!DcjM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5eaef7b8-ddff-4e0e-952b-fd23389006c3_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!DcjM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5eaef7b8-ddff-4e0e-952b-fd23389006c3_1200x630.gif)Handling Concurrency Using a Lock

A simple approach to handle concurrency is using pessimistic concurrency control.

**Pessimistic locking** is a**** mechanism for handling concurrency using a lock. It offers strong consistency, but doesn’t support collaborative editing in real-time. Because it needs a central coordinator to handle data changes, only 1 user can edit at a time. Put simply, only a single document copy is available for write operations at once, while other document copies are read-only.

Besides it doesn’t support offline changes.

[![A Network Request Traveling Across the Earth](https://substackcdn.com/image/fetch/$s_!DrtJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9a1d8d97-4a53-4508-b596-c3b4433ff7d1_1200x630.png)](https://substackcdn.com/image/fetch/$s_!DrtJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9a1d8d97-4a53-4508-b596-c3b4433ff7d1_1200x630.png)A Network Request Traveling Across Earth

Also a network round-trip across the Earth takes 200 milliseconds. 

This might cause a poor user experience. So they do **latency hiding** _._ The idea is to keep a document copy for each user locally and then run operations locally for high responsiveness. Thus creating the illusion of lower latency than reality.

And the system propagates the changes to all users for consistency.

[![Last Write Wins Mechanism](https://substackcdn.com/image/fetch/$s_!9i9b!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcc0d5d70-9bd6-4d02-9e61-7402b5be2af3_1200x630.png)](https://substackcdn.com/image/fetch/$s_!9i9b!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcc0d5d70-9bd6-4d02-9e61-7402b5be2af3_1200x630.png)Last Write Wins Mechanism Overwriting Latest Data

A simple approach for latency hiding is using the **last-write-wins** mechanism.

Yet it resolves a conflict without waiting for coordination by applying the most recent update. So there’s a risk of data loss when there are concurrent changes in high-latency networks.

It might be a good choice when concurrency is low. But it isn’t suitable for this use case.

Onward.

[![How Differential Synchronization Works](https://substackcdn.com/image/fetch/$s_!Eu-r!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5939b5b7-7e3c-45e2-8cdf-d5c3110bdf26_1200x630.png)](https://substackcdn.com/image/fetch/$s_!Eu-r!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5939b5b7-7e3c-45e2-8cdf-d5c3110bdf26_1200x630.png)How Differential Synchronization Works

An alternative approach to latency hiding is through **differential synchronization**.

It keeps a document copy for each user and tracks the changes locally. The system doesn’t send the entire document when something changes, but only the difference (**diff**).

Yet there’s a performance overhead in sending a diff for every change. Also differential synchronization only tracks diffs, and not the reason behind a change. So conflict resolution might be difficult.

While resolving conflicts manually affects the user experience.

[![A Document Represented by Revision Log in Operational Transformation](https://substackcdn.com/image/fetch/$s_!MYBC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc50fc86f-4501-4b71-adaa-6288a7adbb31_1200x630.png)](https://substackcdn.com/image/fetch/$s_!MYBC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc50fc86f-4501-4b71-adaa-6288a7adbb31_1200x630.png)Operational Transformation Representing a Document Using Revision Log

So Google Docs uses an optimistic concurrency control technique called [operational transformation](https://en.m.wikipedia.org/wiki/Operational_transformation) (**OT**). 

OT is an algorithm to show document changes without wait times on high-latency networks. It allows different document copies to accept write operations at once. Also it handles conflict resolution automatically without locks or user interventions. 

Besides OT tolerates divergence among document copies and converges them later.

Think of **operational transformation** as an event-passing mechanism; it ensures each user has the same document state even with unsynchronized changes.

With OT, the system saves each change as an event. Put simply, a change doesn’t affect the underlying character of a document; instead, it adds an event to the revision log. The system then displays the document by replaying the revision log from its start.

Operational transformation saves a document as a set of operations, but it's complex to implement properly.

* * *

## How Does Google Docs Work

Google Docs uses a client-server architecture for simplicity.

Here’s how it works:

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fhow-does-google-docs-work&utm_source=paywall&utm_medium=web&utm_content=156121692)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fhow-does-google-docs-work&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture