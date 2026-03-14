---
tags:
  - payment
  - system-design
  - security
  - uber
  - token
---
# Uber Payment System

## Key Idea

Uber never handles raw card data.  
Uber provider SDK to store access token from provider (use raw data to auth first, then store token)
Uber stores only the token and uses it to charge users.
## Problems

Can not keep raw payment data in user device => easy to stole
Can not store raw payment data in server => policy, reliable
#### 1\. Security

User add payment info on their mobile app.
Mobile device could be stolen.
=> Get risks to store in device.

#### 2\. Disbursement (giải ngân, chi trả)

A single payment has to be split across different trip participants:

  * Driver
  * Uber platform
  * Tax, fees, and so on

i.e., money cannot simply move directly from rider to driver.

#### 3\. Reliability

Payments include “external systems” such as banks, card networks, and payment providers.
But there’s a risk of network failures, timeouts, and partial outages.
And this could create a dreadful (kinh khủng, sợ hãi) user experience.

* * *

## Payment System Design

Here’s how Uber payment architecture works:

### 1\. Add Payment Information

A user adds payment details, such as credit card number & CVV, to the mobile app.

Yet it’s RISKY for them to store this data because:

  * It’s hard to secure

  * It requires heavy legal compliance

So they use payment provider SDK in the mobile app to collect sensitive details.

Software development kit (**SDK**) doesn’t send card information to Uber servers.

Instead, it “securely” collects credit card information and sends it directly to the payment provider, along with contextual metadata. Metadata includes Uber account details, payment method type (card or wallet), and so on.

[![](https://substackcdn.com/image/fetch/$s_!O34A!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feea384c6-253e-4245-b2e2-72f0b0fdf936_2471x392.png)](https://substackcdn.com/image/fetch/$s_!O34A!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feea384c6-253e-4245-b2e2-72f0b0fdf936_2471x392.png)

Payment provider then returns a unique token,,, which acts as a ‘safe reference’ for the credit card. The token represents the card; think of it as a (reusable) permission to charge the card.

Plus, the payment provider binds the received metadata to the token to scope its usage and validation.

From this point on, mobile app uses the token instead of the credit card.

i.e., Uber or mobile device has NO sensitive data.

IMPORTANT NOTE:

A stolen token is useless because it’s created by the payment provider, tied to Uber’s account and app, and restricted to specific uses (such as charges made only by Uber). So another mobile device or app cannot use it to charge the card.

## Related  
[[system-design-one]]