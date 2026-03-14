[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Amazon S3 Achieves 99.999999999% Durability

### #38: Break Into Incredible Amazon Engineering (7 minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Mar 05, 2024

186

16

23

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Amazon S3 provides a mind-boggling level of durability. If you want to learn more, scroll to the bottom and find the references._

  * _[Share this post](https://newsletter.systemdesign.one/p/amazon-s3-durability/?action=share) & I'll send you some rewards for the referrals._

November 2010 - Paris, France.

A young software engineer named Julia works at a Pharmaceutical startup.

[![Amazon S3 Durability](https://substackcdn.com/image/fetch/$s_!i6gz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F810a82b2-fe2b-434c-9a8f-51c8e9f63fa0_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!i6gz!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F810a82b2-fe2b-434c-9a8f-51c8e9f63fa0_1200x630.gif)

They create many log files each day.

And durability is important to their business model.

Yet they stored the data in an in-house tiny server room.

Life was good.

And seasons passed.

But one day.

The hard disk of their main data server crashed.

And ruined it all.

Julia noticed that the log files of a main customer couldn't be found.

So she was Sad.

Luckily they had a backup server.

[![Amazon S3 Durability; Data Backup](https://substackcdn.com/image/fetch/$s_!mzZa!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb6d8617b-3115-41fb-8494-3eb6eeb45c5b_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!mzZa!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb6d8617b-3115-41fb-8494-3eb6eeb45c5b_1200x630.gif)

But she knew that they were looking for trouble with the current setup.

So she searches the internet for a cheap and _durable_ storage solution.

And reads about Amazon S3.

She was dazzled by its durability numbers.

Amazon Simple Storage Service (**S3**) is an object storage.

It stores unstructured data without hierarchy.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

## Amazon S3 Durability

Amazon S3 offers _11 nines_ of durability - 99.999999999%.

Put another way, a _single_ data object out of _10 thousand objects_ might get lost in _10 million years_.

**Durability** is about the prevention of data loss.

And here’s how Amazon S3 provides extreme durability:

### 1\. Data Redundancy

 _Mechanical hard disks_ provide large storage at small costs.

So they're still _popularly_ used in the cloud.

Yet they _fail_ often.

And it could happen due to a physical shock or electrical failure.

[![Hard Disk Failures at Scale](https://substackcdn.com/image/fetch/$s_!kwRB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F141737ff-4595-4177-b233-bc08f0385d79_1587x920.png)](https://substackcdn.com/image/fetch/$s_!kwRB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F141737ff-4595-4177-b233-bc08f0385d79_1587x920.png)Hard Disk Failures at Scale

A solution is to _replicate_ the data across many hard disks.

So data could be _recovered_ from another disk if a failure occurs.

Put another way, _replication_ improves _durability_.

But there's still a risk of data loss if _all the hard disks_ that store a _specific_ data object fail.

[![Splitting Data Into Chunks Using Erasure Coding](https://substackcdn.com/image/fetch/$s_!K5Lo!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9cb6f94f-f415-44fe-a772-232b2d089525_1932x1143.png)](https://substackcdn.com/image/fetch/$s_!K5Lo!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9cb6f94f-f415-44fe-a772-232b2d089525_1932x1143.png)Splitting Data Into Chunks Using Erasure Coding

So they use _erasure coding_ to _reduce_ the _probability_ of __ data loss.

It's a _replication_ technique.

**Erasure coding** splits a data object into chunks called _data_ _shards_. 

And creates extra chunks called _parity shards_.

While the original data object can be _recreated_ from a _subset_ of the shards.

[![Storing Shards Across Many Disks for Data Redundancy](https://substackcdn.com/image/fetch/$s_!wH4Z!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F12d6f281-d6e2-4cbf-9d6d-9a39ebc21a00_1542x581.png)](https://substackcdn.com/image/fetch/$s_!wH4Z!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F12d6f281-d6e2-4cbf-9d6d-9a39ebc21a00_1542x581.png)Storing Shards Across Many Disks for Data Redundancy

They store the erasure-coded _shards_ across _many_ hard disks. 

Thus _reducing_ the _probability_ of data loss.

Put another way, flexible replication at many levels improves durability.

In the above figure, shards are shown in different colors.

[![Re-Replicating Shards to a Failed Hard Disk at Scale](https://substackcdn.com/image/fetch/$s_!G5-C!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F395cfcf4-85dd-4acb-8877-031dfb78b71f_1542x564.png)](https://substackcdn.com/image/fetch/$s_!G5-C!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F395cfcf4-85dd-4acb-8877-031dfb78b71f_1542x564.png)Re-Replicating Shards of a Failed Hard Disk at Scale

They run _background processes_ to _monitor_ the _health_ of storage devices.

And quickly replace a failed device to re-replicate its data.

Besides they _don't_ keep empty storage devices.

Instead some _free space_ in __ each storage device.

Thus many storage devices could participate in recovery. 

And provide _high recovery_ _throughput_ through parallelization.

### 2\. Data Integrity

They _send data_ from a user through _many services_ before storing it in S3.

Yet there's a risk of _data corruption_ due to _bit flips_ in network devices.

**Bit flip** is an unwanted change of bits from 0 to 1, or vice versa.

And it could happen due to a _noise_ in the communication channel or _hardware failure_.

Also _TCP_ _doesn't_ _detect bit flips_ because it’s a higher-level protocol.

[![Client SDK Adding Checksums to Detect Data Corruption](https://substackcdn.com/image/fetch/$s_!uk1v!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8c0d89a2-d5f3-4892-82ea-cf7873da01ad_1067x359.png)](https://substackcdn.com/image/fetch/$s_!uk1v!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8c0d89a2-d5f3-4892-82ea-cf7873da01ad_1067x359.png)Client SDK Adding Checksums to Detect Data Corruption

So they add _checksums_ to data using Amazon client SDK.

And it helps them to _detect corrupted data_ on its arrival at S3.

Put another way, they do data integrity checks using checksums.

Imagine **checksum** to be a fingerprint of the data object. 

So 2 _different_ data objects _wouldn’t_ have the same checksum.

And they use checksum algorithms like CRC32C and SHA-1 for _performance_.

Besides they use the _HTTP trailer_ to send checksum.

Because it allows sending extra data at the end of chunked data.

Thus _avoiding_ the need to scan data twice and check data integrity at _scale_.

[![Adding Checksums to Erasure-Coded Data Before Storing It in S3](https://substackcdn.com/image/fetch/$s_!JnvB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8dc9d09e-dea0-420b-a8f3-a33ebcb9de80_2279x544.png)](https://substackcdn.com/image/fetch/$s_!JnvB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8dc9d09e-dea0-420b-a8f3-a33ebcb9de80_2279x544.png)Adding Checksums to Erasure-Coded Data Before Storing It in S3

They erasure code the data before storing it in S3.

And add _extra checksums to erasure-coded shards._

Because it allows data integrity checks during reads.

While S3 calculates around 4 billion checksums per second.

[![Reversing Data Transformations Using Bracketing](https://substackcdn.com/image/fetch/$s_!fgdg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F652ade25-25ec-44cc-9288-4e00ca47cf04_2319x475.png)](https://substackcdn.com/image/fetch/$s_!fgdg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F652ade25-25ec-44cc-9288-4e00ca47cf04_2319x475.png)Reversing Data Transformations Using Bracketing

Besides they do _bracketing_ _before_ _returning_ a _successful_ _response_ to the user.

**Bracketing** is the process of reversing the entire set of transformations on the data.

It ensures that a _data object_ can be _recreated_ from individual _shards_.

Put another way, it validates stored data against uploaded data.

### 3\. Data Audit

A failed hard disk should be replaced fast.

Also data must be _re-replicated_ for durability.

So they _match_ the _repair_ rate of hard disks with its _failure_ rate.

Yet the _failure_ rate _isn’t_ always _predictable_. Because it could be influenced by weather or power outages.

So they run a _separate service_ to _detect_ hard disk failures.

And _scale_ the _repair_ service accordingly.

[![Scaling Repair Service Based on Failures](https://substackcdn.com/image/fetch/$s_!_HaE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff30df264-3c18-4dd4-8fe7-8a146db3c790_1440x858.png)](https://substackcdn.com/image/fetch/$s_!_HaE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff30df264-3c18-4dd4-8fe7-8a146db3c790_1440x858.png)Scaling Repair Service Based on Failures

Also _bit flips_ occur on _hard disks_.

And could _corrupt_ a specific _data_ _sector_.

Put another way, the storage device may be functional but a specific data sector won’t be readable.

[![Periodic Durability Audit of Stored Data](https://substackcdn.com/image/fetch/$s_!GCD4!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff94a1aef-e92e-4b73-966a-37c86bf95da8_980x322.png)](https://substackcdn.com/image/fetch/$s_!GCD4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff94a1aef-e92e-4b73-966a-37c86bf95da8_980x322.png)Periodic Durability Audit of Stored Data

So they _periodically_ scan the storage device to check _data integrity_.

And use _checksums_ stored with erasure-coded shards to _audit_ the data.

Besides they _re-replicate_ the data if a bad sector is found.

### 4\. Data Isolation

They store _metadata_ and _file_ _content_ _separately_ in S3.

[![Amazon S3 Architecture](https://substackcdn.com/image/fetch/$s_!kdBi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd4a8d78a-96d5-4c78-a6a9-18e87b14935d_2006x884.png)](https://substackcdn.com/image/fetch/$s_!kdBi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd4a8d78a-96d5-4c78-a6a9-18e87b14935d_2006x884.png)Amazon S3 Architecture

The metadata gets stored in a database.

While file content gets stored as chunks in massive arrays of hard disks.

Also they run _physically separated data centers_ within an availability zone.

Put another way, an **availability zone** is a group of _isolated_ data centers.

[![Availability Zone Design](https://substackcdn.com/image/fetch/$s_!64hM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8299e37c-77ea-4ec4-8ab6-78f01ebe5a98_1346x447.png)](https://substackcdn.com/image/fetch/$s_!64hM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8299e37c-77ea-4ec4-8ab6-78f01ebe5a98_1346x447.png)Availability Zone Design

And each availability zone gets a _separate_ networking infrastructure and power supply.

Thus _reducing_ the _risk_ of simultaneous failures and _improving_ durability.

Yet there’s a risk of data loss due to _user mistakes_.

For example, a user could _accidentally delete or overwrite_ a data object.

So they design the software better to avoid such situations.

[![Amazon S3 Versioning](https://substackcdn.com/image/fetch/$s_!62eM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F46a727eb-0f25-4707-a4ef-f2a96510e3de_1307x637.png)](https://substackcdn.com/image/fetch/$s_!62eM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F46a727eb-0f25-4707-a4ef-f2a96510e3de_1307x637.png)Amazon S3 Versioning

They provide _versioning_ in S3 to _track_ data _object changes_.

And every change to an object will get a version ID.

Also a _delete marker_ is used to delete a version.

Because it _delays_ the actual deletion.

Thus avoiding accidental overwriting or deletion of a data object.

Besides they support _object lock_ in S3 to avoid user mistakes.

It prevents permanent object deletion during a specified period.

Also they offer _point-in-time backups_ for durability.

### 5\. Engineering Culture

They _deploy_ a change only after a _durability review_.

Because it forces them to think about possible cases and simplify design decisions.

Put another way, durability reviews help them _understand the risks_ in software design.

[![Durability Review](https://substackcdn.com/image/fetch/$s_!qVCM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8083cb1c-935f-4dc0-8623-b272413e4884_1043x1138.png)](https://substackcdn.com/image/fetch/$s_!qVCM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8083cb1c-935f-4dc0-8623-b272413e4884_1043x1138.png)Durability Review

They keep a _written document_ for durability reviews. 

Also a _threat model_ with a list of things that could go wrong.

Thus creating an _engineering culture_ that constantly thinks about durability.

Besides they use proven _mathematical models_ to calculate durability.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!4Qs3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)](https://substackcdn.com/image/fetch/$s_!4Qs3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3570f62-db57-47c1-ad42-95367ac706f4_800x60.png)

They made durability a part of the architecture and their engineering culture.

Besides they are extremely focused on durability _fundamentals_.

And consider durability as an _ongoing_ process.

* * *

Consider subscribing to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

#### NK’s Recommendations

  * [High Growth Engineer](https://careercutler.substack.com/): Actionable tips to grow faster in your software engineering career.

Author: [Jordan Cutler](https://open.substack.com/users/58854493-jordan-cutler?utm_source=mentions)

  * [The Hustling Engineer](https://thehustlingengineer.substack.com/): Get 2 min weekly tips to grow your software engineering career.

Author: [Hemant Pandey](https://open.substack.com/users/58770480-hemant-pandey?utm_source=mentions)

* * *

[![Author NK; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/amazon-s3-durability?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Zapier Automates Billions of Tasks](https://substackcdn.com/image/fetch/$s_!YaTI!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1fc4a18e-4392-483c-b3bd-945e6885e668_1280x720.gif)How Zapier Automates Billions of Tasks[Neo Kim](https://substack.com/profile/135589200-neo-kim)·February 25, 2024[Read full story](https://newsletter.systemdesign.one/p/zapier-architecture)](https://newsletter.systemdesign.one/p/zapier-architecture)

[![How Canva Supports Real-Time Collaboration for 135 Million Monthly Users](https://substackcdn.com/image/fetch/$s_!IALn!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F44d95ef4-5647-4ae5-b868-3647265167c0_1280x720.gif)How Canva Supports Real-Time Collaboration for 135 Million Monthly Users[Neo Kim](https://substack.com/profile/135589200-neo-kim)·February 18, 2024[Read full story](https://newsletter.systemdesign.one/p/rsocket)](https://newsletter.systemdesign.one/p/rsocket)

* * *

### References

  * [AWS re: Invent 2022 - Deep Dive on Amazon S3](https://www.youtube.com/watch?v=v3HfUNQ0JOE)

  * [Beyond eleven nines - How Amazon S3 is built for durability](https://www.youtube.com/watch?v=qJoATSh5CZY)

  * [Amazon S3 Fundamentals: Durability and availability](https://www.youtube.com/watch?v=soZJGPtdmJQ)

  * [Beyond 11 9s of durability: Data protection with Amazon S3](https://www.youtube.com/watch?v=YsIjt88E6iw)

  * [Data protection in Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/DataDurability.html)

  * [MDN - HTTP Trailer](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Trailer)

  * [What Causes Bit Flips In Computer Memory?](https://blog.robertelder.org/causes-of-bit-flips-in-computer-memory/)

  * [Understanding Erasure Coding](https://stonefly.com/blog/understanding-erasure-coding/)

186

16

23

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Jade Wilson's avatar](https://substackcdn.com/image/fetch/$s_!tGfL!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F98108c42-35f4-479c-b526-096d180d5288_625x625.jpeg)](https://substack.com/profile/199051090-jade-wilson?utm_source=comment)

[Jade Wilson](https://substack.com/profile/199051090-jade-wilson?utm_source=substack-feed-item)

[Mar 6, 2024](https://newsletter.systemdesign.one/p/amazon-s3-durability/comment/51031498 "Mar 6, 2024, 8:30 AM")Edited

Liked by Neo Kim

Great walkthrough. I really like the visuals and the way you structure you sentences to have breaks between them.

Just reading between the lines, the way they do data redundancy seems costly, I know they say it's a cheap solution but would be good if you have any insights on how much that costs to have such high durability this way?

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/amazon-s3-durability/comment/51031498)

[![Rushik's avatar](https://substackcdn.com/image/fetch/$s_!aV4-!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fd5c09d91-82cb-4f5f-8c1e-1291a1154307_144x144.png)](https://substack.com/profile/105858058-rushik?utm_source=comment)

[Rushik](https://substack.com/profile/105858058-rushik?utm_source=substack-feed-item)

[Mar 5, 2024](https://newsletter.systemdesign.one/p/amazon-s3-durability/comment/50989589 "Mar 5, 2024, 6:17 PM")

Liked by Neo Kim

"Besides they use the HTTP trailer to send checksum.

Because it allows sending extra data at the end of chunked data.

Thus avoiding the need to scan data twice and check data integrity at scale."

What do we mean by 'avoiding the need to scan data twice'? Why do need to scan data twice to calculate checksum? I don't quite get it. Can someone explain please?

ReplyShare

[4 replies by Neo Kim and others](https://newsletter.systemdesign.one/p/amazon-s3-durability/comment/50989589)

[14 more comments...](https://newsletter.systemdesign.one/p/amazon-s3-durability/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture