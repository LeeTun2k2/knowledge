[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Virtual Waiting Room Architecture That Handles High-Demand Ticket Sales at SeatGeek

### #29: How Virtual Waiting Room Handles Ticket Sales (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Dec 19, 2023

60

7

11

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines the architecture of the virtual waiting room at SeatGeek. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/virtual-waiting-room/?action=share) & I'll send you some rewards for the referrals._

May 2019 - Paris, France.

Eva wants a ticket to the Taylor Swift concert.

Yet she never had luck with buying tickets.

The tickets will be on sale at a specific time on SeatGeek, an online ticket platform.

Also only a limited number of tickets are available.

[![Virtual waiting room; concert](https://substackcdn.com/image/fetch/$s_!ckkU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F97cb91b5-326c-4f18-822f-2a14837bb5f9_800x500.png)](https://substackcdn.com/image/fetch/$s_!ckkU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F97cb91b5-326c-4f18-822f-2a14837bb5f9_800x500.png)

She sees a virtual waiting room page on SeatGeek and then gets to buy the tickets.

She was dazzled.

SeatGeek allows the users who arrive earlier to buy the tickets. And limit the number of users who access the sales page.

A virtual waiting room is like a queue.

Besides auto scaling isn’t fast enough to provide a good user experience with high traffic. So a virtual waiting room is necessary.

[![Absorbing High Traffic and Piping It as Constant Traffic](https://substackcdn.com/image/fetch/$s_!hrd_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F98c526f3-db46-4593-ad0d-7ead15f71b47_800x500.gif)](https://substackcdn.com/image/fetch/$s_!hrd_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F98c526f3-db46-4593-ad0d-7ead15f71b47_800x500.gif)Absorbing High Traffic and Piping It as Constant Traffic

Here’s how the virtual waiting room handles high-demand ticket sales :

  * It absorbs traffic spikes and pipes it as constant traffic to the infrastructure

  * It provides a fair way to sell tickets using the First-in-First-out (**FIFO**) principle

  * It prevents service outages due to high-traffic

* * *

## [Technically (Featured)](https://technically.dev/)

Technically explains software and hardware in a simple and engaging way so you can impress your boss.

[![Technically Newsletter](https://substackcdn.com/image/fetch/$s_!sBmw!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb8f6cea-72fe-4737-a99c-643b703e576c_828x209.png)](https://substackcdn.com/image/fetch/$s_!sBmw!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb8f6cea-72fe-4737-a99c-643b703e576c_828x209.png)

[Try it](https://technically.dev/)

* * *

## Virtual Waiting Room Tech Stack

They run a backend and content delivery network (**CDN**).

They use a static HTML page to create the virtual waiting room and run it on CDN.

The backend consists of AWS Lambda and DynamoDB.

While CDN consists of an in-memory data store, they use Fastly CDN.

_AWS Lambda_ is a serverless computing service. While _DynamoDB_ is a fully managed NoSQL database.

[![Virtual Waiting Room Tech Stack](https://substackcdn.com/image/fetch/$s_!ZFuX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffc04f056-ce7c-4679-aff5-2c9e166c08bf_2363x687.png)](https://substackcdn.com/image/fetch/$s_!ZFuX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffc04f056-ce7c-4679-aff5-2c9e166c08bf_2363x687.png)Virtual Waiting Room Tech Stack

They store the queue in DynamoDB and keep a cache of it in CDN.

Put another way, DynamoDB is set up as the primary data store. And CDN as an edge cache.

The user access validation and routing logic runs on CDN. Because it avoids a request round trip.

Yet the state synchronization between DynamoDB and CDN is necessary.

They use DynamoDB because it supports _Dynamo Stream_ for data synchronization.

Also it offers garbage collection using the time-to-live (**TTL**) attribute. Put another way, the queue gets automatically removed after the ticket sales.

_Dynamo Stream_ captures and triggers events for any change in DynamoDB tables.

But DynamoDB throttles if a single partition receives more requests than it supports. 

So they partition DynamoDB and use the scatter-gather pattern for data queries.

They use Lambda because it simplifies infrastructure management and prevents cascading failures. 

Also it’s easy to scale and provides great support for concurrent executions.

* * *

## Virtual Waiting Room Workflow

The system prevents traffic to the protected zone before the sales start.

Instead the users get routed to the virtual waiting room.

A web page where people buy tickets is called the **protected zone**.

The user gets an entry to the protected zone only if they have an access token. And the access token is given when the sales start.

Also CDN stores the protected zone state in its cache. Thus it's possible to route the user to the virtual waiting room without talking to the backend.

[![Virtual Waiting Room Workflow](https://substackcdn.com/image/fetch/$s_!eZU-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc890e707-a338-4ff1-b03b-f97af62ea133_3661x1116.png)](https://substackcdn.com/image/fetch/$s_!eZU-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc890e707-a338-4ff1-b03b-f97af62ea133_3661x1116.png)Virtual Waiting Room Workflow

Here’s the workflow during sales:

  1. The client traffic gets routed to CDN

  2. The users in the virtual waiting room get a _visitor token_ with an associated timestamp. It contains their arrival time

  3. The virtual waiting room (CDN) opens a websocket connection to the API Gateway. It sends the user's visitor token to the backend

  4. API Gateway talks to Lambda functions

  5. The Lambda function registers the timestamp in DynamoDB. It guarantees the user's position in the queue

  6. The exchanger Lambda function runs periodically. It swaps the visitor token for an _access token_

  7. Dynamo Stream updates the CDN cache to synchronize it with DynamoDB

  8. The notifier Lambda function consumes the Dynamo Stream. It gives the access token to the user via websocket

  9. The access token gets validated and the user gets routed to the protected zone

  10. The user buys the ticket from the protected zone

They use a _leaky bucket_ to control the number of users that enter the protected zone at any point. The leaky bucket is implemented in the backend.

[![Leaky Bucket Storing Access Tokens](https://substackcdn.com/image/fetch/$s_!AE4m!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F850488a0-f280-49d2-9082-23250faa2691_981x867.png)](https://substackcdn.com/image/fetch/$s_!AE4m!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F850488a0-f280-49d2-9082-23250faa2691_981x867.png)Leaky Bucket Storing Access Tokens

The leaky bucket stores access tokens of the users that enter the protected zone.

Put another way, the leaky bucket size equals the protected zone capacity.

So a user gets routed to the virtual room waiting room if the bucket is full. And a 429 HTTP status code gets returned.

But the user gets routed to the protected zone if there is a space in the bucket. Also an access token gets added to the bucket.

They cache the returned status code in CDN to avoid extra requests to the backend if the bucket is full.

Besides each music concert is mapped to a different protected zone. And gets a separate leaky bucket.

[![Streaming Data Changes to CDN](https://substackcdn.com/image/fetch/$s_!7N0L!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb7f6eef-f222-4cbe-b4d4-e042111c3752_3480x493.png)](https://substackcdn.com/image/fetch/$s_!7N0L!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feb7f6eef-f222-4cbe-b4d4-e042111c3752_3480x493.png)Streaming Data Changes to CDN

They use the _transactional outbox pattern_ to update DynamoDB and CDN.

The transactional outbox pattern offers reliable messaging in distributed systems.

Any change to the protected zone table in DynamoDB is streamed using Dynamo Stream. And a message relay Lambda updates the data change in CDN.

They use Dynamo Stream because it provides an ordered flow of information.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/virtual-waiting-room?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![5 Reasons Why Zoom Was Able to Support 300 Million Video Calls a Day](https://substackcdn.com/image/fetch/$s_!bbxV!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb7b8cf4e-f008-4efa-93d9-05143a420d33_1280x720.png)5 Reasons Why Zoom Was Able to Support 300 Million Video Calls a Day[NK](https://substack.com/profile/135589200-nk)·December 11, 2023[Read full story](https://newsletter.systemdesign.one/p/zoom-architecture)](https://newsletter.systemdesign.one/p/zoom-architecture)

[![How to Scale an App to 10 Million Users on AWS](https://substackcdn.com/image/fetch/$s_!yp2f!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F80856407-8ea4-49b0-8d0c-eb9d303356c9_1280x720.gif)How to Scale an App to 10 Million Users on AWS[NK](https://substack.com/profile/135589200-nk)·December 6, 2023[Read full story](https://newsletter.systemdesign.one/p/aws-scale)](https://newsletter.systemdesign.one/p/aws-scale)

* * *

## References

  * https://www.infoq.com/presentations/ticketing-system-virtual-waiting-room/

  * https://developer.fastly.com/solutions/tutorials/waiting-room/

  * https://blog.carbonfive.com/using-redis-sorted-sets-to-build-a-scalable-real-time-web-waiting-list/

  * https://aws.amazon.com/solutions/implementations/virtual-waiting-room-on-aws/

60

7

11

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![proflead's avatar](https://substackcdn.com/image/fetch/$s_!2cvc!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc15f16ad-bd1d-4c60-890a-9b9dbe9e6822_515x515.jpeg)](https://substack.com/profile/167353474-proflead?utm_source=comment)

[proflead](https://substack.com/profile/167353474-proflead?utm_source=substack-feed-item)

[Dec 28, 2023](https://newsletter.systemdesign.one/p/virtual-waiting-room/comment/46135482 "Dec 28, 2023, 1:09 AM")

Liked by Neo Kim

Nice one! Thank you!

ReplyShare

[![Anton Zaides's avatar](https://substackcdn.com/image/fetch/$s_!m9AG!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe37a1acd-c9a1-4968-b60d-907005004d84_1728x1728.jpeg)](https://substack.com/profile/121956618-anton-zaides?utm_source=comment)

[Anton Zaides](https://substack.com/profile/121956618-anton-zaides?utm_source=substack-feed-item)

[Dec 19, 2023](https://newsletter.systemdesign.one/p/virtual-waiting-room/comment/45699690 "Dec 19, 2023, 8:41 PM")

Liked by Neo Kim

Really interesting! And an original approach to solving spikes :)

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/virtual-waiting-room/comment/45699690)

[5 more comments...](https://newsletter.systemdesign.one/p/virtual-waiting-room/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture