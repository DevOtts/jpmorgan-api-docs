---
title: "Checkout"
description: "Pre-built solution for secure online payment acceptance"
category: "Payments"
---

# Checkout

Checkout is a pre-built solution that allows you to accept online payments securely on your website.

## Availability

Checkout is available for businesses and consumers in:

| Supported regions | Supported presentment currencies | Supported languages |
| --- | --- | --- |
| - United States (US)<br>- United Kingdom (UK)<br>- European Union (EU)<br>- Canada (CA) | AED, AFN, ALL, AMD, ANG, AOA, ARS, AUD, AWG, AZN, BAM, BBD, BDT, BGN, BIF, BMD, BND, BOB, BRL, BSD, BTN, BWP, BYN, BZD, CAD, CDF, CHF, CLP, CNY, COP, CRC, CVE, CZK, DJF, DKK, DOP, DZD, EGP, ETB, EUR, FJD, FKP, GBP, GEL, GHS, GIP, GMD, GTQ, GYD, HKD, HNL, HRK, HTG, HUF, IDR, ILS, INR, ISK, JMD, JPY, KES, KHR, KMF, KRW, KYD, KZT, LAK, LBP, LKR, LRD, LSL, MAD, MDL, MGA, MKD, MMK, MNT, MOP, MRU, MUR, MVR, MWK, MXN, MYR, MZN, NAD, NGN, NIO, NOK, NPR, NZD, PAB, PEN, PGK, PHP, PKR, PLN, PYG, QAR, RON, RSD, RWF, SAR, SBD, SCR, SEK, SGD, SHP, SLL, SOS, SRD, STN, SZL, THB, TJS, TOP, TRY, TTD, TWD, TZS, UAH, UGX, USD, UYU, UZS, VND, VUV, WST, XAF, XCD, XOF, XPF, YER, ZAR, ZMW | - US English<br>- US Spanish<br>- UK English<br>- French<br>- Canadian French<br>- German<br>- Spanish<br>- Portuguese<br>- Dutch |

Regions, currencies, and languages supported by Checkout


## Before you begin

You must first configure **Checkout Settings** in **Commerce Center** like branding, payment information, etc.

Perform the following steps to configure Checkout settings:

1. Log in to [Commerce Center](https://jpmorgan.com/commerce-center).
2. Go to **Settings**.
3. Click **Checkout** from the drop-down list.
4. Select configuration settings as per your business needs.
5. When you are done, click **Publish**.

Checkout offers the capability to configure multiple checkout instances for a merchant using **Checkout Segments**. **Segments** is aflexible, low-code feature that supports multiple fully customizable use cases.

You can find more information by accessing the user guides under **Support** in Commerce Center.

## How Checkout works

![Checkout process flow image](https://developer.payments.jpmorgan.com/api/download/en/docs/commerce/online-payments/capabilities/checkout/overview-cfs/Ovw_ProcFlow.png?type=image)

The following steps help you get started with Checkout:

1. **Request access** — Follow the steps in [Getting started](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/getting-started) to set up access to the Checkout API.

2. **Create a Checkout session** — Learn how to [create a checkout session](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/how-to/create-checkout-session).

3. Determine the type of integration method you require:


   - **[Drop-UI](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/drop-in-ui):** Uses a JavaScript form on your payment page to collect, encrypt, and transfer cardholder data securely to J.P. Morgan for payment processing.


     - **Set up the drop-in UI JavaScript library**— Learn how to [set up a drop-in UI](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/how-to/setup-drop-in).
   - **[Hosted Payment Page](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/hosted-payment):** Redirects consumers from your website to the Hosted Payment Page at the end of their shopping experience, where they can see their order details and shipping information along with their payment information.

   - **[Pay by Link](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/pay-by-link)**: Allows you to receive funds from your consumers using a payment link sent via email or text.


     - **Manage product catalog**— Learn how to [create a product](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/how-to/create-product).

     - **Manage payment links**— Learn how to [manage payment links](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/how-to/manage-payment-links).
4. **3D Secure authentication**— Learn about [3DS authentication](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/3ds-authentication) and how it works in Checkout.
5. **Fraud prevention**— Learn about [fraud prevention](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/fraud-prevention) and how fraud auto screening works.
6. **Level 2 and Level 3 data**— Learn about [level 2 and level 3 data](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/level2-and-level3) support to qualify for lower interchange fees.
7. **Soft merchant descriptors**— Learn about [soft merchant descriptors](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/soft-merchant-descriptors) and how to use them.
8. **Manage your notifications** — Learn about [Notifications](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/notifications) and how to [manage checkout notifications](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/how-to/manage-checkout-notifications).

Tip

Checkout provides Safetech and Network tokenization. For more information, refer to the [Tokenization API guide](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/tokenization/index).

## Benefits of Checkout

## PCI compliance made simple

Our team of engineers and designers continuously strive to design a frictionless and secure online payment experience, which helps to increase conversions and sales on your behalf.

## Protect your business

Improve authorization rates by reducing PCI scope and creating a secure authenticated web environment.

## Optimize and simplify your online payment experience

Provide an optimal experience across mobile, tablet, and desktop interfaces. You can even customize the colors, fonts, shapes, and brand settings to match the look and feel of your Checkout site.

## Protect your consumers

Protect yourself and your consumers by:

- Protecting consumers' payment data (both in-transit and stored).
- Securing authentication methods to help verify consumers.
- Managing all back-end requirements such as payment processing, fraud, and tokenization.

## Reduce friction during checkout

Making it easy for consumers to input their payment information and helping them spot errors in real time through:

- Real-time card validation
- Descriptive error messages
- Card brand identification

## Tokenize card and bank accounts

Get tokens for the cards or bank accounts without going through Zero Dollar Authorization (ZDA) thereby providing you with some cost-savings.

## Supported browsers

Checkout is supported by the following browsers:

- Chrome

- Firefox

- Safari

- Edge


## Related

[Create a checkout session](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/how-to/create-checkout-session)

[Drop-in UI](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/drop-in-ui)

[Set up a drop-in UI](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/how-to/setup-drop-in)

[Hosted Payments Page](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/hosted-payment)

[Pay by Link](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/pay-by-link)

[Create a product](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/how-to/create-product)

[Manage payment links](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/how-to/manage-payment-links)

[3DS authentication](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/3ds-authentication)

[Fraud prevention](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/fraud-prevention)

[Notifications](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/notifications)

[Manage checkout notifications](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/checkout/how-to/manage-checkout-notifications)

Is this page helpful?