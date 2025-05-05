---
title: "Core Concepts"
description: "Fundamental concepts for Online Payments API"
category: "Guide"
---

# Core concepts

The Online Payments API and Checkout API support multiple methods of payment and the operations needed to manage the entire payment lifecycle. Online Payments is designed for merchants, system integrators, and service providers in need of either a single payment API for local or global processing or a pre-built solution with multiple integration methods to suit your needs.

## API functionality

### Instant response

The APIs send acknowledgements instantly via synchronous response. The final status is sent via a webhook/asynchronous notification if you have enabled [notifications](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/notifications/index).

### Encryption and security

Online Payments and Checkout use [OAuth2.0](https://developer.payments.jpmorgan.com/api/commerce/online-payments/oauth-authentication) for industry-standard authentication.

### Throttling and TPS limits

The J.P. Morgan Developer Portal is built to scale with client requirements. It runs on a cloud infrastructure to support higher processing and bursts of traffic.

J.P. Morgan does not currently prescribe any transaction per second (TPS) limits or throttling. However, some clearing systems may prescribe specific TPS limits that the bank must adhere to depending on the market.

To help mitigate any issues arising from this, we run a dynamic throttling solution that sends payments to beneficiary banks at a more manageable volume, which results in higher success rates. We are highly experienced in working with high TPS clients in several markets and can enable you to handle bursts and high-volume peaks.

[Contact us](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/support) if you have queries on TPS limit management.

### Timeouts and errors

J.P. Morgan imposes an expiration time of 70 seconds for transaction processing.

If the payment request times out, no corresponding record is created to determine the payment status. When this happens, the payment status response returns as empty.

### Environments

Our client testing and production environments are available 24/7 with 99.99%+ uptime.

Is this page helpful?