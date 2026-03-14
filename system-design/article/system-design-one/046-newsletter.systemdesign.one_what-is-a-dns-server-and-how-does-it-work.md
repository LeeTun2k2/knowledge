[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How DNS Works 🔥

### #70: Distributed Hierarchical Database for 5.35B Users (2 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

May 02, 2025

169

23

18

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how DNS works. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, the internet was a quiet place with only a tiny number of sites.

Each server was assigned a unique number, called the Internet Protocol (**IP**) address.

It lets users reach the server via efficient routing over the network.

[![What Is a DNS Server and How Does It Work](https://substackcdn.com/image/fetch/$s_!qb4z!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff9f6b748-0af9-48f2-84f3-d31ee01f8ee1_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!qb4z!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff9f6b748-0af9-48f2-84f3-d31ee01f8ee1_1200x630.gif)

Yet it became difficult for users to remember each site’s IP address.

So they created a text file to map site names to IP addresses.

Although it temporarily solved their naming problem, it became extremely difficult to scale because the number of sites exploded.

So they set up a hierarchical distributed database called the Domain Name System (**DNS**).

Onward.

* * *

## What Is a DNS Server and How Does It Work

Think of DNS as a phone book; it returns the IP address on a site name lookup.

Let’s dive in!

The browser checks its cache for the IP address when the user enters a site name.

[![Browser Checking Its Cache for IP Address](https://substackcdn.com/image/fetch/$s_!etou!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd51bd388-fd76-45dc-8fe5-9298b8589492_830x557.png)](https://substackcdn.com/image/fetch/$s_!etou!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd51bd388-fd76-45dc-8fe5-9298b8589492_830x557.png)Browser Checking Its Cache for IP Address

The browser then queries the operating system’s cache to check if it has the site's IP address.

[![Operating System Checking Its Cache for IP Address](https://substackcdn.com/image/fetch/$s_!xgM5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F26096346-0d2c-4e5d-85d6-a02b29563a46_1452x832.png)](https://substackcdn.com/image/fetch/$s_!xgM5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F26096346-0d2c-4e5d-85d6-a02b29563a46_1452x832.png)Operating System Checking Its Cache for IP Address

If not, the browser sends a request to the resolver server to find the IP address.

Think of the **resolver server** as a component to find the correct DNS server for a site name.

The Internet Service Provider (**ISP**) maintains the resolver server, and it checks its cache for the IP address.

[![Browser Querying the Resolver Server for Correct DNS Server](https://substackcdn.com/image/fetch/$s_!Rl7F!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9db4f46b-4198-4137-acd1-fe71ba79495d_2326x832.png)](https://substackcdn.com/image/fetch/$s_!Rl7F!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9db4f46b-4198-4137-acd1-fe71ba79495d_2326x832.png)Browser Querying the Resolver Server for the Correct DNS Server

The resolver server then sends a query to the closest root server. There are _13 root servers_ _across the world_.

[![Resolver Server Querying the Root Server to Find Right TLD Server](https://substackcdn.com/image/fetch/$s_!YWqk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62a3d68c-e11e-4313-86b0-c4d236e5f4a0_1452x832.png)](https://substackcdn.com/image/fetch/$s_!YWqk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62a3d68c-e11e-4313-86b0-c4d236e5f4a0_1452x832.png)Resolver Server Querying the Root Server to Find the Right TLD Server

Yet a root server doesn’t store the IP address.

Instead, it forwards the query to the correct top-level domain (**TLD**) server, such as .com or .org.

Imagine the **root server** as a DNS component to find the right TLD server.

[![Resolver Server Querying the TLD Server to Find Authoritative Name Server](https://substackcdn.com/image/fetch/$s_!MR6f!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcedbf645-2154-4508-92ef-b361e52a2e3a_1452x832.png)](https://substackcdn.com/image/fetch/$s_!MR6f!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcedbf645-2154-4508-92ef-b361e52a2e3a_1452x832.png)Resolver Server Querying the TLD Server to Find the Authoritative Name Server

But TLD doesn’t store IP addresses either.

Instead, it routes the query to the correct authoritative name server. 

The **authoritative name server** contains the IP address; it responds to the resolver server.

[![The Authoritative Name Server Returns IP Address](https://substackcdn.com/image/fetch/$s_!d64x!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8cd6029d-9202-4bad-b98a-d46e32e292c4_1452x832.png)](https://substackcdn.com/image/fetch/$s_!d64x!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8cd6029d-9202-4bad-b98a-d46e32e292c4_1452x832.png)The Authoritative Name Server Returns IP Address

The resolver then returns the IP address to the browser. While the browser queries the site’s web server directly to access it.

* * *

#### TL;DR:

[![DNS Architecture Overview](https://substackcdn.com/image/fetch/$s_!jSCC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F887d1a08-f5a4-40b2-b7d9-0b514105761d_1256x903.png)](https://substackcdn.com/image/fetch/$s_!jSCC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F887d1a08-f5a4-40b2-b7d9-0b514105761d_1256x903.png)DNS Architecture Overview

DNS-DNS queries use Transmission Control Protocol (**TCP**) for reliability and security. While client-DNS queries use User Datagram Protocol (**UDP**) for low overhead and fast response. [1](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work#footnote-1-162185378)

The resolver server stores the IP address in its cache with an expiry time. This allows the server to handle future requests from different users quickly.

While the user’s operating system and browser cache DNS values, it helps to reduce bandwidth usage and latency.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋🏼 Say hi on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 100K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

**TL;DR** 🕰️

You can find a [summary of this article on Twitter](https://x.com/systemdesignone/status/1918235858538201412). Please consider a retweet if you find it helpful.

* * *

[![How Figma Scaled to 4M Users—Without Fancy Databases 🔥](https://substackcdn.com/image/fetch/$s_!_ceu!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8618846e-b8ff-47a4-892d-e0628abbf730_1280x720.gif)How Figma Scaled to 4M Users—Without Fancy Databases 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·April 16, 2025[Read full story](https://newsletter.systemdesign.one/p/postgres-scale)](https://newsletter.systemdesign.one/p/postgres-scale)

[![How Apple Pay Handles 41 Million Transactions a Day Securely 💸](https://substackcdn.com/image/fetch/$s_!6aLm!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff75ede44-6b6f-4e22-ae4d-dc762586955d_1280x720.gif)How Apple Pay Handles 41 Million Transactions a Day Securely 💸[Neo Kim](https://substack.com/profile/135589200-neo-kim)·March 27, 2025[Read full story](https://newsletter.systemdesign.one/p/how-does-apple-pay-work)](https://newsletter.systemdesign.one/p/how-does-apple-pay-work)

* * *

### References

  * [Development of the Domain Name System](https://cseweb.ucsd.edu/classes/wi01/cse222/papers/mockapetris-dns-sigcomm88.pdf)

  * [Domain Names - Concepts and Facilities](https://www.rfc-editor.org/rfc/rfc882.html)

  * [Domain Names - Implementation and Specification](https://datatracker.ietf.org/doc/html/rfc883)

  * [What is DNS? | How DNS works](https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/)

  * [Domain Name System by Wikipedia](https://en.wikipedia.org/wiki/Domain_Name_System)

  * [What is an Authoritative DNS server?](https://www.cloudns.net/blog/authoritative-dns-server/)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

[1](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work#footnote-anchor-1-162185378)

Thanks to [Rafa Páez](https://open.substack.com/users/12296261-rafa-paez?utm_source=mentions)’s comment.

169

23

18

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Rafa Páez's avatar](https://substackcdn.com/image/fetch/$s_!XBKZ!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4d66e45-9f60-42c0-a039-faa7da4c791e_800x800.jpeg)](https://substack.com/profile/12296261-rafa-paez?utm_source=comment)

[Rafa Páez](https://substack.com/profile/12296261-rafa-paez?utm_source=substack-feed-item)

[May 2, 2025](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work/comment/113904516 "May 2, 2025, 3:15 PM")

Liked by Neo Kim

Great post, Neo. The visuals are highly helpful in understanding how this protocol works.

Just a minor detail I read at the end. DNS not only uses TPC. DNS primarily uses UDP (User Datagram Protocol) for most queries and responses due to its speed and low overhead. However, it also utilizes TCP for specific tasks like zone transfers, which require reliable and large data transfers.

ReplyShare

[2 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work/comment/113904516)

[![Ilya Sudakov's avatar](https://substackcdn.com/image/fetch/$s_!_lny!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ee5b541-51c1-4acd-93e2-79151ced3aa7_1320x1320.jpeg)](https://substack.com/profile/285485253-ilya-sudakov?utm_source=comment)

[Ilya Sudakov](https://substack.com/profile/285485253-ilya-sudakov?utm_source=substack-feed-item)

[May 2, 2025](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work/comment/113831114 "May 2, 2025, 9:46 AM")

Liked by Neo Kim

Thank you very much for this concise article!

It’s important to remember that there can be numerous other caches between the Operating System Cache and the Resolver System Cache. For example, there’s an Internet Provider Cache. Google collaborates with providers and strategically places their cache near users to ensure instantaneous service loading.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work/comment/113831114)

[21 more comments...](https://newsletter.systemdesign.one/p/what-is-a-dns-server-and-how-does-it-work/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture