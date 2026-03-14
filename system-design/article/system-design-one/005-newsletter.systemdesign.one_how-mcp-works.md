[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# MCP - A Deep Dive

### #110: Understanding Model Context Protocol

[![Eric Roby's avatar](https://substackcdn.com/image/fetch/$s_!eHwq!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd814203-f8c7-4690-a5ee-4e1bd2e950a5_1252x1252.png)](https://substack.com/@codingwithroby)[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Eric Roby](https://substack.com/@codingwithroby) and [Neo Kim](https://substack.com/@systemdesignone)

Dec 26, 2025

∙ Paid

275

40

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

  *  _[Share this post](https://newsletter.systemdesign.one/p/how-mcp-works/?action=share) & I'll send you some rewards for the referrals._

Every AI tool you use is starving for “context[1](https://newsletter.systemdesign.one/p/how-mcp-works#footnote-1-182336702)”.

Your AI needs your codebase. Your custom agents[2](https://newsletter.systemdesign.one/p/how-mcp-works#footnote-2-182336702) need database access. But getting that context to them? That is where things get messy.

When large language models[3](https://newsletter.systemdesign.one/p/how-mcp-works#footnote-3-182336702) (LLMs) emerged, each link between AI and data sources required a custom integration built from scratch.

Onward.

* * *

### [Cut Code Review Time & Bugs in Half (Sponsor)](https://coderabbit.link/neo-kim)

[![](https://substackcdn.com/image/fetch/$s_!dxaG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2f261d46-97c0-4298-880b-ae5268d15822_1600x800.png)](https://coderabbit.link/neo-kim)

Code reviews are critical but time-consuming.

**[CodeRabbit](https://coderabbit.link/neo-kim)** acts as your AI co-pilot, providing instant code review comments and potential impacts of every pull request.

Beyond just flagging issues, CodeRabbit provides one-click fix suggestions and lets you define custom code quality rules using AST Grep patterns, catching subtle issues that traditional static analysis tools might miss.

CodeRabbit has so far reviewed more than 10 million PRs, installed on 2 million repositories, and used by 100 thousand open-source projects.

CodeRabbit is free for all open-source repos.

[Get Started Today](https://coderabbit.link/neo-kim)

* * *

I want to introduce [Eric Roby](https://www.linkedin.com/in/codingwithroby/) as a guest author.

[![](https://substackcdn.com/image/fetch/$s_!T637!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62ac556a-3c71-48ad-ad80-794b441ba8b9_1600x801.png)](https://www.linkedin.com/in/codingwithroby/)

He’s a senior backend and AI engineer focused on building real-world systems and teaching developers how to do the same. He runs the YouTube channel [codingwithroby](https://www.youtube.com/@codingwithroby), where he focuses on scalable backend architecture and integrating AI into production applications.

Through his content and courses, he helps engineers go beyond tutorials, think in systems, and develop the skills that actually matter for senior-level roles.

Find him on:

  * [LinkedIn](https://www.linkedin.com/in/codingwithroby/)

  * [Substack](https://codingwithroby.substack.com/)

* * *

In November 2024, Anthropic launched the Model Context Protocol (MCP) to fix this problem. This wasn’t just another developer tool. It aimed to tackle a key infrastructure issue in AI engineering.

The challenge?

Every AI application needs to connect to data sources. Until now, they had to build custom integrations from scratch. This approach creates a fragmented ecosystem that does not scale.

For example, the world isn’t frozen in time, but LLMs are…

If you want an AI to “monitor server logs,” you can’t feed it a file. You would have to build a data pipeline that polls an API every few seconds. Filter out noise, then push only the relevant anomalies into the AI’s context window[4](https://newsletter.systemdesign.one/p/how-mcp-works#footnote-4-182336702). If the API changes its rate limits or response format, your entire agent breaks.

A better way to think of MCP is like a USB-C port.

**The Old Way (Before USB):**

If you bought a mouse, it had a PS/2 plug. A printer had a massive parallel port. A camera had a proprietary cable. If you wanted to connect these to a computer, the computer needed a specific physical port for each one.

[![](https://substackcdn.com/image/fetch/$s_!-9tm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3d3f117-ae5d-4bda-8f05-d7c5d9913c5c_1200x630.png)](https://substackcdn.com/image/fetch/$s_!-9tm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3d3f117-ae5d-4bda-8f05-d7c5d9913c5c_1200x630.png)

> This is the “N×M” problem:
> 
> Each device maker had to figure out how to connect to each computer. So, computer makers had to create ports for every device.

**The MCP Way (With USB-C):**

Now, everything uses a standard port.

  * Computer (AI Model[5](https://newsletter.systemdesign.one/p/how-mcp-works#footnote-5-182336702)) needs one USB-C port. It doesn’t need to know if you are plugging in a hard drive or a microphone; it just speaks “USB.”

  * Device (Data Source) requires a USB-C port. It doesn’t need to know if it’s plugged into a Mac, a PC, or an iPad.

[![](https://substackcdn.com/image/fetch/$s_!ZbTU!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7d3b9ca3-1d3f-4587-90bb-5079031caf69_1200x630.png)](https://substackcdn.com/image/fetch/$s_!ZbTU!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7d3b9ca3-1d3f-4587-90bb-5079031caf69_1200x630.png)

> The result: you can plug anything into anything instantly.

The analogy makes sense in theory. But to understand why MCP matters, you need to see how painful the current reality actually is…

* * *

## **The Integration Complexity Problem**

Every AI assistant needs context to be useful:

Claude[6](https://newsletter.systemdesign.one/p/how-mcp-works#footnote-6-182336702) needs access to your codebase. ChatGPT may require your Google Drive files. Custom agents need database connections. But how do these AI systems actually get that context?

Traditionally, each connection requires a custom integration.

If you’re building an AI coding assistant, you need to:

  1. Write code to connect to the GitHub API.

  2. Add authentication and security.

  3. Create a way to change GitHub’s data format into a version that your AI can understand.

  4. Handle rate limiting, errors, and edge cases.

  5. Repeat this entire process for GitLab, Bitbucket, and every other source control system.

This creates the N×M problem:

> ‘N’ AI assistants multiplied by ‘M’ data sources equals N×M unique integrations that need to be built and maintained.

When Cursor wants to add Notion support, they build it from scratch. When GitHub Copilot wants the same thing, they build it again. The engineering effort gets duplicated across every AI platform.

The traditional approaches have fundamental limitations:

**Static, build-time integration** :

Integrations are hard-coded into applications. You can’t add a new data source without updating the application itself. This makes rapid experimentation impossible and forces users to wait for official support.

**Application-specific security implementations** :

Every integration reinvents authentication, authorization, and data protection. This leads to inconsistent security models and increases the attack surface.

**No standard way to discover** :

AI systems can’t find out what capabilities a data source has. Everything needs clear programming. This makes it hard to create agents that adapt. They can’t use the new tools on their own.

The result is a ‘broken’ ecosystem!

Innovation is stuck because of integration work. Users can access only the connections that application developers create. So that’s a mess.

Now let’s look at how MCP cleans it up…

* * *

## **How MCP Solves It**

MCP’s solution is straightforward: _define a single protocol that functions across all systems._

Instead of building N×M integrations, you build N clients (one per AI application) and M servers (one per data source). The total integration work drops from N×M to N+M.

[![](https://substackcdn.com/image/fetch/$s_!oaRu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7905437a-565b-4195-a9d0-6e25ea4a573b_1282x782.png)](https://substackcdn.com/image/fetch/$s_!oaRu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7905437a-565b-4195-a9d0-6e25ea4a573b_1282x782.png)

When a new AI assistant[7](https://newsletter.systemdesign.one/p/how-mcp-works#footnote-7-182336702) wants to support all existing data sources, it just needs to implement the MCP client protocol once.

When a new data source wants to be available to all AI assistants, it just needs to implement the MCP server protocol once.

[![](https://substackcdn.com/image/fetch/$s_!68S0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1c753bb-86da-49e5-83ff-4bd21e9bc386_1600x878.png)](https://substackcdn.com/image/fetch/$s_!68S0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1c753bb-86da-49e5-83ff-4bd21e9bc386_1600x878.png)

Two critical architectural features power this efficiency:

**1\. Dynamic Capability Discovery**

In traditional integrations, the AI application[8](https://newsletter.systemdesign.one/p/how-mcp-works#footnote-8-182336702) must know the data source details in advance.

If the API changes, the application breaks…

MCP flips this. It uses a “handshake” model. When an AI connects to an MCP server, it asks, _“What can you do?”_.

The server returns a list of available resources and tools in real time.

> RESULT:
> 
> You can add a new tool to your database server, like a “Refund User” function. The AI agent finds it right away the next time it connects. You won’t need to change any code in the AI application.

**2\. Decoupling Intelligence from Data**

MCP separates the thinking system (AI model) from the knowing system (the data source).

  * **For Data Teams:** They can build robust, secure MCP servers for their internal APIs without worrying about which AI model will use them.

  * **For AI Teams:** They can swap models without having to rebuild their data integrations.

This decoupling means your infrastructure doesn’t become obsolete whenever a new AI model gets released.

You build your data layer once, and it works with whatever intelligence layer you choose to plug into it.

An AI agent using MCP doesn’t need to know in advance what tools are available. It simply connects, negotiates capabilities, and uses them as needed. This enables a level of flexibility and scale that is impossible with traditional static integrations.

The high-level concept is straightforward.

But the real elegance is in how the architecture actually works under the hood…

* * *

## **MCP Architecture Deep Dive**

MCP’s architecture has three main layers.

These layers separate concerns and allow for easy scalability:

### **The Three-Layer Model**

**Hosts** are user-facing apps.

This includes the Claude Desktop app, IDEs like VSCode, or custom AI agents you create. Hosts are where users connect with the AI. They are also where requests begin. They do more than display the UI; they orchestrate the entire user experience.

The host application interprets the user’s prompt.

It decides whether external data or tools are needed to fulfill the request. If access is necessary, the host creates and manages several internal clients. It keeps one client for each MCP server it connects to.

The protocol layer handles the mechanics of data access.

[![](https://substackcdn.com/image/fetch/$s_!uVP4!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F55ff0034-3085-49e0-94f7-a6c9f6053079_1600x874.png)](https://substackcdn.com/image/fetch/$s_!uVP4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F55ff0034-3085-49e0-94f7-a6c9f6053079_1600x874.png)

**Clients** are protocol-speaking connection managers that run on hosts.

Each client maintains a dedicated 1:1 connection with a single MCP server. They serve as the translation layer.

They convert abstract AI requests from the host into clear MCP messages. These messages, like tools/call or resources/read, can be understood by the server. Clients do more than send messages. They manage the entire session lifecycle. This includes handling connection drops, reconnections, and state.

When a connection starts, the client takes charge of capability negotiation. It asks the server which tools, resources, and prompts it supports.

[![](https://substackcdn.com/image/fetch/$s_!3xua!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6631f37b-c4f7-4264-8db5-e80359018de6_1320x718.png)](https://substackcdn.com/image/fetch/$s_!3xua!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6631f37b-c4f7-4264-8db5-e80359018de6_1320x718.png)

**Servers** provide context.

They act as the layer that connects real-world systems, such as PostgreSQL databases, GitHub repositories, or Slack workspaces.

A server connects the MCP protocol to the data source. It translates MCP requests into the system’s native operations. For example, it turns an MCP read request into a SQL `SELECT` query. They are very flexible. They can run on a user’s machine for private access or remotely as a cloud service.

Servers share their available capabilities when connected. They inform the client about the Resources, Prompts, and Tools they offer.

[![](https://substackcdn.com/image/fetch/$s_!BR49!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7a282e1e-16b7-4166-abce-a65712862097_1320x718.png)](https://substackcdn.com/image/fetch/$s_!BR49!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7a282e1e-16b7-4166-abce-a65712862097_1320x718.png)

### **Core Primitives**

MCP defines three fundamental primitives that servers can expose:

**1 Resources**

They’re context-controlled by applications. They provide data that the AI can read but not change.

Resources might be:

  * Database query results.

  * Files from a file system.

  * API responses from a web service.

  * Documentation or knowledge base articles.

**2 Prompts**

They’re reusable instruction templates with variables. Think of them as parametrised prompts that can be shared across applications.

Users and applications can invoke prompts with different values for code and focus areas. Prompts help standardize common AI workflows.

Plus, they make it easy to share effective prompting techniques.

**3 Tools**

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fhow-mcp-works&utm_source=paywall&utm_medium=web&utm_content=182336702)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fhow-mcp-works&for_pub=systemdesignone&change_user=false)

PreviousNext

[![Eric Roby's avatar](https://substackcdn.com/image/fetch/$s_!eHwq!,w_52,h_52,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd814203-f8c7-4690-a5ee-4e1bd2e950a5_1252x1252.png)](https://substack.com/@codingwithroby?utm_source=byline)| A guest post by| [Eric Roby](https://substack.com/@codingwithroby?utm_campaign=guest_post_bio&utm_medium=web)Senior Backend & AI Engineer passionate about teaching others.| [Subscribe to Eric](https://codingwithroby.substack.com/subscribe?)  
---|---  
  
© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture