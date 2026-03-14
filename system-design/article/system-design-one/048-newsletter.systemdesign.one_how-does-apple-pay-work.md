[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Apple Pay Handles 41 Million Transactions a Day Securely 💸

### #68: Break Into Apple Pay Architecture (5 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Mar 27, 2025

114

4

8

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how Apple Pay works. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-does-apple-pay-work/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

January 2023 - London, United Kingdom.

A student named Kenji is on a tourist visit.

Yet he spends so much time waiting in queue to buy tickets for the underground train.

So he was sad and upset.

[![How Does Apple Pay Work](https://substackcdn.com/image/fetch/$s_!PCkN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fee31094a-dd89-4f74-b83c-97dc3cb8c153_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!PCkN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fee31094a-dd89-4f74-b83c-97dc3cb8c153_1200x630.gif)

He hears about the tap & pay service called Apple Pay from the station officer.

Although doubtful about its safety, he decides to try it.

Onward.

* * *

#### [“Monolith to Microservices Migration — What to Expect (10 Challenges + Frameworks to Overcome Them)” Ebook by Cerbos - Sponsor](https://solutions.cerbos.dev/monolith-to-microservices-migration-ebook?utm_campaign=monolith_to_microservices_LP&utm_source=system_design&utm_medium=email&utm_content=&utm_term=)

[![Monolith to microservices migration](https://substackcdn.com/image/fetch/$s_!aHgF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4c514e7f-9859-48c9-a8fe-d010ee54d546_7680x4320.png)](https://solutions.cerbos.dev/monolith-to-microservices-migration-ebook?utm_campaign=monolith_to_microservices_LP&utm_source=system_design&utm_medium=email&utm_content=&utm_term=)

Transitioning to microservices is tough. It’s not just a technical shift, but an organizational one too.

From defining service boundaries to managing decentralized data and handling interservice communication, many things can go wrong.

This 80+ page ebook breaks down common migration challenges with examples from dev teams at Uber, Spotify & Netflix, helping you understand the obstacles before you hit them.

[Download FREE eBook](https://solutions.cerbos.dev/monolith-to-microservices-migration-ebook?utm_campaign=monolith_to_microservices_LP&utm_source=system_design&utm_medium=email&utm_content=&utm_term=)

* * *

## How Does Apple Pay Work

Here’s a simplified version of Apple Pay architecture:

### Chapter 1: A Credit Card, a Wallet, and an iPhone

Kenji enters his credit card details into the Apple Wallet.

Yet Apple doesn’t store it on the iPhone or Apple servers.

Instead they send credit card details and iPhone metadata to the payment network, such as Visa or MasterCard, in encrypted form.

[![Apple Sends Credit Card Details and iPhone Metadata to Payment Network](https://substackcdn.com/image/fetch/$s_!Db35!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1c96d2cf-3906-44f0-b181-d3fffe0bd011_968x225.png)](https://substackcdn.com/image/fetch/$s_!Db35!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1c96d2cf-3906-44f0-b181-d3fffe0bd011_968x225.png)Apple Sends Credit Card Details and iPhone Metadata to Payment Network

The iPhone metadata includes its model number and secure element ID. Imagine the **secure element ID** as a unique number assigned to each iPhone.

The payment network verifies the credit card details and generates a device account number (**[DAN](https://www.checkout.com/blog/what-is-a-device-account-number)**). Think of DAN as a unique random number representing the credit card number, but irreversible.

Also the _DAN is_ _unique and specific_ _to each credit card and iPhone._

[![Payment Network Sends DAN to iPhone](https://substackcdn.com/image/fetch/$s_!4dwe!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e0ebc10-d194-4213-adca-ec5c4e2b8ae6_1069x243.png)](https://substackcdn.com/image/fetch/$s_!4dwe!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e0ebc10-d194-4213-adca-ec5c4e2b8ae6_1069x243.png) Payment Network Sends DAN to iPhone

The payment network sends DAN to the iPhone in encrypted form. 

And the iPhone stores it inside the **[secure element](https://en.wikipedia.org/wiki/Secure_element)** , a highly specialized chip, so nobody can access it. Besides Apple servers don’t store DAN for security and privacy.

While the _payment network keeps a copy of the DAN, credit card details, and iPhone metadata_.

Let’s keep going!

### Chapter 2: A Single Tap to Open the Gate

Kenji has no internet connection due to roaming.

Yet he taps his iPhone on the card reader at the train station gate.

[![Apple Pay to Buy Train Tickets](https://substackcdn.com/image/fetch/$s_!4EWP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fabf49462-4269-4a53-8646-58a99a0f18cb_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!4EWP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fabf49462-4269-4a53-8646-58a99a0f18cb_1200x630.gif)

The iPhone communicates with the card reader via near-field communication (**[NFC](https://en.wikipedia.org/wiki/Near-field_communication)**). Consider NFC as a short-range, wireless communication standard.

The card reader creates a transaction record, which contains details such as the payment amount and date.

The card reader then sends these details to the iPhone via NFC.

[![Card Reader Sends Transaction Record to iPhone via NFC](https://substackcdn.com/image/fetch/$s_!Dha9!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6930f762-6aa7-4bc0-b1e3-16703d30b135_861x214.png)](https://substackcdn.com/image/fetch/$s_!Dha9!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6930f762-6aa7-4bc0-b1e3-16703d30b135_861x214.png)Card Reader Sends Transaction Record to iPhone via NFC

Yet Apple Pay must validate the transaction by checking Kenji’s involvement in it. 

So they ask for his biometric information, such as a touch ID or face ID. They don’t store the biometric information on Apple servers, but only on the secure enclave in the iPhone. 

Imagine the **[secure enclave](https://support.apple.com/en-gb/guide/security/sec59b0b31ff/web)** as a separate processor inside the iPhone, isolated from the rest of the system.

[![Secure Enclave Compares Biometric Information With Stored Data](https://substackcdn.com/image/fetch/$s_!jc1B!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd9111ae4-e719-4f75-bd9d-c3d059ffbef5_1010x214.png)](https://substackcdn.com/image/fetch/$s_!jc1B!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd9111ae4-e719-4f75-bd9d-c3d059ffbef5_1010x214.png)Secure Enclave Compares Biometric Information With Stored Data

The secure enclave fetches the encrypted biometric information when Kenji places his finger on the touch ID. 

It confirms his identity by comparing the data with the stored biometric information.

This approach ensures biometric information never leaves the iPhone.

Ready for the best part?

### Chapter 3: No Credit Card, No Cash, No Problem

The secure enclave confirms Kenji’s identity and signals the secure element.

Think of the **secure element** as a highly specialized chip in the iPhone. It combines DAN and transaction details to create a unique cryptogram. This is called the **request cryptogram**.

[![iPhone Creates the Request Cryptogram](https://substackcdn.com/image/fetch/$s_!5Z3o!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff050483d-375d-444a-8141-eb1026dd88eb_1110x214.png)](https://substackcdn.com/image/fetch/$s_!5Z3o!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff050483d-375d-444a-8141-eb1026dd88eb_1110x214.png)iPhone Creates the Request Cryptogram

Imagine the **[cryptogram](https://support.apple.com/en-gb/guide/security/secc1f57e189/web)** as a time-based, single-use password to prove a transaction’s legitimacy.

[![iPhone Sends Request Cryptogram and Transaction Details to Payment Network](https://substackcdn.com/image/fetch/$s_!bae1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feeaae80d-fb9b-4a82-9b93-e8472078a646_886x227.png)](https://substackcdn.com/image/fetch/$s_!bae1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feeaae80d-fb9b-4a82-9b93-e8472078a646_886x227.png)iPhone Sends Request Cryptogram and Transaction Details to Payment Network

The iPhone sends an authorization request to the payment network. It contains the request cryptogram and transaction details. Put simply, _DAN never leaves the iPhone for security._

[![The Payment Network Validates the Request Cryptogram](https://substackcdn.com/image/fetch/$s_!txbB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6efcea81-3c9b-4e7a-bc35-31fbeebd0dae_954x228.png)](https://substackcdn.com/image/fetch/$s_!txbB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6efcea81-3c9b-4e7a-bc35-31fbeebd0dae_954x228.png) The Payment Network Validates the Request Cryptogram

The payment network regenerates the request cryptogram by combining the transaction details and DAN copy. _A payment is valid only if the cryptogram values are equal._

### Chapter 4: The Problem With Contactless Payment

Yet the train station keeps Kenji's gate closed until his payment gets confirmed.

[![Payment Network Creates the Response Cryptogram](https://substackcdn.com/image/fetch/$s_!Om1H!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faea17925-8b03-487c-af32-55e7db2ca285_965x232.png)](https://substackcdn.com/image/fetch/$s_!Om1H!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faea17925-8b03-487c-af32-55e7db2ca285_965x232.png)Payment Network Creates the Response Cryptogram

So the payment network creates a response code, a number representing the payment status. Then combines it with the DAN and the request cryptogram to create a new cryptogram. This is called the **response cryptogram**.

[![Payment Network Sends Response Code on Successful Payment](https://substackcdn.com/image/fetch/$s_!upZ6!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F25e26106-2159-4c3b-928e-bc76d6711d4d_897x232.png)](https://substackcdn.com/image/fetch/$s_!upZ6!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F25e26106-2159-4c3b-928e-bc76d6711d4d_897x232.png)Payment Network Sends Response Code on Successful Payment

The payment network sends a response to the card reader. It includes only the response code and response cryptogram. Put simply, _it_ _doesn’t send DAN and request cryptogram._

[![iPhone Validates the Response Cryptogram](https://substackcdn.com/image/fetch/$s_!gEi3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc07a0086-281f-4d97-8166-70f1dda51a9a_954x218.png)](https://substackcdn.com/image/fetch/$s_!gEi3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc07a0086-281f-4d97-8166-70f1dda51a9a_954x218.png) iPhone Validates the Response Cryptogram

The iPhone regenerates the response cryptogram using the request cryptogram, DAN, and response code. 

The card reader considers a payment network authentic only if cryptogram values are equal. While the response code represents the payment status.

* * *

A credit card displays sensitive information, so there’s a risk of information misuse. 

But Apple Pay is safer as it doesn’t show credit card information to anyone.

Apple Pay relies on the [EMV](https://en.wikipedia.org/wiki/EMV) contactless payment specification as part of its security model. Think of **EMV** as a chip-based card payment authentication mechanism.

[![Apple Pay Architecture](https://substackcdn.com/image/fetch/$s_!tmaB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F73fcdf47-22e6-4f9f-ad69-b3e283dfcdbd_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!tmaB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F73fcdf47-22e6-4f9f-ad69-b3e283dfcdbd_1200x630.gif)

While Kenji visited the main tourist sights in London and happily returned home.

* * *

Subscribe to get simplified case studies delivered straight to your inbox:

Subscribe

* * *

[![Author Neo Kim; System design case studies](https://substackcdn.com/image/fetch/$s_!bEFk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8f94ab8c-0d67-4775-992e-05e09ab710db_320x320.png)](https://www.linkedin.com/in/nk-systemdesign-one/)**Find me on[LinkedIn](https://www.linkedin.com/in/nk-systemdesign-one/) | [YouTube](https://www.youtube.com/@systemdesignone?sub_confirmation=1) | [Twitter](https://x.com/intent/follow?screen_name=systemdesignone) | [Threads](https://www.threads.net/@systemdesignone) | [Instagram](https://www.instagram.com/systemdesignone/)**

* * *

**Want to advertise in this newsletter?** 📰

If your company wants to reach a tech audience, you may want to [advertise with me](https://newsletter.systemdesign.one/p/sponsorship).

* * *

Thank you for supporting this newsletter. Consider sharing this post with your friends and get rewards. Y’all are the best.

[![system design newsletter](https://substackcdn.com/image/fetch/$s_!6oWl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)](https://substackcdn.com/image/fetch/$s_!6oWl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e739087-a910-4643-be36-997b6dd5b4af_800x500.png)

[Share](https://newsletter.systemdesign.one/p/how-does-apple-pay-work?utm_source=substack&utm_medium=email&utm_content=share&action=share)

* * *

* * *

[![1 Simple Technique to Scale Microservices Architecture 🚀](https://substackcdn.com/image/fetch/$s_!MCWQ!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F84c96d29-cf27-47f0-9e1c-78ca1fa5f461_1280x720.gif)1 Simple Technique to Scale Microservices Architecture 🚀[Neo Kim](https://substack.com/profile/135589200-neo-kim)·February 4, 2025[Read full story](https://newsletter.systemdesign.one/p/how-to-scale-microservices)](https://newsletter.systemdesign.one/p/how-to-scale-microservices)

[![How Bluesky Works 🦋](https://substackcdn.com/image/fetch/$s_!GyYg!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F449c6406-d812-4547-bc2a-8e55c325ce3c_1280x720.gif)How Bluesky Works 🦋[Neo Kim](https://substack.com/profile/135589200-neo-kim)·January 22, 2025[Read full story](https://newsletter.systemdesign.one/p/how-does-bluesky-work)](https://newsletter.systemdesign.one/p/how-does-bluesky-work)

* * *

### References

  * [Apple Pay Official Site](https://www.apple.com/apple-pay/)

  * [How to use Apple Pay | Apple Support](https://www.youtube.com/watch?v=V7eh_xe5SXQ)

  * [Tap to Pay on iPhone security](https://support.apple.com/en-gb/guide/security/sec72cb155f4/1/web/1)

  * [What is a Device Account Number?](https://www.checkout.com/blog/what-is-a-device-account-number)

  * [Apple Platform Security - Secure Enclave](https://support.apple.com/en-gb/guide/security/sec59b0b31ff/web)

  * [EMV Payment Method](https://en.wikipedia.org/wiki/EMV)

  * [What are the EMV® Specifications?](https://www.emvco.com/knowledge-hub/what-are-the-emv-specifications/)

  * [EMV Cryptogram for Credit Card Processing](https://www.youtube.com/watch?v=h8TxLRN-SuM)

  * [The Secure Element Chip: How It Keeps Your Ledger Secure](https://www.ledger.com/academy/security/the-secure-element-whistanding-security-attacks)

  * [Why Tap-to-Pay Is Safer Than a Credit Card Swipe](https://www.youtube.com/watch?v=S7dWigI7Soc)

  * [Apple Pay Statistics You Should Know](https://jkcp.com/apple-pay-statistics-you-should-know/)

  * Block diagrams created with [Eraser](https://app.eraser.io/auth/sign-up?ref=neo)

114

4

8

Share

PreviousNext

#### Discussion about this post

CommentsRestacks

![User's avatar](https://substackcdn.com/image/fetch/$s_!TnFC!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack.com%2Fimg%2Favatars%2Fdefault-light.png)

[![Can Rau's avatar](https://substackcdn.com/image/fetch/$s_!_ZKo!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F4b4109e1-c79d-47d6-9989-5a697618a285_1704x1704.jpeg)](https://substack.com/profile/110574807-can-rau?utm_source=comment)

[Can Rau](https://substack.com/profile/110574807-can-rau?utm_source=substack-feed-item)

[Mar 27, 2025](https://newsletter.systemdesign.one/p/how-does-apple-pay-work/comment/103769382 "Mar 27, 2025, 12:51 PM")

Liked by Neo Kim

Great write-up and nice graphics 🤌

Just wanted to mention that if I understood your sentence correctly I think this "The iPhone sends an authorization request to the payment network." is not correct, as Apple Pay without any internet, just like the credit or debit card itself, so the iPhone itself doesn't have to communicate with the payment network, it's all done by the PoS.

ReplyShare

[1 reply by Neo Kim](https://newsletter.systemdesign.one/p/how-does-apple-pay-work/comment/103769382)

[![Saloni Khandelwal's avatar](https://substackcdn.com/image/fetch/$s_!5kYe!,w_32,h_32,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa37649be-0feb-4c8a-86fe-90e72fead0b2_144x144.png)](https://substack.com/profile/18525579-saloni-khandelwal?utm_source=comment)

[Saloni Khandelwal](https://substack.com/profile/18525579-saloni-khandelwal?utm_source=substack-feed-item)

[May 18, 2025](https://newsletter.systemdesign.one/p/how-does-apple-pay-work/comment/118049000 "May 18, 2025, 2:41 AM")

An article on Unified Payments Interface (UPI) would be amazing explaining how is it supporting 1.45 Billion people of India with no fee from the users.

ReplyShare

[2 more comments...](https://newsletter.systemdesign.one/p/how-does-apple-pay-work/comments)

TopLatestDiscussions

No posts

### Ready for more?

Subscribe

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture