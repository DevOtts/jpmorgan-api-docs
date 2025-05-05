---
title: "update a payment"
description: "Guide for payment processing and authorization"
category: "Payments"
---


# Update a payment

Learn how to update an existing payment for the following use cases:

- Incremental authorization
- Partial reversal authorization
- Void an authorization

## Before you begin

Review the following prerequisites to update a payment:

1. [Authorize the payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/auth-and-capture-payment) with manual or automatic delayed capture.

2. Store the `transactionId` from the approved authorization response.

## Incremental authorization

Use an incremental authorization to increase the authorized amount when appropriate. In your `PATCH` request, send the full authorization amount, which is the total of the original authorization amount and the additional amount needed.

Include the following important fields for an incremental authorization:

- Send the `transactionId` from the approved authorization response in the `id` path parameter.
- Specify the appropriate amount in the `amount` field.

The following example shows the minimum required fields for an incremental authorization request:

**HTTP method**: `PATCH`

**Endpoint**: `/payments/{id}`

Json
Copy

```json
{
    "amount": 15000
}
```

Response:

Json
Copy

```json
{
    "transactionId": "bd359c65-f17b-4be5-b02f-e43db6eead6b",
    "requestId": "3aec628c-3b71-4177-b1ef-35532b644db4",
    "transactionState": "AUTHORIZED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "411234XXXXXX4113",
            "networkResponse": {
                "addressVerificationResult": "NOT_REQUESTED",
                "networkTransactionId": "013227692169878",
                "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E"
            },
            "paymentTokens": [\
                {\
                    "tokenProvider": "SAFETECH",\
                    "responseStatus": "ERROR",\
                    "tokenServiceResponseMessage": "Merchant record not found or not enabled for Acquirer Token",\
                    "tokenServiceResponseCode": "BAD_REQUEST"\
                }\
            ]
        }
    },
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "NOT_STORED",
    "isVoid": false,
    "transactionDate": "2025-08-15T11:44:21.778Z",
    "approvalCode": "tst394",
    "hostMessage": "Approved",
    "amount": 15000,
    "currency": "USD",
    "remainingAuthAmount": 15000,
    "totalAuthorizedAmount": 15000,
    "hostReferenceId": "U1AiWGxAmCKu58itIp54J3",
    "merchant": {
        "merchantId": "000017904371",
        "merchantSoftware": {
            "companyName": "Payment company",
            "productName": "Application name"
        },
        "merchantCategoryCode": "4899"
    },
    "paymentRequest": {
        "paymentRequestId": "bd359c65-f17b-4be5-b02f-e43db6eead6b",
        "paymentRequestStatus": "PENDING",
        "authorizations": [\
            {\
                "authorizationId": "bd359c65-f17b-4be5-b02f-e43db6eead6b",\
                "amount": 15000,\
                "transactionStatusCode": "AUTHORIZED",\
                "authorizationType": "INCREMENTAL"\
            }\
        ]
    }
}
```

## Partial reversal authorization

Use a partial reversal authorization to reduce the authorized amount when the final capture is less than the original authorization amount. This action tells the issuer to release the hold on unused funds.

- Send the `transactionId` from the approved authorization response in the `id` path parameter.
- Set the `captureMethod` field to `NOW` to capture the authorization immediately. Set the `captureMethod` field to `MANUAL` to capture the authorization later in a separate API request.
- Specify the appropriate amount in the `amount` field. The amount value in a partial reversal authorization request is the replacement authorization amount. For example, to reverse $70 of an original $100 authorization, send $30 in your `PATCH` request.

The following example is a partial reversal authorization request that also captures the payment immediately:

**HTTP method**: `PATCH`

**Endpoint**: `/payments/{id}`

Json
Copy

```json
{
    "captureMethod": "NOW",
    "amount": 3000
}
```

Response:

Json
Copy

