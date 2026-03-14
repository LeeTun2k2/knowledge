[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How to Scale Code Reviews 🔥

### #78: How Great Software Engineers Do Code Reviews (4 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jul 25, 2025

78

4

10

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines tips for reviewing code. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-to-do-code-review/?action=share) & I'll send you some rewards for the referrals._

Once upon a time, there was a 2-person startup.

Yet they had a tiny website and only a few customers.

So they merged the code directly into the main branch.

But one morning, their site became extremely popular.

And the number of customers and feature requests started to skyrocket.

[![How to Do Code Review](https://substackcdn.com/image/fetch/$s_!Pkys!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda3f9bd9-cc06-4c1d-9c19-ec717e419950_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!Pkys!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda3f9bd9-cc06-4c1d-9c19-ec717e419950_1200x630.gif)

So they hired more developers and implemented extra features.

Yet each developer wrote code with their personal preferences.

Thus worsening the code quality and standards. Also increasing the number of bugs.

So they started doing code reviews before merging new code into the codebase.

A **code review** means someone other than the author reviews the code.

Although it temporarily solved their code quality problem, there were newer issues.

Here are some of them:

#### 1\. Developer Velocity

They followed a strict review process and spent time on minor issues.

Put simply, they asked for low-priority changes on a tight deadline ticket.

It means delayed pull requests and slow developer velocity[1](https://newsletter.systemdesign.one/p/how-to-do-code-review#footnote-1-164575633).

Besides having teams across different time zones worsened the situation.

#### 2\. Personal Conflict

They didn’t have set guidelines for code reviews.

So developers forced their personal preferences during reviews.

It means unnecessary disagreements and demotivated developers.

Also feedback with a toxic and mean tone made things worse.

#### 3\. Miscommunication

There’s a risk of misunderstanding feedback in remote or asynchronous reviews.

Also a reviewer must understand new code and how it works with the codebase.

So the reviewer might get overloaded with code reviews and parallel development tasks.

Besides the reviewers might miss bugs even with thorough reviews, especially in large pull requests.

Onward.

* * *

I’m happy to partner with **[CodeRabbit](https://coderabbit.link/neo-kim)** on this newsletter. I’ve seen code reviews delaying feature deliveries by days and overloading reviewers. I genuinely believe CodeRabbit solves this problem.

[Try CodeRabbit](https://coderabbit.link/neo-kim)

* * *

## How to Do Code Review

Let’s dive in:

### 1\. Best Practices

Both the author and the reviewer should respect each other’s time and efforts.

Here are some **guidelines for the code author** :

  * Follow the project's style guide[2](https://newsletter.systemdesign.one/p/how-to-do-code-review#footnote-2-164575633).

  * Keep the changes small to make the reviews easy.

  * Review the code yourself before asking others to save time.

  * Use existing code patterns, and not personal preferences, if a style is unspecified.

  * Tag only the fewest number of reviewers to save everyone’s time.

  * Write a clear message about the changes for the reviewer to understand easily.

  * Use facts and data to resolve design debates, rather than opinions.

Code reviews are each software engineer's responsibility.

Here are some **guidelines for the reviewer** :

  * Respond to a review request within 24 hours.

  * Reserve at least one calendar slot each day for code reviews.

  * Keep the reviews polite and constructive; don’t criticize the author.

  * Document common review points and use a review checklist for consistent reviews.

  * Discuss the issue directly with the author when there’s a disagreement. Then document the solution for future reference.

  * Share the documentation in review comments if necessary to encourage knowledge sharing.

  * Approve the pull request when it’s good enough and allow minor issues to be fixed later.

Remember, code reviews are about making progress and not perfection. So just make sure each change maintains or improves the codebase's health.

Let’s keep going!

### 2\. Code Review Flow

Version control platforms, such as GitHub and GitLab, include pull request features for code review. They allow inline comments, approvals, and automated checks. Also there is peer review software, such as [Gerrit](https://www.gerritcodereview.com/), for advanced workflows.

[![Code Review Flow](https://substackcdn.com/image/fetch/$s_!kAzu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8ed4404e-6f60-44b3-8ddf-80e0d0822fcd_1869x699.png)](https://substackcdn.com/image/fetch/$s_!kAzu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8ed4404e-6f60-44b3-8ddf-80e0d0822fcd_1869x699.png)Code Review Flow

Here’s the code review workflow from a developer perspective:

  1. Pull Request

     * Write code on a separate Git branch to isolate changes.

[![Coderabbit Giving Instant Feedback](https://substackcdn.com/image/fetch/$s_!qzSH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feeef9a7c-ed97-40e9-b48b-18e8d1b9d98d_1600x814.png)](https://coderabbit.link/bM7fHiE)[CodeRabbit Giving Instant Feedback](https://coderabbit.link/bM7fHiE)

     * Get instant feedback and fix suggestions with code editor extensions, such as [CodeRabbit](https://coderabbit.link/bM7fHiE). Thus catching problems even before creating a pull request[3](https://newsletter.systemdesign.one/p/how-to-do-code-review#footnote-3-164575633) and saving infrastructure resources.

     * Commit code with a clear commit message and push it to the remote repository.

     * Open a pull request from the current branch to the target branch.

  2. Continuous Integration

     * Run automated checks on the code, such as unit tests, static code analysis[4](https://newsletter.systemdesign.one/p/how-to-do-code-review#footnote-4-164575633), security scans, and linting[5](https://newsletter.systemdesign.one/p/how-to-do-code-review#footnote-5-164575633), to ensure code correctness. It finds style issues, code smells, or security vulnerabilities, so reviewers can focus on high-level feedback.

[![](https://substackcdn.com/image/fetch/$s_!mIP1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F77f81483-2ed5-42bf-8869-426653024ccf_1985x1504.png)](https://github.com/ls1intum/Artemis/pull/8037#discussion_r1494109998)[CodeRabbit](https://coderabbit.link/neo-kim) [Finding Potential Issues](https://github.com/ls1intum/Artemis/pull/8037)

     * Find issues and get auto-fix suggestions using [CodeRabbit](https://coderabbit.link/neo-kim). This approach improves developer velocity.

  3. Code Review

     * Tag one or more relevant team members to ask for review.

[![CodeRabbit Summarizing Changes](https://substackcdn.com/image/fetch/$s_!XHUp!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc5fa05f6-99a4-4ec4-85fe-a2dea9ac04d5_1965x1625.png)](https://github.com/ls1intum/Artemis/pull/8037)[CodeRabbit](https://coderabbit.link/neo-kim) [Summarizing Changes](https://github.com/ls1intum/Artemis/pull/8037)

     * Generate a summary of code changes using [CodeRabbit](https://coderabbit.link/neo-kim). It helps reviewers understand complex changes quickly. And assess the impact on the codebase.

     * The reviewer checks the changed files and leaves constructive feedback via comments.

  4. Code Update

     * Fix the code based on the reviewer’s comments.

     * Upload changes to the same pull request and make sure automated checks pass again.

     * Reply to the reviewer's feedback and resolve comments.

  5. Deploy

     * At least one reviewer approves the pull request.

     * Merge the pull request into the target branch and trigger continuous deployment (**CD**).

     * CD pipeline builds and deploys the change to the staging environment.

     * Once the staging tests pass, the change gets released to production.

**Continuous integration** means merging code changes regularly into a central repository. While **Continuous deployment** is about automatically releasing changes that pass checks into production.

**[CodeRabbit](https://coderabbit.link/neo-kim)** is an AI code review tool to reduce developer workload.

Here’s how it helps with code review flow:

  * Do routine checks and reduce the time spent on reviews from days to minutes. Thus improving developer velocity.

  * Do consistent reviews without getting tired or biased. Thus reducing the risk of missing important issues from human mistakes.

  * Provide mentorship to junior engineers through detailed feedback.

Put simply, [CodeRabbit](https://coderabbit.link/neo-kim) complements human reviewers. It serves 1+ million repositories and has reviewed over 10 million pull requests. And it remains the most installed app on GitHub and GitLab. 

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Find me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a 160K+ tech audience, [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-to-do-code-review?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Reddit Works 🔥](https://substackcdn.com/image/fetch/$s_!NUAh!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5736f335-13e1-4f99-8a16-bcd184a43ddb_1280x720.png)How Reddit Works 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·July 3, 2025[Read full story](https://newsletter.systemdesign.one/p/reddit-architecture)](https://newsletter.systemdesign.one/p/reddit-architecture)

[![Forward Proxy vs Reverse Proxy ✨](https://substackcdn.com/image/fetch/$s_!GbPK!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdeb4aa7f-0108-4d2c-8120-d889c072ab39_1280x720.png)Forward Proxy vs Reverse Proxy ✨[Neo Kim](https://substack.com/profile/135589200-neo-kim)·July 11, 2025[Read full story](https://newsletter.systemdesign.one/p/forward-proxy-vs-reverse-proxy)](https://newsletter.systemdesign.one/p/forward-proxy-vs-reverse-proxy)

* * *

### References

  * [Cut Code Review Time With CodeRabbit](https://coderabbit.link/neo-kim)

  * [VS Code Extension by CodeRabbit](https://coderabbit.link/bM7fHiE)

  * [CodeRabbit revolutionizes code review with Claude](https://www.anthropic.com/customers/coderabbit)

  * [50% faster merge and 50% fewer bugs: How CodeRabbit built its AI code review agent with Google Cloud Run](https://cloud.google.com/blog/products/ai-machine-learning/how-coderabbit-built-its-ai-code-review-agent-with-google-cloud-run)

  * [The Standard of Code Review](https://google.github.io/eng-practices/review/reviewer/standard.html)

  * [The challenges of code reviews](https://about.gitlab.com/blog/challenges-of-code-reviews/)

  * [AI Code Review Tool — CodeRabbit Replaces Me, And I Like It](https://tomaszs2.medium.com/ai-code-review-tool-coderabbit-replaces-me-and-i-like-it-b1350a9cda58)

  * [Use pull requests for code review](https://support.atlassian.com/bitbucket-cloud/docs/use-pull-requests-for-code-review/)

  * [AI code review](https://www.ibm.com/think/insights/ai-code-review)

[1](https://newsletter.systemdesign.one/p/how-to-do-code-review#footnote-anchor-1-164575633)

Speed and efficiency of delivering high-quality software.

[2](https://newsletter.systemdesign.one/p/how-to-do-code-review#footnote-anchor-2-164575633)

[Rules](https://google.github.io/styleguide/) for writing clean, consistent, readable code.

[3](https://newsletter.systemdesign.one/p/how-to-do-code-review#footnote-anchor-3-164575633)

Proposal for merging code changes into the main branch.

[4](https://newsletter.systemdesign.one/p/how-to-do-code-review#footnote-anchor-4-164575633)

Checks code for errors, bugs, and style issues without running it.

[5](https://newsletter.systemdesign.one/p/how-to-do-code-review#footnote-anchor-5-164575633)

Checks code for style errors and formatting issues.

78

4

10

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Jul 25](https://newsletter.systemdesign.one/p/how-to-do-code-review/comment/138766578 "Jul 25, 2025, 12:00 PM")

Liked by Neo Kim

Solid breakdown, Neo.

I think everybody should approach code reviews as a tool for learning or teaching.

Loved the emphasis on small, clear changes and respectful feedback. 

One thing I’d add: don’t skip reviewing tests; well-written tests can catch issues early and explain intent better than comments.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-to-do-code-review/comment/138766578)

[![Tolulade Ademisoye's avatar](https://substackcdn.com/image/fetch/$s_!-4cc!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd971094f-00a0-4ed3-b249-9baf0385074c_960x960.jpeg)](https://substack.com/profile/9951966-tolulade-ademisoye?utm_source=comment)

[Tolulade Ademisoye](https://substack.com/profile/9951966-tolulade-ademisoye?utm_source=substack-feed-item)

[Jul 26](https://newsletter.systemdesign.one/p/how-to-do-code-review/comment/139049013 "Jul 26, 2025, 4:43 AM")

Liked by Neo Kim

Great read, thanks for sharing 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-to-do-code-review/comment/139049013)

[2 more comments...](https://newsletter.systemdesign.one/p/how-to-do-code-review/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture