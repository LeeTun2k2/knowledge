[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How JWT Works ✨

### #71: Break Into JSON Web Token (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 10, 2025

249

14

23

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how JSON Web Token works. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-jwt-works/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, there lived a software engineering student named Keiko.

Although extremely bright, she never worked on any real-world projects.

So she struggled with coding.

[![How JWT Works](https://substackcdn.com/image/fetch/$s_!16my!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F146fb83d-a0b8-4ee6-8e3f-31bb255b09c8_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!16my!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F146fb83d-a0b8-4ee6-8e3f-31bb255b09c8_1200x630.gif)

Until one day, when she decided to build a weather app.

And shared it with her friends.

Yet she had no idea how user authorization worked.

HTTP is stateless; it means each request should include the necessary information.

And her friends had to enter their username and password on each page for authorization.

It was frustrating.

[![Session Store Tracking User Sessions](https://substackcdn.com/image/fetch/$s_!eXlE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a66c935-829d-4378-b283-0dab52832b88_1136x252.png)](https://substackcdn.com/image/fetch/$s_!eXlE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a66c935-829d-4378-b283-0dab52832b88_1136x252.png)Session Store Tracking User Sessions

So she set up a **session store** on the server:

  * It tracks user sessions and gives a session ID to the client.

  * And the client includes the session ID with each request header.

The server then checks the session store to identify the client for access.

Although it temporarily solved the authorization problem, there were newer issues.

Her tiny app became popular among university students across the world.

So she had to install more servers and put them behind a load balancer.

But it became difficult to manage sessions with many servers. 

Because the load balancer might route requests from a user to different servers.

[![Sticky Sessions to Route Requests From a User to the Same Server](https://substackcdn.com/image/fetch/$s_!sq_X!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0539a9b4-fa37-4d4c-aff8-056e14c7dc18_991x586.png)](https://substackcdn.com/image/fetch/$s_!sq_X!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0539a9b4-fa37-4d4c-aff8-056e14c7dc18_991x586.png)Sticky Sessions to Route Requests From a User to the Same Server

So she set up **sticky sessions**. It means routing requests from a specific user to the same server using their user ID or IP address.

Yet it doesn’t scale well because of stateful servers and uneven load distribution.

[![Shared Session Store for Tracking User Sessions Across Servers](https://substackcdn.com/image/fetch/$s_!dZVB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6449e5cc-9791-4f97-8728-44567a417602_1440x222.png)](https://substackcdn.com/image/fetch/$s_!dZVB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6449e5cc-9791-4f97-8728-44567a417602_1440x222.png)Shared Session Store for Tracking User Sessions Across Servers

So she installed a **shared session store**. It keeps session data separate in a central store.

A shared session store allows stateless servers. But introduces coupling and becomes a single point of failure.

So she set up JSON Web Token (**JWT**) for authorization.

[![Authentication vs Authorization](https://substackcdn.com/image/fetch/$s_!SvmQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F794a4480-e027-41c0-aa81-ad8dd9b9b8ae_1200x630.png)](https://substackcdn.com/image/fetch/$s_!SvmQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F794a4480-e027-41c0-aa81-ad8dd9b9b8ae_1200x630.png)Authentication vs Authorization

Think of JWT as a badge to enter a building. A user gets the badge from the front desk after verification. Then they can access authorized rooms without verifying their identity again.

_The server doesn’t store session information with JWT_. Instead JWT includes necessary authorization information, and the server checks the JWT on each request.

Imagine **authentication** as permission to enter a building. And **authorization** as the permission to enter specific rooms of the building.

Onward.

* * *

### [Linear: Issue tracking without the noise - Sponsor](https://linear.app/partners/system-design-newsletter)

Simple, robust, and blazingly fast.

[![](https://substackcdn.com/image/fetch/$s_!vFnx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F25c8fc6e-1c3a-4209-8cc7-cf11dc540c3a_6400x3361.png)](https://linear.app/partners/system-design-newsletter)

Linear is purpose-built for engineers:

  * Create issues in seconds

  * Automate your Git workflows

  * Navigate your entire workflow with keyboard shortcuts

Less grunt work, less context switching. More focus, more flow. See for yourself with $250 off.

[Explore Linear](https://linear.app/partners/system-design-newsletter)

* * *

## How JWT Works

Let’s dive in:

### **1\. JWT Structure**

JWT doesn’t look like a JSON object, but a set of characters. 

It has 3 parts:

  * Header: It specifies the signing algorithm, such as RSA or HMAC

  * Payload: It contains data and expiry time

  * Signature: It shows the payload’s authenticity

The **payload** is [base64url](https://base64.guru/standards/base64url) encoded for compactness and ease of transmission across networks.

[![What JWT Looks Like](https://substackcdn.com/image/fetch/$s_!yqWa!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F28d15b54-f8ca-4f3c-b433-bc63e2c58844_1200x630.png)](https://substackcdn.com/image/fetch/$s_!yqWa!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F28d15b54-f8ca-4f3c-b433-bc63e2c58844_1200x630.png)[What JWT Looks Like](https://jwt.io/#debugger-io)

The server combines the header and payload with a secret key. Then it performs the signing algorithm to create the **signature**. Only the server has access to the secret key.

The server recomputes the signature using the header and payload each time it receives a JWT. Thus checking if the payload was modified.

Ready for the best part?

### **2\. JWT Workflow**

[![JWT Workflow Explained](https://substackcdn.com/image/fetch/$s_!YQSy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d4d41ad-9d21-4843-8376-97c33dfc29f7_944x239.png)](https://substackcdn.com/image/fetch/$s_!YQSy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d4d41ad-9d21-4843-8376-97c33dfc29f7_944x239.png) JWT Workflow Explained

Here’s the JWT authorization workflow:

  1. The user enters the name and password for authentication

  2. The server creates a JWT using the authorization information

  3. The server signs the JWT and gives it to the client

  4. The client includes JWT in HTTP headers for authorization

The server allows a request only if the signature on the JWT is valid.

### **3\. JWT Security**

There’s a risk of JWT getting stolen and used for false authorization. 

[![How to Secure JWT](https://substackcdn.com/image/fetch/$s_!vo2v!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc723c8dd-b164-42dc-b984-115a29058a65_865x292.png)](https://substackcdn.com/image/fetch/$s_!vo2v!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc723c8dd-b164-42dc-b984-115a29058a65_865x292.png)How to Secure JWT

Here are some ways to avoid this problem:

  * Send JWT over HTTPS for security

  * Set an expiry time on the JWT to limit the damage

  * Assign minimum roles to JWT to reduce the damage

Yet there’s no way to invalidate a stolen JWT. A workaround is to include the stolen JWT in a denial list on the server.

* * *

JWT remains a popular authorization mechanism on the internet and in microservices architecture. It makes it easier to transfer information across services.

Solving a problem makes the next one easier.

So build projects. And join my free newsletter to elevate your system design skills.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋 Say hi on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 150K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-jwt-works?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

**TL;DR** 🕰️

You can find a summary of the article [here](https://substack.com/@systemdesignone/note/c-116012013?utm_source=notes-share-action&r=28q5ao). Consider a repost if you find it helpful.

* * *

[![How DNS Works 🔥](https://substackcdn.com/image/fetch/$s_!n997!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc91ac4a1-1a9e-4ed0-9b34-9c97a2e70e2e_1280x720.gif)How DNS Works 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·May 2, 2025[Read full story](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work)](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work)

[![How Figma Scaled to 4M Users—Without Fancy Databases 🔥](https://substackcdn.com/image/fetch/$s_!_ceu!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8618846e-b8ff-47a4-892d-e0628abbf730_1280x720.gif)How Figma Scaled to 4M Users—Without Fancy Databases 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·April 16, 2025[Read full story](https://newsletter.systemdesign.one/p/postgres-scale)](https://newsletter.systemdesign.one/p/postgres-scale)

* * *

### References

  * [Introduction to JSON Web Tokens](https://jwt.io/introduction)

  * [JSON Web Token - RFC](https://datatracker.ietf.org/doc/html/rfc7519)

  * [JSON Web Token Structure](https://auth0.com/docs/secure/tokens/json-web-tokens/json-web-token-structure)

  * [Authentication vs Authorization: Key Differences](https://www.fortinet.com/resources/cyberglossary/authentication-vs-authorization)

  * [What is HTTPS?](https://www.cloudflare.com/en-gb/learning/ssl/what-is-https/)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

249

14

23

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[May 10, 2025](https://newsletter.systemdesign.one/p/how-jwt-works/comment/116021440 "May 10, 2025, 12:48 PM")

Liked by Neo Kim

The biggest thing about JWTs: since they’re stateless, they scale really well. 

No need for the server to remember who you are between requests.

Nice read, Neo!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-jwt-works/comment/116021440)

[![Petar Ivanov's avatar](https://substackcdn.com/image/fetch/$s_!Hqxs!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb236a7ab-735e-49d2-bbe8-98b1f901b169_500x500.jpeg)](https://substack.com/profile/10269058-petar-ivanov?utm_source=comment)

[Petar Ivanov](https://substack.com/profile/10269058-petar-ivanov?utm_source=substack-feed-item)

[May 10, 2025](https://newsletter.systemdesign.one/p/how-jwt-works/comment/116017144 "May 10, 2025, 12:26 PM")

Liked by Neo Kim

It's also important to mention that even if we try to embed some more information in the token, it will get rejected from the Server because it won't be valid.

Great article, Neo!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-jwt-works/comment/116017144)

[12 more comments...](https://newsletter.systemdesign.one/p/how-jwt-works/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture