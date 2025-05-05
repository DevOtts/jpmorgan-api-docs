---
title: "payment enhancements"
description: "Guide for payment processing and authorization"
category: "Payments"
---


# Payment enhancements

The Online Payments API offers additional services and features to cover all aspects of payments from simple to advanced.

## Availability

Availability of payment enhancement capabilities differs per region. See the following table to determine what capability is available where.

| Description | US | Australia | Canada | EU | UK | Notes |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 3-D Secure |  |  |  | Y | Y |  |  |
| Address verification service (AVS) | Y | Y | Y | Y | Y | This feature is unavailable for eftpos Australia |  |
| Card verification value (CVV) | Y | Y | Y | Y | Y |  |  |
| Consumer profile management (CPM) | Y |  | Y |  |  |  |  |
| Debt Repayment | Y |  | Y |  |  | Supported for Mastercard and Visa, prepaid and debit cards; limited to MCCs 6012 and 6051; set up during boarding; no API impact |  |
| Device fingerprint collection | Y |  |  |  |  |  |  |
| Direct Pay | Y |  |  |  |  |  |  |
| Dynamic statement descriptors | Y | Y | Y | Y | Y | This feature is unavailable for eftpos Australia |  |
| Encryption | Y | Y | Y |  |  |  |  |
| Fraud prevention | Y |  | Y |  |  |  |  |
| Incremental authorizations | Y |  | Y | Y | Y | Supported for Visa, Mastercard, Discover, and American Express |  |
| Partial authorizations | Y |  | Y |  |  |  |  |
| Purchase card (level 2) | Y |  | Y | Y | Y |  |  |
| Purchase card (level 3) | Y |  |  |  |  |  |  |
| Real-time account updater (RTAU) | Y |  | Y | Y | Y | Supported for Visa |  |
| Recurring payments | Y | Y | Y | Y | Y |  |  |
| Soft merchant data | Y |  | Y | Y | Y |  |  |
| Stored payments | Y | Y | Y | Y | Y |  |  |
| Tokenization (network) | Y | Y | Y |  |  | This feature is unavailable for eftpos Australia |  |
| Tokenization (Safetech) | Y | Y | Y | Y | Y | This feature is unavailable for eftpos Australia |  |

Payment enhancement availability by region


## How payment enhancements work

1. Determine which capabilities you need for the requirements of your solution.
2. Evaluate if the capability is supported for the region you require.
3. Work with your implementation manager to make sure capabilities are enabled in your client configuration.
4. Learn more about the specifics of each capability in this section.

## Related

[3-D Secure](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/3d-secure)

[Consumer profile management](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/profile-management)

[Direct Pay](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/direct-pay)

[Encryption](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/encryption)

[Fraud prevention](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/fraud-prevention)

[Partial authorizations](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/auth-and-capture-payment#partial-authorization-support)

[Real-time account updater](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/account-updater)

[Recurring payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/recurring-payments)

[Stored payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/stored-payments)

[Tokenization](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/tokenization)

Is this page helpful?
