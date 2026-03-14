[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# System Design Interview Question: Design Spotify

### #93: System Design Interview (13 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)[![Hayk's avatar](https://substackcdn.com/image/fetch/$s_!HEFn!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9c35f951-cd99-4dba-988a-52a681a5b4a7_800x800.png)](https://substack.com/@hayksimonyan)

[Neo Kim](https://substack.com/@systemdesignone) and [Hayk](https://substack.com/@hayksimonyan)

Oct 06, 2025

∙ Paid

184

29

25

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post will help you prepare for the system design interview._

  * _[Share this post](https://newsletter.systemdesign.one/p/spotify-system-design/?action=share) & I’ll send you some rewards for the referrals._

Building a music streaming platform like Spotify is a classic system design problem.

It includes audio delivery, metadata management, and everything in between. 

Let’s figure out how to design it during a system design interview.

* * *

I want to introduce [Hayk Simonyan](https://linkedin.com/in/hayksimonyan) as a guest author.

[![](https://substackcdn.com/image/fetch/$s_!IuO8!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb7b32914-3c35-4318-9bbb-79d41a466417_1200x630.png)](https://youtube.com/@hayk.simonyan)

He’s a senior software engineer specializing in helping mid-level engineers break through their career plateaus and secure senior roles.

If you want to master the essential system design skills and land senior developer roles, I highly recommend checking out Hayk’s **[YouTube channel](https://youtube.com/@hayk.simonyan)**.

His approach focuses on what top employers actually care about: system design expertise, advanced project experience, and elite-level interview performance.

Onward.

* * *

### [Zero Trust for AI: Securing MCP Servers eBook by Cerbos (Sponsor)](https://solutions.cerbos.dev/zero-trust-for-ai-securing-mcp-servers?utm_campaign=system_design_oct_2025&utm_source=newsletter&utm_medium=email&utm_content=&utm_term=)

[![](https://substackcdn.com/image/fetch/$s_!21UO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcafe8d80-f546-4564-aff3-807019cbb440_2400x1280.png)](https://solutions.cerbos.dev/zero-trust-for-ai-securing-mcp-servers?utm_campaign=system_design_oct_2025&utm_source=newsletter&utm_medium=email&utm_content=&utm_term=)

MCP servers are becoming critical components in AI architectures, but they are creating a fundamental new risk that traditional security controls weren’t designed to address.

Left unsecured, they’re a centralized point of failure for data governance.

This **[eBook](https://solutions.cerbos.dev/zero-trust-for-ai-securing-mcp-servers?utm_campaign=system_design_oct_2025&utm_source=newsletter&utm_medium=email&utm_content=&utm_term=)** will show you how to secure MCP servers properly, using externalized, fine-grained authorization. Inside the ebook, you will find:

  * How MCP servers fit into your broader risk management and compliance framework

  * Why MCP servers break the traditional chain of identity in enterprise systems

  * How role-based access control fails in dynamic AI environments

  * Real incidents from Asana and Supabase that demonstrate these risks

  * The externalized authorization architecture (PEP/PDP) that enables Zero Trust for AI systems

Get the practical blueprint to secure MCP servers before they become your biggest liability.

[Download FREE eBook](https://solutions.cerbos.dev/zero-trust-for-ai-securing-mcp-servers?utm_campaign=system_design_oct_2025&utm_source=newsletter&utm_medium=email&utm_content=&utm_term=)

* * *

## Requirements & Assumptions

We’re looking at roughly 500k users listening to about 30 million songs. The core requirements include:

  * Artists can upload their songs.

  * Users can search and play songs.

  * Users can create and manage playlists.

  * Users can maintain profiles.

  * Basic monitoring and observability (health checks, error tracking, performance metrics).

Nothing fancy yet.

For audio formats, we use Ogg and AAC files[1](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-1-174962283) with different bitrates[2](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-2-174962283) for adaptive streaming[3](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-3-174962283). For example,

  * 64kbps for mobile data saving, 

  * 128kbps for standard quality,

  * 320kbps for premium users.

On average, one song file at normal audio quality (standard bitrate) takes about 3MB of storage.

While the main constraints are fast playback start times, minimal rebuffering, and straightforward operations. We handle rebuffering using adaptive quality switching.

* * *

## Capacity Planning

Let’s crunch some numbers to understand what we’re working with:

  * **Song storage:**

    * 3MB × 30M songs ≈ 90TB of raw audio data. 

    * This doesn’t include replicas across different regions. 

    * It also doesn’t include versioning overhead when artists re-upload songs.

    * That’s why we’re looking at 2-3x this amount.

  * **Song metadata:**

    * Each song needs a title, artist references, duration, file URLs, and so on. 

    * At roughly 100 bytes per song × 30M songs ≈ 3GB. 

    * That’s not so much compared to the audio.

  * **User metadata:**

    * User profiles, preferences, and playlist data are ~1KB × 500k users ≈ 0.5GB.

  * **Daily bandwidth:**

    * The average listen time is 3.5 minutes at 128-160kbps; that’s roughly 3-4MB per stream.

    * Let’s assume each user streams 10-15 songs daily.

    * This leads to significant egress costs[4](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-4-174962283).

The key insight is that audio dominates both storage costs and bandwidth. The metadata is just a small part in comparison.

Let’s dive in!

* * *

## Spotify System Design: High Level Architecture

The architecture breaks down into these key components:

[![](https://substackcdn.com/image/fetch/$s_!oeSq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff75f0592-00de-48e0-b72b-3ef2111c7ef1_1600x707.png)](https://substackcdn.com/image/fetch/$s_!oeSq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff75f0592-00de-48e0-b72b-3ef2111c7ef1_1600x707.png)

#### 1\. Mobile App (Client)

The user-facing application handles UI, search, playback controls, and playlist management.

  * It makes REST API[5](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-5-174962283) calls to fetch metadata and manages local playback state.

  * Client streams audio directly from blob storage[6](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-6-174962283) or CDN[7](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-7-174962283) using signed URLs[8](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-8-174962283).

  * And cache recently played songs locally for faster playback on the same song replay.

  * When things go wrong, it uses retry logic[9](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-9-174962283) for API calls.

The client handles network interruptions gracefully by pausing playback until connectivity returns.

#### 2\. Load Balancer

The load balancer spreads incoming requests across many API servers to prevent server overload.

  * It could use round-robin or least-connections algorithms.[10](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-10-174962283)

  * Also it performs health checks every 30 seconds and removes unhealthy instances from rotation.

  * This is essential for managing traffic spikes during album releases.

Besides, it provides high availability against server failures and enables zero-downtime deployments[11](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-11-174962283).

#### 3\. Web/API Layer

The application servers are stateless; they handle business logic, authentication, and data access.

  * They validate JWT tokens[12](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-12-174962283) and query the database for song metadata.

  * They generate signed URLs for audio access and also save user actions for analytics.

  * For reliability, they implement circuit breakers for database connections. Circuit breakers stop requests to failing services, preventing cascading failures.

  * They use connection pooling to manage resources. This means reusing database connections instead of creating new ones for each request.

They also provide fallback responses for non-critical features when dependencies are down.

#### 4\. Blob Storage

Object storage systems like AWS S3[13](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-13-174962283) could hold all the audio files. 

  * Files are organized in a hierarchical structure like /artist/album/song.ogg

  * They’re accessed via signed URLs that expire after a few hours for security.

  * Blob storage offers virtually unlimited scalability. It also comes with built-in durability and cost-effectiveness for large files. They replicate the storage across many regions for durability. 

However, it has higher latency than local storage and potentially higher egress costs. We could instead use a distributed file system[14](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-14-174962283) like Hadoop Distributed File System (**HDFS**). But blob storage is more managed and reliable for most use cases.

* * *

## System Workflow

Now that we’ve covered the high-level architecture, let’s explore the request workflow:

#### Read Workflow

Here’s what happens when the user **plays a song** :

  1. User hits play → App sends a GET request `/songs/{id}`

  2. API authentication → API server validates the JWT token.

  3. Metadata lookup → API server queries SQL database for song details (metadata).

  4. URL generation → API server creates a signed URL for blob storage access.

  5. Audio streaming → App fetches chunks of audio (range requests[15](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-15-174962283)) directly from blob storage using HTTP-based adaptive streaming (HLS or DASH[16](https://newsletter.systemdesign.one/p/spotify-system-design#footnote-16-174962283)), which allows smooth playback, automatic bitrate switching, and broad device compatibility.

  6. Analytics → App periodically calls `POST /songs/{id}/play` to track plays and listening time.

[![](https://substackcdn.com/image/fetch/$s_!Bkt6!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7380754b-3e4c-480e-9886-f89392bea01a_1600x1561.png)](https://substackcdn.com/image/fetch/$s_!Bkt6!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7380754b-3e4c-480e-9886-f89392bea01a_1600x1561.png)

#### Write Workflow

Here’s what happens when the **artist uploads a song** :

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fspotify-system-design&utm_source=paywall&utm_medium=web&utm_content=174962283)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fspotify-system-design&for_pub=systemdesignone&change_user=false)

PreviousNext

[![Hayk's avatar](https://substackcdn.com/image/fetch/$s_!HEFn!,w_52,h_52,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9c35f951-cd99-4dba-988a-52a681a5b4a7_800x800.png)](https://substack.com/@hayksimonyan?utm_source=byline)| A guest post by| [Hayk](https://substack.com/@hayksimonyan?utm_campaign=guest_post_bio&utm_medium=web)I help fullstack developers break out of the mid-tier trap and scale into multi six-figure remote careers.| [Subscribe to Hayk](https://hayksimonyan.substack.com/subscribe?)  
---|---  
  
© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture