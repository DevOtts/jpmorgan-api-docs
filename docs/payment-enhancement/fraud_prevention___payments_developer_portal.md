---
title: "fraud prevention"
description: "Guide for payment processing and authorization"
category: "Payments"
---


Online Payments

# Fraud prevention

Safetech fraud tools are designed to help you retain legitimate purchases while preventing fraudulent transactions by helping you determine whether an accountholder is a fraud risk. You can do this by sending a fraud analysis-only transaction.

## Before you begin

Fraud analysis requests are passed to the Safetech fraud scoring system for risk evaluation and the payment transaction is not sent to the issuer. Safetech fraud evaluates the payment and consumer transaction information and sends you a risk evaluation response where you can then decide if you want to submit a payment transaction request, enhance your current risk programs, or customize your approach to risk management.

Note

The fraud check request is supported for [cards](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-methods/cards) and [ACH](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-methods/ach) payment methods.

## Initiate a fraud check request

Submit a POST call to the `/fraudcheck` endpoint to initiate a fraud analysis request. Customize your request by including your choice of request attributes and the associated data values you want evaluated in the fraud score. For additional information on creating and sending a fraud check request, learn [how to obtain a fraud score](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/fraud-scores).

Tip

For additional information regarding use of page encryption with our Fraud Check service, refer to the [Encryption](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/encryption) page.

For additional information regarding J.P. Morgan-issued Safetech acquirer tokens with our Fraud Check service, refer to the [Tokenization](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/tokenization) page.

The following table lists request attributes and data requirements:

| Field | Description |
| --- | --- |
| cardholderBrowserInformation | Consumer's HTTP browser type |
| isFraudRuleReturn | Indicates if the triggered fraud checking rules are returned. Valid values include the following:<br> <br>- **True**— Indicates that rules should be returned<br>- **False**— Indicates that rules should not be returned<br>- <Blank> — Indicates that rules are not returned |
| aNITelephoneNumber | Phone number provided by a customer during a transaction with a merchant.<br>**Notes**:<br>- This field should be sent when the transaction is conducted over the phone<br>- If a customer automatic number identification (ANI) is unsupported by the call center, enter **0123456789** into this field. |
| fraudCheckShoppingCart | This field can be populated with user-defined values, shopping cart data, or both. Supports up to **999** characters.<br>There can be multiple strings sent in a single record.<br>**Notes**:<br>- Each item must be separated by a “pipe” (\|) character<br>  - The “pipe” (\|) character cannot be used as part of the data.<br>- To embed ampersands (&), equal signs (=), and/or “pipe” (\|) characters within the data itself, the data provided must be Uniform Resource Identifier (URI) encoded. For example:<br>  - An ampersand (&) is **%26**<br>  - A “pipe” (\|) character is **%7C**<br>    <br>  - An equal sign (=) is **%3D**<br>- If user-defined data must be previously-defined (e.g., **UPROMOCODE=40&\|**):<br>   <br>  - **U** — Constant<br>  - **PROMOCODE**— The previously-defined label<br>  - **40** — Rule value<br>- If related to a date, the format is **YYYY-MM-DD**<br>- If related to time, the format is **HH:MM:SS**, where **HH** is in 24-hour format.<br>- **&** — Constant<br>- For shopping cart data, the following criteria applies:<br>  - May be URI-encoded<br>  - A “pipe” (\|) character indicates the end of a string<br>  - Must contain five sub-elements, such as in the following example: **T=TV&I=SKU123&D=42 Plasma&Q=1&P=70000&\|**<br>    - **T** — **Type**: In the example above, the type is a television.<br>    - **I**— **Item**: In the example above, the item is the stock keeping unit (SKU)<br>    - **D** — **Description**: In the example above, the description is a plasma television<br>    - **Q** — **Quantity**: This value must be numeric.<br>    - **P** — **Price**: This value must be numeric (two-decimal value implied) |
| sessionId | Merchant-generated fraud session ID<br>**Notes**:<br>- Should be unique for a period of 30 days<br>- Only able to support upper- and lower-cased alphabetical (A-Z, a-z) or numeric (0-9) values<br>- Supports a maximum of 32 characters |
| fencibleItemAmount | Total fencible values of goods sold (two-decimal value implied). |
| websiteRootDomainName | Short name for the merchant’s website |
| shippingDescription | Method of shipment selected for an item. Valid values include the following:<br>- **C** — Lowest cost<br>- **D** — Carrier designated by customer<br>- **E** — Electronic delivery\* <br>- **G** — Ground\*<br>- **I** — International<br>- **M** — Military<br>- **N** — Next day/overnight\*<br>- **O** — Other/standard<br>- **P** — Store pickup\*<br>- **S** — Same day\*<br>- **T** — Two-day service\*<br>**Note**: Values marked with an asterisk (\*) pertain to American Express. |

Fraud check request field definitions


## Receiving fraud check response

After a fraud analysis-only request was evaluated using the requested scoring criteria, the risk decision and elements object are returned to you in the response data. You can use the response information to enhance current risk programs or to customize your approach to risk management.

If a high fraud score is returned, you are solely responsible for evaluating the `riskDecision` before proceeding with a transaction.

The following table defines the attributes listed in a fraud check response:

| Field | Description |
| --- | --- |
| fraudRiskScore | Fraud scoring is based on various scoring criteria, and is a numerical representation of the relative risk of each transaction screened. |
| fraudRuleAction | The decision response code for a given rule based on the rules configuration. Valid values include the following:<br>- **A** — Approve<br>- **D** — Decline<br>- **E** — Manager review<br>- **R** — Review<br>- <blank> — No decision response code is returned |
| digitalAlertRuleName | Provides a comma-delimited list of the rules triggered<br>**Notes**: <br>- The first four positions in the list are the number of rules returned, followed by an equal (=) sign<br>- If more rules are triggered than can fit in the record, the last character in this field is a plus (+) sign<br>- If no rules are triggered, **0000=** is returned |
| fraudStatus | A unique code that indicates the fraud status of a transaction (e.g., **Y001**) |
| fraudStatusDescription | Description of the fraud status |
| fraudRiskResponse | A fraud status is returned for every fraud inquiry once the transaction is approved or declined. The first character type of **fraudStatus** is one of the following:<br>- **X** — Pre-authorization check<br>- **Y** — Post-authorization check |
| fraudEvaluatorTransactionId | Unique ID used to identify fraud assessment |
| highestRiskCountry | Related country with highest probability of fraud.<br>**Note**: This is the two-character country code, as defined in the International Organization for Standardization (ISO) 3166. |
| highestRiskRegion | Estimated region of a consumer. Examples:<br> <br>- **ca**— California<br>- **CA**— Canada<br>**Note**: If the region is returned as lowercase, it is a state or province. If the region is returned as uppercase, it is a country. |
| cardType | Payment method brand identified during Safetech fraud scoring. Valid values include the following:<br>- **AMEX** — American Express<br>- **AUBC** — Australian Bankcard<br>- **CHEK** — Check<br>- **CHIN** — China Pay<br>- **DISC** — Discover<br>- **DNRS** — Diner’s Club<br>- **JCB** — Japan Credit Bureau (JCB) International<br>- **MSRO** — Maestro<br>- **MSTR** — Mastercard<br>- **VISA** — Visa<br>- **VISE** — Visa Electron<br>- **UNKN** — Other<br>- <Blank> — May be returned |
| fourteenDaysTransactionCount | Total number of prior sales by this consumer in the last 14 days |
| sixHoursTransactionCount | Total number of prior sales by this consumer in any six hour window over the last 14 days |
| fourteenDaysCardRecordCount | Number of cards associated with transaction that the fraud system has recorded |
| fourteenDaysDeviceRecordCount | Number of devices associated with the transaction that the fraud system has recorded |
| fourteenDaysEmailRecordCount | Number of emails associated with the transaction that the fraud system has recorded |
| deviceLayers | The device layers are as follows:<br>- **Operating system** — The network, operating system, and secure sockets layer (SSL) return definite information<br>- **Flash settings** — Queries the device to determine if it is Flash-capable. If so, device data is gathered via Flash.<br>- **JavaScript settings** — Queries the device to determine if it is JavaScript capable. If so, device data is gathered via JavaScript.<br>- **Cookie settings** — Queries HTTP headers for a number of device-associated data.<br>- **Browser** — Queries the high-level browser to collect information about the browser, such as the user-agent string and browser type.<br>**Note**: Each layer is delimited by periods (e.g., **329B0D35C3.41111A0B4A**) |
| sessionMatchIndicator | Indicates a corresponding Kaptcha record. Valid values include the following:<br>- **Y** — Yes<br>- **N** — No<br>- <Blank> — May be returned |
| hashedDigitalDeviceFingerprintIdentifier | Unique fingerprint of the device used to place an order |
| deviceTimestamp | Local date/time setting of the device in **YYYY-MM-DD+HH:MM** format |
| deviceLocalTimeZone | Time zone of the device. The value listed represents the number of minutes from Greenwich Meantime. Divide by 60 to get number of hours |
| deviceRegion | The region or state where the consumer's device resides. Examples:<br>- **ca** — California<br>- **CA** — Canada<br>**Note**: If the region is returned as lowercase, it is a state or province. If the region is returned as uppercase, it is a country. |
| deviceCountry | Country where the consumer’s device resides.<br>**Note**: This is the two-character country code, as defined in the **ISO 3166** standard. |
| digitalDeviceType | Device placing the order of a mobile nature (e.g., iPhone™, Android™, Blackberry™, iPad™, etc.) |
| deviceNetworkType | Network type associated with consumer Valid values include the following:<br>- **A** — Anonymous<br>- **H** — High School<br>- **L** — Library<br>- **N** — Normal<br>- **O** — Open Proxy<br>- **P** — Prison |
| deviceProxyServer | Indicates if the device is using a proxy. Valid values include the following:<br>- **Y** — Yes <br>- **N** — No <br>- <Blank> — May be returned |
| browserJavaScriptEnabled | Indicates if the device’s browser allows JavaScript. Valid values include the following:<br>- **Y** — Yes <br>- **N** — No <br>- <Blank> — May be returned |
| browserAdobeFlashEnabled | Indicates if the device’s browser allows Flash. Valid values include the following:<br>- **Y** — Yes <br>- **N** — No <br>- <Blank> — May be returned |
| browserCookiesEnabled | Indicates if the device’s browser allows cookies. Valid values include the following:<br>- **Y** — Yes <br>- **N** — No <br>- <Blank> — May be returned |
| deviceBrowserCountry | Country where the browser resides<br>**Note**: This is a two-character country code, as defined in the **ISO 3166** standard. |
| deviceBrowserLanguage | Indicates the language of the browser<br>**Note**: This is a two character language code, as defined in the **ISO 639-1** standard. |
| mobileDevice | Indicates if the device is a mobile device. Valid values include the following:<br>- **Y** — Yes <br>- **N** — No <br>- <Blank> — May be returned |
| deviceWirelessCapability | Indicates if the device has wireless capabilities. Valid values include the following:<br>- **Y** — Yes <br>- **N** — No <br>- <Blank> — May be returned |
| deviceVoiceControlled | Indicates if the device is voice controlled. Valid values include the following:<br>- **Y** — Yes <br>- **N** — No <br>- <Blank> — May be returned |
| deviceRemotelyControlCapability | Indicates if the device is a remotely controlled computer. Valid values include the following:<br>- **Y** — Yes <br>- **N** — No <br>- <Blank> — May be returned |

Fraud score response attributes


## Fraud status codes

The Safetech service returns a response code for approved or declined requests. This is returned in the `fraudStatus` element of the response message.

The first character of the `fraudStatus` element can be used to identify the type of response returned from the Safetech service. The following table lists the possible fraud status codes and their meanings:

| Fraud status code | Description |
| --- | --- |
| A000 | Fraud score is successful |
| K201 | Version number is missing. Internal to Merchant Services. |
| K202 | Mode is missing |
| K203 | Merchant ID is missing |
| K204 | Session ID is missing |
| K205 | Risk inquiry service (RIS) transaction ID is missing |
| K211 | Currency code is missing |
| K212 | Total authorization amount is missing |
| K221 | Email address is missing |
| K222 | Phone number is missing |
| K223 | Website ID is missing |
| K231 | Payment type is missing |
| K232 | Payment type of card is missing |
| K233 | Payment type of magnetic ink character recognition (MICR) is missing |
| K235 | Payment token (amount) is missing |
| K241 | Customer internet protocol (IP) address is missing |
| K251 | Merchant acknowledgement flag is missing |
| K261 | POST is missing |
| K271 | Product type code is missing |
| K272 | Product item code is missing |
| K273 | Product description is missing |
| K274 | Product quantity is missing |
| K275 | Product price is missing |
| K301 | Version number is invalid |
| K302 | Mode is invalid |
| K303 | Merchant ID is invalid |
| K304 | Session ID is invalid |
| K305 | RIS transaction ID is invalid |
| K311 | Currency code is invalid |
| K312 | Total authorization amount is invalid |
| K321 | Consumer’s email address is invalid |
| K322 | Consumer’s phone number is invalid |
| K323 | Website ID is invalid |
| K324 | RIS response format is invalid |
| K331 | Transaction type is invalid |
| K332 | Card used as payment is invalid |
| K333 | Payment type of MICR is invalid |
| K335 | Google checkout account ID is invalid |
| K341 | Consumer IP address is invalid |
| K351 | Merchant acknowledgement flag is invalid |
| K362 | Shopping cart data is invalid |
| K371 | Product type code is invalid |
| K372 | Product item code is invalid |
| K373 | Product description is invalid |
| K374 | Product quantity is invalid |
| K375 | Product price is invalid |
| K399 | Label either does not exist in the agent web console (AWC), or was associated with the wrong data type |
| K401 | Extra data was included in the transaction |
| K402 | Payment types were mismatched |
| K403 | Customer phone number was sent, but was unnecessary |
| K404 | Payment token was sent, but was unnecessary |
| K501 | Scoring request was sent, but was not authorized |
| K502 | Merchant ID was sent, but was not authorized |
| K503 | IP address was sent, but was not authorized |
| K504 | A time-out of fraud detection service occurred |
| K601 | System error has occurred |
| K602 | Submission of data that appears to be a security breach. This may come from a bad IP, email address, etc. |
| K701 | Header is missing from transaction |
| T998 | Internal server error where the fraud system is unreachable |
| T999 | Fraud system is unreachable |
| X001 | Merchant is not enabled for Safetech fraud scoring |
| X002 | Method of payment is unsupported for Safetech scoring |
| X003 | Action code is unsupported for Safetech fraud scoring |
| X004 | Transaction type is unsupported for Safetech fraud scoring |
| X005 | Safetech merchant ID is not sent on transaction |
| X006 | Safetech merchant ID supplied does not match the division setup on file |
| X008 | Invalid shopping cart data. RIS inquiry was not attempted. |
| X009 | Invalid user-defined field data. RIS inquiry was not attempted |
| Y001 | Authorization timed out. RIS inquiry not attempted. |

Fraud status codes


## Related

[Encryption](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/encryption)

[How to obtain a fraud score](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/fraud-scores)

Is this page helpful?

API reference

1. On this page
2. [Before you begin](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/fraud-prevention#before-you-begin)
3. [Initiate a fraud check request](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/fraud-prevention#initiate-a-fraud-check-request)
4. [Receiving fraud check response](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/fraud-prevention#receiving-fraud-check-response)
5. [Fraud status codes](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/fraud-prevention#fraud-status-codes)
6. [Related](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/fraud-prevention#related)
