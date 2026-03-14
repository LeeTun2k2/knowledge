[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Shopify Handled 30TB per Minute With a Modular Monolith Architecture 🔥

### #63: Break Into Modular Monolith Architecture (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Dec 22, 2024

∙ Paid

241

5

15

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines modular monolith architecture. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/modular-monolith/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, 3 people decided to sell snowboarding equipment online.

And set up an e-commerce platform.

Yet their business model failed.

So they pivoted to create a software-as-a-service model with the e-commerce platform and called it Shopify.

Their growth rate was explosive.

But they wanted to keep it simple and reduce costs.

So they built Shopify using a classic monolith architecture in Ruby on Rails.

[![modular monolith](https://substackcdn.com/image/fetch/$s_!gsak!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F61a1dee0-5808-4600-9b46-8d47e70613f2_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!gsak!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F61a1dee0-5808-4600-9b46-8d47e70613f2_1200x630.gif)

As time passed, they added features to scale the business.

Yet it increased the codebase complexity.

And became difficult to understand & maintain the code.

So they _considered_ moving to a microservices architecture.

Although it’ll temporarily solve their issues, it might create newer problems.

Here are some of them:

#### 1\. Infrastructure Costs

Each microservice needs a separate deployment pipeline.

Besides [service discovery](https://systemdesign.one/what-is-service-discovery/) and [API gateway](https://www.redhat.com/en/topics/api/what-does-an-api-gateway-do) should be set up.

So it’ll increase the effort and costs.

#### 2\. System Reliability

There’s a risk of failure from an [unreliable network](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing).

Also security vulnerabilities must be updated in each microservice.

So it’ll increase the system complexity.

Onward.

* * *

## Modular Monolith

They wanted the benefits of monolith and microservices architecture.

But ditch the problems coming with it.

So they set up a modular monolith.

Here’s how:

### 1\. Design Principles

They designed each module around a specific business domain.

  * It gave _high cohesion_.

Think of**cohesion** as grouping functionalities from a business area within a module. Put simply, related functionalities get stored in a specific module.

[![High Cohesion and Low Coupling in Modules](https://substackcdn.com/image/fetch/$s_!CJzj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7c038f8b-bbe0-4325-b335-8c6d60b92b8f_1200x630.png)](https://substackcdn.com/image/fetch/$s_!CJzj!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7c038f8b-bbe0-4325-b335-8c6d60b92b8f_1200x630.png)High Cohesion and Low Coupling of Modules

Also they created a _public interface_ for each module and hid the internal logic & data behind it. It let them:

  * Achieve _low coupling._

  * Understand the behavior of a module easily.

  * Modify a module without breaking others.

Imagine **coupling** as a measure of dependency between 2 modules.

A module will always depend on something.

Yet dependencies must be minimized.

So they merged modules with many dependencies on each other. (It happens when domain boundaries aren’t set properly.)

They designed modules to communicate with each other via public API - function calls. And threw errors if anything got accessed outside the public API.

[![Modularity vs Deployment Complexity in Architectural Patterns](https://substackcdn.com/image/fetch/$s_!YBT_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7ac1ba89-2064-4be8-bd88-2b58a3557b87_1200x630.png)](https://substackcdn.com/image/fetch/$s_!YBT_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7ac1ba89-2064-4be8-bd88-2b58a3557b87_1200x630.png)Modularity vs Deployment Complexity of Architectural Patterns

Each module can be deployed as a separate [process](https://en.wikipedia.org/wiki/Process_\(computing\)) for fault isolation.

Yet it’ll increase [data serialization](https://en.wikipedia.org/wiki/Serialization) costs and network overhead.

So they deploy every module within the same process.

And the best part?

### 2\. Code Organization

They organized code so each module contains a business domain. Put simply, each directory represents a specific business domain. For example, the code for orders and payments modules will be in separate directories.

[![The Directory Structure of a Modular Monolith](https://substackcdn.com/image/fetch/$s_!RMOr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff457adfd-9298-483a-8d64-1b07505d8c26_586x736.png)](https://substackcdn.com/image/fetch/$s_!RMOr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff457adfd-9298-483a-8d64-1b07505d8c26_586x736.png)The Directory Structure of a Modular Monolith

Also they set up separate database schemas for each module. And avoided transactions across them to avoid coupling & achieve data isolation.

### 3\. Benefits

A modular monolith is a better way to organize code than a classic monolith. Here are its benefits:

[![A Modular Monolith Limits the Area of Code Change](https://substackcdn.com/image/fetch/$s_!HLHw!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffe244d7f-e342-491d-acd5-1915727d3ef8_727x367.png)](https://substackcdn.com/image/fetch/$s_!HLHw!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffe244d7f-e342-491d-acd5-1915727d3ef8_727x367.png)A Modular Monolith Limits the Area of Code Change

  * It’s easier to modify code as the area of code change is limited. (Compared to [layered architecture](https://www.baeldung.com/cs/layered-architecture) in a classic monolith.)

  * Developer productivity is high because the codebase is easier to understand.

  * There’s no network overhead because functional calls are enough. (Compared to microservices.)

  * It’s easier to deploy because there’s only a single app.

  * The infrastructure costs are low because there’s only a single codebase.

Besides a modular monolith makes the transition to microservices easier.

### 4\. Tradeoffs

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fmodular-monolith&utm_source=paywall&utm_medium=web&utm_content=152631474)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fmodular-monolith&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture