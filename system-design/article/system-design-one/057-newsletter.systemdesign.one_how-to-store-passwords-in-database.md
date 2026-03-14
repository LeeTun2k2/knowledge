[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Databases Keep Passwords Securely 🔒

### #61: Break Into Security Fundamentals (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Nov 18, 2024

302

9

18

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how the database stores passwords securely. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-to-store-passwords-in-database/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, there lived a junior software engineer.

He worked for a tech company named Hooli.

Although he was bright, he never got promoted.

So he was sad and frustrated.

[![How to Store Passwords in Database](https://substackcdn.com/image/fetch/$s_!oKlN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7db66c6-291a-4c0c-939c-a8c2ebe7581f_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!oKlN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7db66c6-291a-4c0c-939c-a8c2ebe7581f_1200x630.gif)

Until one day, he had the idea to apply for a new job.

And the interviewer asked him 1 simple question,

_“How to securely store passwords in the database?”._

Yet he failed to answer and the interview was over in 5 minutes.

So he studied it later to fill the knowledge gap.

Onward.

* * *

## How to Store Passwords in Database

He failed the interview so you don’t have to.

And here’s how you can answer it:

### 1\. Hashing

A hacker can retrieve the passwords easily if they’re stored in plaintext form.

So they transform the password using a hash function.

[![Hash Function Transforming a Password](https://substackcdn.com/image/fetch/$s_!GGit!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1c50a5d2-9afb-4287-bf01-bac1535dd5a5_1488x240.png)](https://substackcdn.com/image/fetch/$s_!GGit!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1c50a5d2-9afb-4287-bf01-bac1535dd5a5_1488x240.png)Hash Function Transforming a Password

A hash function creates a unique string value from a password - **fingerprint**. Also the transformation is one-sided. Put simply, it’s impossible to find the password from a fingerprint.

A popular choice for the hash function is [bcrypt](https://en.wikipedia.org/wiki/Bcrypt) because: (1) it’s slow. (2) it needs a ton of computing power. (3) it needs a lot of memory. Thus making it difficult to run many password-cracking attempts for the hacker.

[![Hashing Explained With an Analogy](https://substackcdn.com/image/fetch/$s_!BzSs!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5b645997-d827-4b0f-8675-f69cca8e2634_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!BzSs!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5b645997-d827-4b0f-8675-f69cca8e2634_1200x630.gif)Hashing Explained With an Analogy

Think of the **[hash function](https://en.wikipedia.org/wiki/Hash_function)** as mixing colors - it’s difficult to find the original colors from the new color.

[![Workflow for Hashing a Password](https://substackcdn.com/image/fetch/$s_!rH0m!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7b999ea2-b1dd-4dec-81c5-25a24f7b5c85_1219x232.png)](https://substackcdn.com/image/fetch/$s_!rH0m!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7b999ea2-b1dd-4dec-81c5-25a24f7b5c85_1219x232.png)Workflow for Hashing a Password

Here’s how it works:

  * The server generates a fingerprint from the given password when the user creates an account.

  * The password isn’t stored in the database, instead the fingerprint is.

  * The fingerprint is regenerated whenever the user enters the password.

The regenerated fingerprint gets compared against the value in the database. And the user is given access only if the values are equal.

Ready for the best part?

### 2\. Salting

A hacker might crack the password from the fingerprint using a rainbow table.

So they add salt to the password.

[![Rainbow Table](https://substackcdn.com/image/fetch/$s_!VAZX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F205ed361-0b64-4daa-afc7-f3c33e7cfa7d_1200x630.png)](https://substackcdn.com/image/fetch/$s_!VAZX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F205ed361-0b64-4daa-afc7-f3c33e7cfa7d_1200x630.png)Rainbow Table

Think of the **[rainbow table](https://en.wikipedia.org/wiki/Rainbow_table)** as a map between _pre-computed_ _fingerprints_ and passwords.

While **[salt](https://en.wikipedia.org/wiki/Salt_\(cryptography\))** is a random string.

[![Adding Salt to the Password to Create a Unique Fingerprint](https://substackcdn.com/image/fetch/$s_!yruf!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F946572a9-3a1c-4f99-afd0-28e38be4d346_1488x240.png)](https://substackcdn.com/image/fetch/$s_!yruf!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F946572a9-3a1c-4f99-afd0-28e38be4d346_1488x240.png)Adding Salt to the Password to Create a Unique Fingerprint

And each user gets a unique salt, thus generating different fingerprints. Put simply, 2 users with the same password will have different fingerprints.

Also the rainbow table wouldn’t work after salting because of unique fingerprints. It invalidates the pre-computed values in the rainbow table.

[![Workflow for Salting a Password](https://substackcdn.com/image/fetch/$s_!NxUn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd7066cdb-dc5d-4bbc-9ac6-80a117e1c5dd_1219x225.png)](https://substackcdn.com/image/fetch/$s_!NxUn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd7066cdb-dc5d-4bbc-9ac6-80a117e1c5dd_1219x225.png)Workflow for Salting a Password

Here’s what happens when the user creates an account:

  * The server creates a unique salt for the user.

  * The server combines the salt with the given password and hashes it.

They store the salt alongside the fingerprint in the database.

[![Workflow for Validating the Password](https://substackcdn.com/image/fetch/$s_!qXCO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe7a37203-0ddf-41dd-a02e-f2a20e6176f9_1219x284.png)](https://substackcdn.com/image/fetch/$s_!qXCO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe7a37203-0ddf-41dd-a02e-f2a20e6176f9_1219x284.png)Workflow for Validating the Password

Here’s what happens when the user enters a password:

  * The server retrieves the salt for the specific user from the database.

  * The server combines the entered password with salt to generate a fingerprint.

The server checks if the fingerprints are the same.

Ready for the next technique?

### 3\. Stretching

The hacker might do a brute force attack to crack the password.

Imagine **brute forcing** as trying out all number combinations on a number lock.

So they do stretching.

[![How Key Stretching Works](https://substackcdn.com/image/fetch/$s_!EQPc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F802a93a2-94c6-457a-87da-3a5383c8aa56_2744x310.png)](https://substackcdn.com/image/fetch/$s_!EQPc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F802a93a2-94c6-457a-87da-3a5383c8aa56_2744x310.png)How Key Stretching Works

Think of **[stretching](https://en.wikipedia.org/wiki/Key_stretching)** as applying the same hash function many times. Thus brute forcing becomes slower and more difficult.

* * *

[systemdesignone](https://instagram.com/systemdesignone)

[![](https://substackcdn.com/image/fetch/$s_!GtPs!,w_640,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F__ss-rehost__IG-meta-DCgycKXt_WX.jpg)](https://instagram.com/p/DCgycKXt_WX)

A post shared by [@systemdesignone](https://instagram.com/systemdesignone)

A randomly generated password is always more secure. (Also keep it over 12 characters.)

It’ll slow down the hacker and make it difficult to crack the password.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.threads.net/@systemdesignone)**Follow me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Threads](https://www.threads.net/@systemdesignone) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-to-store-passwords-in-database?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

[![How Google Ads Was Able to Support 4.77 Billion Users With a SQL Database 🔥](https://substackcdn.com/image/fetch/$s_!xO_f!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc65080a9-b897-48b0-b638-f3951ae85ebb_1280x720.gif)How Google Ads Was Able to Support 4.77 Billion Users With a SQL Database 🔥[Neo Kim](https://substack.com/profile/135589200-neo-kim)·November 9, 2024[Read full story](https://newsletter.systemdesign.one/p/cloud-spanner-database)](https://newsletter.systemdesign.one/p/cloud-spanner-database)

[![How Amazon S3 Works ✨](https://substackcdn.com/image/fetch/$s_!9_cP!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8ac2dd2-9f0b-4c6b-8c88-8ae6197b5647_1280x720.gif)How Amazon S3 Works ✨[Neo Kim](https://substack.com/profile/135589200-neo-kim)·October 25, 2024[Read full story](https://newsletter.systemdesign.one/p/s3-architecture)](https://newsletter.systemdesign.one/p/s3-architecture)

* * *

* * *

### References

  * [The definitive guide to form-based website authentication](https://stackoverflow.com/questions/549/the-definitive-guide-to-form-based-website-authentication)

  * [Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)

  * [Hash function](https://en.wikipedia.org/wiki/Hash_function)

  * [Salt (cryptography)](https://en.wikipedia.org/wiki/Salt_\(cryptography\))

  * [How to store salt?](https://security.stackexchange.com/questions/17421/how-to-store-salt)

  * [bcrypt](https://en.wikipedia.org/wiki/Bcrypt)

  * [Rainbow table](https://en.wikipedia.org/wiki/Rainbow_table)

  * [Key stretching](https://en.wikipedia.org/wiki/Key_stretching)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

302

9

18

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![sravan's avatar](https://substackcdn.com/image/fetch/$s_!Tfxb!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Forange.png)](https://substack.com/profile/3129292-sravan?utm_source=comment)

[sravan](https://substack.com/profile/3129292-sravan?utm_source=substack-feed-item)

[Nov 18, 2024](https://newsletter.systemdesign.one/p/how-to-store-passwords-in-database/comment/77758875 "Nov 18, 2024, 6:22 PM")

Liked by Neo Kim

How would you able to compare the passwords if you hash out them with the salt? hashing a one way 

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-to-store-passwords-in-database/comment/77758875)

[![Aram Tchekrekjian's avatar](https://substackcdn.com/image/fetch/$s_!7RTK!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fc18401a8-6369-4dc0-ba66-2d620225d54d_512x512.jpeg)](https://substack.com/profile/25135247-aram-tchekrekjian?utm_source=comment)

[Aram Tchekrekjian](https://substack.com/profile/25135247-aram-tchekrekjian?utm_source=substack-feed-item)

[Nov 25, 2024](https://newsletter.systemdesign.one/p/how-to-store-passwords-in-database/comment/78742295 "Nov 25, 2024, 8:29 AM")

Liked by Neo Kim

Amazing read Neo, easy and direct to the point. Nice visuals as well. Good job.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-to-store-passwords-in-database/comment/78742295)

[7 more comments...](https://newsletter.systemdesign.one/p/how-to-store-passwords-in-database/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture