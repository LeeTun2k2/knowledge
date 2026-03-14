[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# 9 Best Practices for API Security ⚔️

### #87: Break Into API Security (7 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 12, 2025

∙ Paid

77

2

7

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines API security best practices. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/api-security-best-practices/?action=share) & I'll send you some rewards for the referrals._

_I created the block diagrams in this newsletter with_ _[Eraser](https://app.eraser.io/auth/sign-up?ref=neo)._

Once upon a time, there was a two-person startup.

Yet they had only a few customers and limited features.

So they ran their site using a tiny number of application programming interfaces (**APIs**).

[![API Security Best Practices](https://substackcdn.com/image/fetch/$s_!-ZRU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3db0a7a-1dd4-41a5-aa18-2b2054b84df4_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!-ZRU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3db0a7a-1dd4-41a5-aa18-2b2054b84df4_1200x630.gif)

But one day, their site became extremely popular.

So they set up a microservices architecture for scalability.

Yet they knew little about API security.

And sent sensitive data in plain text through the API.

Also some of their public APIs often failed because of spiky traffic.

Besides it became difficult to keep APIs secure as they added more features.

So they researched the best practices to secure APIs.

Onward.

* * *

### [CodeRabbit: Free AI Code Reviews in VS Code - Sponsor](https://coderabbit.link/bM7fHiE)

[![CodeRabbit: AI Code Reviews in VS Code](https://substackcdn.com/image/fetch/$s_!qzSH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feeef9a7c-ed97-40e9-b48b-18e8d1b9d98d_1600x814.png)](https://coderabbit.link/bM7fHiE)

**[CodeRabbit](https://coderabbit.link/bM7fHiE)** brings real-time, AI-powered code reviews straight into VS Code, Cursor, and Windsurf. It lets you:

  * Get contextual feedback on every commit, not just at the PR stage

  * Catch bugs, security flaws, and performance issues as __ you code

  * Apply AI-driven suggestions instantly to implement code changes

  * Do code reviews in your IDE for free and in your PR for a paid subscription

[Install in VS Code for FREE](https://coderabbit.link/bM7fHiE)

* * *

## API Security Best Practices

Here’s what they learned:

### 1\. HTTPS

HTTP stands for Hypertext Transfer Protocol.

Think of HTTP as a set of rules for transferring information on the Internet.

But it sends data in plaintext without encryption. It means someone on the public network can easily access or change the information. Thus making it insecure. 

So it’s necessary to have Hypertext Transfer Protocol Secure (HTTPS) on the API.

HTTPS encrypts data before sending it. Imagine **HTTPS** as HTTP running over an extra protocol to keep information secure.

The extra protocol is called Transport Layer Security (**TLS**). Think of TLS as a technique to encrypt data sent between the client and server.

[![How HTTPS Works](https://substackcdn.com/image/fetch/$s_!KCfB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa01b4805-faf0-431b-8a8e-0eedbd395232_664x208.png)](https://substackcdn.com/image/fetch/$s_!KCfB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa01b4805-faf0-431b-8a8e-0eedbd395232_664x208.png)How HTTPS Works

Here's how it works:

  1. The client connects to the API endpoint

  2. The server gives a digital certificate to prove its identity

  3. The browser checks the certificate against the trusted certificate authorities

  4. Both sides agree on a shared encryption key

  5. Then each request gets encrypted with that key

Thus making it secure.

Here are some best practices:

  * Use the latest version of TLS supported by the platform

  * Use HTTP Strict Transport Security (**HSTS**)

  * Use strong ciphers

HSTS gets sent as an [HTTP response header](https://newsletter.systemdesign.one/p/http-headers?open=false#%C2%A7strict-transport-security) (`Strict-Transport-Security`). It tells the browser always to use HTTPS for this domain for a specific period. 

While strong ciphers, such as [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) or [ChaCha20](https://protonvpn.com/blog/chacha20), ensure the encrypted connection is secure.

Let’s keep going!

### 2\. Authentication

Anybody on the internet can call a public API.

Yet only verified users should access the data. So API authentication is necessary; it verifies a user’s identity before giving them system access.

[![How Oauth Works](https://substackcdn.com/image/fetch/$s_!YZSj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe026e626-2b4b-48fb-84ba-ed6c609545b3_613x252.png)](https://substackcdn.com/image/fetch/$s_!YZSj!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe026e626-2b4b-48fb-84ba-ed6c609545b3_613x252.png)How OAuth Works

Here are some best practices:

  * Use OAuth and OpenID Connect (**OIDC**)

    * Think of OAuth as a standard protocol for authorization

    * The client asks for access to the system

    * The authorization server checks the identity and gives them a token

    * The API then checks the token before allowing any action

    * While OIDC is a standard way to handle user authentication on top of OAuth

  * Use multi-factor authentication (**MFA**)

    * It adds extra steps to the authentication for security

    * The client logs in with their username and password

    * The system asks for an MFA code from the authenticator app

    * The authorization server gives a token only if both checks pass

    * The API then checks the token before allowing any action

  * Use short-lived tokens

    * Keep the token lifespan short

    * And refresh them often to reduce risk if a token gets stolen

Ready for the next technique?

### 3\. Authorization

Authorization decides what an authenticated client may do in a system.

Imagine **authentication** as permission to enter a building. While **authorization** is like the key to enter only specific rooms once inside.

[![Authentication vs Authorization](https://substackcdn.com/image/fetch/$s_!SvmQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F794a4480-e027-41c0-aa81-ad8dd9b9b8ae_1200x630.png)](https://substackcdn.com/image/fetch/$s_!SvmQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F794a4480-e027-41c0-aa81-ad8dd9b9b8ae_1200x630.png)Authentication vs Authorization

Here are some best practices:

  * Use Role-Based Access Control (**RBAC**)

    * Group users by their responsibility or function

    * Give separate permissions to each role

    * Then assign users to specific roles so they get access permissions

    * The system checks the role permissions before allowing actions

  * Use clear permission policies

    * Write clear rules that explain who can access what and when

    * Review and update permissions regularly to match the latest responsibilities

Proper access control reduces risk. So always give only the least privilege needed to get the job done.

Ready for the best part?

### 4\. Rate Limiting and Throttling

A popular API might get too many requests at once. 

And this can overload the server and affect its performance. So it’s necessary to rate limit and throttle requests.

**Rate limiting** means controlling the number of requests a client can make to an API within a time window. It protects the API from abuse.

[![How Rate Limiting and Throttling Works](https://substackcdn.com/image/fetch/$s_!zO-a!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a075b1-50ef-4adf-864e-5f80a82fe59d_745x243.png)](https://substackcdn.com/image/fetch/$s_!zO-a!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a075b1-50ef-4adf-864e-5f80a82fe59d_745x243.png)How Rate Limiting and Throttling Work

Here’s how it works:

  1. The client sends requests through a rate limiter

  2. The rate limiter tracks requests from a client by its IP address, user ID, or API key

  3. The extra requests get rejected if the limit exceeds (response status code:` 429 Too Many Requests`)

  4. The [counter](https://en.wikipedia.org/wiki/Token_bucket) then resets after the time window ends

**Throttling** means slowing down requests instead of dropping them. It adds a delay between requests using a queue. Thus preventing server overload.

[![Rate Limiting vs Throttling](https://substackcdn.com/image/fetch/$s_!uxzj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F630d90b7-1aaa-425e-80f4-7d18bf4ab2be_1222x660.png)](https://substackcdn.com/image/fetch/$s_!uxzj!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F630d90b7-1aaa-425e-80f4-7d18bf4ab2be_1222x660.png)Throttling vs Rate Limiting

Rate limits and throttling keep an API stable. Yet it has to be configured properly for a good user experience.

### 5\. Input Validation

There’s a risk of some requests being malformed or malicious. So it’s necessary to validate and sanitize requests before processing them.

**Validation** checks the request structure, while **sanitization** removes unsafe data.

[![Validating and Sanitizing Requests Before Processing](https://substackcdn.com/image/fetch/$s_!Iwvp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F627178dc-a2be-4240-bcf5-c6cd2e282c67_1067x235.png)](https://substackcdn.com/image/fetch/$s_!Iwvp!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F627178dc-a2be-4240-bcf5-c6cd2e282c67_1067x235.png)Validating and Sanitizing Requests Before Processing

Here’s how it works:

  * Use schema validation to ensure the request has the expected structure

  * Sanitize requests using proven libraries and tools to block malicious content

This prevents malicious scripts, SQL injections, and malformed data.

**SQL injection** means sending malicious commands to the database via API requests. Think of it like smuggling prohibited items inside a shampoo bottle at airport security.

Only safe requests should reach the system. So validate and sanitize public APIs.

### 6\. Logging and Monitoring

Prevention is better than cure.

Because of this, it’s necessary to track API activity for unusual traffic or suspicious behavior. Think of it like having security cameras and alarms for safety.

[![Monitoring API for Reliability](https://substackcdn.com/image/fetch/$s_!qSxk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7e5293c8-39a5-4175-bf5c-be4e43d08382_1154x367.png)](https://substackcdn.com/image/fetch/$s_!qSxk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7e5293c8-39a5-4175-bf5c-be4e43d08382_1154x367.png)Monitoring API for Reliability

Here are some best practices:

  * Log metadata, such as IP address, timestamp, and headers; but avoid sensitive data

  * Use dashboards or security information and event management (**[SIEM](https://en.wikipedia.org/wiki/Security_information_and_event_management)**) tools to find anomalies and raise alerts

SIEM collects logs from different services and analyzes them to find suspicious activity.

Monitoring turns logs into early warnings. So use them to avoid API abuse before it causes damage.

Let’s keep going!

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

### 7\. Security Audit

There is a risk of misconfiguration and security vulnerabilities. So it’s necessary to perform security audits regularly.

A security audit means checking the system to find and fix vulnerabilities before attackers exploit them. Think of it like hiring a locksmith to test all the doors in the house to ensure they lock properly.

[![Stress Testing an API](https://substackcdn.com/image/fetch/$s_!KD-J!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F25fb1979-b63c-4142-bc01-ddc4e7b7232c_890x621.png)](https://substackcdn.com/image/fetch/$s_!KD-J!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F25fb1979-b63c-4142-bc01-ddc4e7b7232c_890x621.png)Stress Testing an API

Here are some best practices:

  * [Fuzz test](https://en.wikipedia.org/wiki/Fuzzing) APIs with unexpected inputs to catch bugs or failures

  * [Stress test](https://en.wikipedia.org/wiki/Stress_testing_\(software\)) APIs by overloading them with many calls to check if they stay reliable

Security audits expose vulnerabilities. So use them to fix issues early.

### 8\. Dependency Management

Third-party code may contain hidden vulnerabilities.

And a system is only as secure as its weakest part. So it’s necessary to keep libraries and frameworks up to date.

Imagine APIs as a city’s traffic control system. If a traffic light fails, accidents might happen.

[![Tracking Vulnerable Dependencies](https://substackcdn.com/image/fetch/$s_!0Rul!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff0dbbc1e-f1da-4cbc-8546-d96989a85403_880x481.png)](https://substackcdn.com/image/fetch/$s_!0Rul!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff0dbbc1e-f1da-4cbc-8546-d96989a85403_880x481.png)Tracking Vulnerable Dependencies

Here are some best practices:

  * Track vulnerable dependencies**** using tools like [Dependabot](https://github.com/dependabot), [Snyk](https://snyk.io/), or [npm audit](https://docs.npmjs.com/cli/v9/commands/npm-audit)

  * Patch security problems early

  * Remove unused dependencies

Ready for the best technique?

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fapi-security-best-practices&utm_source=paywall&utm_medium=web&utm_content=173246532)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fapi-security-best-practices&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture