[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Does HTTPS Work 🔥

### #81: Fundamental Communication Protocol of the Web (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Aug 07, 2025

145

7

11

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how HTTPS works. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-does-https-work/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, the internet was a quiet place with only a tiny number of servers.

And the users communicated with servers by sending and receiving files.

They shared files through File Transfer Protocol (**FTP**), email attachments, and floppy disks.

[![How Does HTTPS Work](https://substackcdn.com/image/fetch/$s_!455c!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F165275fe-5f02-4752-8a8e-1803320182fd_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!455c!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F165275fe-5f02-4752-8a8e-1803320182fd_1200x630.gif)

Yet it became difficult to transfer information in a simple and standardized way.

So they created the Hypertext Transfer Protocol (HTTP).

Think of **HTTP** as a set of rules for transferring information on the Internet.

Although it temporarily solved the data transfer problem, there were new issues.

Here are some of them:

#### 1\. Privacy and Integrity

HTTP sends data in plaintext without encryption.

It means someone on the public network can easily access or change the information.

So it’s insecure.

#### 2\. Authentication

HTTP doesn’t support a mechanism to check if the server is the right one.

It means someone else could pretend to be the server and steal the user's data.

So it became difficult to transfer sensitive information, such as passwords or credit card numbers.

Onward.

* * *

### [Ship faster, with context-aware AI that speaks your language - Sponsor](https://refactoring.link/aug-sd)

**[Augment Code](https://refactoring.link/aug-sd)** is the only AI coding agent built for real engineering teams.

It understands your codebase—across 10M+ lines, 10k+ files, and every repo in your stack—so it can actually help: writing functions, fixing CI issues, triaging incidents, and reviewing PRs.  
All from your IDE or terminal. No vibes. Just progress.

[Learn more about Augment](https://refactoring.link/aug-sd)

* * *

## How Does HTTPS Work

Let’s dive in!

They wanted a secure version of HTTP.

So they created Hypertext Transfer Protocol Secure (HTTPS).

[![Transferring Data Securely Through HTTPS](https://substackcdn.com/image/fetch/$s_!H6W0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F07f7b466-ab43-41b4-b3ba-e61085e1d99e_2794x698.png)](https://substackcdn.com/image/fetch/$s_!H6W0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F07f7b466-ab43-41b4-b3ba-e61085e1d99e_2794x698.png)Transferring Data Securely Through HTTPS

Imagine **HTTPS** as HTTP running over an extra protocol to keep information secure.

The extra protocol is called Transport Layer Security (TLS). Think of **TLS** as a mechanism to encrypt data sent between the client and server.

TLS creates a secure connection using a process called the **TLS handshake**. It's used to establish an HTTPS connection when a user visits a site.

Here’s how it works:

[![Browser Starting the TLS Handshake](https://substackcdn.com/image/fetch/$s_!e7Et!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F81909ff7-41f2-48c5-9047-6d83c66af66f_2689x698.png)](https://substackcdn.com/image/fetch/$s_!e7Et!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F81909ff7-41f2-48c5-9047-6d83c66af66f_2689x698.png)Browser Starting the TLS Handshake

The browser sends a message to the server.

It includes the list of supported cryptographic algorithms, TLS versions. And also a randomly generated string called _client random_.

[![Server Responding With TLS Certificate](https://substackcdn.com/image/fetch/$s_!Duq8!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe09ef9bc-68c0-496a-8eeb-f3095f3f1a4d_2689x698.png)](https://substackcdn.com/image/fetch/$s_!Duq8!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe09ef9bc-68c0-496a-8eeb-f3095f3f1a4d_2689x698.png)Server Responding With TLS Certificate

The server then responds with its TLS certificate and supported cryptographic algorithm. Besides it includes a randomly generated string called _server random_.

Yet it’s important to confirm the server identity for authenticity. So the browser verifies the received TLS certificate with the certificate authority. Imagine the **certificate authority** as a trusted organization that verifies server identity.

But both client and server must have the same key to encrypt session data efficiently.

[![Browser Sending the Pre-Master Secret](https://substackcdn.com/image/fetch/$s_!uzUU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6f35f4fc-8b32-4d85-b47e-b4c7ec62d5e7_2689x698.png)](https://substackcdn.com/image/fetch/$s_!uzUU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6f35f4fc-8b32-4d85-b47e-b4c7ec62d5e7_2689x698.png)Browser Sending the Pre-Master Secret

So the browser sends a temporary key (**pre-master secret**) to the server. While it’s encrypted using the server’s public key, which was taken from the TLS certificate.

Yet only the private key can decrypt the data that was encrypted using a public key. So the server decrypts the received pre-master secret using its _private key_. Thus making this data transfer secure.

Think of the **public key** as an email address; anybody can send messages to it. While the **private key** is like the inbox password, only the user with the password can read emails.

And the best part?

[![Browser and Server Generates the Session Key](https://substackcdn.com/image/fetch/$s_!jqrg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F677a0078-23a5-4776-bd49-658d8445ea40_2689x902.png)](https://substackcdn.com/image/fetch/$s_!jqrg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F677a0078-23a5-4776-bd49-658d8445ea40_2689x902.png)Browser and Server Generate the Session Key

Both browser and server use the pre-master secret, server random, and client random to compute the _same_ session key.

Think of the **session key** as a symmetric cryptographic key that can encrypt and decrypt data. It’s valid either for a set period or for as long as communication is ongoing.

All future communication then gets encrypted using the session key. It means nobody can see the messages on the public network.

Yet it’s necessary to check if the TLS handshake was successful.

So the browser sends a finished message, which is encrypted using the session key. The server then responds with a finished message, encrypted using the session key.

This marks the completion of a TLS handshake.

Put simply, the TLS certificate handles authentication, while the TLS protocol handles encryption.

Thus providing authenticity, confidentiality, and integrity in data transfer.

* * *

HTTP is rarely used on public networks now.

Yet it's still in use on internal networks and legacy systems where data sensitivity is low. Because it offers convenience with simplicity.

While HTTPS has become the fundamental communication protocol of the internet.

And search engines like Google give preference to HTTPS-enabled sites in their search results. Besides HTTPS is necessary for compliance in transferring sensitive information: financial data.

But there’s an overhead with the setup and maintenance of a TLS certificate. Also HTTPS can be slightly slower than HTTP because of encryption overhead. Yet HTTP/2 and HTTP/3 offset this overhead with better performance.

So always use HTTPS on public sites because of its security and trust advantages.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Find me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Neo’s recommendation** 🚀

**[ByteSizedBets](https://bytesizedbets.com/)** delivers bite-sized deep dives on emerging devtools, career growth, and smart bets for builders.

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 165K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-does-https-work?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

**TL;DR** 🕰️

You can find a summary of this article [here](https://www.linkedin.com/posts/nk-systemdesign-one_http-vs-https-explained-in-2-mins-or-less-activity-7359187677348274176-W5TS). Consider a repost if you find it helpful.

* * *

[![How Amazon S3 Achieves Strong Consistency Without Sacrificing 99.99% Availability 🌟](https://substackcdn.com/image/fetch/$s_!kEaw!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F95ebfdc1-79b4-41da-9bad-b0c0432ceeb4_1280x720.png)How Amazon S3 Achieves Strong Consistency Without Sacrificing 99.99% Availability 🌟[Neo Kim](https://substack.com/profile/135589200-neo-kim)·July 29, 2025[Read full story](https://newsletter.systemdesign.one/p/s3-strong-consistency)](https://newsletter.systemdesign.one/p/s3-strong-consistency)

[![How to Improve Availability Using Deployment Patterns ★](https://substackcdn.com/image/fetch/$s_!QwFP!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3bc05d4c-2244-4261-9c2e-62440ace9957_1280x720.png)How to Improve Availability Using Deployment Patterns ★[Neo Kim](https://substack.com/profile/135589200-neo-kim)·August 5, 2025[Read full story](https://newsletter.systemdesign.one/p/deployment-patterns)](https://newsletter.systemdesign.one/p/deployment-patterns)

* * *

### References

  * [HTTP vs. HTTPS: What You Need to Know](https://blog.hubspot.com/website/http-vs-https)

  * [Why is HTTP not secure? | HTTP vs. HTTPS](https://www.cloudflare.com/en-gb/learning/ssl/why-is-http-not-secure/)

  * [The Transport Layer Security (TLS) Protocol](https://www.rfc-editor.org/rfc/rfc8446)

  * [How to Get a Free SSL Certificate](https://www.wpbeginner.com/beginners-guide/how-to-get-a-free-ssl-certificate-for-your-wordpress-website/)

  * [How does the browser verify the validity of a given server certificate?](https://security.stackexchange.com/questions/56389/ssl-certificate-framework-101-how-does-the-browser-verify-the-validity-of-a-giv)

  * [What’s the Difference Between HTTP and HTTPS?](https://aws.amazon.com/compare/the-difference-between-https-and-http/)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

145

7

11

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Aug 7](https://newsletter.systemdesign.one/p/how-does-https-work/comment/143025788 "Aug 7, 2025, 12:10 PM")

Liked by Neo Kim

The world runs on this.

Thanks for the simple explanation, Neo!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-does-https-work/comment/143025788)

[![NAROTAM KUMAR MISHRA's avatar](https://substackcdn.com/image/fetch/$s_!wly4!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe0adf002-9469-4677-885f-5d7f3db9bbb6_96x96.png)](https://substack.com/profile/186954413-narotam-kumar-mishra?utm_source=comment)

[NAROTAM KUMAR MISHRA](https://substack.com/profile/186954413-narotam-kumar-mishra?utm_source=substack-feed-item)

[Aug 7](https://newsletter.systemdesign.one/p/how-does-https-work/comment/143030398 "Aug 7, 2025, 12:28 PM")

Liked by Neo Kim

Very useful, thanks for sharing!!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-does-https-work/comment/143030398)

[5 more comments...](https://newsletter.systemdesign.one/p/how-does-https-work/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture