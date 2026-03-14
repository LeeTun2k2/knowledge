[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Uber Computes ETA at Half a Million Requests per Second

### #26: And How Online Maps Work Explained Like You’re Twelve (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Dec 03, 2023

313

26

21

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Uber computes ETA accurately at half a million requests per second. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/uber-eta/?action=share) & I'll send you some rewards for the referrals._

September 2014 - Prague, Czech Republic.

Maria has an important meeting in 15 minutes.

She calls a taxi.

But the trip took a lot longer than expected. 

So she was late and upset.

[![Uber ETA](https://substackcdn.com/image/fetch/$s_!02ID!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcfa4745e-d969-4ebb-a0bf-c5e15004e320_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!02ID!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcfa4745e-d969-4ebb-a0bf-c5e15004e320_1200x630.gif)

She hears about the new ride-sharing app called Uber from a coworker.

She installs it immediately and was dazzled by the ETA accuracy.

* * *

## [Weekly Olio (Featured)](https://www.weeklyolio.com/?utm_source=strategybreak&utm_medium=newsletter&utm_campaign=partnership)

Knick-knacks on tech, startups, business, and life. Read by 25k+ people from around the world, Weekly Olio already counts the who’s who of investors, startup founders, economists, and industry professionals among their subscribers. You can be one too!

[![Weekly Olio](https://substackcdn.com/image/fetch/$s_!uPAn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F777d7bfe-7270-4db2-8fcd-0aba971ec53c_1200x1200.png)](https://substackcdn.com/image/fetch/$s_!uPAn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F777d7bfe-7270-4db2-8fcd-0aba971ec53c_1200x1200.png)

[Try it](https://www.weeklyolio.com/?utm_source=strategybreak&utm_medium=newsletter&utm_campaign=partnership)

* * *

The time estimated to travel from point A to B is called the Expected Time of Arrival (**ETA**).

Uber computes ETA in 4 scenarios:

  * Eyeball: when the rider enters a destination in the app

  * Dispatch: to [find a car](https://newsletter.systemdesign.one/p/how-does-uber-find-nearby-drivers) to pick up the rider in the shortest waiting time

  * Pick up: to find the time needed to pick up the rider

  * On-trip: to provide live updates on time to reach the destination

[![Uber ETA Use Cases](https://substackcdn.com/image/fetch/$s_!Xnlc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fedb9831d-e023-4502-9717-83d52617aec6_1200x630.png)](https://substackcdn.com/image/fetch/$s_!Xnlc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fedb9831d-e023-4502-9717-83d52617aec6_1200x630.png)ETA Use Cases

A single trip usually takes around 1000 ETA requests.

Yet computing ETA is a difficult problem. Because the distance between the source and destination is not a straight line. 

Instead it consists of complex street networks and highways.

The smart engineers at Uber used simple ideas to solve this difficult problem.

* * *

## Uber ETA

Here’s how Uber computes ETA accurately at extreme scales:

### 1\. Routing Algorithm

They represent the physical map as a graph.

[![Uber ETA; Graph representation of a map](https://substackcdn.com/image/fetch/$s_!OhVW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc0c76343-86c0-4be5-8027-2ba644e5000d_1200x630.png)](https://substackcdn.com/image/fetch/$s_!OhVW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc0c76343-86c0-4be5-8027-2ba644e5000d_1200x630.png)Graph Representation of a Map

Every road intersection is modeled as a node.

While every road segment is modeled as a directed edge.

So computing ETA becomes finding the shortest path in a directed weighted graph.

Dijkstra’s algorithm is known for finding the shortest path in a graph.

But the time complexity of Dijkstra’s algorithm is O(n logn). And _n_ is the number of road intersections or nodes in the graph.

San Francisco Bay Area alone has half a million road intersections. So Dijkstra’s algorithm is not enough at Uber's scale.

**So they** _**partitioned**_**the graph. And then** _**precomputed**_**the best path within each partition.**

[![Uber ETA; Routing algorithms](https://substackcdn.com/image/fetch/$s_!I3oE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F52855d65-b189-432f-ad41-6305f181ee61_1200x630.png)](https://substackcdn.com/image/fetch/$s_!I3oE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F52855d65-b189-432f-ad41-6305f181ee61_1200x630.png) Partitioning and Precomputing Best Path in Partitions

Thus interacting with boundaries of graph partitions is enough to find the best path.

Imagine a dense graph mapped to a circle.

[![Uber ETA; Partitioning](https://substackcdn.com/image/fetch/$s_!e228!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fea822ec1-207c-4f35-b9bc-a12e901d37a0_2382x892.png)](https://substackcdn.com/image/fetch/$s_!e228!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fea822ec1-207c-4f35-b9bc-a12e901d37a0_2382x892.png)Finding the Best Path by Interacting With Only Partition Boundaries

Every single node in the circle must be traversed to find the best path between 2 points.

So time complexity would be the area of the circle: pi * r^2

While partitioning and precomputing make it more efficient. 

It becomes possible to find the best path by interacting with only the nodes on the circle boundary.

So time complexity would be the perimeter of the circle: 2 * pi * r

Put another way, the time complexity to find the best path in the San Francisco Bay Area gets reduced from 500 Thousand to 700.

### 2\. Traffic Information

The traffic on the road segments must be considered to find the fastest path between 2 points.

While traffic is a function of the time of the day, weather, and number of vehicles on the road.

[![Uber ETA; Traffic layer](https://substackcdn.com/image/fetch/$s_!Hoso!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9670ddbb-1e51-4dcd-8a50-2a469628cc26_1200x630.png)](https://substackcdn.com/image/fetch/$s_!Hoso!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9670ddbb-1e51-4dcd-8a50-2a469628cc26_1200x630.png)Populating Edge Weights of the Graph With Traffic Information

They used traffic information to populate the edge weights of the graph. Because it makes the ETA more accurate.

Besides they combined aggregated historical speed information with real-time speed information. Because extra traversal data makes traffic information more accurate.

### 3\. Map Matching

GPS signals can get noisy and sparse especially when the vehicle enters a tunnel.

Also the multipath effect could worsen the GPS signal. The _multipath effect_ occurs when buildings reflect the GPS signal.

A poor GPS signal decreases the ETA accuracy.

[![Uber ETA; Map matching](https://substackcdn.com/image/fetch/$s_!V029!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1122625f-15dc-4b1c-978e-17488e1e2ae6_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!V029!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1122625f-15dc-4b1c-978e-17488e1e2ae6_1200x630.gif)Map Matching

So they do map matching to find the best ETA.

Map matching works by mapping raw GPS signals to actual road segments.

[![Uber ETA; Map matching transformation](https://substackcdn.com/image/fetch/$s_!pNKm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe0a01092-6983-4f8e-9508-fc1b9dc3b486_1898x540.png)](https://substackcdn.com/image/fetch/$s_!pNKm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe0a01092-6983-4f8e-9508-fc1b9dc3b486_1898x540.png)Matching GPS Signals to Road Segments

They use the [Kalman filter](https://en.wikipedia.org/wiki/Kalman_filter) for map matching. It takes GPS signals and matches them to road segments.

Imagine the Kalman filter as a person who makes a correct guess about something's location. The new and old information is taken into consideration for guessing.

Besides they use the [Viterbi algorithm](https://en.wikipedia.org/wiki/Viterbi_algorithm) to find the most probable road segments. It's a dynamic programming approach.

Imagine the Viterbi algorithm as a person who figures out the correct story even if some words were spelled wrong. They do that by looking at the nearby words and fixing the mistakes so that the story makes more sense.

* * *

A rider is likely to avoid future trips if the actual trip time is higher than ETA.

Also more than 18 million Uber trips are completed daily.

So at Uber's scale, a bad ETA could cost them billions of USD in loss.

The current approach allowed them to scale to half a million requests per second.

* * *

👋 PS - Are you unhappy at your current job?

While preparing for system design interviews to get your dream job can be stressful.

Don't worry, I'm working on content to help you pass the system design interview. I'll make it easier - you spend only a few minutes each week to go from 0 to 1. Yet paid subscription fees will be higher than current pledge fees.

So pledge now to get access at a lower price.

_"This newsletter is the perfect place to learn system design from big tech deep dives."_ Alexandre

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/uber-eta?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

* * *

[![How Uber Finds Nearby Drivers at 1 Million Requests per Second](https://substackcdn.com/image/fetch/$s_!wM7I!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1eac74fb-4f58-4723-8d82-7f9b7a178ae1_1280x720.gif)How Uber Finds Nearby Drivers at 1 Million Requests per Second[NK](https://substack.com/profile/135589200-nk)·January 4, 2024[Read full story](https://newsletter.systemdesign.one/p/how-does-uber-find-nearby-drivers)](https://newsletter.systemdesign.one/p/how-does-uber-find-nearby-drivers)

[![What Happens When You Type google.com in the Browser?](https://substackcdn.com/image/fetch/$s_!JDEg!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc4f1940e-7b40-4498-8e3f-3012ae74dc06_1280x720.png)What Happens When You Type google.com in the Browser?[NK](https://substack.com/profile/135589200-nk)·November 26, 2023[Read full story](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser)](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser)

* * *

## References

  * [Uber Tech Day: What's My ETA? The Billion-Dollar Question](https://www.youtube.com/watch?v=FEebOd-Pdwg)

  * [Map Matching at Uber](https://www.youtube.com/watch?v=ChtumoDfZXI)

  * [DeeprETA: An ETA Post-processing System at Scale](https://arxiv.org/pdf/2206.02127.pdf)

  * [Hidden Markov Map Matching Through Noise and Sparseness](https://www.ismll.uni-hildesheim.de/lehre/semSpatial-10s/script/6.pdf)

  * [How Uber Predicts Arrival Times](https://www.youtube.com/watch?v=CJTitzj0qBo)

  * [DeepETA: How Uber Predicts Arrival Times Using Deep Learning](https://www.uber.com/en-DE/blog/deepeta-how-uber-predicts-arrival-times/)

313

26

21

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Katarina Petruzzo's avatar](https://substackcdn.com/image/fetch/$s_!zjpa!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1a063cd1-df9b-4c65-a3e7-7ecae2b7929b_180x320.png)](https://substack.com/profile/174490603-katarina-petruzzo?utm_source=comment)

[Katarina Petruzzo](https://substack.com/profile/174490603-katarina-petruzzo?utm_source=substack-feed-item)

[Dec 4, 2023](https://newsletter.systemdesign.one/p/uber-eta/comment/44765136 "Dec 4, 2023, 3:30 PM")

Liked by Neo Kim

I could surely use this any time my friends ask “what’s your ETA”... I always grossly underestimate how much time I need ha!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/uber-eta/comment/44765136)

[![Jordan Cutler's avatar](https://substackcdn.com/image/fetch/$s_!tLUQ!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F670bb162-5a63-4fd2-8253-f98c28d446a7_1168x1168.jpeg)](https://substack.com/profile/58854493-jordan-cutler?utm_source=comment)

[Jordan Cutler](https://substack.com/profile/58854493-jordan-cutler?utm_source=substack-feed-item)

[Dec 3, 2023](https://newsletter.systemdesign.one/p/uber-eta/comment/44703138 "Dec 3, 2023, 1:46 PM")

Liked by Neo Kim

Great article with simple explanations. Very cool to find out how this works under the hood. Appreciate the article, NK

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/uber-eta/comment/44703138)

[24 more comments...](https://newsletter.systemdesign.one/p/uber-eta/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture