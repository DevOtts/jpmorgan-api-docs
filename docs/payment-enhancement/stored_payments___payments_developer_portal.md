---
title: "stored payments"
description: "Guide for payment processing and authorization"
category: "Payments"
---


# Stored payments

Cardholder-initiated stored payments are transactions in which a cardholder, who has stored their credentials with a merchant, takes an action to initiate a purchase, but elects to use the card on file rather than entering a new card number.

The key distinction between [recurring](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/recurring-payments) and cardholder-initiated stored payments is what triggers the transaction. If the cardholder actively does something to initiate the transaction, such as place an order or log into a portal to pay a bill, it is considered a cardholder-initiated stored payment. Conversely, a recurring payment occurs automatically without the cardholder taking any action to initiate the payment.

## How stored payments work

1. Cardholder credentials are stored on the initial transaction by including the below two fields and values in the payment request:
1. `initiatorType` set to `CARDHOLDER`

2. `accountOnFile` set to `TO_BE_STORED`
2. Subsequent payments then need to include the below two fields to use the stored payment credentials:
1. `initiatorType` set to `CARDHOLDER`

2. `accountOnFile` set to `STORED`

## Initial stored payment request

The following example shows an initial stored payment request:

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
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "TO_BE_STORED",
    "originalTransactionId": "12345",
    "accountHolder": {
        "referenceId": "1245",
        "fullName": "John Doe",
        "email": "example@example.com",
        "IPAddress": "123.12.123.1",
        "billingAddress": {
            "line1": "123 Main Street",
            "line2": "Apartment 2",
            "city": "Tampa",
            "state": "FL",
            "postalCode": "33785"
        }
    },
    "shipTo": {
        "shippingAddress": {
            "line1": "123 Main Street",
            "line2": "Apartment 2",
            "city": "Tampa",
            "state": "FL",
            "postalCode": "33785"
        }
    }
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
    "recurring": null,
    "accountHolder": {
        "referenceId": "1245",
        "IPAddress": "104.18.127.1"
    },
    "shipTo": null
}
```

## Subsequent stored payments requests

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
            "accountNumber": "xxxxxxxxxxxx0026",
            "expiry": {
                "month": 10,
                "year": 2040
            }
        }
    },
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "STORED",
    "originalTransactionId": "16444",
    "accountHolder": {
        "referenceId": "1245",
        "fullName": "John Doe",
        "email": "example@example.com",
        "IPAddress": "123.12.123.1",
        "billingAddress": {
            "line1": "123 Main Street",
            "line2": "Apartment 2",
            "city": "Tampa",
            "state": "FL",
            "postalCode": "33785"
        }
    },
    "shipTo": {
        "shippingAddress": {
            "line1": "123 Main Street",
            "line2": "Apartment 2",
            "city": "Tampa",
            "state": "FL",
            "postalCode": "33785"
        }
    }
}
```

Response:

Json
Copy

```json
{
    "transactionId": "d461bd0c-2f6d-4b85-9ce7-552651d0edea",
    "requestId": "10cc0270-7bed-11e9-a188-1763956dd7f6",
    "transactionState": "AUTHORIZED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "411234XXXXXX4113",
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
                "cardVerificationResultCode": "M"
            }
        }
    },
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "STORED",
    "transactionDate": "2025-05-04T20:04:25.025Z",
    "approvalCode": "tst982",
    "hostMessage": "Approved",
    "amount": 10000,
    "currency": "USD",
    "hostReferenceId": "G0i19bLbf0L02Z9g1GiEz4",
    "merchant": {
        "merchantId": "998482157632",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        }
    },
    "recurring": {
        "recurringSequence": "FIRST",
        "agreementId": "1235",
        "paymentAgreementExpiryDate": "2040-10-30"
    },
    "shipTo": {
        "shippingDescription": "Personal Item shipping to my home"
    }
}
```

## Related

[Authorize and capture a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/auth-and-capture-payment)

[Recurring payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/recurring-payments)

Is this page helpful?
