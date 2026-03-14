[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Google Search Works 🔥

### #57: Break Into Google Search Engine Architecture (6 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Sep 30, 2024

168

3

24

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

How can you quickly find the answer to a question on the internet?

Simply enter the question into a web search engine - Google.

  * _[Share this post](https://newsletter.systemdesign.one/p/search-engine-architecture/?action=share) & I'll send you some rewards for the referrals._

_This post outlines Google search architecture. You will find references at the bottom of this page if you want to go deeper._

_Note: This post is based on my research and may differ from real-world implementation._

It began as a university research project by 2 students. 

They wanted to build a web search engine - and got outstanding results.

Yet they didn’t have the management skills to start a business + wanted to focus on their studies.

[![Search Engine Architecture](https://substackcdn.com/image/fetch/$s_!ykFL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe0a673ce-40e2-4d73-8cf3-d9833c6f3b28_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!ykFL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe0a673ce-40e2-4d73-8cf3-d9833c6f3b28_1200x630.gif)

So they decided to sell their project for 1 million USD and approached Yahoo - _rejected_.

But it wasn’t the end - as they believed in the potential of their project.

And the best part?

They managed to get venture capital funding & named their tiny project Google.

Their growth rate was mind-boggling.

Although explosive growth is a good problem to have, there were newer problems.

A good search engine must be: (1) effective - search results must be relevant. (2) efficient - response time should be low.

* * *

### [This Post Summary - Instagram](https://www.instagram.com/systemdesignone/)

I wrote a summary of this post

(save it for later):

[systemdesignone](https://instagram.com/systemdesignone)

[![](https://substackcdn.com/image/fetch/$s_!mfNG!,w_640,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F__ss-rehost__IG-meta-DAignVht8KG.jpg)](https://instagram.com/p/DAignVht8KG)

A post shared by [@systemdesignone](https://instagram.com/systemdesignone)

And I’d love to connect if you’re on Instagram:

[Follow Instagram](https://www.instagram.com/systemdesignone/)

* * *

## Search Engine Architecture

They used simple ideas to build Google.

Here’s how:

### 1\. Crawling 🕷️

There are trillions of URLs on the internet - making it difficult to crawl every page.

Yet it's important to include all relevant web pages in search results. So they use a _distributed_ web crawler. It gives performance and scalability.

Imagine the **web crawler** as a software to automatically download text + media from a website.

They run web crawlers in data centers across the world. It lets them keep the crawler closer to the website, thus reducing bandwidth usage.

[![Links Connecting Pages on a Website](https://substackcdn.com/image/fetch/$s_!dS0n!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc452b7b7-0ddc-44c7-a645-534b2faf7eda_1320x593.png)](https://substackcdn.com/image/fetch/$s_!dS0n!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc452b7b7-0ddc-44c7-a645-534b2faf7eda_1320x593.png)Links Connecting Pages on a Website

Yet there’s no central database containing all web pages on the internet.

And it’s difficult to find new and updated web pages. So they: (1) follow links from known pages. (2) use the sitemap to find pages without any links pointing towards it.

Think of the **sitemap** as an [XML](https://blog.hubspot.com/website/what-is-xml-file) file with information about pages of a specific website.

[![How Google Crawler Works](https://substackcdn.com/image/fetch/$s_!OP4u!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe924860c-1d23-4aee-bca3-e58864869ff1_1637x346.png)](https://substackcdn.com/image/fetch/$s_!OP4u!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe924860c-1d23-4aee-bca3-e58864869ff1_1637x346.png)How Google Crawler Works

Here’s how Google crawler works:

  * It stores URLs to crawl in a queue called the **URLserver**.

  * It checks the [robots.txt file](https://developers.google.com/search/docs/crawling-indexing/robots/intro#:~:text=A%20robots.txt%20file%20tells,web%20page%20out%20of%20Google.) to understand if crawling is allowed on a specific URL.

  * It fetches the URL from the queue and makes an HTTP request (if allowed).

  * It keeps a local [DNS](https://www.cloudflare.com/en-gb/learning/dns/what-is-dns/) cache for performance.

  * It renders the page for finding new URLs on the page. (Similar to a web browser.)

  * It saves crawled pages in compressed form on a distributed storage server.

  * It doesn’t crawl pages behind a login. (Put simply, only public URLs are crawled.)

  * It checks the HTTP response code to make sure a website isn’t overloaded. (For example, HTTP response code [500](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) means slow down.)

It determines when & how often to crawl a website using an internal algorithm.

[![How Google Process a Web Page](https://substackcdn.com/image/fetch/$s_!jzQD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa16962f8-3d42-497b-9207-0e8391a0a049_1232x399.png)](https://substackcdn.com/image/fetch/$s_!jzQD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa16962f8-3d42-497b-9207-0e8391a0a049_1232x399.png)How Google Process a Web Page

They render web pages because most websites use JavaScript to show full content.

And the rendered page gets parsed for new URLs, which then gets added to the queue for crawling.

Ready for the next technique?

### 2\. Indexing 📖

The search results must be relevant.

Yet it's difficult to return the correct web page without understanding its content.

So they process crawled pages and store the found information in a database called the **index**.

[![Types of Index Powering Google Search](https://substackcdn.com/image/fetch/$s_!Vdax!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F811ee326-8eef-418c-8d72-4b3083bc1f80_1200x630.png)](https://substackcdn.com/image/fetch/$s_!Vdax!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F811ee326-8eef-418c-8d72-4b3083bc1f80_1200x630.png)Indexes Powering Google Search

Besides they keep 2 types of index: (1) forward index - list the words on a specific page and their position. (2) inverted index - maps a word to the list of documents containing it.

They sort the forward index to create the inverted index.

[![Inverted Index Types Based on Sorting](https://substackcdn.com/image/fetch/$s_!ICLX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc068fd84-36ca-4ff7-9a72-d9c52da704d6_1200x630.png)](https://substackcdn.com/image/fetch/$s_!ICLX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc068fd84-36ca-4ff7-9a72-d9c52da704d6_1200x630.png)Inverted Index Types Based on Sorting

Yet they receive many single-word queries. (For example, “YouTube”.)

And the same inverted index isn't enough to handle single-word and multi-word queries. (Merging the document list is difficult.)

So they keep 2 types of inverted indexes for performance. It's categorized based on the way documents are _sorted_.

  * The inverted index sorted by word rank gets queried first - it answers most queries.

While the inverted index sorted by document ID gets queried for multi-word queries.

[![Creating Index + Lexicon From Crawled Pages](https://substackcdn.com/image/fetch/$s_!6yHq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F41234070-ca22-4139-b38e-8e741e22191e_1199x514.png)](https://substackcdn.com/image/fetch/$s_!6yHq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F41234070-ca22-4139-b38e-8e741e22191e_1199x514.png)Creating Index + Lexicon From Crawled Pages

Here’s how the Google index works:

  * Each web page (**document**) is assigned a unique document ID.

  * The words found in documents get stored in **Lexicon**. It’s an in-memory storage, which is used to correct spelling mistakes in search queries.

  * The storage location of each document get recorded in the **document server**. It’s used to create page titles and snippets shown in search results.

  * The links found in the parsed document are stored in a separate database. It’s used to rank documents.

The document server and index server are _partitioned_ by document ID. (Also replicated for capacity.)

Onward.

### 3\. Searching 🔍

They receive 100+ million queries a day with spelling mistakes.

Yet it’s important to guess the query correctly to show relevant results.

So they clean the query via text transformation.

[![Cleaning the Query via Text Transformation](https://substackcdn.com/image/fetch/$s_!VHQ7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F15049f7c-ee08-41e3-ad2d-4576774cf84e_1200x630.png)](https://substackcdn.com/image/fetch/$s_!VHQ7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F15049f7c-ee08-41e3-ad2d-4576774cf84e_1200x630.png)Cleaning the Query via Text Transformation

And use [language models](https://en.wikipedia.org/wiki/Language_model) to find word synonyms + correct spelling mistakes.

[![Google Search Query Workflow](https://substackcdn.com/image/fetch/$s_!d12_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F70569812-d558-49a8-8582-c6a046fc419f_1749x269.png)](https://substackcdn.com/image/fetch/$s_!d12_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F70569812-d558-49a8-8582-c6a046fc419f_1749x269.png)Google Search Query Workflow

Here’s the Google search workflow:

  * Lexicon finds word IDs for words in the search query.

  * The cleaned query gets forwarded to the index.

  * The inverted index returns a sorted list of documents matching the query.

  * The top k results are shown to reduce response time.

They cache index results and document snippets for performance.

Besides a copy of the index is kept in each data center. And search queries get routed to the closest data center for low latency.

Also it’s important to find duplicate content and show only [canonical pages](https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls) in the search results.

So they: (1) group similar pages & show the pages based on the user’s language, location, and so on. (2) use HTML annotations to find the canonical page.

* * *

### TL;DR 🚀

[![How Google Search Works](https://substackcdn.com/image/fetch/$s_!FDN6!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4c22f90a-3ac9-407b-9c58-b669c5b8d141_2177x297.png)](https://substackcdn.com/image/fetch/$s_!FDN6!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4c22f90a-3ac9-407b-9c58-b669c5b8d141_2177x297.png)How Google Search Works

  * Crawling: find URLs on the internet.

  * Indexing: understand page content & make it searchable.

  * Searching: rank & show results.

* * *

The same Yahoo that rejected Google plummeted to ~1% in market share.

While Google became one of the most valuable companies in the world ~2 trillion USD.

And laid the foundation of the modern Internet.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/search-engine-architecture?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![Amazon Frugal Architecture Explained 💰](https://substackcdn.com/image/fetch/$s_!SP5j!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fec5ec1d6-4ac7-42ff-a29b-b4fe6c730056_1280x720.gif)Amazon Frugal Architecture Explained 💰[Neo Kim](https://substack.com/profile/135589200-neo-kim)·September 15, 2024[Read full story](https://newsletter.systemdesign.one/p/frugal-architecture)](https://newsletter.systemdesign.one/p/frugal-architecture)

[![How Amazon Lambda Works 🔥](https://substackcdn.com/image/fetch/$s_!UT0O!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F531b744f-385b-4ae9-a70d-3d2e79ffbfed_1280x720.gif)How Amazon Lambda Works 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·September 3, 2024[Read full story](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work)](https://newsletter.systemdesign.one/p/how-does-aws-lambda-work)

* * *

### References

  * [The Anatomy of a Large-Scale Hypertextual Web Search Engine](http://infolab.stanford.edu/~backrub/google.html)

  * [In-depth guide to how Google Search works](https://developers.google.com/search/docs/fundamentals/how-search-works)

  * [Building Software Systems At Google and Lessons Learned](https://www.youtube.com/watch?v=modXC5IWTJI)

  * [How Google Processes JavaScript](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics#how-googlebot-processes-javascript)

  * [Learn about sitemaps](https://developers.google.com/search/docs/crawling-indexing/sitemaps/overview)

  * [Overview of Google crawlers and fetchers (user agents)](https://developers.google.com/search/docs/crawling-indexing/overview-google-crawlers)

  * [How Google Search crawls pages](https://www.youtube.com/watch?v=JuK7NnfyEuc)

  * [How Google Search indexes pages](https://www.youtube.com/watch?v=pe-NSvBTg2o)

  * [How Google Search serves pages](https://www.youtube.com/watch?v=lgQazesEjO4)

  * [A Google documentary | Trillions of questions, no easy answers](https://www.youtube.com/watch?v=tFq6Q_muwG0)

  * [How Google Search organizes information](https://www.google.com/search/howsearchworks/how-search-works/organizing-information/)

  * [A Search Engine Architecture Based on Collection Selection](https://www.youtube.com/watch?v=KpZpsu2wM1s)

  * [Remember When Yahoo Turned Down $1 Million To Buy Google?](https://finance.yahoo.com/news/remember-yahoo-turned-down-1-132805083.html?guccounter=1)

  * [How Many Google Searches Are There Per Day? (August 2024)](https://explodingtopics.com/blog/google-searches-per-day)

  * [Google Stops Using MapReduce: This Does Not Impact Google Search](https://www.seroundtable.com/google-stops-mapreduce-28296.html)

  * [How Many Websites Are There in the World?](https://siteefy.com/how-many-websites-are-there/)

168

3

24

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![TOMEK's avatar](https://substackcdn.com/image/fetch/$s_!wZzd!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3cc6ac5a-b069-4fd2-b19a-d66e92b16328_512x512.png)](https://substack.com/profile/263501530-tomek?utm_source=comment)

[TOMEK](https://substack.com/profile/263501530-tomek?utm_source=substack-feed-item)

[Oct 8, 2024](https://newsletter.systemdesign.one/p/search-engine-architecture/comment/71857549 "Oct 8, 2024, 4:32 PM")

Liked by Neo Kim

The story of Google reminds us that a strong foundation in technology and user needs can lead to unprecedented growth.

ReplyShare

[![Raul Junco's avatar](https://substackcdn.com/image/fetch/$s_!ue6D!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco?utm_source=comment)

[Raul Junco](https://substack.com/profile/98661477-raul-junco?utm_source=substack-feed-item)

[Oct 1, 2024](https://newsletter.systemdesign.one/p/search-engine-architecture/comment/70919486 "Oct 1, 2024, 1:05 AM")

Liked by Neo Kim

I only can imagine the process to generate a unique document ID at this scale.

Great breakdown, Neo! 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/search-engine-architecture/comment/70919486)

[1 more comment...](https://newsletter.systemdesign.one/p/search-engine-architecture/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture