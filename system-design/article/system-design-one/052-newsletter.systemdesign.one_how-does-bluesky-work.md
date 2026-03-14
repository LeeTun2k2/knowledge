[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Bluesky Works 🦋

### #65: Break Into Bluesky Architecture (17 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jan 22, 2025

∙ Paid

141

13

18

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines Bluesky architecture; you will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-does-bluesky-work/?action=share) & I'll send you some rewards for the referrals._

_Note: I wrote this post after reading their engineering blog and documentation._

Once upon a time, Twitter had only a few million users.

And each user interaction went through their centralized servers.

So content moderation was easy.

Yet their growth rate was explosive and became one of the most visited sites in the world.

So they added automation and human reviewers to scale moderation.

[![Moderation in a Centralized Architecture](https://substackcdn.com/image/fetch/$s_!8s6E!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0770a392-d349-44c5-b6ea-9daf0703581a_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!8s6E!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0770a392-d349-44c5-b6ea-9daf0703581a_1200x630.gif)Moderation in a Centralized Architecture

Although it temporarily solved their content moderation issues, there were newer problems. Here are some of them:

  * The risk of error increases if a single authority decides the moderation policies.

  * There’s a risk of bias when a single authority makes moderation decisions.

  * Managing moderation at scale needs a lot of effort; it becomes a bottleneck.

So they set up [Bluesky](https://bsky.app/): a research initiative to build a decentralized social network.

Onward.

* * *

A decentralized architecture distributes control across many servers.

[![Popular Decentralized Architectures](https://substackcdn.com/image/fetch/$s_!MWt4!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feaf3db58-1823-41b2-9c76-644ba56a5907_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!MWt4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feaf3db58-1823-41b2-9c76-644ba56a5907_1200x630.gif)Popular Decentralized Architectures

Here are 3 popular decentralized architectures in distributed systems:

  * Federated architecture: client-server model; but different people run parts of the system and parts communicate with each other.

  * Peer-to-Peer architecture: there’s no difference between client and server; each device acts as both.

  * Blockchain architecture: distributed ledger for consensus and trustless interactions between servers.

_Bluesky uses a federated architecture_ _for its familiar client-server model, reliability, and convenience._ So each server could be run by different people and servers communicate with each other over HTTP.

Think of the federated network as email; a person with Gmail can communicate with someone using Protonmail.

Yet building a decentralized social network at scale is difficult.

So smart engineers at Bluesky used simple ideas to solve this hard problem.

* * *

_I wrote a summary of this post (save it for later):_

[![Bluesky Architecture Summary](https://substackcdn.com/image/fetch/$s_!9F4-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd3f9afbd-e80f-4091-9f42-f89914e4874c_1668x636.png)](https://bsky.app/profile/systemdesign.one/post/3lgdgwgaskk26)

* * *

## How Does Bluesky Work

They created a decentralized open-source framework to build social networking apps, called Authenticated Transfer Protocol (**[ATProto](https://atproto.com/)**), and built Bluesky on top of it.

Put simply, Bluesky doesn’t run separate servers; instead, ATProto servers distribute messages to each other.

A user’s data is shared across apps built on ATProto; Bluesky is one of the apps. So if a user switches between apps on ATProto, such as a photo-sharing app or blogging app, they don’t lose their followers ([social graph)](https://en.wikipedia.org/wiki/Social_graph).

[![Bluesky Explained With an Analogy](https://substackcdn.com/image/fetch/$s_!dgSv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6e587a76-97aa-4869-8854-33f695f8daa0_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!dgSv!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6e587a76-97aa-4869-8854-33f695f8daa0_1200x630.gif)Bluesky Explained With an Analogy

Imagine ATProto as Disneyland Park and Bluesky as one of its attractions. A single ticket is enough to visit all the attractions in the park. And if you don’t like one of the attractions, try another one in the park.

Here’s how Bluesky works:

### 1\. User Posts

A [post](https://docs.bsky.app/docs/tutorials/creating-a-post) is a short status update by a user; it includes text and images. 

The text content and timestamp of the post are stored in a **[repository](https://atproto.com/guides/data-repos)**. _Think of the repository as a collection of data published by a single user._ [SQLite](https://www.sqlite.org/) is used as its data layer for simplicity; each repository gets a separate SQLite database.

The data records are encoded in [CBOR](https://cbor.io/), a compact binary format, before storing it in SQLite for low costs.

[![Storing User Data in Repositories](https://substackcdn.com/image/fetch/$s_!Jk4g!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F54c1e71c-88f8-4c76-b1b5-77542f10a603_1200x630.png)](https://substackcdn.com/image/fetch/$s_!Jk4g!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F54c1e71c-88f8-4c76-b1b5-77542f10a603_1200x630.png)Storing User Data in Repositories

Repositories of different users are stored on a **[data server](https://atproto.com/guides/glossary#pds-personal-data-server)** ; they set up many data servers for scale.

[![Internals of Data Server](https://substackcdn.com/image/fetch/$s_!q_UD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F136eeb66-f7b9-40b3-8f70-eac8ac5800eb_1200x630.png)](https://substackcdn.com/image/fetch/$s_!q_UD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F136eeb66-f7b9-40b3-8f70-eac8ac5800eb_1200x630.png)Internals of Data Server

 _A data server exposes HTTP to handle client requests._ Put simply, the data server acts as a proxy for all client interactions. Also it manages user authentication and authorization. The data server includes tooling to automatically apply updates in the federated architecture.

They run 6 million user repositories on a single data server at 150 USD per month.

Think of the user repository as a Git repo and the data server as GitHub. It’s easy to move a Git repo from GitHub to GitLab. Similarly, a user repository is movable from one data server to another.

Besides it’s possible to use [alternative clients](https://docs.bsky.app/showcase?tags=client) for Bluesky. Yet it’s necessary to maintain a standard data schema for interactions. So separate **[data schemas](https://atproto.com/guides/lexicon)** and API endpoints are defined for each app on ATProto, including Bluesky.

A user's repository doesn’t store information about actions performed by their followers such as comments or likes on their post. Instead, it’s stored only in the repository of the follower who took the action.

A post is shown to the user’s followers.

Yet it’s expensive to push updates to each follower’s repository. So information is collected from every data server using the **[crawler](https://atproto.com/guides/glossary#relay)**.

The crawler doesn’t index data but forwards it.

[![Crawler Collecting Information From Data Servers to Generate the Stream](https://substackcdn.com/image/fetch/$s_!xC2q!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8063e71c-325f-4adb-814f-b6bed48dd1f7_1200x630.png)](https://substackcdn.com/image/fetch/$s_!xC2q!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8063e71c-325f-4adb-814f-b6bed48dd1f7_1200x630.png)Crawler Collecting Information From Data Servers to Generate the Stream

Here’s how it works:

  * The crawler subscribes for updates on the data server: new posts, likes, or comments.

  * The data server notifies the crawler about updates in real time over [websockets](https://en.wikipedia.org/wiki/WebSocket).

  * The crawler collects information from data servers and generates a **[stream](https://atproto.com/specs/sync#firehose)**.

Consider the generated stream as a log over websockets; put simply, the crawler combines each user’s actions into a single TCP connection.

[![Aggregating a Post’s Data From Stream Using the Index Server](https://substackcdn.com/image/fetch/$s_!OmVj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff7b47f9a-e99c-481f-a6cb-187b7a810837_1200x630.png)](https://substackcdn.com/image/fetch/$s_!OmVj!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff7b47f9a-e99c-481f-a6cb-187b7a810837_1200x630.png)Aggregating a Post’s Data From Stream Using the Index Server

A user's post is shown to followers only after counting the likes, comments, and reposts on it.

Yet the stream doesn’t contain this information. So the stream’s data is aggregated using the **[index server](https://atproto.com/guides/glossary#app-view)** ; it transforms raw data into a consumable form by processing it. Imagine the index server as a data presentation layer.

The index server is built using the [Go](https://go.dev/) language for concurrency. A NoSQL database, [ScyllaDB](https://www.scylladb.com/), is used as its data layer for horizontal scalability.

A reference to the user's post ID is added to the follower’s repository when they like or repost a post. So the total number of likes and reposts is calculated by crawling every user repository and adding the numbers.

[![Displaying a Post to the User](https://substackcdn.com/image/fetch/$s_!Qo5q!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6021378f-484e-491e-8231-d81b65ab0562_1200x630.png)](https://substackcdn.com/image/fetch/$s_!Qo5q!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6021378f-484e-491e-8231-d81b65ab0562_1200x630.png)Displaying a Post to the User

Here’s the workflow for displaying a post:

  * A user’s request is routed via their data server to the index server.

  * The data server finds the people a user follows by looking at their repository.

  * The index server creates a list of post IDs in reverse chronological order.

  * The index server expands the list of post IDs to full posts with content.

The index server then responds to the client.

In short, a user repository stores primary data, while the index server stores derived data from repositories.

The index server is the most read-heavy service; so, its results are cached using [Redis](https://en.wikipedia.org/wiki/Redis), an in-memory storage, for performance.

JSON Web Token (**[JWT](https://jwt.io/)**) is used for authentication between Bluesky services.

[![Serving Media Files via CDN for Efficiency](https://substackcdn.com/image/fetch/$s_!cRbJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F70b2c93a-651c-4413-845b-9d8f8cc445e3_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!cRbJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F70b2c93a-651c-4413-845b-9d8f8cc445e3_1200x630.gif)Serving Media Files via CDN for Efficiency

Media files, such as images and videos, are stored on the data server’s disk for simplicity. A cryptographic ID (**CID**) is used to reference the media files in the repository. The index server fetches the media files from the data server on user request and caches them on the content delivery network (**[CDN](https://www.cloudflare.com/en-gb/learning/cdn/what-is-a-cdn/)**) for efficiency.

### 2\. User Profile

A user updates only their repository when they follow someone. 

Their repository adds a reference to the user’s unique decentralized identifier (**[DID](https://www.w3.org/TR/did-core/)**) to indicate follow.

[![Counting Number of Followers Using the Index Server](https://substackcdn.com/image/fetch/$s_!FJBc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2a931cf1-78f5-4e36-9b7c-5777960cdd47_1200x630.png)](https://substackcdn.com/image/fetch/$s_!FJBc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2a931cf1-78f5-4e36-9b7c-5777960cdd47_1200x630.png)Counting the Number of Followers Using the Index Server

The number of followers for a user is found by indexing every repository. This is similar to how Google finds inbound links to a web page; all documents on the web are crawled.

A user account includes a handle based on the domain name (**[DNS](https://en.wikipedia.org/wiki/Domain_Name_System)**); it keeps things simple.

[![Storing User Data Using Decentralized ID](https://substackcdn.com/image/fetch/$s_!aR6u!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5da95c5e-9f67-4478-ae53-074202c4e3d0_1200x630.png)](https://substackcdn.com/image/fetch/$s_!aR6u!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5da95c5e-9f67-4478-ae53-074202c4e3d0_1200x630.png)Storing User Data Using Decentralized ID

And each user automatically gets a handle from the ‘bsky.social’ subdomain upon account creation. Yet posts are stored using DID, and the user handle is displayed along with the posts. So changes to a user's handle don’t affect their previous posts.

A user’s DID is immutable, but the handle is mutable; put simply, a user’s handle is reassignable to a custom domain name.

Here’s how a user handle with a custom domain name is verified on Bluesky:

  * The user enters their custom domain name in the Bluesky account settings.

  * Bluesky generates a unique text value for the user: public key.

  * The user stores this value in the [DNS TXT record](https://www.cloudflare.com/en-gb/learning/dns/dns-records/dns-txt-record/) of the custom domain name.

The index server then periodically checks the DNS TXT record and validates the user handle. Imagine **DNS TXT record** as a text data field for domain name settings.

### 3\. User Timeline and Feed

A user’s timeline is created by arranging their posts in reverse chronological order.

[![Displaying a User’s Timeline](https://substackcdn.com/image/fetch/$s_!3laO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7cbbac3a-145b-452c-b768-6c6f5caa7ca7_1200x630.png)](https://substackcdn.com/image/fetch/$s_!3laO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7cbbac3a-145b-452c-b768-6c6f5caa7ca7_1200x630.png)Displaying a User’s Timeline

Yet a user’s repository doesn’t contain information about likes and comments received on a post. So the request is sent to the index server; it returns the user’s timeline with aggregated data.

Besides the timeline of [popular users](https://vqv.app/) is cached for performance.

A feed is created from posts by people a user follows.

Bluesky supports [feeds](https://bsky.app/feeds) with custom logic, and there are 50K+ custom feeds available. 

Consider the **custom feed** as a filter for a specific list of keywords or users.

[![Feed Generator Creating a Custom Feed](https://substackcdn.com/image/fetch/$s_!1TB1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6480dafb-74e1-4cb1-acd3-e051ee70029f_1200x630.png)](https://substackcdn.com/image/fetch/$s_!1TB1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6480dafb-74e1-4cb1-acd3-e051ee70029f_1200x630.png)Feed Generator Creating a Custom Feed

Here’s how it works:

  * The crawler generates a stream from data servers.

  * The **[feed generator](https://github.com/bluesky-social/feed-generator)** , a separate service,**** consumes the stream.

  * The feed generator filters, sorts, and ranks the content based on custom logic.

  * The feed generator creates a list of post IDs.

[![Serving a Custom Feed to the User](https://substackcdn.com/image/fetch/$s_!zlZ1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F70dc4776-1c24-4456-a510-f1911efa1566_1200x630.png)](https://substackcdn.com/image/fetch/$s_!zlZ1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F70dc4776-1c24-4456-a510-f1911efa1566_1200x630.png)Serving a Custom Feed to the User

The index server then populates the feed’s content on a user request.

**Cursor-based pagination** is used to fetch the feed; it offers better performance. 

It includes an extra parameter in API requests and responses. The cursor parameter points to a specific item in the dataset; for example, the post’s timestamp to fetch feed until a specific post.

[![Cursor Based Pagination for Performance](https://substackcdn.com/image/fetch/$s_!8GY7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F68039127-e900-42ab-b877-294cde1cfd7d_1200x630.png)](https://substackcdn.com/image/fetch/$s_!8GY7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F68039127-e900-42ab-b877-294cde1cfd7d_1200x630.png)Cursor-Based API Pagination for Performance

Here’s how it works:

  * A sequential unique column, such as the post's timestamp, is chosen for pagination.

  * The user requests include the cursor parameter to indicate the result’s offset.

  * The index server uses the cursor parameter to paginate the result dataset.

  * The index server responds with the cursor parameter for future requests.

The client decides the cursor parameter’s window size based on its viewport. The cursor-based pagination scales well as it prevents a full table scan in the index server.

### 4\. Content Moderation

The content in Bluesky is analyzed using the **[moderation service](https://docs.bsky.app/docs/advanced-guides/moderation)** ; it assigns a [label](https://atproto.com/specs/label) to the post.

Imagine a **label** as metadata to categorize content. Besides it’s possible to apply a label manually. A user chooses to hide or show a warning for posts with a specific label.

This preference is stored on their data server.

[![Assigning Label to Content Using Moderation Service](https://substackcdn.com/image/fetch/$s_!l5RD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb3726763-1c11-457e-ac7b-78771ea8ba8c_1200x630.png)](https://substackcdn.com/image/fetch/$s_!l5RD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb3726763-1c11-457e-ac7b-78771ea8ba8c_1200x630.png)Assigning Label to Content Using Moderation Service

Here’s how Bluesky moderation works:

  * The moderation service consumes the stream from the crawler.

  * The moderation service analyzes the content and assigns a label to it.

  * The index server stores the content along with its label.

  * The user requests are routed via the data server to the index server.

  * The data server includes label IDs in HTTP request headers before forwarding them.

  * The index server applies the label setting in the response.

Also a data server operator does basic moderation. 

The data server filters out the muted users before responding to the client.

Put simply, a user can post about anything, but the content is moderated before being shown to the public.

### 5\. Social Proof

Bluesky allows a user to see how many people they follow also follow a specific user: [social proof](https://www.ernberck.com/social-proof-explained/).

[![Social Proof Feature in Bluesky](https://substackcdn.com/image/fetch/$s_!Vp7c!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbdfa878c-9f86-4216-b158-23e5190be649_1200x630.png)](https://substackcdn.com/image/fetch/$s_!Vp7c!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbdfa878c-9f86-4216-b158-23e5190be649_1200x630.png)Social Proof Feature in Bluesky

For example, when I visit [Jay’s](https://bsky.app/profile/jay.bsky.team) Bluesky profile, it shows how many people I follow also follow her.

_Let’s dive into the different approaches they used to build this feature._

[![Computing Social Proof by Querying the Database](https://substackcdn.com/image/fetch/$s_!z0Em!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5181e604-bf4e-403d-b05b-24237ddae5bb_1200x630.png)](https://substackcdn.com/image/fetch/$s_!z0Em!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5181e604-bf4e-403d-b05b-24237ddae5bb_1200x630.png) Computing Social Proof by Querying the Database

Here’s a basic solution:

  * Find the people I follow by querying the database.

  * Do separate parallel queries for each user to find the people they follow.

  * Check if any of them also follow Jay.

But this approach won’t scale as the number of parallel queries increases with the number of people a user follows.

[![Social Proof Represented as a Set Intersection Problem](https://substackcdn.com/image/fetch/$s_!S6th!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffdf07a68-bb11-42a3-b907-1cfe0d11c31c_1200x630.png)](https://substackcdn.com/image/fetch/$s_!S6th!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffdf07a68-bb11-42a3-b907-1cfe0d11c31c_1200x630.png)Social Proof Represented as a Set Intersection Problem

A scalable approach is to convert it into a set intersection problem:

  * Set 1 tracks the people I follow.

  * Set 2 tracks the people who follow Jay.

The intersection of these sets gives the expected result.

[![An In-Memory Graph Service to Track Follows](https://substackcdn.com/image/fetch/$s_!MYpk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffd5ec4ce-672c-46ee-8060-0a178266d1e2_1200x630.png)](https://substackcdn.com/image/fetch/$s_!MYpk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffd5ec4ce-672c-46ee-8060-0a178266d1e2_1200x630.png)An In-Memory Graph Service to Track Follows

An in-memory graph service prevents expensive database queries and performs intersections quickly. Yet [Redis Sets](https://redis.io/docs/latest/develop/data-types/sets/) don’t use different CPU cores at once. So here’s how they implemented a minimum viable product:

  * Each user has a 32-character DID.

  * The DID values are [converted](https://en.wikipedia.org/wiki/String_interning) into uint64 to reduce memory usage.

  * Each user’s DID maintains 2 sets: people they follow and people who follow them.

But it still consumes a lot of memory and takes extra time to start.

So they optimized the graph service by implementing it using [Roaring Bitmaps](https://vikramoberoi.com/posts/a-primer-on-roaring-bitmaps-what-they-are-and-how-they-work/).

_Yet let’s take a step back and learn Bitmaps to better understand Roaring Bitmaps._

A **Bitmap** represents binary states using bit arrays.

[![Tracking People I Follow Using Bitmap](https://substackcdn.com/image/fetch/$s_!nZcF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffeb45103-0891-48cb-b221-e12698e753cb_1200x630.png)](https://substackcdn.com/image/fetch/$s_!nZcF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffeb45103-0891-48cb-b221-e12698e753cb_1200x630.png)Tracking People I Follow Using Bitmap

Imagine Bluesky has only 100 users.

The index value of the people I follow is set to 1 on the Bitmap; the people I follow are then found by walking the Bitmap and recording the index of non-zero bits. A constant time lookup tells whether I follow a specific user. Although Bitmaps do faster bitwise operations, it’s inefficient for sparse data. 

A 100-bit long Bitmap is needed even if I follow only a single user; so, it won’t scale for Bluesky’s needs.

A better approach is to compress the Bitmap using **[Run-Length Encoding](https://en.wikipedia.org/wiki/Run-length_encoding)**.

It turns consecutive 0s and 1s into a number for reduced storage. For example, if I follow the last 10 users, only the last 10 indices are marked by 1. With Run-Length Encoding, it’s stored as 90 0s and 10 1s, thus low storage costs.

But this approach won’t scale for randomly populated data, and a lookup walks the entire Bitset to find an index.

Ready for the best technique? **Roaring Bitmaps**.

Think of it as compressed Bitmaps, but 100 times faster. _A Roaring Bitmap splits data into containers and each container uses a different storage mechanism._

[![Roaring Bitmaps to Construct Social Proof](https://substackcdn.com/image/fetch/$s_!CqLI!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe311658f-7272-41f2-8c88-9055d70de140_1200x630.png)](https://substackcdn.com/image/fetch/$s_!CqLI!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe311658f-7272-41f2-8c88-9055d70de140_1200x630.png) Roaring Bitmaps to Track Social Proof

Here’s how it works:

  * The dense data is stored in a container using Bitmap; it uses a fixed-size bit array.

  * The data with a large contiguous range of integers are stored in a container using run-length encoding; which reduces storage costs.

  * The sparse data is stored in a container as integers in a sorted array; it reduces storage needs.

Put simply, Roaring Bitmaps use different containers based on data sparsity.

The set intersection is done in parallel and a container is converted into a different format for the set intersection. Also the graph data is stored and transferred over the network in Roaring Bitmap’s [serialization format](https://github.com/dgraph-io/sroar).

Many instances of the graph service are run for high availability and [rolling updates](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/).

### 6\. Short Video

Bluesky supports short videos up to 90 seconds long.

The video is streamed through HTTP Live Streaming (**[HLS](https://datatracker.ietf.org/doc/html/rfc8216)**). Think of HLS as a group of text files; it’s a standard for adaptive bitrate video streaming.

A client dynamically changes video quality based on network conditions.

[![How Adaptive Bitrate Video Streaming Works](https://substackcdn.com/image/fetch/$s_!X0qi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8c3238ce-644a-4043-91a3-25d0079ad8fa_1200x630.png)](https://substackcdn.com/image/fetch/$s_!X0qi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8c3238ce-644a-4043-91a3-25d0079ad8fa_1200x630.png)How Adaptive Bitrate Video Streaming Works

Here’s how it works:

  * A video is encoded into different quality levels: 480p, 720p, 1080p, and so on.

  * Each encoded video is split into small segments of 5 seconds long.

The client checks the network conditions and requests video segments of the right quality.

HLS uses Playlist files to manage encoded video segments. 

Imagine the **Playlist** as a text file, and there are 2 types of Playlists:

  * Master playlist: list of all video quality levels available.

  * Media playlist: list of all video segments for a specific video quality.

[![Selecting a Video Quality Level Based on Network Conditions](https://substackcdn.com/image/fetch/$s_!A-jd!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F84a7e2c5-0f89-4744-9ddb-b6df230d3e91_1200x630.png)](https://substackcdn.com/image/fetch/$s_!A-jd!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F84a7e2c5-0f89-4744-9ddb-b6df230d3e91_1200x630.png)Selecting a Video Quality Level Based on Network Conditions

First, the client downloads the Master playlist to find available quality levels. Second, it selects a Media playlist based on network conditions. Third, it downloads the video segments in the sequence defined on the Media playlist.

The videos and playlists are cached in CDN to reduce costs and handle bandwidth needs. The stream requests are routed to CDN via [302 HTTP redirects](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302).

The video views are tracked by [counting](https://systemdesign.one/distributed-counter-system-design/) requests to its Master playlist file.

And the response for the Master playlist includes a Session ID; it’s included in future requests for Media playlists to track the user session.

Besides the video seconds watched by a user are found by checking the last fetched video segment.

A video subtitle is stored in text format and has a separate Media playlist file.

The Master playlist includes a reference to subtitles; here’s how it works:

  * The client finds the available subtitles by querying the Master playlist.

  * The client downloads a specific subtitle's Media playlist based on the language selected by the user.

The client then downloads the subtitle segments defined in the Media Playlist.

* * *

Subscribe to get simplified system design case studies delivered to your inbox:

Subscribe

* * *

### Bluesky Quality Attributes

Let’s keep going:

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fhow-does-bluesky-work&utm_source=paywall&utm_medium=web&utm_content=153583246)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fhow-does-bluesky-work&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture