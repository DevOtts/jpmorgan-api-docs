---
title: "Authorize and Capture a Payment"
description: "Guide for payment authorization and capture processes"
category: "How-To"
---

# Authorize and capture a payment

Authorizing a payment is the first step of the payment request process. When you submit a payment request, J.P. Morgan sends an authorization request to the issuing bank to obtain approval for the payment. If approved, the second step of the payment request is to capture the funds from the cardholder's account.

In this guide, learn about the following payment types and features:

- Authorization with automatic immediate capture
- Authorization with automatic delayed capture
- Authorization with manual capture
  - Manual capture
  - Partial capture
  - Multi-capture
- Partial authorization support

- Retrieve authorization details

## Before you begin

- Determine your payment use cases including how you need to support capture and if you need to support partial authorizations.
- Understand the minimum requirements for a simple payment.

The following table represents the minimum requirements of a simple card authorization request:

|     |     |
| --- | --- |
| **Parameter** | **Description** |
| captureMethod | When set to `NOW` in an approved request, the response contains the following:<br>- transactionState — `CLOSED`<br>  <br>- transactionStatusCode — `CAPTURED`<br>  <br>When set to `DELAYED` or `MANUAL` in an approved request, the response contains the following:<br>- transactionState — `AUTHORIZED`<br>  <br>- transactionStatusCode — `AUTHORIZED` |
| amount | Specifies the monetary value of the transaction performed |
| currency | Identifies the currency for the amount fields. This is the 3 character ISO 4217 currency code. |
| paymentMethodType | The method of payment used for a transaction |
| card | The card payment instrument used for a transaction |
| accountNumber | The card number used for a transaction |
| expiry | The expiration date associated with the provided account number. |
| month | The month when the card expires. |
| year | The year when the card expires. |
| merchant | Merchant information that includes the following:<br>- merchantSoftware<br>  <br>- companyName<br>  <br>- productName |

Simple card authorization request requirements


## Authorization with automatic immediate capture

Authorization with automatic capture can be triggered by a single API call.  The capture is initiated immediately after an approved authorization. Sending `captureMethod` as `NOW` initiates the authorization with automatic immediate capture.  This is also the default payment behavior if you omit this field.

The following example request represents the minimum requirements for a successful card authorization and automatic/immediate capture. The amount in this sample uses implied decimal places according to the currency. For example, **$10** USD is sent as **1000**.

**HTTP method**: `POST`

**Endpoint**: `/payments`

Json
Copy

```json
{
  "captureMethod": "NOW",
  "amount": 1000,
  "currency": "USD",
  "merchant": {
    "merchantSoftware": {
      "companyName": "Payment Company",
      "productName": "Application Name"
    }
  },
  "paymentMethodType": {
    "card": {
      "accountNumber": "4012000033330026",
      "expiry": {
        "month": 5,
        "year": 2027
      }
    }
  }
}
```

Note

You cannot void or adjust the amount of this transaction after you have initiated the capture. The only option available is to refund.

An HTTP status of `200` indicates a successful response. The following fields indicate result:

- `responseStatus`
- `responseCode`
- `responseMessage`

Tip

Refer to the [Online payments response codes](https://developer.payments.jpmorgan.com/api/commerce/online-payments/online-payments/error-codes) for more information on these fields.

Response:

Json
Copy

```json
{
    "transactionId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",
    "requestId": "40902d7b-4566-485c-bd07-7aef69557f55",
    "transactionState": "CLOSED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "401200XXXXXX0026"
        }
    },
    "captureMethod": "NOW",
    "captureTime": "2023-09-18T18:04:26.898Z",
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "NOT_STORED",
    "isVoid": false,
    "transactionDate": "2023-09-18T18:04:26.898Z",
    "approvalCode": "tst021",
    "hostMessage": "Approved",
    "amount": 1000,
    "currency": "USD",
    "remainingRefundableAmount": 1234,
    "remainingAuthAmount": 0,
    "hostReferenceId": "xf1kGP1Q9zajkzcQ7SIpA6",
    "merchant": {
        "merchantId": "000017904371",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        },
        "merchantCategoryCode": "4899"
    },
    "paymentRequest": {
        "paymentRequestId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",
        "paymentRequestStatus": "CLOSED",
        "authorizations": [\
            {\
                "authorizationId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",\
                "amount": 1234,\
                "transactionStatusCode": "CAPTURED",\
                "authorizationType": "INITIAL"\
            }\
        ],
        "captures": [\
            {\
                "captureId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",\
                "amount": 1234,\
                "transactionStatusCode": "CLOSED",\
                "captureRemainingRefundableAmount": 1234\
            }\
        ]
    }
}
```

## Authorization with automatic delayed capture

Process an authorization with delayed automatic capture in a single API call. Capture occurs after a default delay of 120 minutes. To configure a different capture delay, work with your relationship manager. Send `captureMethod` with the value `DELAYED` to indicate an authorization with automatic delayed capture.

Before the automatic capture of the payment, refer to [update a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/update-a-payment) if you need to increment, partially reverse, or void the payment.

The following is an example of an authorization request with delayed capture:

**HTTP method**: `POST`

**Endpoint**: `/payments`

Json
Copy

```json
{
  "captureMethod": "DELAYED",
  "amount": 1000,
  "currency": "USD",
  "merchant": {
    "merchantSoftware": {
      "companyName": "Payment Company",
      "productName": "Application Name"
    }
  },
  "paymentMethodType": {
    "card": {
      "accountNumber": "4012000033330026",
      "expiry": {
        "month": 5,
        "year": 2027
      }
    }
  }
}
```

An HTTP status of `200` indicates a successful response. The following fields indicate result:

- `responseStatus`
- `responseCode`
- `responseMessage`

Tip

Refer to the [Online payments response codes](https://developer.payments.jpmorgan.com/api/commerce/online-payments/online-payments/error-codes) for more information on these fields.

Response:

Json
Copy

```json
{
    "transactionId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",
    "requestId": "40902d7b-4566-485c-bd07-7aef69557f55",
    "transactionState": "AUTHORIZED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "401200XXXXXX0026"
        }
    },
    "captureMethod": "DELAYED",
    "captureTime": "2023-09-18T18:04:26.898Z",
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "NOT_STORED",
    "isVoid": false,
    "transactionDate": "2023-09-18T18:04:26.898Z",
    "approvalCode": "tst021",
    "hostMessage": "Approved",
    "amount": 1000,
    "currency": "USD",
    "remainingRefundableAmount": 1234,
    "remainingAuthAmount": 0,
    "hostReferenceId": "xf1kGP1Q9zajkzcQ7SIpA6",
    "merchant": {
        "merchantId": "000017904371",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        },
        "merchantCategoryCode": "4899"
    },
    "paymentRequest": {
        "paymentRequestId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",
        "paymentRequestStatus": "PENDING",
        "authorizations": [\
            {\
                "authorizationId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",\
                "amount": 1234,\
                "transactionStatusCode": "AUTHORIZED",\
                "authorizationType": "INITIAL"\
            }\
        ]
    }
}
```

## Authorization with manual capture

Authorization with manual capture is a dual message API call. The first call authorizes the payment only and will not capture until you complete the second call. Sending `captureMethod` as `MANUAL` initiates the authorization request.

The following example shows the minimum required fields for the authorization request of a manual capture payment:

**HTTP method**: `POST`

**Endpoint**: `/payments`

Json
Copy

```json
{
    "captureMethod": "MANUAL",
    "amount": 1234,
    "currency": "USD",
    "merchant": {
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        }
    },
    "paymentMethodType": {
        "card": {
            "accountNumber": "4012000033330026",
            "expiry": {
                "month": 5,
                "year": 2025
            }
        }
    }
}
```

An HTTP status of `200` indicates a successful response. The following fields indicate result:

- `responseStatus`
- `responseCode`
- `responseMessage`

Tip

Refer to the [Online payments response codes](https://developer.payments.jpmorgan.com/api/commerce/online-payments/online-payments/error-codes) for more information on these fields.

Response:

Json
Copy

```json
{
    "transactionId": "e070ee86-c652-45ae-b04f-a2aaf181deef",
    "requestId": "35d3b986-7847-4289-bf34-bff405177753",
    "transactionState": "AUTHORIZED",
    "responseStatus": "SUCCESS",
    "responseCode": "APPROVED",
    "responseMessage": "Transaction approved by Issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "401200XXXXXX0026",
            "networkResponse": {
                "addressVerificationResult": "NOT_REQUESTED",
                "additionalData": {
                    "electronicCommerceIndicator": "7",
                    "authorizationResponseCategory": "A - Approved"
                },
                "networkTransactionId": "014072692161152",
                "networkResponseCode": "00"
            }
        }
    },
    "captureMethod": "MANUAL",
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "NOT_STORED",
    "isVoid": false,
    "transactionDate": "2024-03-12T13:35:06.802Z",
    "approvalCode": "tst294",
    "hostMessage": "Approved",
    "amount": 1234,
    "currency": "USD",
    "remainingAuthAmount": 1234,
    "hostReferenceId": "ou7it5WnKQ8x0T9R7hIMz2",
    "merchant": {
        "merchantId": "000017904374",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        },
        "merchantCategoryCode": "4899"
    },
    "paymentRequest": {
        "paymentRequestId": "e070ee86-c652-45ae-b04f-a2aaf181deef",
        "paymentRequestStatus": "PENDING",
        "authorizations": [\
            {\
                "authorizationId": "e070ee86-c652-45ae-b04f-a2aaf181deef",\
                "amount": 1234,\
                "transactionStatusCode": "AUTHORIZED",\
                "authorizationType": "INITIAL"\
            }\
        ]
    }
}
```

### Manual full capture

A manual full capture is submitted separately from the transaction's authorization and completes the capture of the full authorized amount of an authorization initiated with `captureMethod` set to `MANUAL`.

- Use the `transactionId` from the authorization as the path parameter for `{id}`.

- Set the `captureMethod` to `NOW`.

The following is an example manual full capture request of a previously authorized payment:

**HTTP method**: `POST`

**Endpoint**: `/payments/{id}/captures`

Json
Copy

```json
{
    "captureMethod": "NOW"
}
```

Response:

Json
Copy

```json
{
    "transactionId": "7ffceee3-adf7-42ba-8c02-77fb5ef008ed",
    "requestId": "3c65b967-8624-426f-8a81-6a63c9c74781",
    "transactionState": "CLOSED",
    "responseStatus": "SUCCESS",
    "responseCode": "ACCEPTED",
    "responseMessage": "Transaction accepted",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "401200XXXXXX0026",
            "networkResponse": {
                "addressVerificationResult": "NOT_REQUESTED",
                "additionalData": {
                    "electronicCommerceIndicator": "7"
                },
                "networkTransactionId": "014071692163531"
            }
        }
    },
    "captureMethod": "NOW",
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "NOT_STORED",
    "isVoid": false,
    "transactionDate": "2024-03-11T21:46:18.685Z",
    "approvalCode": "tst420",
    "hostMessage": "Approved",
    "isAmountFinal": true,
    "amount": 1000,
    "currency": "USD",
    "remainingRefundableAmount": 1000,
    "remainingAuthAmount": 0,
    "totalAuthorizedAmount": 1000,
    "hostReferenceId": "V3oEIjve7csJdiqTskFsF2",
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
        "paymentRequestId": "7ffceee3-adf7-42ba-8c02-77fb5ef008ed",
        "paymentRequestStatus": "CLOSED",
        "authorizations": [\
            {\
                "authorizationId": "7ffceee3-adf7-42ba-8c02-77fb5ef008ed",\
                "amount": 1000,\
                "transactionStatusCode": "CAPTURED",\
                "authorizationType": "INITIAL"\
            }\
        ],
        "captures": [\
            {\
                "captureId": "7ffceee3-adf7-42ba-8c02-77fb5ef008ed",\
                "amount": 1000,\
                "transactionStatusCode": "CLOSED",\
                "captureRemainingRefundableAmount": 1000\
            }\
        ]
    }
}
```

### Partial capture

For a partial capture, the amount captured could be higher or lower than the amount that was earlier authorized. Once the partial capture request is submitted, the system detects whether a new payment authorization or partial reversal is needed and performs the appropriate action.

- Use the `transactionId` from the authorization as the path parameter for `{id}`.

- Set the `captureMethod` to `NOW`.
- Specify the capture amount.
- Ensure that `isAmountFinal` is set to `true`.

The following is an example of a partial capture request for a previously-authorized payment:

**HTTP method**: `POST`

**Endpoint**: `/payments/{id}/captures`

Json
Copy

```json
{
    "captureMethod": "NOW",
    "amount": "1000",
    "isAmountFinal": true
}
```

Response:

Json
Copy

```json
{
    "transactionId": "d8c8d733-8017-42d6-8d15-fa46ab2b7fb6",
    "requestId": "c93bf8a0-a6b9-4e4e-8c48-7a30d0a9b734",
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
    "transactionDate": "2024-03-11T14:54:15.540Z",
    "approvalCode": "TST632",
    "hostMessage": "Approved",
    "isAmountFinal": true,
    "amount": 1000,
    "currency": "USD",
    "remainingRefundableAmount": 1000,
    "remainingAuthAmount": 0,
    "totalAuthorizedAmount": 1234,
    "hostReferenceId": "QVdhhpsg8lofky4xJWY687",
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
        "paymentRequestId": "d8c8d733-8017-42d6-8d15-fa46ab2b7fb6",
        "paymentRequestStatus": "CLOSED",
        "authorizations": [\
            {\
                "authorizationId": "d8c8d733-8017-42d6-8d15-fa46ab2b7fb6",\
                "amount": 1234,\
                "transactionStatusCode": "CAPTURED",\
                "authorizationType": "INITIAL"\
            }\
        ],
        "captures": [\
            {\
                "captureId": "d8c8d733-8017-42d6-8d15-fa46ab2b7fb6",\
                "amount": 1000,\
                "transactionStatusCode": "CLOSED",\
                "captureRemainingRefundableAmount": 1000\
            }\
        ]
    }
}
```

### Multi-capture

A manual multi-capture is used if an order is being fulfilled through multiple split shipments. A single authorization can be used for multiple captures.

- Use the `transactionId` from the authorization as the path parameter for `{id}`.
- Set the `isFinalCapture` to `false`.

Tip

- `multiCaptureSequenceNumber` is the shipment number.
- `multiCaptureRecordCount` is the total number of shipments required to fulfill the order.

The following example shows the minimum required fields for a full capture request on a previously authorized payment.

**HTTP method**: `POST`

**Endpoint**: `/payments/{id}/captures`

Json
Copy

```json
{
    "amount": 100,
    "multiCapture": {
        "multiCaptureSequenceNumber": 1,
        "multiCaptureRecordCount": 2,
        "isFinalCapture": false
    }
}
```

Response:

Json
Copy

```json
{
    "transactionId": "e9468a04-7ad5-47ad-b2a8-cd3d76be0f6a",
    "requestId": "252f0171-9512-4f54-94e5-5c046dc55eab",
    "transactionState": "CLOSED",
    "responseStatus": "SUCCESS",
    "responseCode": "ACCEPTED",
    "responseMessage": "Transaction accepted",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "isBillPayment": true,
            "maskedAccountNumber": "401200XXXXXX0026",
            "networkResponse": {
                "cardVerificationResult": "MATCH",
                "networkTransactionId": "013094692162180",
                "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E"
            }
        }
    },
    "captureMethod": "NOW",
    "initiatorType": "MERCHANT",
    "transactionDate": "2023-04-04T21:04:33.764Z",
    "isAmountFinal": true,
    "amount": 100,
    "currency": "USD",
    "remainingRefundableAmount": 100,
    "remainingAuthAmount": 1134,
    "hostReferenceId": "aTWjsNr8fCy2ZjS6LdpEm",
    "merchant": {
        "merchantId": "000017904371",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name",
            "version": "1.235"
        }
    },
    "paymentRequest": {
        "paymentRequestId": "e9468a04-7ad5-47ad-b2a8-cd3d76be0f6a",
        "paymentRequestStatus": "OPEN",
        "authorizations": [\
            {\
                "authorizationId": "e9468a04-7ad5-47ad-b2a8-cd3d76be0f6a",\
                "amount": 1234,\
                "transactionStatusCode": "AUTHORIZED",\
                "authorizationType": "INITIAL"\
            }\
        ],
        "captures": [\
            {\
                "captureId": "e9468a04-7ad5-47ad-b2a8-cd3d76be0f6a",\
                "amount": 100,\
                "transactionStatusCode": "CLOSED",\
                "captureRemainingRefundableAmount": 100\
            }\
        ]
    }
}
```

For the final request, ensure that `isFinalCapture` is set to `true`.

The following example shows the minimum required fields for the second and final request for the multi-capture:

Json
Copy

```json
{
    "amount": 100,
    "multiCapture": {
        "multiCaptureSequenceNumber": 2,
        "multiCaptureRecordCount": 2,
        "isFinalCapture": true
    }
}
```

Response:

Json
Copy

```json
{
    "transactionId": "22b88670-272d-4d69-a011-b728b7b45991",
    "requestId": "f9c75448-0764-4eee-b67f-d6e9d2fb167f",
    "transactionState": "CLOSED",
    "responseCode": "ACCEPTED",
    "responseStatus": "SUCCESS",
    "responseMessage": "Transaction accepted",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "isBillPayment": true,
            "maskedAccountNumber": "401200XXXXXX0026",
            "networkResponse": {
                "cardVerificationResult": "MATCH",
                "networkTransactionId": "013094692162180",
                "paymentAccountReference": "Q1J4Z28RKA1EBL470G9XYG90R5D3E"
            }
        }
    },
    "captureMethod": "NOW",
    "initiatorType": "MERCHANT",
    "transactionDate": "2023-04-04T21:04:33.764Z",
    "isAmountFinal": true,
    "amount": 100,
    "currency": "USD",
    "remainingRefundableAmount": 100,
    "remainingAuthAmount": 1034,
    "hostReferenceId": "aTWjsNr8fCy2ZjS6LdpEm",
    "merchant": {
        "merchantId": "000017904371",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name",
            "version": "1.235"
        },
        "merchantCategoryCode": "4899"
    },
    "paymentRequest": {
        "paymentRequestId": "e9468a04-7ad5-47ad-b2a8-cd3d76be0f6a",
        "paymentRequestStatus": "CLOSED",
        "authorizations": [\
            {\
                "authorizationId": "e9468a04-7ad5-47ad-b2a8-cd3d76be0f6a",\
                "amount": 1234,\
                "transactionStatusCode": "AUTHORIZED",\
                "authorizationType": "INITIAL"\
            }\
        ],
        "captures": [\
            {\
                "captureId": "22b88670-272d-4d69-a011-b728b7b45991",\
                "amount": 100,\
                "transactionStatusCode": "CLOSED",\
                "captureRemainingRefundableAmount": 100\
            },\
            {\
                "captureId": "e9468a04-7ad5-47ad-b2a8-cd3d76be0f6a",\
                "amount": 100,\
                "transactionStatusCode": "CLOSED",\
                "captureRemainingRefundableAmount": 100\
            }\
        ]
    }
}
```

## Partial authorization support

Support partial authorization of a payment to receive approval for a portion of the original amount when the full amount cannot be approved.

- To support partial authorization, send the value `SUPPORTED` in the `partialAuthorizationSupport` field.
- If you do not support partial authorization, send the value `NOT_SUPPORTED` in the `partialAuthorizationSupport` field. In this scenario, a transaction is approved for the full amount only.

After you complete a partially authorized payment, to collect the remaining balance on an additional payment method, you must send a new payment request.

The following example supports partial authorization and receives a partial approval in the response:

**HTTP method**: `POST`

**Endpoint**: `/payments`

Json
Copy

```json
{
    "captureMethod": "NOW",
    "amount": 1234,
    "currency": "USD",
    "merchant": {
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name",
            "version": "1.235"
        }
    },
    "paymentMethodType": {
        "card": {
            "accountNumber": "4012000033330026",
            "expiry": {
                "month": 5,
                "year": 2027
            }
        }
    },
    "partialAuthorizationSupport": "SUPPORTED"
}
```

Response:

Note

The `partialAuthorization` field is set to `true`, and the transaction `amount` contains the approved amount value.

Json
Copy

```json
{
    "transactionId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",
    "requestId": "40902d7b-4566-485c-bd07-7aef69557f55",
    "transactionState": "CLOSED",
    "responseStatus": "SUCCESS",
    "responseCode": "PARTIAL_APPROVAL",
    "responseMessage": "Transaction partially approved by issuer",
    "paymentMethodType": {
        "card": {
            "cardType": "VI",
            "cardTypeName": "VISA",
            "maskedAccountNumber": "401200XXXXXX0026"
        }
    },
    "captureMethod": "NOW",
    "captureTime": "2023-09-18T18:04:26.898Z",
    "initiatorType": "CARDHOLDER",
    "accountOnFile": "NOT_STORED",
    "isVoid": false,
    "transactionDate": "2023-09-18T18:04:26.898Z",
    "approvalCode": "tst021",
    "hostMessage": "Approved",
    "amount": 1000,
    "currency": "USD",
    "remainingRefundableAmount": 1000,
    "remainingAuthAmount": 0,
    "hostReferenceId": "xf1kGP1Q9zajkzcQ7SIpA6",
    "merchant": {
        "merchantId": "000017904371",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name"
        },
        "merchantCategoryCode": "4899"
    },
    "partialAuthorizationSupport": "SUPPORTED",
    "partialAuthorization": true,
    "paymentRequest": {
        "paymentRequestId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",
        "paymentRequestStatus": "CLOSED",
        "authorizations": [\
            {\
                "authorizationId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",\
                "amount": 1000,\
                "transactionStatusCode": "CAPTURED",\
                "authorizationType": "INITIAL"\
            }\
        ],
        "captures": [\
            {\
                "captureId": "9f186d77-cb1b-4a9f-bb44-b5be91b891ac",\
                "amount": 1000,\
                "transactionStatusCode": "CLOSED",\
                "captureRemainingRefundableAmount": 1000\
            }\
        ]
    }
}
```

## Retrieve authorization details

After making an authorization request, you can review the authorization details. Complete the following steps to retrieve authorization details:

1. Send a `GET` request to the `/payments` endpoint one of two ways:

1. Use the `requestId` returned in the original response as a query parameter.
2. Use the `transactionId` returned in the original response as a path parameter.

## Related

[Online Payments response codes](https://developer.payments.jpmorgan.com/api/commerce/online-payments/online-payments/error-codes)

[Refund a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/refund-payment)

[Update a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/update-a-payment)

[Verify a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/verify-payment)

Is this page helpful?