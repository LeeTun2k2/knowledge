[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Hashnode Generates Feed at Scale

### #33: Learn More - Awesome Personalized Feed Architecture (5 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jan 21, 2024

63

6

7

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how the feed gets created at scale on Hashnode. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/feed-architecture/?action=share) & I'll send you some rewards for the referrals._

June 2020 - Bangalore, India.

Two engineers create a blogging platform for software developers to share knowledge. And called it Hashnode.

Yet they faced difficulties with user engagement and content discovery on their platform.

So they created a _feed_.

[![A User Scrolling Through the Feed to Find New Articles](https://substackcdn.com/image/fetch/$s_!9oOG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb262aa98-e1c8-40c5-8cf0-851e0dcbabee_800x800.gif)](https://substackcdn.com/image/fetch/$s_!9oOG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb262aa98-e1c8-40c5-8cf0-851e0dcbabee_800x800.gif)[A User Scrolling Through the Feed to Find New Articles](https://giphy.com/gifs/insta-feed-scroll-9SJaONs482vvfqp3z9)

A **feed** is a personalized list of articles a user sees when they log into the platform. It’s created based on a user’s interests, preferences, and activities within the platform. Put another way, a feed is a collection of articles from authors that a user follows.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!dbzr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F33b89eb8-2e83-40bc-b278-c648fde37647_800x60.png)](https://substackcdn.com/image/fetch/$s_!dbzr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F33b89eb8-2e83-40bc-b278-c648fde37647_800x60.png)

## Feed Architecture

Here’s how Hashnode generates feed at scale:

### 1\. Ranking Articles

Everyone gets a unique feed because each user follows different authors and tags.

Yet they don’t use machine learning to compute personalized feeds.

Instead they use a _ranking approach_ to keep it simple. Put another way, a score gets assigned to each article based on different factors.

[![Factors Considered by Hashnode in Ranking Articles](https://substackcdn.com/image/fetch/$s_!xMd_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f3361c7-8c5d-48fc-bc2f-1a8bbc8de701_1048x903.png)](https://substackcdn.com/image/fetch/$s_!xMd_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f3361c7-8c5d-48fc-bc2f-1a8bbc8de701_1048x903.png)Factors Considered by Hashnode in Ranking Articles

They _normalize_ the likes, views, and comments on each article to keep the feed diverse.

Besides they reduce the rank of an article if it was already shown on a user's feed to keep the feed fresh.

### 2\. Feed Generation

Their main page displays the feed. So feed generation should be fast.

Yet it’s expensive to create a _personalized_ feed because a user might follow many authors. And articles from each author must be fetched.

Also expensive database queries should be avoided at scale for performance. And fetching the same data many times without reuse wastes computing resources.

So they _precompute_ the feed for _active_ _users_ and store it in the Redis cache. Put another way, the feed of a user gets computed before they visit it and gets served from memory.

An **active user** is someone who has logged into Hashnode in the last few days.

They compute the feed only for _active_ _users_ to reduce memory usage on Redis.

An _event-driven architecture_ is used to keep the services decoupled and flexible. Put another way, an event gets emitted for each user action like publishing or liking an article.

[![Hashnode Feed Architecture](https://substackcdn.com/image/fetch/$s_!RkLZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8a2b268-621c-48d6-9981-5d196eff6da9_3645x1674.png)](https://substackcdn.com/image/fetch/$s_!RkLZ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8a2b268-621c-48d6-9981-5d196eff6da9_3645x1674.png)Hashnode Feed Architecture

They use _serverless architecture_ to precompute the feed. Because it’s easy to scale and avoids the need for server management.

_AWS EventBridge_ collects events like publishing or liking an article.

**EventBridge** is a serverless service that connects application components via events.

They store the articles in disks on MongoDB. It’s a NoSQL database.

And [Upstash](https://upstash.com/) is their serverless Redis cache provider.

The **metadata cache** stores the author's data and their list of active followers.

A Lambda function queries the metadata cache to generate the feed. It queries the database if the cached data is stale.

A**Lambda** is an event-driven computing service.

They use another Lambda function to compute the feed and store it in the Redis cache.

And each computation gets its own Lambda function for low latency. 

They use AWS Step Function's _[distributed map execution](https://aws.amazon.com/blogs/aws/step-functions-distributed-map-a-serverless-solution-for-large-scale-parallel-data-processing/)_ feature to generate the feed in _parallel_.

**[AWS Step Function](https://aws.amazon.com/step-functions/)** is a serverless orchestration service. It keeps the Lambda functions free of extra logic by triggering and tracking them. Put another way, it’s used to coordinate many Lambda functions.

[![Feed Cache; Feed architecture](https://substackcdn.com/image/fetch/$s_!874V!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd04f893a-3b20-47ec-b6cf-06d64b3d52e3_1705x418.png)](https://substackcdn.com/image/fetch/$s_!874V!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd04f893a-3b20-47ec-b6cf-06d64b3d52e3_1705x418.png)Feed Cache

The details of their feed cache implementation are _unknown_. But _[Redis list](https://redis.io/docs/data-types/lists/)_ is an option to store the user's feed. Put another way, each user gets a separate Redis list.

A Lambda function computes the feed when an article gets published. It then inserts the article ID and author ID into each follower’s Redis list.

They could store a few hundred article IDs in the Redis list to avoid frequent re-computation.

Also only article IDs get stored in the feed cache to prevent duplicating the article data. And a separate cache could store the article data.

The _MGET command_ in Redis can be used to fetch many keys via a single operation to display the feed. Because it’s more efficient than separate GET operations. Put another way, they could fetch data about many articles with a single MGET request for speed.

### 3\. Performance

Hashnode is a read-heavy system because articles get read more often than written.

[![Write vs Read Performance of Feed Cache](https://substackcdn.com/image/fetch/$s_!-LS5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd4f9f17-4a46-411a-a05e-901a5334b999_3175x1225.png)](https://substackcdn.com/image/fetch/$s_!-LS5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd4f9f17-4a46-411a-a05e-901a5334b999_3175x1225.png)Write vs Read Performance of Feed Cache

When an author publishes an article, they insert the article ID into each follower's feed. So it’s a linear time operation, O(n).

While the feed of a user gets fetched with a single Redis request. So it’s a constant time operation, O(1).

So the feed generation can be _slow_ for famous authors with many followers.

A possible solution is to merge the article ID only when the follower accesses their feed. Put another way, the feed of a famous author's followers doesn’t get immediately populated.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!dbzr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F33b89eb8-2e83-40bc-b278-c648fde37647_800x60.png)](https://substackcdn.com/image/fetch/$s_!dbzr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F33b89eb8-2e83-40bc-b278-c648fde37647_800x60.png)

Hundreds of articles get published on Hashnode every day. And the feed lets them get the newest articles to interested users.

Now they serve 2.3 million software developers across the world.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/feed-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Cloudflare Was Able to Support 55 Million Requests per Second With Only 15 Postgres Clusters](https://substackcdn.com/image/fetch/$s_!HiKr!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fef97562a-380e-41fa-8f6d-747c86968e77_1280x720.gif)How Cloudflare Was Able to Support 55 Million Requests per Second With Only 15 Postgres Clusters[NK](https://substack.com/profile/135589200-nk)·January 12, 2024[Read full story](https://newsletter.systemdesign.one/p/postgresql-scalability)](https://newsletter.systemdesign.one/p/postgresql-scalability)

[![How Uber Finds Nearby Drivers at 1 Million Requests per Second](https://substackcdn.com/image/fetch/$s_!wM7I!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1eac74fb-4f58-4723-8d82-7f9b7a178ae1_1280x720.gif)How Uber Finds Nearby Drivers at 1 Million Requests per Second[NK](https://substack.com/profile/135589200-nk)·January 4, 2024[Read full story](https://newsletter.systemdesign.one/p/how-does-uber-find-nearby-drivers)](https://newsletter.systemdesign.one/p/how-does-uber-find-nearby-drivers)

* * *

## References

  * [Hashnode's Feed Architecture](https://engineering.hashnode.com/hashnodes-feed-architecture)

  * [The art of feed curating: Our approach to generating personalized feeds that match users' interests](https://engineering.hashnode.com/the-art-of-feed-curating-our-approach-to-generating-personalized-feeds-that-match-users-interests?source=more_series_bottom_blogs)

  * [Hashnode's Overall Architecture](https://engineering.hashnode.com/hashnodes-overall-architecture)

  * [Twitter: Timelines at Scale](https://www.infoq.com/presentations/Twitter-Timeline-Scalability/)

  * [How is Reddit and Hacker News Ranking Algorithms Used?](https://saturncloud.io/blog/how-are-reddit-and-hacker-news-ranking-algorithms-used/#how-hot-ranking-works)

  * [AWS Step Functions vs AWS Lambda](https://blog.clearscale.com/aws-step-functions-vs-aws-lambda/)

  * [Blogging Start-Up Hashnode Raises $6.7 Million in Series A Funding to Power the Creator Economy for Software Developers and Global Tech Community](https://www.businesswire.com/news/home/20210818005143/en/Blogging-Start-Up-Hashnode-Raises-6.7-Million-in-Series-A-Funding-to-Power-the-Creator-Economy-for-Software-Developers-and-Global-Tech-Community#:~:text=Founded%20by%20Fazle%20Rahman%20\(co,and%20build%20a%20global%20community.)

  * [How did Hashnode give a voice to the developer community](https://www.threado.com/resources/writing-their-legacy-how-did-hashnode-give-a-voice-to-the-developer-community)

  * [Giphy Feed GIF](https://giphy.com/gifs/insta-feed-scroll-9SJaONs482vvfqp3z9)

63

6

7

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Jacob Olson's avatar](https://substackcdn.com/image/fetch/$s_!F7KO!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F69b0da22-c221-45f3-a8de-10b38ca26fda_144x144.png)](https://substack.com/profile/131541748-jacob-olson?utm_source=comment)

[Jacob Olson](https://substack.com/profile/131541748-jacob-olson?utm_source=substack-feed-item)

[Jan 25, 2024](https://newsletter.systemdesign.one/p/feed-architecture/comment/48111819 "Jan 25, 2024, 9:29 PM")

Liked by Neo Kim

Is it not insanely inefficient to pre-compute everyone's feed? That just seems wild to me. Is that a common approach or is it unique to HashNode?

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/feed-architecture/comment/48111819)

[![Basma Taha's avatar](https://substackcdn.com/image/fetch/$s_!Gs6J!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F64b2da40-0961-485e-8e00-5880b7a17702_1920x1920.jpeg)](https://substack.com/profile/170622182-basma-taha?utm_source=comment)

[Basma Taha](https://substack.com/profile/170622182-basma-taha?utm_source=substack-feed-item)

[Jan 25, 2024](https://newsletter.systemdesign.one/p/feed-architecture/comment/48061440 "Jan 25, 2024, 8:19 AM")

Liked by Neo Kim

Great article!

I learned a lot of new information. The most shocking part was discovering that feed preferences use a "ranking algorithm," not just machine learning. I always thought it was only machine learning.

It's also interesting that the feed is pre-calculated in cache before a user visits, saving computer resources.

Thanks, Neo, for teaching us this!

ReplyShare

[4 more comments...](https://newsletter.systemdesign.one/p/feed-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture