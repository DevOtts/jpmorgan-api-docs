---
title: "tokenization"
description: "Guide for payment processing and authorization"
category: "Payments"
---


# Tokenization

Tokenization is the process of replacing sensitive card data with a unique identification token that retains all the essential information without compromising security. This minimizes the amount of card data you need to keep on hand, significantly reduces your payment card industry (PCI) compliance scope and minimizes impacts from potential data intrusion exposure.

J.P. Morgan currently offers a network tokenization solution for the following transactions:

- Recurring transactions
- Card-on-file transactions
- Subsequent transactions


To better protect account numbers, we recommend storing tokens instead of primary account numbers (PANs) for cards-on-file used for recurring purchases or payments. You can create tokens using the following methods:

- Network tokenization
- Managed network tokenization
- Safetech tokenization

## Network tokenization

Network tokenization uses a token value assigned by a payment network. This allows you to use the existing token in the payment transactions without de-tokenizing it prior to payment processing. This same token is then used in any subsequent actions related to the initial payment, including clearing, settlement, refunds, and dispute processing.

Network tokens (NWTs) are secured using a cryptogram and given a unique value obtained from the networks prior to each transaction. This value is used by the network to match the cryptogram generated for the authenticated token requester to the cryptogram being sent in for transaction processing. For merchant-initiated transactions such as recurring payments, NWT transactions can be submitted without a cryptogram, as long as the fields referenced on the [recurring payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/recurring-payments) are used correctly.

Tip

For additional information regarding token provisioning, as well as requesting and using cryptograms, refer to the [Tokenization API](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/tokenization/index).

Because NWTs are issued by the payment network, you can use them with any payment provider as long as the cryptogram and other required fields are used correctly.

When submitting a transaction with an NWT, use the `accountNumber` field for the token value and token expiry month and year. When needed, the cryptogram is placed in the `tokenAuthenticationValue` field. Set the `electronicCommerceIndicator` field value to `7` for Visa, and `5` for Mastercard NWTs when a cryptogram is provided.

**HTTP method**: `POST`

**Endpoints**: `/payments`, `/verifications`, and `/refunds`

The following is a sample request using a network token:

Json
Copy

```json
{
    "captureMethod": "NOW",
    "amount": 10000,
    "currency": "USD",
    "isAmountFinal": "true",
    "initiatorType": "MERCHANT",
    "accountOnFile": "TO_BE_STORED",
    "paymentMethodType": {
        "card": {
            "accountNumber": "4112344112344113",
            "expiry": {
                "month": 12,
                "year": 2027
            },
            "authentication": {
                "electronicCommerceIndicator": "7",
                "tokenAuthenticationValue": "AAABAIcJIo7DIzAgVAkiAAAAAAA="
            }
        }
    },
    "merchant": {
        "merchantSoftware": {
            "companyName": "J.P. Morgan Chase",
            "productName": "Helix Connect",
            "version": "1.0"
        }
    }
}
```

Response:

Json
Copy

```json
{
    "transactionId": "3fa2aa4d-7a93-48ec-b5b9-3f03300a2d17",
    "requestId": "793b2a2b-7577-4efa-8b73-ccb46623e521",
    "transactionState": "CLOSED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "411234XXXXXX4113",
            "networkResponse": {
                "addressVerificationResult": " NOT_REQUESTED",
                "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E",
                "networkResponseCode": "00"
            },
            "authentication": {
                "electronicCommerceIndicator": "7",
                "authenticationValueResponse": {
                    "code": "2",
                    "message": "CAVV passed verification—authentication"
                }
            }
        }
    }
}
```

## Managed network tokenization

Managed network tokenization is an optional service that converts PANs to payment network NWTs. This service requires minimal changes to your integration and additional fields are provided in the response to indicate the processing status. To enroll in the managed network tokenization service, contact your relationship manager.

Tip

To ensure optimized token provisioning and successful tokenized authorization, your managed network token requests to the [Online Payments API](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/index) must also include [stored payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/stored-payments) and [recurring payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/recurring-payments) data field requirements. The sample request below shows an example of a cardholder initiated, stored payment scenario. You can use the sample payload message to generate a similar request for merchant initiated, recurring payment transactions.

**HTTP method**: `POST`

**Endpoint**: `/payments`

The following is a sample managed network tokenization request to receive a network token using the managed network tokenization service:

Json
Copy

```json
{
    "amount": 1234,
    "currency": "USD",
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "TO_BE_STORED",
    "merchant": {
        "merchantSoftware": {
            "companyName": "Shipping UAT",
            "productName": "Application Name",
            "version": 1.235
        },
        "merchantCategoryCode": 4899
    },
    "paymentMethodType": {
        "card": {
            "accountNumber": 4112344112344113,
            "expiry": {
                "month": 5,
                "year": 2022
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
    "transactionId": "4751d11a-c194-4f7e-a433-774fa4ff36ef",
    "requestId": "7bda0564-a5f6-4b4a-91f3-f71f92955a32",
    "transactionState": "CLOSED",
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
                "networkTransactionId": "013291692163387",
                "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E",
                "networkResponseCode": "00"
            },
            "paymentTokens": [\
                {\
                    "tokenProvider": "NETWORK",\
                    "tokenNumber": "xxxxxxxxxxxx1002",\
                    "responseStatus": "SUCCESS",\
                    "tokenServiceResponseCode": "TOKEN_PROCESSING"\
                }\
            ]
        }
    }
}
```

## Safetech tokenization

Safetech tokenization is an optional service that converts PAN and bank account information (such as account and routing numbers) to J.P. Morgan-issued acquirer tokens. These tokens are safely stored in your system instead of the full PAN or bank account information.

When a Safetech token is used within a transaction, J.P. Morgan replaces the token with the account number for authorization. The response contains the Safetech token and masked account number. To enroll in the Safetech tokenization service, contact your relationship manager.

**HTTP method**: `POST`

**Endpoints**: `/payments`, `/verifications`, `/refunds`, and `/fraudcheck`

The following payment example requests a Safetech token:

Json
Copy

```json
{
    "amount": 1234,
    "currency": "USD",
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "TO_BE_STORED",
    "merchant": {
        "merchantSoftware": {
            "companyName": "Shipping UAT",
            "productName": "Application Name",
            "version": 1.235
        },
        "merchantCategoryCode": 4899
    },
    "paymentMethodType": {
        "card": {
            "accountNumber": 4112344112344113,
            "expiry": {
                "month": 5,
                "year": 2022
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
    "transactionId": "4751d11a-c194-4f7e-a433-774fa4ff36ef",
    "requestId": "7bda0564-a5f6-4b4a-91f3-f71f92955a32",
    "transactionState": "CLOSED",
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
                "networkTransactionId": "013291692163387",
                "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E",
                "networkResponseCode": "00"
            },
            "paymentTokens": [\
                {\
                    "tokenProvider": "SAFETECH",\
                    "tokenNumber": "4112340803104113",\
                    "responseStatus": "SUCCESS"\
                }\
            ]
        }
    }
}
```

The following payment example uses a Safetech token:

Json
Copy

```json
{
    "amount": 1234,
    "currency": "USD",
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "TO_BE_STORED",
    "merchant": {
        "merchantSoftware": {
            "companyName": "Shipping UAT",
            "productName": "Application Name",
            "version": 1.235
        },
        "merchantCategoryCode": 4899
    },
    "paymentMethodType": {
        "card": {
            "accountNumberType": "SAFETECH_TOKEN",
            "accountNumber": 4112340803104113,
            "expiry": {
                "month": 5,
                "year": 2022
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
    "transactionId": "f9b9a1ec-8842-4a07-952f-21dad4364183",
    "requestId": "274ab31c-55c7-4388-a65c-83c82b20e949",
    "transactionState": "CLOSED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "accountNumberType": "SAFETECH_TOKEN",
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "411234XXXXXX4113",
            "networkResponse": {
                "addressVerificationResult": "NOT_REQUESTED",
                "networkTransactionId": "013291692160631",
                "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E",
                "networkResponseCode": "00"
            }
        }
    }
}
```

The following payment example requests a Safetech token for the bank account payment method ACH:

Json
Copy

```json
{
    "paymentMethodType": {
        "ach": {
            "accountNumber": "12345678925",
            "financialInstitutionRoutingNumber": "123456789",
            "paymentType": "WEB",
            "accountType": "SAVING"
        }
    }
}
```

Response:

Json
Copy

```json
{
    "transactionId": "da289e65-3bb2-47c3-ba89-d0545a084b16",
    "requestId": "d0630cbe-3bb4-4448-8406-f0f266d2ec5f",
    "transactionState": "CLOSED",
    "responseStatus": "SUCCESS",
    "responseCode": "ACCEPTED",
    "responseMessage": "Request successfully completed",
    "paymentMethodType": {
        "ach": {
            "financialInstitutionRoutingNumber": "123456789",
            "paymentType": "WEB",
            "accountType": "SAVING",
            "maskedAccountNumber": "XXXXXXX8925",
            "ecpProgramName": "STD",
            "paymentTokens": [\
                {\
                    "tokenNumber": "BTKNSTDPRIUP9PHTQKA",\
                    "responseStatus": "SUCCESS",\
                    "tokenServiceResponseCode": "ACCEPTED"\
                }\
            ]
        }
    }
}
```

The following payment example uses a Safetech token as the account number in the bank account payment method ACH:

Json
Copy

```json
{
    "paymentMethodType": {
        "ach": {
            "accountNumber": "BTKNSTDPRIUP9PHTQKA",
            "paymentType": "WEB",
            "accountType": "SAVING"
        }
    }
}
```

Response:

Json
Copy

```json
{
    "transactionId": "f2ae4dd2-ded8-424f-a910-f3d34bb6b936",
    "requestId": "0a327188-f385-41ea-b4b9-5c6545ea5a7a",
    "transactionState": "CLOSED",
    "responseStatus": "SUCCESS",
    "responseCode": "ACCEPTED",
    "responseMessage": "Request successfully completed",
    "paymentMethodType": {
        "ach": {
            "financialInstitutionRoutingNumber": "123456789",
            "paymentType": "WEB",
            "accountType": "SAVING",
            "maskedAccountNumber": "XXXXXXX8925",
            "ecpProgramName": "STD"
        }
    }
}
```

## Related

[Online Payments API](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/index)

[Real-time account updater](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/account-updater)

[Recurring payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/recurring-payments)

[Stored Payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/stored-payments)

[Tokenization API](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/tokenization/index)

Is this page helpful?
