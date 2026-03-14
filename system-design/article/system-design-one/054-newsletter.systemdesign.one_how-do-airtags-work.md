[![The System Design Newsletter](https://substackcdn.com/image/fetch/$s_!W5r-!,w_80,h_80,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1c8067a-95bb-416b-9114-e0b9fb8821d4_256x256.png)](/)

# [The System Design Newsletter](/)

SubscribeSign in

# How Do AirTags Work 

### #64: A Simple Introduction to AirTags Architecture (3 Minutes)

[![Neo Kim's avatar](https://substackcdn.com/image/fetch/$s_!OHOm!,w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc103940f-0d8b-47e7-9a33-013202e17bb8_389x389.jpeg)](https://substack.com/@systemdesignone)

[Neo Kim](https://substack.com/@systemdesignone)

Jan 08, 2025

∙ Paid

153

11

8

Share

Get my system design playbook for FREE on newsletter signup:

Subscribe

* * *

 _This post outlines how AirTag works. You will find references at the bottom of this page if you want to go deeper._

  * _[Share this post](https://newsletter.systemdesign.one/p/how-do-airtags-work/?action=share) & I'll send you some rewards for the referrals._

_Note: This post is based on my research and may differ from real-world implementation._

Once upon a time, there was a sports journalist named Maria.

She had to travel often by flight for work.

But one day she lost her luggage during a flight, and couldn’t track it. 

So she was sad and upset.

[![AirTags for tracking luggage](https://substackcdn.com/image/fetch/$s_!BmfT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5a607407-f2d8-4dae-9fe8-11bc1ce25839_1200x630.gif)](https://substackcdn.com/image/fetch/$s_!BmfT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5a607407-f2d8-4dae-9fe8-11bc1ce25839_1200x630.gif)

She hears about a tracking device, called [Apple AirTag,](https://www.apple.com/airtag/) from a coworker.

And bought it immediately, she was dazzled by its simplicity in tracking luggage.

* * *

An AirTag contains a low-power CPU and a tiny amount of memory.

[![How Do AirTags Work](https://substackcdn.com/image/fetch/$s_!Tzka!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F99914e40-d535-442b-85b3-4d110044aefd_794x1123.png)](https://substackcdn.com/image/fetch/$s_!Tzka!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F99914e40-d535-442b-85b3-4d110044aefd_794x1123.png)

A tracking device must send its accurate location data at regular intervals.

Yet finding location data using GPS, WiFI, or cellular networks consumes power. Besides it’s expensive to maintain a tracking device with GPS functionality.

So smart engineers at Apple used simple ideas to solve this hard problem.

Onward.

* * *

## How Do AirTags Work

An AirTag doesn’t use GPS, WiFi, or a cellular network for communication; instead, it uses Bluetooth Low Energy (**[BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy)**).

Imagine BLE as a low-power wireless technology for communication.

Here’s how AirTag works:

### 1\. Setting up AirTag

They generate a public-private key pair, using [elliptic curve cryptography](https://blog.cloudflare.com/a-relatively-easy-to-understand-primer-on-elliptic-curve-cryptography/), when a user adds an AirTag. 

The key pair is shared between AirTag and the user account. The AirTag sends location data after [encrypting](https://cloud.google.com/learn/what-is-encryption?hl=en) it using the public key. While the user account decrypts the received location data using the private key.

[![Sending Location Data Using End-To-End Encryption](https://substackcdn.com/image/fetch/$s_!lq1L!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb05385ba-38e8-40ef-b0e7-475d4493ca3d_926x462.png)](https://substackcdn.com/image/fetch/$s_!lq1L!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb05385ba-38e8-40ef-b0e7-475d4493ca3d_926x462.png)Sending Location Data With End-To-End Encryption

Think of the **public key** as an email address; anyone can send messages to it. While the **private key** is similar to the password of an inbox; only the user with the password can read emails.

And authenticity of an email can be verified by checking the sender’s email address: **[digital signature](https://cloud.google.com/kms/docs/digital-signatures#:~:text=Digital%20signatures%20rely%20on%20asymmetric,used%20to%20verify%20the%20signature.)**.

Ready for the best part?

### 2\. Broadcasting AirTag’s Location

They send the AirTag’s location, which is near its owner’s iPhone, using Bluetooth, or Ultra Wideband for precision and efficiency.

Think of **[Ultra Wideband](https://en.wikipedia.org/wiki/Ultra-wideband)** as a wireless technology for high-speed, and short-range communication.

[![Sending Location Data of an AirTag Using Bluetooth or Ultra Wideband When Owner’s iPhone Is Nearby](https://substackcdn.com/image/fetch/$s_!98G1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F90387961-b814-42de-93ab-62e76ff792dc_842x298.png)](https://substackcdn.com/image/fetch/$s_!98G1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F90387961-b814-42de-93ab-62e76ff792dc_842x298.png)Sending Location Data of an AirTag Using Bluetooth or Ultra Wideband When Owner’s iPhone Is Nearby

Yet Bluetooth and Ultra Wideband communication won’t work if the owner’s iPhone is far from the AirTag. So they rely on someone else's iPhone which is nearby.

[![AirTag Sending Location Data When Owner’s iPhone Isn’t Nearby](https://substackcdn.com/image/fetch/$s_!VP76!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F961069ee-174c-40d1-8a56-d5a35370c8e5_1522x311.png)](https://substackcdn.com/image/fetch/$s_!VP76!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F961069ee-174c-40d1-8a56-d5a35370c8e5_1522x311.png)AirTag Sending Location Data When Owner’s iPhone Is Far

Here’s how it works:

  * The AirTag _broadcasts its public key every 2 seconds over BLE_.

  * Someone else’s iPhone, which is nearby, receives the broadcast signal. 

  * The iPhone encrypts its location data and timestamp using the received public key.

  * The iPhone uploads the encrypted data to the Apple server over HTTP.

Put simply, AirTag doesn’t send location data; instead, only the public key. The iPhone then includes its location data and encrypts it.

### 3\. Finding AirTag’s Location

## This post is for paid subscribers

[Subscribe](https://newsletter.systemdesign.one/subscribe?simple=true&next=https%3A%2F%2Fnewsletter.systemdesign.one%2Fp%2Fhow-do-airtags-work&utm_source=paywall&utm_medium=web&utm_content=154268600)

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fhow-do-airtags-work&for_pub=systemdesignone&change_user=false)

PreviousNext

© 2026 Neo Kim · [Publisher Privacy](https://newsletter.systemdesign.one/privacy)

Substack · [Privacy](https://substack.com/privacy) ∙ [Terms](https://substack.com/tos) ∙ [Collection notice](https://substack.com/ccpa#personal-data-collected)

[ Start your Substack](https://substack.com/signup?utm_source=substack&utm_medium=web&utm_content=footer)[Get the app](https://substack.com/app/app-store-redirect?utm_campaign=app-marketing&utm_content=web-footer-button)

[Substack](https://substack.com) is the home for great culture