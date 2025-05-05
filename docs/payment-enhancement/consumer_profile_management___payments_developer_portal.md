---
title: "consumer profile management"
description: "Guide for payment processing and authorization"
category: "Payments"
---


# Consumer profile management

Create, store, update, and maintain consumer profiles, including payment card information, at J.P. Morgan. Comply with payment card industry data security standards and simplify the payment process for returning consumers and recurring transactions.

When you create a consumer profile, J.P. Morgan generates a unique profile ID and payment method ID for future transactions. To manage a consumer profile separate from a payment request, refer to the Optimization & Protection [Consumer Profile Management API](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/consumer-profile-management/index).

## Create a profile during a payment

Create a consumer profile along with a payment request (for example, an authorization, sale, verification, or refund). The following fields are required for profile creation during a payment:

- `consumerProfileInfo.consumerProfileRequestType` set to `CREATE`
- `paymentMethodType.card.accountNumber
`
- `paymentMethod.Type.card.expiry
`
- `accountholder.firstName
`
- `accountholder.lastName `

The following sample `POST` `/payments` request creates a consumer profile:

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
            "version": 1.235
        }
    },
    "paymentMethodType": {
        "card": {
            "accountNumber": "4112344112344113",
            "expiry": {
                "month": 12,
                "year": 2027
            },
            "cvv": 411
        },
        "accountHolder": {
            "referenceId": "1245",
            "firstName": "John",
            "lastName": "Smith",
            "email": "example@example.com",
            "mobile": {
                "countryCode": 1,
                "phoneNumber": "1234567890"
            },
            "consumerProfileInfo": {
                "consumerProfileRequestType": "CREATE"
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
    "accountHolder": {
        "consumerProfileInfo": {
            "consumerProfileRequestType": "CREATE",
            "consumerProfileId": "EGQSGO",
            "paymentMethodId": "ce8dc322-0dde-4a8e-a6ca-b2c02eb37bd6"
        }
    }
}
```

When the profile create action is unsuccessful during a payment, the following fields are sent in the response:

- **Profile creation error code** — `consumerProfileInfo.consumerProfileResponseCode`
- **Profile creation error message** — `consumerProfileInfo.consumerProfileResponseMessage`

## Use a profile for a payment

Use payment credentials stored in a consumer profile for a payment (for example, an authorization, sale, verification, or refund). The following fields are required to use a profile for a payment:

- `paymentMethodType.consumerProfile.consumerProfileId`

- `paymentMethodType.consumerProfile.paymentMethodId`

The following sample `POST` `/payments` request uses a consumer profile for the payment:

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
            "version": 1.23
        }
    },
    "paymentMethodType": {
        "consumerProfile": {
            "consumerProfileId": "EGQSGO",
            "paymentMethodId": "ce8dc322-0dde-4a8e-a6ca-b2c02eb37bd6"
        }
    }
}
```

## Related

[Consumer Profile Management API](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/consumer-profile-management/index)

Is this page helpful?
