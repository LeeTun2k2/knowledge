[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# API Versioning - A Deep Dive

### #92: Understanding API Versioning Strategies (14 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)[![Irina Dominte's avatar](https://substackcdn.com/image/fetch/$s_!8Dqu!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F00886069-c40a-4580-884b-7fee7c4b9bce_96x96.png)](https://substack.com/@irinadominte)

[Neo Kim](https://substack.com/@systemdesignone) and [Irina Dominte](https://substack.com/@irinadominte)

Sep 30, 2025

172

6

30

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines API versioning strategies._

  * _[Share this post](https://newsletter.systemdesign.one/p/api-versioning/?action=share) & I'll send you some rewards for the referrals._

Building application programming interfaces (**APIs**) is easy.

But keeping them stable and predictable as the system evolves is difficult.

API consumers depend on API contracts, and even a tiny change can break many integrations. For example, renaming a field, changing a response format, or changing an endpoint could stop someone’s production system from working correctly.

Business requirements change, so it’s necessary to communicate the API adjustments clearly.

That’s where **versioning** comes in. It provides a structured approach for evolving APIs without leaving the API consumers behind.

* * *

I want to introduce [Irina Dominte](https://www.linkedin.com/in/irinascurtu/) as a guest author.

[![](https://substackcdn.com/image/fetch/$s_!o-EY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa577eee7-dd52-41e8-b21d-47aeab8f9684_1200x630.png)](https://www.linkedin.com/in/irinascurtu/)

She’s a software architect, Microsoft MVP, with a passion for distributed systems, .NET, and system design.

Check out her blog and social media:

  * [Irina Codes](https://irina.codes/)

  * [Irina’s Courses](https://bit.ly/irina-courses)

  * [LinkedIn](https://www.linkedin.com/in/irinascurtu/)

You’ll find her speaking at developer conferences around the world and blogging online.

* * *

API Versioning sets guidelines for introducing changes, shows how extensive those changes are, and ensures consumers can transition to new versions at their own pace.

But here’s the catch: just adding a version number in the URL doesn’t automatically prevent all the problems.

I’ve seen many APIs across different domains that expose a `/v1/ `in their URLs.

They add new data, change property names, change their types, remove fields, and so on. But years later, they run the same version even though the API has changed a lot.

And many APIs end up `with v1` in the URL forever because the same organization controls the consumer apps as well.

So why add a version at all?

When different teams at the same company are the API owners and API consumers, version upgrades are “just” code changes. Teams can synchronize deployments, push breaking changes, and update consumers with little risk of breaking changes.

[![](https://substackcdn.com/image/fetch/$s_!0KcQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62086083-0f12-4723-91c1-99415d2dfe22_1214x690.png)](https://substackcdn.com/image/fetch/$s_!0KcQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62086083-0f12-4723-91c1-99415d2dfe22_1214x690.png)

However, when you expose the API publicly and don’t own the consumers, the scenario changes completely. It means you cannot simply break compatibility.

You’re then limited by:

  * Service Level Agreements (**SLA**)

  * Legal contracts

  * Third-party dependencies

…all of which force you to design carefully and communicate clearly.

It’s better to avoid spending nights answering support calls because someone’s app broke when you changed an API endpoint.

Onward.

* * *

### [Build Real-Time Communication Experiences with Stream (Sponsor)](https://getstream.io/?utm_source=newsletter&utm_medium=referral&utm_content=&utm_campaign=neokim)

[![](https://substackcdn.com/image/fetch/$s_!12Ka!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd176d168-2a4a-4f64-8b95-fa1979f87e12_1200x630.png)](https://getstream.io/?utm_source=newsletter&utm_medium=referral&utm_content=&utm_campaign=neokim)

With [Stream’s](https://getstream.io/?utm_source=newsletter&utm_medium=referral&utm_content=&utm_campaign=neokim) SDKs for React, React Native, Flutter, iOS, Android, and more, you can add production-ready **chat** , **voice/video calling** , **activity feeds** , and **AI moderation** to your app—with just a few lines of code. Backed by a global edge network and a generous free Maker plan, start prototyping today and scale to millions.

[Start Coding Free](https://getstream.io/?utm_source=newsletter&utm_medium=referral&utm_content=&utm_campaign=neokim)

* * *

## API as a Contract

It’s helpful to think of APIs as contracts.

The contract describes the shape of the data, the form of the endpoints, and the behavior of the API. The client and the provider both agree to this contract.

[![](https://substackcdn.com/image/fetch/$s_!ycSu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9f3b017e-3e43-4dfa-b619-f0a5e5de054e_1145x400.png)](https://substackcdn.com/image/fetch/$s_!ycSu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9f3b017e-3e43-4dfa-b619-f0a5e5de054e_1145x400.png)

However, the owner adds a new field when business requirements change or when they fix a bug. It means that API contracts change.

And versioning is how we say:

  * Here’s the old contract, still valid.

  * Here’s the new contract, with improvements.

There are three paths forward as the API evolves:

#### 1\. Release a new version in a new location

This means creating `/api/v2/orders` while keeping `/api/v1/orders` around.

The users can use the old version as it is. And switch only when they want the new features. This approach is safe and straightforward for clients, but it creates a lot of work for the API owner.

The API owner has to maintain several versions at the same time. For example, fix bugs, apply security updates, or implement features on all supported versions.

Besides, this approach might become expensive and slow down future development.

#### 2\. Release a backward-compatible version

In this approach, the API changes in a way that doesn’t break existing clients.

This means _additive_ _changes_ , such as including new fields, optional parameters, or endpoints in a response. As everything works the same way as earlier, consumers don’t have to update anything.

This strategy allows gradual growth but might be limiting when large design shifts are necessary.

#### 3\. Break compatibility

In this scenario, the API changes require _each client_ _to upgrade_.

Although it sounds like the worst option, sometimes it’s unavoidable. For example, there might be a need for a different data model, or a serious security flaw needs to be addressed. Or the original API design had issues they couldn't fix easily.

* * *

In reality, **we need a mix of all three** , based on the change type and its impact.

But before we go further, let’s understand these technical terms first:

[![](https://substackcdn.com/image/fetch/$s_!AB-N!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6d8ddf82-550c-41ea-b2c2-5029f067ab0c_561x562.png)](https://substackcdn.com/image/fetch/$s_!AB-N!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6d8ddf82-550c-41ea-b2c2-5029f067ab0c_561x562.png)

**Additive versioning**

It means additive changes to an API without incrementing its version.

You add new fields, parameters, or endpoints, but never remove or rename anything. It’s easy for consumers because they don’t have to change their integrations.

**Explicit versioning**

It means acknowledging when a change breaks compatibility.

You create a new version using a URL, header, or parameter. Thus making it possible to see old behavior in one version and new behavior in another.

I’ll walk you through different approaches to explicit versioning of an API, their benefits, and their drawbacks next.

* * *

## Types of API Versioning

Let’s dive in:

### 1\. Path-Based Versioning

This is one of the most common approaches.
    
    
    https://coolapi.com/api/**v1** /orders

It solves the problem of clarity and visibility because the consumer can easily understand which version they’re using by simply looking at the path.

Thus making it easier for new consumers to follow documentation and configure routing logic.

However, this clarity comes at the cost of stability. Here are some of its downsides:

  * Maintaining many versions creates redundancy and overhead for the API owner.

  * It pollutes URLs with version information.

Although this approach looks simple to implement, here’s the challenge:

Roy Fielding, the creator of REST, said that it represents evolvability. That means the ability to change how a resource looks or behaves without changing its identity. 

[![](https://substackcdn.com/image/fetch/$s_!TWJm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F906b9c3f-7ec5-4b8b-b204-4277e5683bdc_767x265.avif)](https://substackcdn.com/image/fetch/$s_!TWJm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F906b9c3f-7ec5-4b8b-b204-4277e5683bdc_767x265.avif)

And Tim Berners-Lee said, “[Cool URIs don’t change](https://www.w3.org/Provider/Style/URI)”. It means once you publish a URL, don’t change it. While people and systems should be able to rely on it forever.

If we link those together, versioning the URL path might become unnecessary.

So let’s just take a step back for a moment.

  * A REST API represents resources.

  * If `/api/orders/123` points to an order, it should always point to that order, even if the response structure changes.

  * But adding `v1` or `v2` in the path (`/api/v1/orders`) breaks the principle, because it treats the same thing (an order) as if it had two unique identities.

  * The resource (an order) hasn’t changed into something else, but only its representation has changed.

Here's how to best use path-based versioning:

  * Keep older versions alive for a transition period.

  * Announce clear deprecation timelines and provide migration guides.

  * Use shared libraries for common logic so that bug fixes don’t need to be copied everywhere.

Besides, HTTP itself provides mechanisms for communicating versioning and deprecation.

Here are two of the most useful techniques:

**Sunset Header**

An API uses the [Sunset Header (RFC 8594)](https://datatracker.ietf.org/doc/html/rfc8594) to signal when a version becomes unavailable.

For example,
    
    
    Sunset: Wed, 01 Jul 2026 00:00:00 GMT

It tells the consumers that the current version is scheduled to retire on 01 July 2026. And combining this with headers like `Deprecation:true`, the client gets enough time to plan the migration.

**3xx Redirects**

Redirects are another simple way to guide clients from one version to another.

For example,
    
    
    301 Moved Permanently
    
    Location: /api/v2/orders

This allows you to respond to a request `/api/v1/orders `with a 301 status code and a Location header, which specifies where the resource can be found.

Thus helping the consumers who don’t update their code right away by guiding them to the newest version.

[![](https://substackcdn.com/image/fetch/$s_!VzlT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9b7104d4-c451-4921-97a2-38a48ffaa4f0_772x182.png)](https://substackcdn.com/image/fetch/$s_!VzlT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9b7104d4-c451-4921-97a2-38a48ffaa4f0_772x182.png)

But redirects work well when endpoints have changed only a little; otherwise, clients will need code changes.

### 2\. Query Parameter Versioning

This approach keeps the base URL stable and includes the version as a URL parameter.
    
    
    https://coolapi.com/api/orders?**api-version=1.2.3**

It solves the **flexibility problem at the resource level**.

Here are some of its advantages:

  * URL remains stable.

  * The API owner can version endpoints or even individual resources without changing the overall API structure.

  * The consumer doesn’t have to update the base URLs, and different versions can coexist on the same path.

But this approach has its downsides. Here are some of them:

  * Many parameters can make it messy.

  * Routers rely on paths, not query strings; so you’ll need custom logic in the gateway or application to handle different versions.

  * Documentation has to be precise; otherwise, consumers may forget to include the query parameter and end up confused.

  * Caching becomes difficult because of cache pollution, misconfiguration, or wrong cache hits.

  * There’s a risk of consumers receiving the wrong version, as intermediate servers might ignore query parameters.

Here are some best practices for query parameter versioning:

  * Configure your CDN or cache to always include the query parameters in the cache key.

  * Document the parameters in each example call and check their presence on the server side to avoid silent issues.

  * Normalize query strings where possible to avoid duplicate cache entries. For example, treat ``?version=2` and `?version=02` `as the same.

Although this approach gives you resource level control, it might complicate your codebase and caching strategy.

### 3\. Message Payload Versioning

This technique includes the version as part of the request and response body itself.
    
    
    {
      **“version”: “v1"** ,
      “data”: {
        “id”: 123,
        “status”: “shipped”
      }
    }

Payload versioning enables the management of **long-lived data in asynchronous systems**. It means the system stores the payload with version information for later replay in event-driven or queue-based architectures.

For example,

  * You publish an event `{ “version”: “v1”, ... }` to a queue.

  * A service reads it right away or maybe 1 year later.

  * Because the message includes the version, the service quickly understands which format (schema) to use for deserialization.

However, this approach mixes concerns because versioning is part of business data.

Besides, the consumer has to understand and process versions (schemas) correctly. This adds extra complexity for REST APIs with short request/response lifecycles.

[![](https://substackcdn.com/image/fetch/$s_!ITZr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb23de37-98a9-4148-a88d-cd8d8c02a6dd_818x213.png)](https://substackcdn.com/image/fetch/$s_!ITZr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb23de37-98a9-4148-a88d-cd8d8c02a6dd_818x213.png)

Here’s how to best use message payload versioning:

  * Use it only for asynchronous or event-driven systems.

  * Treat messages as fixed facts and create schema registries. This helps consumers safely de-serialize old formats.

Only use payload versioning if you store and reuse messages later (like in queues or logs).

For standard REST APIs, where responses are used immediately and then discarded, it just makes things harder.

### 4\. Header-Based Versioning

This approach passes the version information in the request headers.
    
    
    GET /api/orders
    
    **api-version: 2**

Another way is to version APIs through the `Accept` header.

The client specifies the resource version by requesting a specific media type. This is known as **media type versioning.**

For example,
    
    
    Accept: application/vnd.example+json;api-version=2
    
    Accept: application/vnd.github.v2+json

The API will then respond with the resource formatted for version 2 of the contract.

Here are some advantages of header-based versioning:

  * Allows clients to upgrade to new API versions at their own pace.

  * Keeps URIs stable and self-describing.

  * Aligns with HTTP semantics as metadata belongs in headers.

However, this approach has its downsides. Here are some of them:

  * Harder to debug, log, and cache as version information isn’t visible in the URL.

  * Some proxies or middleware might strip or block custom headers if not set up properly.

Besides, caching gets tricky when the version is in the `Accept` header.

CDNs, reverse proxies, and browsers might not know that different headers mean different versions. To solve this, you add a `Vary: Accept` [header](https://datatracker.ietf.org/doc/html/rfc7231). It tells the cache to keep separate copies for each version and not to mix them up.

Header-based versioning may look hard at first, but it’s the cleanest long-term choice.

Your URLs stay stable, while versions are handled in headers. This keeps APIs consistent, avoids clutter, and follows REST principles. You need extra setup for caching, but the payoff is a smaller, easy-to-maintain API with less technical debt.

Now that we’ve covered versioning types, let’s explore versioning formats.

* * *

## Versioning Formats

A versioning format is the way you label API versions to show what changed or when it changed.

### 1\. Semantic Versioning

Semantic Versioning (**[SemVer](https://semver.org/)**) uses the format `MAJOR.MINOR.PATCH`. This helps to describe the changes included in a release.

  * MAJOR: Breaking changes (clients must actively migrate).

  * MINOR: Backward-compatible changes (new fields, endpoints).

  * PATCH: Bug fixes only.

For example, `2.4.1` indicates the second major release, fourth minor, and first patch.

This format gives consumers expectations:

  * 1.9 → 2.0 is a big deal.

  * But 2.4 → 2.5 should be safe.

### 2\. Calendar Versioning

Calendar versioning (**[CalVer](https://calver.org/)**)__ uses dates instead of numbers.

For example,

  * Ubuntu 24.04: released in April 2024 (YY.MM). 

  * Python 3.12.20231001: released on 1 October 2023 (YYYYMMDD).

This format tells you the release date, which makes it easier to track freshness rather than compatibility.

### 3\. Hash Versioning

Hash versioning (**[HashVer](https://miniscruff.github.io/hashver/)**) uses hashes (like Git commit IDs) instead of numbers or dates. It points to the exact state of the code at a specific time.

For example,

  * v-237a2b4f: shortened Git commit hash of the build.

This format is precise for traceability, but harder for humans to understand changes.

* * *

## Case Studies

It’s one thing to talk theory, but let’s look at how companies are designing their APIs: 

### 1\. Stripe

  * Uses **[header-based versioning](https://docs.stripe.com/api/versioning)** with a `Stripe-Version` header.

  * New accounts are automatically “pinned” to the latest version.

  * Versions are in ([CalVer](https://stripe.com/blog/api-versioning)) date-based format: `Stripe-Version: 2023-10-16`.

  * Most changes are [backward-compatible](https://docs.stripe.com/upgrades); breaking changes are rare and controlled.

  * Clients can override the version on a per-request basis to test or migrate.

  * URLs stay clean as the versioning information lives in headers.

Stripe’s approach makes upgrades safe and client-driven. This aligns well with the principle: _don’t break your consumers without their consent_.

### 2\. GitHub

  * Also uses **header-based versioning** with date identifiers (`X-GitHub-Api-Version: 2022-11-28`).

  * The base URL never changes: https://api.github.com

  * They support each version for **at least 24 months** , giving consumers time to migrate.

  * Additive changes roll out to all versions; breaking changes trigger a new version.

  * Keeps URIs stable, avoids version churn, and treats versions as snapshots in time.

  * If the header is missing, the API defaults to the latest version.

GitHub’s approach offers stable URLs and predictable migrations while reducing unnecessary versions. It highlights another philosophy: _think of your_ _API as incremental snapshots in time_.

Let’s keep going!

* * *

## When to Increase the Version?

There’s no fixed rule. Raise a new version only when the API contract changes meaningfully.

Ask yourself:

  * How many consumers depend on this?

  * Will the change break them?

  * Do SLAs or legal obligations apply?

  * Can you maintain many versions?

Be pragmatic; don’t version every deployment. Do it only when the change affects consumers.

* * *

## Tooling for API Versioning

Versioning isn’t just about design. It also needs tools for routing, documentation, and consistency.

### 1\. API Gateway

An API gateway manages traffic and routes requests to the correct services. It makes it easy to run different API versions in parallel.

[![](https://substackcdn.com/image/fetch/$s_!pawu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2574c630-5d5f-4e36-adf7-cb45fd64e468_793x422.png)](https://substackcdn.com/image/fetch/$s_!pawu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2574c630-5d5f-4e36-adf7-cb45fd64e468_793x422.png)

  * Some examples: [Kong API](https://konghq.com/), [Apigee](https://cloud.google.com/apigee), [AWS API Gateway](https://aws.amazon.com/api-gateway/), [Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/api-management-key-concepts).

  * You can keep `/v1/orders` active while routing `/v2/orders` to a new backend until consumers migrate.

### 2\. Documentation Frameworks

Documentation shows consumers which version they’re using and what changed.

A documentation framework is a tool that helps you write and publish clear API docs so consumers know how to use different versions.

  * Some examples: [OpenAPI / Swagger](https://swagger.io/specification/), [Postman](https://www.postman.com/), [Redoc](https://redocly.com/).

  * You can publish documentation for both old and new API versions, and tools like Swagger will simply show what changed between them.

### 3\. Versioning Libraries

Libraries reduce boilerplate and enforce consistent versioning across endpoints. 

  * Some examples: [.NET API Versioning](https://github.com/dotnet/aspnet-api-versioning), [Spring Boot guides](https://reflectoring.io/spring-boot-api-versioning/), [Express.js middleware](https://www.npmjs.com/package/express-routes-versioning).

  * It lets you define versions in routes or headers, and send requests to the right version of your API automatically.

These tools don’t replace a strategy, but make it easy to manage versions at scale.

* * *

## When to Use Which Strategy

There’s no single “best” way to version APIs. Each strategy solves a unique problem.

The right choice depends on your context:

  * Path-based versioning: internal systems where you control all consumers.

  * Query parameter versioning: resource-level flexibility, and can tolerate routing complexity.

  * Header-based versioning: safest for external or long-lived clients you don’t control.

  * Payload versioning: event-driven systems where messages are replayed later.

And with version formats:

  * SemVer: shows impact (breaking, additive, bug fix).

  * CalVer: shows freshness (release).

  * HashVer: shows traceability (commit or build).

**The bottom line:**

The additive strategy is good enough for most use cases.

So pick the strategy that causes the fewest surprises to consumers. Stability and clear migration paths build trust. That trust is more valuable than any technical detail.

* * *

👋 I’d like to thank [Irina](https://www.linkedin.com/in/irinascurtu/) for writing this newsletter!

  * Also try her blog & courses: **[Irina Codes](https://irina.codes/)** and **[Irina’s Courses](https://bit.ly/irina-courses)**.

It’ll help you become a better software engineer.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**👋 Say hi on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Neo’s recommendation** 🚀

Try **[Ashutosh’s newsletter](https://ashutoshmaheshwari.substack.com)** to get deep dives on system design, AI, and software engineering.

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 170K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter.

You are now 170,001+ readers strong, very close to 172k. Let’s try to get 172k readers by 10 October. Consider sharing this post with your friends and get rewards.

Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/api-versioning?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

172

6

30

Share

PreviousNext

[![Irina Dominte's avatar](https://substackcdn.com/image/fetch/$s_!8Dqu!,w_52,h_52,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F00886069-c40a-4580-884b-7fee7c4b9bce_96x96.png)](https://substack.com/@irinadominte?utm_source=byline)| A guest post by| [Irina Dominte](https://substack.com/@irinadominte?utm_campaign=guest_post_bio&utm_medium=web)Irina is a software architect, Microsoft MVP, with a passion for distributed systems, .NET, and the world around it. | [Subscribe to Irina](https://irinadominte.substack.com/subscribe?)  
---|---  
  
#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Sep 30](https://newsletter.systemdesign.one/p/api-versioning/comment/161419174 "Sep 30, 2025, 12:16 PM")

Liked by Irina Dominte, Neo Kim

Every successful API needs versioning.

It is also a good practice to monitor the number of clients still using older versions and log failed requests that may be due to version mismatches. 

That gives guidance about when to sunset old versions. 

Great Summary!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/api-versioning/comment/161419174)

[![Chinedu Elijah Okoronkwo's avatar](https://substackcdn.com/image/fetch/$s_!dOix!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb52a34f0-3df0-4fe5-b291-a10ea4afa2b2_96x96.jpeg)](https://substack.com/profile/19184198-chinedu-elijah-okoronkwo?utm_source=comment)

[Chinedu Elijah Okoronkwo](https://substack.com/profile/19184198-chinedu-elijah-okoronkwo?utm_source=substack-feed-item)

[Oct 15](https://newsletter.systemdesign.one/p/api-versioning/comment/166526600 "Oct 15, 2025, 8:56 AM")

I appreciate this post. I think I understand better now

ReplyShare

[4 more comments...](https://newsletter.systemdesign.one/p/api-versioning/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture