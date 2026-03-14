[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# Everything You Need to Know About Micro Frontends

### #21: Read Now - How 0.1% Companies Scale Development Teams (8 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Nov 14, 2023

97

10

7

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Micro Frontends work. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/micro-frontends/?action=share) & I'll send you some rewards for the referrals._

Frontend development is hard.

Scaling Frontend development on a large project with many teams is harder.

Image your business idea gets popular and you have many customers. You can split the backend into microservices for scalability.

But Frontend remains a complex monolith. So adding new features and technologies to Frontend becomes challenging.

[![Micro Frontends; Communication paths](https://substackcdn.com/image/fetch/$s_!xvo1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F85036dfe-7464-4b55-865a-e206bf86f61a_1495x1265.png)](https://substackcdn.com/image/fetch/$s_!xvo1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F85036dfe-7464-4b55-865a-e206bf86f61a_1495x1265.png)Communication Paths Between Teams Working on a Monolith Frontend

Also it becomes difficult to scale development teams. Because the teams need to communicate with each other to make changes to the Frontend monolith. And it reduces their development speed. 

So the development efficiency reduces as the number of teams grows due to many communication paths.

[![Micro Frontends; Delivery workflow](https://substackcdn.com/image/fetch/$s_!4gvQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1e4ae5e4-207e-4191-8825-39ff368f4707_3328x1052.png)](https://substackcdn.com/image/fetch/$s_!4gvQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1e4ae5e4-207e-4191-8825-39ff368f4707_3328x1052.png)Delivery Workflow: Monolith Frontend vs Micro Frontend

Besides it's likely that a new feature delivery needs a Frontend change. So the number of Frontend changes needed increases with many teams working on different backend microservices.

A simple solution to this problem is to split the teams to own both backend and Frontend services and distribute tasks fairly among them.

Also full rewrite of the Frontend is expensive. So it's important to support many tech stacks in Frontend from the beginning.

* * *

## [GreatFrontEnd (Featured)](https://www.greatfrontend.com/)

Find top-quality front-end system design resources here on this end-to-end front-end interview preparation platform by ex-FAANG senior engineers.

Note: There are no affiliate links in this post.

[![GreatFrontEnd System Design](https://substackcdn.com/image/fetch/$s_!Omfo!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F48bdb706-5944-494c-b328-41cd6ac10c46_2620x1186.png)](https://substackcdn.com/image/fetch/$s_!Omfo!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F48bdb706-5944-494c-b328-41cd6ac10c46_2620x1186.png)

[Try it](https://www.greatfrontend.com/prepare)

* * *

## What Is a Micro Frontend?

Micro Frontends are an extension of the microservices concept to the Frontend. It’s an architectural and organizational style.

Micro Frontend slices the website into self-contained, domain-driven micro apps. The micro apps then get built, tested, and deployed independently.

Micro Frontends are technology agnostic. So it can be implemented with different tools and frameworks like React, Vue, and Angular.

[![Micro Frontends vs Monolith Frontends](https://substackcdn.com/image/fetch/$s_!FCQb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62e4cf4d-7a16-4533-bd00-e9a1e3f80943_2463x1575.png)](https://substackcdn.com/image/fetch/$s_!FCQb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62e4cf4d-7a16-4533-bd00-e9a1e3f80943_2463x1575.png)Team Organization: Monolith Frontend vs Micro Frontend

Also development teams should own a feature end-to-end: backend to the Micro Frontend. Because it would make them autonomous and efficient. 

So teams should be formed around a business sub-domain instead of a specific technology.

_Domain-driven design_(**DDD**) is the key principle behind Micro Frontends. Put another way, each Micro Frontend represents a business sub-domain. 

So boundaries of Micro Frontends should be set based on the value it provides to the users and not to the developers.

Besides _bounded contexts_ in DDD __ make it harder for accidental coupling to occur. A bounded context is the boundary within a domain where a particular domain model applies.

[![Micro Frontend; Spotify Web UI](https://substackcdn.com/image/fetch/$s_!5ggj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0e3c6011-30cc-42dd-a448-23826eee6c59_2920x1506.png)](https://substackcdn.com/image/fetch/$s_!5ggj!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0e3c6011-30cc-42dd-a448-23826eee6c59_2920x1506.png)Micro Frontends in Spotify Web UI

The image shows how the Spotify Web UI looks with Micro Frontends.

* * *

## How Does Micro Frontend Work?

The container application is the parent app that combines every Micro Frontend. It’s built with minimal HTML, CSS, and JavaScript.

The container application does the following:

  * Render common page elements like headers and footers

  * Render Micro Frontends on demand

  * Cross-cutting concerns like authentication and navigation

Here is how cross-cutting concerns work in Micro Frontends:

### Micro Frontend Authentication

[![Micro Frontends; Authentication Workflow](https://substackcdn.com/image/fetch/$s_!oW4c!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fff21fa06-8386-42ae-bbf3-6ef267242249_2753x1404.png)](https://substackcdn.com/image/fetch/$s_!oW4c!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fff21fa06-8386-42ae-bbf3-6ef267242249_2753x1404.png)Micro Frontend Authentication Workflow

The container application interacts with the server to get an authentication token. The container application then injects the token into each Micro Frontend. The Micro Frontends sends the token with each request to the server.

### Micro Frontend CSS Isolation

There is no namespacing or encapsulation in CSS. So there is a risk that styles might get overridden with Micro Frontends.

Some ways to prevent this problem are by using:

  * BEM CSS naming convention

  * SASS pre-processor

  * CSS modules

  * CSS-in-JS libraries

### Micro Frontend Shared Components

[![Micro Frontends; Lifecycle](https://substackcdn.com/image/fetch/$s_!3kpx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff4e59109-2164-426d-891f-59099a2702f7_3058x2101.png)](https://substackcdn.com/image/fetch/$s_!3kpx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff4e59109-2164-426d-891f-59099a2702f7_3058x2101.png)Lifecycle of Shared Components in Micro Frontends

A shared library offers visual consistency and code reuse in Micro Frontends.

Here is a simple approach to the development of shared components in Micro Frontends:

  * Allow teams to create their own components and duplicate code in the beginning.

  * Extract the code into a library when patterns emerge.

Yet it is important to avoid sharing business logic to prevent coupling.

Also a single team should own the shared component for high quality and consistency. But it should be kept open to contributions from every team.

### Micro Frontend Testing

The test strategies are the same as with Monolithic Frontend. Each Micro Frontend needs automated tests. 

But functional tests to check the correct assembly of the webpage must be kept minimal. Otherwise it increases complexity.

* * *

## Micro Frontend Architecture

The _3 principles_ of Micro Frontend are:

  * Model around the business domain

  * Decentralization via autonomous teams

  * Automation culture for deployment

Each Micro Frontend gets deployed as an individual app. While DOM gets shared between Micro Frontends.

[![Micro Frontends; Architecture evolution](https://substackcdn.com/image/fetch/$s_!Bj0p!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2395b52a-5e02-48a5-a3c0-75ebff0ece47_3668x1848.png)](https://substackcdn.com/image/fetch/$s_!Bj0p!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2395b52a-5e02-48a5-a3c0-75ebff0ece47_3668x1848.png)Architecture Evolution

Micro Frontends are usually set up with the backend for frontend (**BFF**) pattern. 

BFF pattern creates tailored backends for each Micro Frontend. But there is no need for a dedicated backend if the Micro Frontend consumes only a simple API.

### Communication Between Micro Frontends

Micro Frontends shouldn't share their state but communicate via messages or events. Besides communication between Micro Frontends should be kept minimal to prevent coupling.

The ways Micro Frontends could communicate with each other are:

  * Custom events

  * Passing callbacks

  * Routing by using the address bar as a communication mechanism

  * Web workers

* * *

Consider subscribing to get the powerful system design template for FREE:

Subscribe

* * *

## How to Implement Micro Frontend?

[![Micro Frontends implementation](https://substackcdn.com/image/fetch/$s_!PkAA!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F64d68ec5-fb64-456d-86cf-b0b8280ed52a_1614x687.png)](https://substackcdn.com/image/fetch/$s_!PkAA!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F64d68ec5-fb64-456d-86cf-b0b8280ed52a_1614x687.png)Micro Frontend Implementation

A separate server can be set up to render Micro Frontends. And a container app server can be installed to interact with relevant Micro Frontend servers on demand.

Some common _anti-patterns_ in Micro Frontends are:

  * Not setting boundaries properly and creating many Micro Frontends

  * Increased complexity by using many frameworks and tools

  * Dependency hell in Micro Frontends

[![Micro Frontends deployment ](https://substackcdn.com/image/fetch/$s_!IT8Z!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f0f6ccc-3e1f-4049-925d-378e0cf20b3c_2380x840.png)](https://substackcdn.com/image/fetch/$s_!IT8Z!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f0f6ccc-3e1f-4049-925d-378e0cf20b3c_2380x840.png)Micro Frontend Deployment Pipeline

Also each Micro Frontend should get an independent deployment pipeline. Because it reduces the scope and the associated risk.

[![Micro Frontends; Code organization](https://substackcdn.com/image/fetch/$s_!ff4i!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff8792bd5-5e94-4ab1-a80d-a07556f97d32_1282x707.png)](https://substackcdn.com/image/fetch/$s_!ff4i!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff8792bd5-5e94-4ab1-a80d-a07556f97d32_1282x707.png)Micro Frontend Code Organization: Mono Repo vs Poly Repo

**There are 2 ways to organize Micro Frontend code** :

  * Monorepo

  * Polyrepo

A _Monorepo_ uses a single repository to share code and libraries. It’s easier to track and manage. But there is a risk of accidental coupling if boundaries are not set correctly.

While a _Polyrepo_ gives one repository per Micro Frontend. It keeps the code base independent. But it might become difficult to track and manage.

So there is no right or wrong approach but only the right way based on context.

**There are 2 types of Micro Frontends based on composition** : Build-time and Runtime.

### Micro Frontend Build Time Integration

Micro Frontend gets published as a package. And container application includes it as a dependency. 

But this approach creates coupling at the release stage. Put another way, a change in any Micro Frontend needs a release of every Micro Frontend. 

So it's better to avoid build-time integration.
    
    
    {
      "name": "@frontend/container",
      "version": "2.7.1",
      "description": "A micro frontends app",
      "dependencies": {
        "@frontend/micro-app-1": "^3.1.9",
        "@frontend/micro-app-2": "^8.2.1",
      }
    }

### Micro Frontend Runtime Integration

The composition occurs at runtime and the Micro Frontend can be updated on a browser refresh. 

The different ways to set up runtime integration are:

  * iframes

  * JavaScript

  * Web Components

#### iframes

Building Micro Frontends with iframes is the simplest approach. It offers a good degree of isolation for styles and global variables.

But it might become less flexible and difficult to integrate. Also routing, history, and deep linking might get complicated.
    
    
    <iframe id="micro-frontend"></iframe>

#### JavaScript

The Micro Frontend gets added using <script> tag. It then exposes a global function as its entry point. The container application calls the global function of the Micro Frontend to render it.

This is probably the best way to do Micro Frontends because it’s flexible.
    
    
    <script src="https://origin.server/micro-frontend.js"></script>

#### Web Components

The Micro Frontend is defined as a custom HTML element. The container application then instantiates it.

This is a valid approach if you like the web components specification and want to use the browser capabilities. 

But it may not be the right way if you want to define your own interface.

* * *

## Micro Frontend Framework

Micro Frontends is still in its early stages of adoption. Yet some production-ready Micro Frontend frameworks exist:

  * [Luigi](https://luigi-project.io/)

  * [Single SPA](https://single-spa.js.org/)

  * [Open Components](https://opencomponents.github.io/)

  * [Bit](https://bit.cloud/)

  * [Frint.js](https://frint.js.org/)

* * *

## Micro Frontend Advantages

The advantages of Micro Frontends are:

  * Fast delivery due to isolated deployments

  * Fast development cycle due to flexible tech stack and simple codebase

  * Improved maintainability and testability due to a small codebase

  * Scalable development due to autonomous teams

  * Low initial load time because Micro Frontends get loaded on demand

  * High reliability because modules are independent

  * A low entry barrier for new developers due to small codebase and bounded context

  * A decoupled system due to distributed architecture

  * Low coupling due to explicit data flow between different parts of the application

  * Easy rollback with a version switch

  * Faster product feedback via incremental upgrades on the parts of the app

  * Easy to update or rewrite parts of the Frontend

  * Easy to experiment with new technology in an isolated way

* * *

## Micro Frontend Disadvantages

The disadvantages of Micro Frontends are:

  * Increased complexity due to operational overhead

  * Extra work to maintain a consistent user experience

  * High pushback from stakeholders to have visual consistency

  * Potential distributed monolith problem due to changing needs

  * Increased load time when the user navigates across the app. Because relevant Micro Frontend needs to be loaded

  * High bandwidth usage due to duplicate dependencies in Micro Frontends

  * Fragmented ways of work due to increased team autonomy

  * Increased operational and governance complexity due to many repositories, tools, and deployment pipelines

* * *

## Takeaways

### When to Use Micro Frontends

Micro Frontends are a good fit for:

  * Medium to large projects

  * Web projects

  * Productivity first projects that are tolerant to overheads

  * Projects with technical and organizational maturity

### When Not to Use Micro Frontends

Micro Frontends are not a good fit if:

  * Small project

  * No bounded context is set to divide features among teams

  * Not enough automation is set up

  * Not comfortable with decentralized decisions around tooling and development practices 

Micro Frontends are not a silver bullet. But Micro Frontends does solve problems in certain contexts.

Also single-page applications and server-side rendering remain valid options for the Frontend.

The benefits of Micro Frontends could outweigh the costs if risks are managed properly. So it’s important to do your own research.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/micro-frontends?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Discord Boosts Performance With Code-Splitting](https://substackcdn.com/image/fetch/$s_!WoBE!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2586dc0c-2831-4bef-aa1a-b2e44326ad5a_1280x720.png)How Discord Boosts Performance With Code-Splitting[NK](https://substack.com/profile/135589200-nk)·November 8, 2023[Read full story](https://newsletter.systemdesign.one/p/what-is-code-splitting-in-react)](https://newsletter.systemdesign.one/p/what-is-code-splitting-in-react)

[![Microservices Lessons From Netflix](https://substackcdn.com/image/fetch/$s_!ysX3!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F18aa57f7-e167-45f9-bb6f-186efb77a57a_1280x720.png)Microservices Lessons From Netflix[NK](https://substack.com/profile/135589200-nk)·October 31, 2023[Read full story](https://newsletter.systemdesign.one/p/netflix-microservices)](https://newsletter.systemdesign.one/p/netflix-microservices)

* * *

## References

  * https://www.thoughtworks.com/radar/techniques/micro-frontends

  * https://martinfowler.com/articles/micro-frontends.html

  * https://www.infoq.com/presentations/evolution-micro-frontend/

  * https://www.altexsoft.com/blog/micro-frontend/

  * https://www.infoq.com/presentations/dazn-microfrontend/

  * https://www.aplyca.com/en/blog/micro-frontends-what-are-they-and-when-to-use-them

  * https://www.infoq.com/presentations/microfrontend-antipattern/

  * https://www.xenonstack.com/insights/micro-frontend-architecture

  * https://www.infoq.com/news/2018/08/experiences-micro-frontends/

  * https://aws.amazon.com/blogs/architecture/micro-frontend-architectures-on-aws/

97

10

7

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Basma Taha's avatar](https://substackcdn.com/image/fetch/$s_!Gs6J!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F64b2da40-0961-485e-8e00-5880b7a17702_1920x1920.jpeg)](https://substack.com/profile/170622182-basma-taha?utm_source=comment)

[Basma Taha](https://substack.com/profile/170622182-basma-taha?utm_source=substack-feed-item)

[Dec 21, 2023](https://newsletter.systemdesign.one/p/micro-frontends/comment/45831076 "Dec 21, 2023, 7:53 PM")

Liked by Neo Kim

Great break down of the topic.

I don't know much about frontend as I am a backend developer. But, learned a lot of new things from this!

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/micro-frontends/comment/45831076)

[![Vikas Kumar Yadav's avatar](https://substackcdn.com/image/fetch/$s_!tGp2!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9d2329a9-7c6d-4619-af3b-852579eaa135_144x144.png)](https://substack.com/profile/51115121-vikas-kumar-yadav?utm_source=comment)

[Vikas Kumar Yadav](https://substack.com/profile/51115121-vikas-kumar-yadav?utm_source=substack-feed-item)

[Nov 17, 2023](https://newsletter.systemdesign.one/p/micro-frontends/comment/43797689 "Nov 17, 2023, 3:12 AM")

Liked by Neo Kim

I knew about Micro frontend however reading this makes things more concrete. Coincidently the question of micro frontend has been asked to me 2 times in different interviews after I read this. Thanks so much for this well written article :)

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/micro-frontends/comment/43797689)

[8 more comments...](https://newsletter.systemdesign.one/p/micro-frontends/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture