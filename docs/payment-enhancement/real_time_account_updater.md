---
title: "real time account updater"
description: "J.P. Morgan payment processing documentation"
category: "Documentation"
---


# Real-time account updater

The Real-time Account Updater (RTAU) is an optional service which updates account information for Visa cards as part of the payment processing flow. It enables [recurring payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/recurring-payments) or [stored payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/stored-payments) transactions to use updated account information that may have changed due to card expirations or reissues.

## Before you begin

You must enroll for Visa RTAU during the onboarding process. Or to add the service, reach out to your relationship manager. Once this is complete, the following transaction eligibility requirements must also be met:

- Must be a card-not-present transaction.
- The transaction must be a merchant-initiated [recurring payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/recurring-payments) or cardholder-initiated [stored payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/stored-payments) transaction.

- The merchant in the payment request must not have the merchant category codes **5962**, **5966**, or **5967**.
- The transaction must be a Visa transaction.

## Payment request

Once enrolled in the RTAU service, transactions meeting the above eligibility requirements will be enabled for the service. Enrolled merchants should still include the **requestAccountUpdater** field with the field value of **true** or **null** when submitting a POST request to the **/payments** endpoint for a Visa transaction.

The following example shows a Visa transaction using RTAU.

**HTTP method**: POST

**Endpoint**: /payments

Json
Copy

```json
{
    "paymentMethodType": {
        "card": {
            "accountNumber": "4012001037141112",
            "expiry": {
                "month": 5,
                "year": 2025
            },
            "accountUpdater": {
                "requestAccountUpdater": true
            }
        }
    }
}
```

The following example shows a Visa transaction bypassing RTAU.

To bypass the RTAU service for an eligible transaction, the **requestAccountUpdater** field value should be **false** for an enrolled merchant.

**HTTP method:** POST

**Endpoint:** /payments

Json
Copy

```json
{
    "paymentMethodType": {
        "card": {
            "accountNumber": "4012001037141112",
            "expiry": {
                "month": 5,
                "year": 2025
            },
            "accountUpdater": {
                "requestAccountUpdater": false
            }
        }
    }
}
```

## Payment response

In the payment response, there are two objects that contain RTAU response data:

- **accountUpdater** — Displays the new account or expiration date, when available, as well as fields that explain the changes that have taken place on the account.

- **networkAccountUpdater** — Displays the codes and values provided by the payment networks.


The following are examples of possible response scenarios:

**Scenario:** A valid card with no updates

Json
Copy

```json
{
    "accountUpdater": {
        "requestAccountUpdater": true,
        "accountUpdaterResponse": "MATCH_NO_UPDATE",
        "accountUpdaterReasonCode": "SUCCESS",
        "accountUpdaterReasonMessage": "Valid card no update available"
    },
    "networkAccountUpdater": {
        "replacementCode": false,
        "networkResponseCode": "VAU012"
    }
}
```

**Scenario:** A card updated with a new account number

Json
Copy

```json
{
    "accountUpdater": {
        "requestAccountUpdater": true,
        "accountNumber": "4012001037141112",
        "accountUpdaterResponse": "NEW_ACCOUNT",
        "accountUpdaterReasonCode": "SUCCESS",
        "accountUpdaterReasonMessage": "Account Update provided for Account Number"
    },
    "networkAccountUpdater": {
        "accountStatus": "A",
        "replacementCode": true
    }
}
```

**Scenario:** A card updated with a new account number and expiry date

Json
Copy

```json
{
    "accountUpdater": {
        "requestAccountUpdater": true,
        "accountNumber": "4012001037141112",
        "newAccountExpiry": {
            "month": 12,
            "year": 2025
        },
        "accountUpdaterResponse": "NEW_ACCOUNT_AND_EXPIRY",
        "accountUpdaterReasonCode": "SUCCESS",
        "accountUpdaterReasonMessage": "Account Update provided for both account number and expiry"
    },
    "networkAccountUpdater": {
        "accountStatus": "A",
        "replacementCode": true
    }
}
```

**Scenario:** The requested account was closed

Json
Copy

```json
{
    "accountUpdater": {
        "requestAccountUpdater": true,
        "accountUpdaterResponse": "CLOSED_ACCOUNT",
        "accountUpdaterReasonCode": "SUCCESS",
        "accountUpdaterReasonMessage": "Account has been closed"
    },
    "networkAccountUpdater": {
        "accountStatus": "C",
        "replacementCode": false
    }
}
```

**Scenario:** A card updated with a new expiration date

Json
Copy

```json
{
    "accountUpdater": {
        "requestAccountUpdater": true,
        "accountUpdaterResponse": "NEW_EXPIRY",
        "accountUpdaterReasonCode": "SUCCESS",
        "accountUpdaterReasonMessage": "Account Update provided for Account Expiry"
    },
    "networkAccountUpdater": {
        "accountStatus": "E",
        "replacementCode": true
    }
}
```

**Scenario:** A card updated with a new contact for the cardholder

Json
Copy

```json
{
    "accountUpdater": {
        "requestAccountUpdater": true,
        "accountUpdaterResponse": "CONTACT_CARDHOLDER",
        "accountUpdaterReasonCode": "SUCCESS",
        "accountUpdaterReasonMessage": "Contact Cardholder"
    },
    "networkAccountUpdater": {
        "accountStatus": "Q",
        "replacementCode": false
    }
}
```

**Scenario:** There was no match: however, the card issuer supports RTAU

Json
Copy

```json
{
    "accountUpdater": {
        "requestAccountUpdater": true,
        "accountUpdaterResponse": "NO_MATCH_PARTICIPATING_BIN",
        "accountUpdaterReasonCode": "SUCCESS"
    }
}
```

**Scenario:** The card issuer does not support RTAU

Json
Copy

```json
{
    "accountUpdater": {
        "requestAccountUpdater": true,
        "accountUpdaterResponse": "NO_MATCH_NON_PARTICIPATING_BIN",
        "accountUpdaterReasonCode": "ERROR",
        "accountUpdaterReasonMessage": "Issuer does not support real time AU"
    },
    "networkAccountUpdater": {
        "replacementCode": false,
        "networkResponseCode": "VAU009"
    }
}
```

## Related

[Recurring payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/recurring-payments)

[Stored payments](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/stored-payments)

Is this page helpful?
