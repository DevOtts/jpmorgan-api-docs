---
title: "direct pay"
description: "Guide for payment processing and authorization"
category: "Payments"
---


# Direct Pay

Direct Pay is a fast and secure payment service that allows your consumers access to real-time person-to-person (P2P) payments, business-to-consumer (B2C) payments, funds disbursements, insurance claim payouts, and other funds transfer solutions using the existing Visa and Mastercard transaction processing infrastructure.

Rather than using traditional methods such as a check or money order, with Direct Pay, you provide a service to your consumers to “Pull” and “Push” money from and to another consumer or business. No goods or services are involved.

## Before you begin

Direct Pay is an enhancement to Visa and Mastercard card transactions and requires additional setup by J.P. Morgan to activate this feature.

This card-based funds transfer service is only available for use by specific types of merchants (that is, merchant category codes (MCC)) for specific purposes (defined by the funds transfer type) which differ by payment brand.

There are two types of Direct Pay transactions:

1. **Pull funds** — Debit or take funds from a funding source.

2. **Push funds** — Credit or give funds to a receiving target account.

Tip

Implement pull and push funds transfers independently or in combination, depending on your use case. For example, possible scenarios include: push funds for a payroll disbursement, pull funds for a prepaid card top-up, or pull and push funds for a person to person transfer.

## MCC and funds transfer type requirements

The following table lists the supported MCC per funds transfer type for Visa:

| directPaymentType | fundsTransferType | Supported MCC |
| --- | --- | --- |
| PULL\_FUNDS | ACCOUNT\_TO\_ACCOUNT | 4829, 6012, 6211, 8220 |
| PULL\_FUNDS | FUNDS\_TRANSFER | 4829, 6012, 6051, 6540, 7995, 8220 |
| PULL\_FUNDS | PAYROLL\_DISBURSEMENT | 4829, 6540, 8931 |
| PULL\_FUNDS | PERSON\_TO\_PERSON | 4829, 6012 |
| PULL\_FUNDS | PREPAID\_CARD | 4829, 6012, 6051, 6540 |
| PULL\_FUNDS | WALLET\_TRANSFER | 4829, 6012, 6051,6540, 7995 |
| PUSH\_FUNDS | ACCOUNT\_TO\_ACCOUNT | 4829, 6012, 6211, 8220 |
| PUSH\_FUNDS | FUNDS\_DISBURSEMENT<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | All |
| PUSH\_FUNDS | FUNDS\_TRANSFER | 4829, 6012, 6051, 6540, 8220 |
| PUSH\_FUNDS | GAMBLING\_PAY<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | 7800, 7801, 7802, 7995, 9406 |
| PUSH\_FUNDS | GOVERMENT\_DISBURSEMENT<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | 9211, 9222, 9311, 9399, 9402, 9405 |
| PUSH\_FUNDS | LOYALTY\_PAY<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | All |
| PUSH\_FUNDS | MERCHANT\_DISBURSEMENT<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | 4829, 6012 |
| PUSH\_FUNDS | ONLINE\_GAMBLING\_PAY<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | 7800, 7801, 7802, 7995, 9406 |
| PUSH\_FUNDS | PAYROLL\_DISBURSEMENT<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | 8931 |
| PUSH\_FUNDS | PERSON\_TO\_PERSON | 4829, 6012 |
| PUSH\_FUNDS | PREPAID\_CARD | 6012, 6051, 6540 |
| PUSH\_FUNDS | WALLET\_TRANSFER | 4829, 6012, 6051 |

Visa supported MCC per funds transfer type


The following table lists supported merchant and funds transfer types for Mastercard:

| directPaymentType | fundsTransferType | Supported MCCs |
| --- | --- | --- |
| PULL\_FUNDS | ACCOUNT\_TO\_ACCOUNT | 4829 |
| PULL\_FUNDS | FUNDS\_TRANSFER | 4829 |
| PULL\_FUNDS | PAYROLL\_DISBURSEMENT | 4829 |
| PULL\_FUNDS | PERSON\_TO\_PERSON | 4829 |
| PULL\_FUNDS | PREPAID\_CARD | 4829 |
| PULL\_FUNDS | WALLET\_TRANSFER | 4829, 6540 |
| PUSH\_FUNDS | ACCOUNT\_TO\_ACCOUNT | 4829, 6211, 8220 |
| PUSH\_FUNDS | FUNDS\_DISBURSEMENT<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | All |
| PUSH\_FUNDS | FUNDS\_TRANSFER | 4829, 6012, 6051, 6540, 8220 |
| PUSH\_FUNDS | GAMBLING\_PAY<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | 7800, 7801, 7802, 7995, 9406 |
| PUSH\_FUNDS | GOVERMENT\_DISBURSEMENT<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | 9211, 9222, 9311, 9399, 9402, 9405 |
| PUSH\_FUNDS | LOYALTY\_PAY<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | All |
| PUSH\_FUNDS | MERCHANT\_DISBURSEMENT<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | 4829, 6012 |
| PUSH\_FUNDS | ONLINE\_GAMBLING\_PAY<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | 7800, 7801, 7802, 7995, 9406 |
| PUSH\_FUNDS | PAYROLL\_DISBURSEMENT<br>**Note**: Requires `DEBIT_DEPOSIT_ACCOUNT` as the  `fundingSource` | 8931 |
| PUSH\_FUNDS | PERSON\_TO\_PERSON | 4829 |
| PUSH\_FUNDS | PREPAID\_CARD | 6012, 6051, 6540 |
| PUSH\_FUNDS | WALLET\_TRANSFER | 4829, 6012, 6051 |

Mastercard supported merchant and funds transfer types


## Required fields for Direct Pay

### Pull funds

- `fundsTransferType` — Valid values:

  - `ACCOUNT_TO_ACCOUNT`
  - `FUNDS_TRANSFER`
  - `PAYROLL_DISBURSEMENT`
  - `PERSON_TO_PERSON`
  - `PREPAID_CARD`
  - `WALLET_TRANSFER`
- `directPaymentType` — Valid value: `PULL_FUNDS`


### Push funds

- `fundsTransferType` — Valid values:

  - `ACCOUNT_TO_ACCOUNT`
  - `FUNDS_DISBURSEMENT`
  - `FUNDS_TRANSFER`
  - `GAMBLING_PAY`
  - `GOVERNMENT_DISBURSEMENT`
  - `LOYALTY_PAY`
  - `MERCHANT_DISBURSEMENT`
  - `ONLINE_GAMBLING_PAY`
  - `PAYROLL_DISBURSEMENT`
  - `PERSON_TO_PERSON`
  - `PREPAID_CARD`
  - `WALLET_TRANSFER`
- `directPaymentType` — Valid value: `PUSH_FUNDS`
- `transactionReferenceNumber` — Your assigned transaction identifier.
- `accountNumber` (required when payment method is Mastercard) — The funding source account identifier (for example, a device primary account number (DPAN), a token, or the actual account number).
- `fundingSource` — Valid values:

  - `CASH`
  - `CREDIT_ACCOUNT`
  - `CREDIT_CARD`
  - `DEBIT_CARD`
  - `DEBIT_DEPOSIT_ACCOUNT`
  - `MOBILE_MONEY`
  - `PREPAID_CARD`

The following table lists fields available for use with Direct Pay transactions:

| Attribute | Direct Pay type | Required (Y/N) | Definition |
| --- | --- | --- | --- |
| fundsTransferType | Both | Y | Codifies the intended use of a payment using direct pay by the merchant. It determines the data carried in the message, the limits and economics that may apply to the transaction and may be used by the sending and/or receiving issuer to make an authorization decision.<br>Pull funds values: \["ACCOUNT\_TO\_ACCOUNT", “FUNDS\_TRANSFER", "PAYROLL\_DISBURSEMENT", "PERSON\_TO\_PERSON", "PREPAID\_CARD", "WALLET\_TRANSFER"\]<br>Push funds values: \["ACCOUNT\_TO\_ACCOUNT", "FUNDS\_DISBURSEMENT", "FUNDS\_TRANSFER", "GAMBLING\_PAY", "GOVERNMENT\_DISBURSEMENT, "LOYALTY\_PAY", "MERCHANT\_DISBURSEMENT", "ONLINE\_GAMBLING\_PAY", "PAYROLL\_DISBURSEMENT", "PERSON\_TO\_PERSON", "PREPAID\_CARD", "WALLET\_TRANSFER"\] |
| transactionReason | Both | N | Codifies the purpose of the payment based on the standard values defined for the respective market. \["ACCOUNT\_MANAGEMENT", "BONUS\_PAYMENT", "BUS\_TRANSPORT", "BUSINESS\_EXPENSE", "CAR\_INSURANCE", "CASH\_MANAGEMENT\_TRANSFER", "CC\_REIMBURSEMENT", "CHARITY", "COLLECTION", "COMMISSION", "COMPENSATION", "COPYRIGHT", "CREDIT\_CARD\_PAYMENT", "DEBIT\_CARD", "DEBIT\_REIMBURSEMENT", "DEPOSIT", "DIVIDEND", "ELECTRICITY\_BILL", "ENERGIES", "FERRY", "FOREIGN\_EXCHANGE", "GAS\_BILL", "GENERAL\_FEE", "GOVERNMENT\_CASH\_COMP", "GOVERNMENT\_PAYMENT", "HEALTH\_INSURANCE", "INSOLVENCY", "INSTALLMENT", "INSURANCE\_CLAIM", "INSURANCE\_PREMIUM", "INTEREST", "INTRA\_COMPANY", "INVESTMENT", "LABOR\_INSURANCE", "LICENCE\_FEE", "LIFE\_INSURANCE", "LOAN", "MEDICAL\_SERVICE", "MOBILE\_P2B", "MOBILE\_P2P", "MOBILE\_TOPUP", "MUTUAL\_FUND\_INVESTMENT", "NOT\_OTHERWISE", "OTHER\_TEL\_BILL", "OTHERS", "PAYMENT\_ALLOWANCE", "PAYROLL", "PENSION\_FUND", "PENSION\_PAYMENT", "PROPERTY\_INSURANCE", "RAIL\_TRANSPORT", "REFUND\_TAX", "RENT", "RENTAL\_LEASE", "ROYALTY", "SALARY", "SAVING\_RETIREMENT", "SECURITIES", "SETTLEMENT\_ANNUITY", "SOCIAL\_SECURITY", "STUDY", "STUDY\_TUITION", "SUBSCRIPTION", "SUPPLIER", "TAX", "TAX\_INCOME", "TEL\_BILL", "TELEPHONE\_BILL", "TRADE\_SERVICE", "TRAVEL", "TREASURY", "UNEMPLOYMENT\_DISABILITY\_BENEFIT", "UTILITY\_BILL", "VAT", "WATER\_BILL", "WITH\_HOLD"\] |
| directPaymentType | Both | Y | Indicator that specifies whether you want to pull funds from a funding source or push funds to a receiving target.<br>Pull funds value: \["PULL\_FUNDS"\]<br>Push funds value: \["PUSH\_FUNDS"\] |
| serviceFeeAmount | Pull funds only | N | Optional service fee charged by merchant for use of service. |
| currencyConversionFeeAmount | Pull funds only | N | Optional currency conversion fee amount charged by merchant. |

Fields applicable to Direct Pay transactions


| Attribute | Direct Pay type | Required (Y/N) | Definition |
| --- | --- | --- | --- |
| transactionReferenceNumber | Both | Pull — N<br>Push — Y | Identifies a transaction as assigned by a third-party such as the payment gateway, partner bank, facilitator, aggregator, etc. |
| accountNumber | Push funds only | Y (When payment method is Mastercard) | The number assigned to a monetary instrument (such as a card, direct debit account \[DDA\] or other payment account identifier provided for alternative method of payment or local payment solution) sent to the merchant acquirer to facilitate payment for the exchange of goods and services in a financial transaction. These payment account identifiers can be a secure placeholder such as a device primary account number \[DPAN\] or a token or the actual account identifier. |
| taxId | Both | N | An identifier assigned by a government agency that is used by a Tax Authority to administer tax laws or by another government body to administer social and government programs. |
| taxIdType | Both | N | A code that classifies the type of Tax Government Identifier \["SSN", "ITIN", "EIN"\]. |
| line1 | Both | N | First line of street address |
| line2 | Both | N | Second line of street address |
| city | Both | N | A portion of a party's address which is the geographic area that is a municipality with legal power granted by a state/province charter. |
| state | Both | N | Classifies a geographic area that represents a first level, legal and political subdivision of a country as defined in ISO 3166-2 specification (for example, CA, TX, FL). |
| postalCode | Both | N | The postal code of the address |
| countryCode | Both | N | The country code of the address based on Alpha-3 ISO standards |
| addressTypeCode | Both | N | Codifies the classification given to the various addresses captured for a sender, such as Doing Business As Address, Legal Entity Address, and Tax Address. |
| fundingSource | Both | Pull — N<br>Push — Y | Identifies the source of funds. \["CASH", "CREDIT\_ACCOUNT", "CREDIT\_CARD", "DEBIT\_CARD", "DEBIT\_DEPOSIT\_ACCOUNT", "MOBILE\_MONEY", "PREPAID\_CARD"\] |

Fields applicable to Direct Pay transactions — sender information


The following is an example of a Direct Pay pull funds transaction:

**HTTP method**: `POST`

**Endpoint**: `/payments`

**Scenario**: Direct Pay pull funds request

Json
Copy

```json
{
    "directPay": {
        "fundsTransferType": "ACCOUNT_TO_ACCOUNT",
        "transactionReason": "CREDIT_CARD_PAYMENT",
        "directPaymentType": "PULL_FUNDS",
        "serviceFeeAmount": 123,
        "sender": {
            "transactionReferenceNumber": "1234567890123456",
            "accountNumber": "5214XXXXXX94",
            "fundingSource": "CREDIT_CARD",
            "taxId": "123456789",
            "taxIdType": "SSN",
            "firstName": "John",
            "middleName": "D",
            "lastName": "Doe",
            "address": {
                "line1": "1234 Main Street",
                "line2": "",
                "city": "BEDFORD",
                "state": "NH",
                "countryCode": "USA",
                "postalCode": "03109"
            }
        },
        "birthDate": "1975-06-15"
    }
}
```

The following is an example of a Direct Pay push funds transaction:

**HTTP method**: `POST`

**Endpoint**: `/payments`

**Scenario**: Direct Pay push funds request

Json
Copy

```json
{
    "directPay": {
        "fundsTransferType": "ACCOUNT_TO_ACCOUNT",
        "transactionReason": "CREDIT_CARD_PAYMENT",
        "directPaymentType": "PUSH_FUNDS",
        "sender": {
            "transactionReferenceNumber": "1234567890123456",
            "accountNumber": "5214XXXXXX94",
            "fundingSource": "DEBIT_CARD",
            "taxId": "123456789",
            "taxIdType": "SSN",
            "firstName": "John",
            "middleName": "D",
            "lastName": "Doe",
            "address": {
                "line1": "1234 Main Street",
                "line2": "",
                "city": "BEDFORD",
                "state": "NH",
                "countryCode": "USA",
                "postalCode": "03109"
            },
            "birthDate": "1975-06-15"
        }
    }
}
```

## Transaction limits

### Pull funds

Refer to the following table for transaction limits on a Direct Pay pull funds transaction.

| fundsTransferType | Visa (US and Canada) | Mastercard (US and Canada) |
| --- | --- | --- |
| `ACCOUNT_TO_ACCOUNT` | $50,000 | US: $50,000<br>Canada: $25,000 |
| `FUNDS_TRANSFER` | $50,000 | US: $50,000<br>Canada: $25,000 |
| `PAYROLL_DISBURSEMENT` | $125,000 | $125,000 |
| `PERSON_TO_PERSON` | $50,000 | $10,000 |
| `PREPAID_CARD` | $50,000 | $25,000 |
| `WALLET_TRANSFER` | $50,000 | $25,000 |

