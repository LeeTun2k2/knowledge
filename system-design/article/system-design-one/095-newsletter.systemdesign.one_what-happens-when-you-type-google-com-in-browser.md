[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# What Happens When You Type google.com in the Browser?

### #24: Learn More - What Happens When You Type a URL Into Your Browser? (6 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Nov 26, 2023

70

4

7

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

This is a popular interview question and something that a typical person does every day. 

Yet most people don’t know what happens when they type google.com in the browser and press enter.

  * _[Share this post](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser/?action=share) & I'll send you some rewards for the referrals._

## What Happens When You Type google.com in the Browser?

Here is an overview:

  1. DNS resolution

  2. TCP 3-way handshake

  3. HTTPS upgrade

  4. HTTP request-response

  5. Browser rendering the server response

I’ll teach you each step in detail. But let me introduce this awesome newsletter before that.

* * *

### [Quastor (Featured)](https://www.quastor.org/?utm_source=systemdesignnewsletter&utm_medium=crosspromo&utm_campaign=first)

Quastor is a free newsletter that analyzes over 200 Big Tech engineering blogs and sends you summaries of the most interesting posts. It’s a fantastic way to learn about the strategies and technologies companies use to scale up to serve billions of users.

Past articles include _How Quora scaled MySQL to 100k+ Queries Per Second_ and _How OpenAI trained ChatGPT_.

Join 40,000+ developers who read Quastor. It’s free!

[Try it](https://www.quastor.org/?utm_source=systemdesignnewsletter&utm_medium=crosspromo&utm_campaign=first)

* * *

### 1\. What is Domain Name Resolution?

The browser uses the internet protocol (**IP**) address of the Google server to transfer data. 

But domain names are needed because it's difficult to remember numbers (IP addresses).

Domain Name System (**DNS**) is a database that stores mapping from a domain name (google.com) to its IP address (142.250.185.78). Put another way, DNS is similar to a telephone book.

[![DNS hierarchy](https://substackcdn.com/image/fetch/$s_!_skV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4de11e6-06d7-4ca9-9a2e-d738196ffd4f_1726x1021.png)](https://substackcdn.com/image/fetch/$s_!_skV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe4de11e6-06d7-4ca9-9a2e-d738196ffd4f_1726x1021.png)DNS Hierarchy

The domain name space is an inverted tree structure. DNS resolution works by traversing the inverted tree from top to bottom.

There are _13 root servers_ with replicas set up across the world. And top-level domain in the DNS hierarchy is called the _root domain_.

[![URL structure](https://substackcdn.com/image/fetch/$s_!chFN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd326e8cc-436e-400a-aadc-8b0c741728bd_1133x615.png)](https://substackcdn.com/image/fetch/$s_!chFN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd326e8cc-436e-400a-aadc-8b0c741728bd_1133x615.png)URL Structure

A URL can be divided into different parts:

  * Protocol: Informs web server on which protocol to use like HTTPS or FTP

  * Subdomain: A service offered by a website like Google Search or Maps

  * Domain: Name of the website

  * Top-level domain: Type of the organization entity like .edu or .org

DNS resolution is sequential. Yet subsequent steps get executed only if the previous step doesn’t have the DNS entry.

[![What is Domain Name Resolution](https://substackcdn.com/image/fetch/$s_!ZKH_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbc4b6640-2faf-46d8-9a5f-e6525fee1ac1_2430x1129.png)](https://substackcdn.com/image/fetch/$s_!ZKH_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbc4b6640-2faf-46d8-9a5f-e6525fee1ac1_2430x1129.png)Domain Name Resolution

Here is how DNS gets resolved:

  1. The browser checks its local cache for the DNS entry

  2. The browser checks whether the operating system cache contains the DNS entry. It does that by executing a system call

  3. The browser makes a DNS request to the home router or gateway server to check its local cache

  4. The router forwards the DNS request to the internet service provider (**ISP**) to check its DNS cache

  5. DNS resolver queries the root servers

  6. DNS resolver queries the top-level domain servers like .com or .org

  7. DNS resolver queries Authoritative name servers like google.com

Each component in the DNS resolution caches the result when a DNS query gets resolved. Also DNS entry is assigned a time-to-live (**TTL**) expiry limit.

### 2\. What Is a Three-Way Handshake in TCP?

The browser uses Hypertext Transfer Protocol (**HTTP**) to transfer data.

HTTP is an abstract protocol. It’s part of the application layer or layer 7 in the Open Systems Interconnection (**OSI**) model. Besides it transfers data in a human-readable format.

While Transmission Control Protocol (**TCP**) is a low-level protocol. It belongs to the transport layer or layer 4 in the OSI model. It allows the detection of errors and the retransmission of corrupted data packets.

TCP uses a _bi-directional_ communication channel. So it makes a _three-way_ handshake to create one.

[![What Is a Three-Way Handshake in TCP](https://substackcdn.com/image/fetch/$s_!Bazc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe5bd2973-6ade-40c3-8137-c6adc185b6c2_1375x1171.png)](https://substackcdn.com/image/fetch/$s_!Bazc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe5bd2973-6ade-40c3-8137-c6adc185b6c2_1375x1171.png)Three-Way Handshake in TCP

This is how a browser opens a TCP connection to the server:

  1. The browser sends a SYN request with a random sequence number

  2. The servers respond with SYN-ACK. The acknowledgment number is created by incrementing the received sequence number by 1. Also the server sends a random sequence number.

  3. The browser sends ACK. The acknowledgment number is created by incrementing the received sequence number by 1.

### 3\. How to Upgrade From HTTP to HTTPS?

HTTP doesn’t encrypt the data. So anybody can eavesdrop on the data packets.

While HTTPS encrypts the data to prevent man-in-the-middle attacks.

So Hypertext Transfer Protocol Secure (**HTTPS**) can be considered as an extension of Hypertext Transfer Protocol (**HTTP**).

[![How to Upgrade From HTTP to HTTPS](https://substackcdn.com/image/fetch/$s_!T61l!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94189ea8-2c59-4daf-96dd-db7b6345a630_1375x1386.png)](https://substackcdn.com/image/fetch/$s_!T61l!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94189ea8-2c59-4daf-96dd-db7b6345a630_1375x1386.png)HTTPS Upgrade

Here is how the upgrade from HTTP to HTTPS works:

  1. The browser makes an HTTPS upgrade request to the server using HTTP request headers

  2. The server sends a Secure Sockets Layer (**SSL**) certificate signed by the Certificate Authority (**CA**). It contains the public key of the origin server

  3. The browser uses the public key of the CA to validate the SSL certificate. It does that by checking the digital signature. Also most modern browsers store the public keys of the popular CA

  4. The browser creates a new symmetric private key. It then encrypts the new symmetric private key with the server’s public key

  5. The server decrypts the new symmetric private key

  6. Both browser and server use the new symmetric private key for further communication. So all messages are encrypted

In simple words, HTTP upgrade request uses _asymmetric_ encryption. While communication that occurs after an HTTPS upgrade uses _symmetric_ encryption.

Asymmetric encryption uses a key pair: public and private keys. While symmetric encryption uses a single private key to encrypt and decrypt the messages.

Asymmetric encryption can be compared to an email service. The public key is like the email address while the private key is like the password. So anybody with the email address can verify the sender of the email. But only the owner can access the email using the password.

### 4\. HTTP Request Response

The browser makes a GET request to view the Google webpage. 

While a POST request is used to search a keyword via the search engine.

An HTTP request consists of different entities:

  * Uniform Resource Locator (**URL**)

  * HTTP headers

  * HTTP body (optional)

The HTTP method (**Verb**) defines the type of action to be performed on the server. The popular HTTP methods are:

[![HTTP Request Response; HTTP Methods](https://substackcdn.com/image/fetch/$s_!zSyI!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff659cfaf-da0a-4dea-b234-0919d263620d_2000x428.png)](https://substackcdn.com/image/fetch/$s_!zSyI!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff659cfaf-da0a-4dea-b234-0919d263620d_2000x428.png)HTTP Methods

This is how the server handles an HTTP request:

  1. The server forwards the HTTP request to the request handlers. The request handler is a piece of code defined in any programming language like Python, Node.js, or Java

  2. The request handler checks the HTTP request headers (content-type, content-encoding, cookies, etc)

  3. The request handler validates the HTTP request body

  4. The request handler generates a response in the content type (JSON or XML) requested by the client

The popular HTTP headers are:

[![HTTP Request Response; HTTP headers](https://substackcdn.com/image/fetch/$s_!u18r!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4cd0eb0f-987b-49ef-9bc2-777e44989a50_2004x669.png)](https://substackcdn.com/image/fetch/$s_!u18r!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4cd0eb0f-987b-49ef-9bc2-777e44989a50_2004x669.png)HTTP Headers

### 5\. How Browser Renders Server Response

The browser usually makes different HTTP requests to get the data to render a website. The types of files needed are CSS, JavaScript, and Images.

The browser then [renders](https://newsletter.systemdesign.one/i/136403116/what-is-critical-rendering-path) the HTML with the received data.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Does Netflix Work?](https://substackcdn.com/image/fetch/$s_!mkdo!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1d73ba16-80c9-4237-ba4a-95b88698d1bc_1280x720.gif)How Does Netflix Work?[NK](https://substack.com/profile/135589200-nk)·November 16, 2023[Read full story](https://newsletter.systemdesign.one/p/how-does-netflix-work)](https://newsletter.systemdesign.one/p/how-does-netflix-work)

[![Everything You Need to Know About Micro Frontends](https://substackcdn.com/image/fetch/$s_!tijZ!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fedb1b927-4c3a-4b8d-a9e2-650da7093495_1280x720.png)Everything You Need to Know About Micro Frontends[NK](https://substack.com/profile/135589200-nk)·November 14, 2023[Read full story](https://newsletter.systemdesign.one/p/micro-frontends)](https://newsletter.systemdesign.one/p/micro-frontends)

* * *

## References

  * https://systemdesign.one/what-happens-when-you-type-url-into-your-browser/

  * https://newsletter.systemdesign.one/p/what-is-critical-rendering-path

70

4

7

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![veeru's avatar](https://substackcdn.com/image/fetch/$s_!L19P!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd261c4c9-7dc0-47b8-b26a-4e7ac538d083_144x144.png)](https://substack.com/profile/185795855-veeru?utm_source=comment)

[veeru](https://substack.com/profile/185795855-veeru?utm_source=substack-feed-item)

[Dec 4, 2023](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser/comment/44785999 "Dec 4, 2023, 8:29 PM")Edited

Liked by Neo Kim

simple and elegant. small nit pick is step #4 of HTTPS upgrade. The browser creates a private "session key" (not a private key). again, very small nit pick, but when you say "symmetric private key", there is no such thing because a private key is always associated with asymmetricity.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser/comment/44785999)

[![Healthy Developer's avatar](https://substackcdn.com/image/fetch/$s_!dUMY!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2e91599-8892-4fcb-bd1f-c6c961acf1bd_718x573.png)](https://substack.com/profile/174051539-healthy-developer?utm_source=comment)

[Healthy Developer](https://substack.com/profile/174051539-healthy-developer?utm_source=substack-feed-item)

[Nov 29, 2023](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser/comment/44479866 "Nov 29, 2023, 7:20 PM")

Liked by Neo Kim

super cool

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser/comment/44479866)

[2 more comments...](https://newsletter.systemdesign.one/p/what-happens-when-you-type-google-com-in-browser/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture