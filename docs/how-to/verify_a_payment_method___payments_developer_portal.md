---
title: "Verify a Payment Method"
description: "Guide for payment processing and authorization"
category: "Payments"
---

# Verify a payment method

In this guide, you will learn how to verify a payment method prior to use without holding funds from the consumer's account.

## Before you begin

Payment verification is not available for all payment methods. Refer to the **Notes** column in the table of the [Payment methods overview](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-methods/index) page for additional information.

Determine what you need to verify:

- Payment account validation — does the card exist and is it active?
- Card address (AVS) validation — does the address provided match?
- Card CVV validation — does the CVV provided match?
- Card email validation (American Express only) — does the email address provided match?
- Card phone number validation (American Express only) — does the phone number provided match?

Tip

Verification for ACH is available in the US. More information about card verifications including regions and applicable cards can be found in the [cards](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-methods/cards) payment method page.

## Verify a card

Verifying a card validates a consumer's payment account and determines if the account number is in good standing.

The following example shows the required fields for a card verification request:

**HTTP method**: `POST`

**Endpoint**: `/verifications`

Json
Copy

```json
{
    "merchant": {
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        }
    },
    "currency": "USD",
    "paymentMethodType": {
        "card": {
            "accountNumber": "4012000033330026",
            "expiry": {
                "month": "5",
                "year": "2027"
            }
        }
    }
}
```

Response:

Json
Copy

```json
{
    "transactionId": "722bb5fd-7c27-442e-a9da-3f8a3c15b468",
    "requestId": "fc90352e-ebeb-4b19-8621-d439cf27cd07",
    "currency": "USD",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "hostMessage": "Transaction accepted",
    "paymentMethodType": {
        "card": {
            "cardTypeName": "VISA",
            "cardType": "VI",
            "maskedAccountNumber": "401200XXXXXX0026",
            "cardTypeIndicators": {
                "issuanceCountryCode": "USA",
                "isLevel3Eligible": false,
                "isDurbinRegulated": false
            },
            "networkResponse": {
                "addressVerificationResult": "NOT_REQUESTED",
                "networkTransactionId": "013228692165455"
            }
        }
    },
    "merchant": {
        "merchantId": "000017904371",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name",
            "version": "1.235"
        },
        "merchantCategoryCode": "4899"
    },
    "transactionDate": "2023-08-16T23:30:43.033Z",
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "NOT_STORED",
    "hostReferenceId": "r8dnofa5dUdnWWiwQuGYC",
    "approvalCode": "tst262"
}
```

An HTTP status of `200` indicates a successful response. The following fields indicate result:

- `responseStatus`
- `responseCode`
- `responseMessage`

Tip

Refer to [Online payments response codes](https://developer.payments.jpmorgan.com/api/commerce/online-payments/online-payments/error-codes) for more information on these fields.

### AVS details

To verify card AVS details, include the following fields in addition to the required fields above:

- `accountHolder.billingAddress.line1`
- `accountHolder.billingAddress.line2`
- `accountHolder.billingAddress.city`
- `accountHolder.billingAddress.state`
- `accountHolder.billingAddress.postalCode`
- `accountHolder.billingAddress.countryCode`

For AVS validation, the following fields indicate result:

- `addressVerificationResult`
- `addressVerificationResultCode`

### CVV details

To verify card CVV details, include the following fields in addition to the required fields above:

- `paymentMethodType.card.cvv`

For CVV validation, the following fields indicate result:

- `cardVerificationResult`
- `cardVerificationResultCode`

### Email details

To verify card **accountHolder** email details (American Express only), include the following fields in addition to the required fields above:

- `accountHolder.email  `

For card email validation, the following fields indicate result:

- `emailVerificationResult`
- `emailVerificationResultCode `

### Phone details

To verify card `accountHolder` phone details (American Express only), include the following fields in addition to the required fields above:

- `accountHolder.phone.countryCode`
- `accountHolder.phone.phoneNumber  `

For card phone number validation, the following fields indicate result:

- `phoneVerificationResult`
- `phoneVerificationResultCode  `

## Retrieve verification details

After making a verification request, you can review the verification details. Complete the following steps to retrieve verification details:

Send a `GET` request to the `/verifications` endpoint one of two ways:

1. Use the `requestId` returned in the original response as a query parameter in a new `GET` request.
2. Use the `transactionId` returned in the original response as a path parameter in a new `GET` request.

## Related

[Authorize and capture a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/auth-and-capture-payment)

[Update a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/update-a-payment)

[Refund a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/refund-payment)

[Online Payments response codes](https://developer.payments.jpmorgan.com/api/commerce/online-payments/online-payments/error-codes)

Is this page helpful?