Pull funds transaction limits for Visa and Mastercard


### Push funds

Refer to the following table for transaction limits on a Direct Pay push funds transaction.

| fundsTransferType | Visa (US and Canada) | Mastercard (US and Canada) |
| --- | --- | --- |
| `ACCOUNT_TO_ACCOUNT` | $50,000 | US: $50,000<br>Canada: $25,000 |
| `FUNDS_DISBURSEMENT` | $125,000 | $125,000 |
| `FUNDS_TRANSFER` | $50,000 | US: $50,000<br>Canada: $25,000 |
| `GAMBLING_PAY` | $125,000 | US: $50,000<br>Canada: $10,000 |
| `GOVERNMENT_DISBURSEMENT` | $125,000 | $125,000 |
| `LOYALTY_PAY` | $125,000 | $125,000 |
| `MERCHANT_DISBURSEMENT` | $125,000 | $125,000 |
| `ONLINE_GAMBLING_PAY` | $125,000 | US: $50,000<br>Canada: $10,000 |
| `PAYROLL_DISBURSEMENT` | $125,000 | $125,000 |
| `PERSON_TO_PERSON` | $50,000 | $10,000 |
| `PREPAID_CARD` | $50,000 | $50,000 |
| `WALLET_TRANSFER` | $50,000 | US: $50,000<br>Canada: $25,000 |

Push funds transaction limits for Visa and Mastercard


Is this page helpful?

![JP Morgan Payments Logo](https://cdn.cookielaw.org/logos/543c7d55-271b-4044-98d3-fa9b38ea3028/e1738e55-2e8a-44e0-a6ec-eeb4e24d9e35/03aea8b9-e702-46be-a863-fed9fe734c09/jpm-payments.png)

## Privacy Preference Center

When you visit any website, it may store or retrieve information on your browser, mostly in the form of cookies. This information might be about you, your preferences or your device and is mostly used to make the site work as you expect it to. The information does not usually directly identify you, but it can give you a more personalized web experience. Because we respect your right to privacy, you can choose not to allow some types of cookies. Click on the different category headings to find out more and change our default settings. However, blocking some types of cookies may impact your experience of the site and the services we are able to offer.


[More information](https://cookiepedia.co.uk/giving-consent-to-cookies)

Allow All

### Manage Consent Preferences

#### Strictly Necessary Cookies

Always Active

These cookies allow the technical operation of our websites or apps (e.g., cookies that enable you to navigate a website or app, and to use its features). Some may also increase the usability of our websites or apps by remembering your choices (e.g., language, region, login details, and so on). You may be able to disable some or all necessary cookies by adjusting your browser settings. If you choose to do so, however, you may experience reduced functionality or be prevented from using our websites or apps altogether.

#### Performance Cookies

Performance Cookies

These cookies help us enhance the performance and usability of our websites or apps. If you choose not to accept these cookies you may experience less than optimal performance.

#### Marketing Cookies

Marketing Cookies

These cookies help us evaluate the effectiveness of our marketing campaigns or to provide better targeting for marketing. These cookies may collect personal data such as your name as well as information about how you interact with our websites, apps or marketing materials.

#### Analytics Cookies

Analytics Cookies

These cookies help us ensure that we understand our audience as clearly as possible, and that any information that is provided to you is as relevant as possible to
your interests and preferences.

Back Button

### Cookie List

Search Icon

Filter Icon

Clear

checkbox labellabel

ApplyCancel

ConsentLeg.Interest

checkbox labellabel

checkbox labellabel

checkbox labellabel

Reject AllConfirm My Choices

[![Powered by Onetrust](https://cdn.cookielaw.org/logos/static/powered_by_logo.svg)](https://www.onetrust.com/products/cookie-consent/)