```json
{
    "transactionId": "4800ef65-f9e8-4c65-b99e-d537efca9ed1",
    "requestId": "a039333c-029a-4c1b-accb-51edc60b378f",
    "transactionState": "CLOSED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "isBillPayment": true,
            "maskedAccountNumber": "411234XXXXXX4113",
            "networkResponse": {
                "addressVerificationResult": "NOT_REQUESTED",
                "additionalData": {
                    "electronicCommerceIndicator": "7"
                },
                "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E"
            },
            "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E"
        }
    },
    "captureMethod": "NOW",
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "NOT_STORED",
    "isVoid": false,
    "transactionDate": "2024-03-11T20:37:14.582Z",
    "approvalCode": "TST388",
    "hostMessage": "Approved",
    "isAmountFinal": true,
    "amount": 3000,
    "currency": "USD",
    "remainingRefundableAmount": 3000,
    "remainingAuthAmount": 0,
    "totalAuthorizedAmount": 10000,
    "hostReferenceId": "jmSdNuBeaa4uaLSHJFxnR7",
    "merchant": {
        "merchantId": "000017904374",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name",
            "version": "1.235"
        },
        "merchantCategoryCode": "4899"
    },
    "paymentRequest": {
        "paymentRequestId": "4800ef65-f9e8-4c65-b99e-d537efca9ed1",
        "paymentRequestStatus": "CLOSED",
        "authorizations": [\
            {\
                "authorizationId": "4800ef65-f9e8-4c65-b99e-d537efca9ed1",\
                "amount": 10000,\
                "transactionStatusCode": "CAPTURED",\
                "authorizationType": "INITIAL"\
            }\
        ],
        "captures": [\
            {\
                "captureId": "4800ef65-f9e8-4c65-b99e-d537efca9ed1",\
                "amount": 3000,\
                "transactionStatusCode": "CLOSED",\
                "captureRemainingRefundableAmount": 3000\
            }\
        ]
    }
}
```

## Void an authorization

Void a transaction if there is no intent to complete the purchase. This action tells the issuer to release the hold on funds. J.P. Morgan sends an authorization reversal to the issuer when we receive a void request. Void only applies to authorization requests that have not been captured.

- Send the `transactionId` from the approved authorization response in the `id` path parameter.
- Set the `isVoid` field to `true`.

The following example shows the minimum required fields to void a payment:

**HTTP method**: `PATCH`

**Endpoint**: `/payments/{id}`

Json
Copy

```json
{
    "isVoid": true
}
```

Response:

Json
Copy

```json
{
    "transactionId": "7dcf2f28-2ce2-4f31-985b-e6c8d6138329",
    "requestId": "b53710da-608e-4fc2-b3cd-6ee9e1af96df",
    "transactionState": "VOIDED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "isBillPayment": true,
            "maskedAccountNumber": "411234XXXXXX4113",
            "networkResponse": {
                "addressVerificationResult": "NOT_REQUESTED",
                "additionalData": {
                    "electronicCommerceIndicator": "7"
                },
                "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E"
            },
            "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E"
        }
    },
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "NOT_STORED",
    "isVoid": true,
    "transactionDate": "2024-03-11T20:39:29.007Z",
    "approvalCode": "tst101",
    "hostMessage": "Approved",
    "isAmountFinal": true,
    "amount": 1234,
    "currency": "USD",
    "remainingAuthAmount": 0,
    "hostReferenceId": "3nMRv8pnX9lWXEIQezTqx3",
    "merchant": {
        "merchantId": "000017904374",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name",
            "version": "1.235"
        },
        "merchantCategoryCode": "4899"
    },
    "paymentRequest": {
        "paymentRequestId": "7dcf2f28-2ce2-4f31-985b-e6c8d6138329",
        "paymentRequestStatus": "CANCELLED",
        "authorizations": [\
            {\
                "authorizationId": "7dcf2f28-2ce2-4f31-985b-e6c8d6138329",\
                "amount": 1234,\
                "transactionStatusCode": "VOIDED",\
                "authorizationType": "INITIAL"\
            }\
        ]
    }
}
```

## Related

[Authorize and capture a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/auth-and-capture-payment)

[Online Payments response codes](https://developer.payments.jpmorgan.com/api/commerce/online-payments/online-payments/error-codes)

Is this page helpful?
