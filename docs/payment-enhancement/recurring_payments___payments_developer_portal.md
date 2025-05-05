---
title: "recurring payments"
description: "Guide for payment processing and authorization"
category: "Payments"
---


# Recurring payments

Recurring payments are used when your consumer agrees to set up a recurring payment using the payment information securely saved to their profile. These payments occur at regular intervals and your consumer does not need to take any manual action to initiate these payments going forward.

The key distinction between recurring and cardholder-initiated [stored payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/stored-payments) is what triggers the transaction. If the cardholder actively does something to initiate the transaction, such as place an order or log into a portal to pay a bill, it is considered a cardholder-initiated stored payment. Conversely, a recurring payment occurs automatically without the cardholder taking any action to initiate the payment.

## How recurring payments work

It is important to identify who initiated the payment, the payment frequency, status of stored payment account data, and agreement ID. The first transaction is initiated by the consumer, whereby they authorize subsequent transactions that are initiated by the merchant.

1. Cardholder credentials are stored on the initial transaction by including the below four fields and values in the payment request.
1. `recurring.recurringSequence` set to `FIRST`
2. `initiatorType` set to `CARDHOLDER`
3. `accountOnFile` set to `TO_BE_STORED`
4. `recurring.agreementId` set to the `agreementId` in the recurring series
2. Subsequent payments then need to include the below four fields to use the stored payment credentials:
1. `recurring.recurringSequence` set to `SUBSEQUENT`
2. `initiatorType` set to `MERCHANT`
3. `accountOnFile` set to `STORED`
4. `recurring.agreementId` set to the `agreementId` in the recurring series
5. `card.originalNetworkTransactionId` set to the network-generated ID that is returned on the first/initial transaction. This field is required for certain card networks and must contain the same value received from the initial recurring request. The following card networks and regions require the `card.originalNetworkTransactionId` attribute:

      1. Visa primary account number (EU)
      2. Visa payment token (US, EU)
      3. Mastercard (EU)
      4. Discover payment token (US, EU)
      5. American Express (EU)

## Initial recurring payment request

The following example shows an initial recurring payment request:

**HTTP method**: `POST`

**Endpoint**: `/payments`

Json
Copy

```json
{
    "captureMethod": "NOW",
    "amount": 10000,
    "currency": "USD",
    "merchant": {
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        },
        "merchantCategoryCode": "4899"
    },
    "paymentMethodType": {
        "card": {
            "accountNumber": "4012000033330026",
            "expiry": {
                "month": 10,
                "year": 2040
            }
        }
    },
    "recurring": {
        "recurringSequence": "FIRST",
        "agreementId": "agreementid12345"
    },
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "TO_BE_STORED",
    "originalTransactionId": "12345"
}
```

Response:

Json
Copy

```json
{
    "transactionId": "c9d21e9e-9263-4c6e-a6b7-ef60cbceeb49",
    "requestId": "10cc0270-7bed-11e9-a188-1763956dd7f6",
    "transactionState": "AUTHORIZED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "401200XXXXXX0026",
            "cardTypeIndicators": {
                "issuanceCountryCode": "USA",
                "isDurbinRegulated": false,
                "cardProductTypes": [\
                    "PINLESS_DEBIT"\
                ]
            },
            "networkResponse": {
                "addressVerificationResult": "NOT_REQUESTED",
                "addressVerificationResultCode": "",
                "cardVerificationResultCode": "M",
                "networkTransactionId": "012125692162451"
            }
        }
    },
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "TO_BE_STORED",
    "transactionDate": "2025-05-04T20:04:25.025Z",
    "approvalCode": "tst904",
    "hostMessage": "Approved",
    "amount": 10000,
    "currency": "USD",
    "remainingRefundableAmount": 1500,
    "hostReferenceId": "sGD8MSXuJan0ytfXIJAP57",
    "merchant": {
        "merchantId": "998482157632",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        },
        "merchantCategoryCode": "4899"
    },
    "recurring": {
        "recurringSequence": "FIRST",
        "agreementId": "agreementid12345"
    }
}
```

## Subsequent recurring payment requests

The following example shows a subsequent stored payment request:

**HTTP method**: `POST`

**Endpoint**: `/payments`

Json
Copy

```json
{
    "captureMethod": "NOW",
    "amount": 10000,
    "currency": "USD",
    "merchant": {
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        },
        "merchantCategoryCode": "4899"
    },
    "paymentMethodType": {
        "card": {
            "accountNumber": "4012000033330026",
            "originalNetworkTransactionId": "012125692162451",
            "expiry": {
                "month": 10,
                "year": 2040
            }
        }
    },
    "recurring": {
        "recurringSequence": "SUBSEQUENT",
        "agreementId": "agreementid12345"
    },
    "initiatorType": "MERCHANT",
    "accountOnFile": "STORED",
    "originalTransactionId": "12345"
}
```

Response:

Json
Copy

```json
{
    "transactionId": "c9d21e9e-9263-4c6e-a6b7-ef60cbceeb49",
    "requestId": "10cc0270-7bed-11e9-a188-1763956dd7f6",
    "transactionState": "AUTHORIZED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "401200XXXXXX0026",
            "originalNetworkTransactionId": "012125692162451",
            "cardTypeIndicators": {
                "issuanceCountryCode": "USA",
                "isDurbinRegulated": false,
                "cardProductTypes": [\
                    "PINLESS_DEBIT"\
                ]
            },
            "networkResponse": {
                "addressVerificationResult": "NOT_REQUESTED",
                "addressVerificationResultCode": "",
                "cardVerificationResultCode": "M",
                "networkTransactionId": "012125692162451"
            }
        }
    },
    "initiatorType": "MERCHANT",
    "accountOnFile": "STORED",
    "transactionDate": "2025-05-04T20:04:25.025Z",
    "approvalCode": "tst904",
    "hostMessage": "Approved",
    "amount": 10000,
    "currency": "USD",
    "remainingRefundableAmount": 1500,
    "hostReferenceId": "sGD8MSXuJan0ytfXIJAP57",
    "merchant": {
        "merchantId": "998482157632",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        },
        "merchantCategoryCode": "4899"
    },
    "recurring": {
        "recurringSequence": "SUBSEQUENT",
        "agreementId": "agreementid12345"
    }
}
```

## Related

[Authorize and capture a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/auth-and-capture-payment)

[Stored payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/stored-payments)

Is this page helpful?
