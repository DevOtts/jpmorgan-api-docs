---
title: "3D Secure Payments"
description: "Implementation guide for 3D Secure authentication"
category: "Authentication"
---

# 3-D Secure

3-D Secure (3DS) is an authentication service that allows you to verify the identity of your cardholder prior to completing the payment authorization or verification. A successful authentication can:

- Reduce fraudulent card transactions
- Shift the cost liability of fraud-related chargebacks to the card issuer

- Help you to meet EU/UK Payment Services Directive 2 (PSD2) Strong Customer Authentication (SCA) requirements

## Who should use 3DS authentication?

In general, you would leverage 3DS authentication if you have consumers in Europe (or another market where 3DS is required), or if you want to reduce your card payments risk liability. We support the following 3DS options on our Commerce platform:

- [Orchestrated 3DS](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/3d-secure#orchestrated-3ds) — J.P. Morgan handles the 3DS authentication flow for you as part of your card payment transaction process.
- [3DS API](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/3-d-secure/index) — Manage the entire 3DS workflow yourself separate from the card payment process.
- [Pass-through 3DS](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/3d-secure#passthrough-3ds) — Pass-through 3DS values obtained from an independent 3DS provider in your payment transactions to J.P. Morgan.

Attention

3DS is supported in the [Online Payments API](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/index) by using J.P. Morgan to [orchestrate the authentication flow](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/3d-secure#orchestrated-3ds), or by [allowing pass-through of 3DS values](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/3d-secure#passthrough-3ds) from an independent authentication provider. Additionally, J.P. Morgan's client managed [3DS API](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/3-d-secure/index) can be accessed and used independently from the Online Payments API for more control over the authentication process.

Note

To begin authenticating cardholders using 3DS, you must first choose and integrate with a 3DS authentication provider. This can be J.P. Morgan or a third-party authentication vendor.

## How 3DS works

![3-D Secure Process Flow](https://developer.payments.jpmorgan.com/api/download/en/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/3d-secure-cfs/3DS_Procflow.png?type=image)

1. Your consumer places an order on your website.
2. If you determine that the purchase warrants authentication, initiate a 3DS authentication with your 3DS authentication provider. Reasons a transaction warrants authentication may include:
1. The authentication is mandated by a regulation like PSD2 in Europe.
2. The cardholder is a new consumer with whom you have no prior business relationship.
3. The item being purchased is a common target for fraudulent purchases.
3. Your 3DS provider passes the authentication request through to the card issuer.
4. The card issuer processes the authentication one of two ways:
1. [Frictionless](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/3-d-secure/index#frictionless-authentications) — The issuer completes the authentication using only information from the authentication API call and device information from the browser.
2. [Challenge](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/3-d-secure/index#challenge-authentications) — The issuer prompts the cardholder to provide additional information to complete the authentication.
5. The issuer returns the authentication results to you via your 3DS authentication provider.
6. If you decide to process a transaction using the [Online Payments API](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/index) for your consumer's order based on a successful authentication response, include the authentication result details in your `/payments` or `/verifications` request.

Tip

Including the successful 3DS authentication results in your authorization transaction shifts the liability for a fraud-related chargeback to the issuer, a payment brand benefit.

Specific regions (such as EU, UK, India, and Australia) have regulations requiring e-commerce merchants to utilize authentication protocols, such as 3DS. Payment network mandates also provide guidance on how and when to utilize 3DS with their specific card products. You are ultimately responsible for knowing whether 3DS is applicable to you.

## Strong Customer Authentication (SCA) exemption

EU merchants may request an SCA exemption to avoid performing 3DS authentication and passing 3DS authentication results in the purchase authorization. To request an SCA exemption, populate the paymentMethodType.card.authentication. `SCAExemptionReason` field with one of the following exemption reasons:

- `TRUSTED_MERCHANT` — Transactions are eligible for SCA exemption when a merchant is "allow" listed by the cardholder.
- `SECURE_CORPORATE_PAYMENT` — Secure corporate or business-to-business (B2B) payments over dedicated payment processes and protocols are exempted from SCA.
- `TRANSACTION_RISK_ANALYSIS` — Transactions are eligible for SCA exemption if transaction fraud rates are below established thresholds defined by PSD2/Regulatory Technical Standards (RTS).
- `LOW_VALUE_PAYMENT` — Transactions are eligible for SCA exemption when the transaction amount is below the limit established by PSD2/RTS.
- `MERCHANT_INITIATED_TRANSACTION` — Transactions processed within the merchant-initiated transaction (MIT) framework are exempt from SCA. The initial transaction must meet SCA requirements.
- `RECURRING_PAYMENT` — Recurring transactions are eligible for SCA exemption. The initial transaction must meet SCA requirements.
- `SCA_DELEGATION` — Transactions are eligible for SCA exemptions when an issuer has delegated authentication responsibility to a third-party wallet provider or to a merchant.

The following table lists permitted card payment methods per exemption reason:

| Exemption reason | Eligible card brands |
| --- | --- |
| TRUSTED_MERCHANT | Visa |
| SECURE_CORPORATE_PAYMENT | Visa, Mastercard, International Maestro, and American Express |
| TRANSACTION_RISK_ANALYSIS | Visa, Mastercard, and International Maestro |
| LOW_VALUE_PAYMENT | Visa, Mastercard, and International Maestro |
| MERCHANT_INITIATED_TRANSACTION | Mastercard and International Maestro |
| RECURRING_PAYMENT | Mastercard and International Maestro |
| SCA_DELEGATION | Visa, Mastercard, and International Maestro |

Permitted card brands per exemption reason


## Orchestrated 3DS

Orchestrated 3DS leverages J.P. Morgan's own 3DS authentication server to perform authentication as part of the payment authorization or verification request.

![Orchestration challenge flow](https://developer.payments.jpmorgan.com/api/download/en/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/3d-secure-cfs/OrchestrationChallengeFlowRevised2.png?type=image)

The orchestrated 3DS Online Payments API requirements, for payments and verifications respectively, are as follows:

- **HTTP method**: `POST`
- **Endpoints**: `/payments` and `/verifications`
- **API field**: "paymentMethodType.card.<payment|verification>AuthenticationRequest.authenticationReturnUrl": "https://merchantreturnurl.com"


Tip

If you do not provide a return URL, J.P. Morgan does not attempt 3DS orchestration and, instead, proceeds directly to authorization.

| Fields |
| --- |
| **Note**: Fields preceded by a single asterisk (\*) are required for 3DS authentication. Fields preceded by a double asterisk (\*\*) are required unless a market or regional mandate restricts the sending of this information. |
| paymentMethodType.card.<payment|verification>AuthenticationRequest (object)<br>**\*** authenticationReturnUrl<br>              authenticationSupportUrl<br>              accountType<br>              authenticationType<br>threeDSRequestorAuthenticationInfo<br>              threeDSAuthenticationTimestamp<br>                            threeDSAuthenticationData<br>                            threeDSChallengeType<br>                            requestorAuthenticationMethod<br>                            \*authenticationPurpose<br>threeDSPurchaseInfo<br>**\*** purchaseDate<br>               recurringAuthorizationDayCount<br>**\*** threeDomainSecureTransactionType<br>               consumerInstallmentAuthorizationCount<br>               recurringAuthorizationExpirationDate<br>               authenticationAmount<br>threeDSAccountAdditionalInfo<br>              consumerAccountCreateLength<br>              consumerAccountCreateTimestamp<br>              consumerAccountUpdateLength<br>              consumerAccountUpdateTimestamp<br>              consumerAccountPasswordUpdateTimestamp<br>              consumerAccountPasswordChangeLength<br>              consumerAccountShippingAddressLength<br>              consumerAccountFirstShippingDate<br>              twentyFourHoursTransactionCount<br>              previousYearTransactionCount<br>              consumerAccount24HoursAddCardCount<br>              sixMonthTransactionCount<br>              consumerAccountSuspiciousActivityIndicator<br>              consumerAccountShipNameIdenticalIndicator<br>              consumerPaymentAccountLength<br>              consumerPaymentAccountEnrollmentDate<br>              consumerAccountAddressIdenticalIndicator<br>threeDSRequestorPriorAuthenticationInfo<br>             priorAcsTransactionId<br>             authenticationMethod<br>             authenticationTimestamp<br>threeDSPurchaseRisk<br>            shipmentType<br>            deliveryTimeframe<br>            orderEmailAddress<br>            productRepurchaseIndicator<br>            productAvailabilityCode<br>            preOrderDate<br>            purchasedPrepaidCardTotalAmount<br>            purchasedPrepaidCardCount<br>            prepaidCardCurrency |
| accountHolder<br>           referenceId<br>           consumerIdCreationDate<br>           phone<br>           countryCode<br>**\*\*** firstName<br>**\***\*lastName<br>**\*\*** email<br>           billingAddress<br>**\*\*** line1<br>                      line2<br>**\*\*** city<br>**\*\*** postalCode<br>                      state<br>                      countryCode<br>                      phone<br>**\*\*** countryCode<br>**\*\*** phoneNumber |
| browserInfo<br>**\*** browserAcceptHeader<br>**\*** deviceIPAddress<br>**\*** browserLanguage<br>**\*** browserColorDepth<br>**\*** browserScreenHeight<br>**\*** browserScreenWidth<br>**\*** deviceLocalTimeZone<br>**\*** browserUserAgent<br>**\*** challengeWindowSize<br>**\*** javaEnabled<br>**\*** javaScriptEnabled |
| shipTo<br>          firstName<br>          lastName<br>          line1<br>          line2<br>          city<br>          postalCode<br>          state<br>          countryCode |

Fields used for orchestrated 3DS


### Initiating orchestrated 3DS

**Step 1: Set up HTML**

The Online Payments API enables the orchestrated authentication process by allowing the browser to create a frame targeted to the `authenticationOrchestrationUrl` attribute..

The following is an HTML sample for creating the iframe:

Plaintext
Copy

```plaintext
<!DOCTYPE html>
 <html lang="en">
 <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <link rel="stylesheet" href="styles.css">
     <title>Merchant Site</title>
 </head>
 <body>
     <iframe
         id="hiddenIframe"
         frameborder="0"
         style="
             display: none;
             width: 800px;
             height: 600px;"
         name="hiddenIframe">
     </iframe>
     <script src="../orchestration.js"></script>
 </body>
 </html>
```

Implement the following function to be called in a later step:

Javascript
Copy

```javascript
// orchestration.js
const iframeRef = document.getElementById("hiddenIframe");

// Triggers orchestration in your front end application
function orchestrationFormSubmit
(iframeRef, orchestrationUrlFromResponse) {
const form = document.createElement("form");
form.action = orchestrationUrlFromResponse

// Takes the query parameters from the orchestrationUrl and applies them to the javascript form that will be submitted

const signedUrl = new URL(form.action)
signedUrl.searchParams.forEach(function(value, key) {
const signatureInformation = document.createElement("input")
signatureInformation.name = key;
signatureInformation.value = value;
form.appendChild(signatureInformation);
})
form.method = "GET";
form.target = iframeRef.name

document.body.appendChild(form);
form.submit();
document.body.removeChild(form);
}
```

**Step 2: Call Online Payments API**

Make your `/payments` or `/verifications` request as normal, including the appropriate 3DS fields. Be sure to collect browser info prior to making the request. Use the following example for a `/payments` request:

Json
Copy

```json
{
    "amount": 14500,
    "currency": "USD",
    "paymentMethodType": {
        "card": {
            "accountNumber": "4112344112344113",
            "expiry": {
                "month": 5,
                "year": 2040
            },
            "paymentAuthenticationRequest": {
                "authenticationReturnUrl": "merchantCheckoutPage.com",
                "threeDSRequestorAuthenticationInfo": {
                    "authenticationPurpose": "PAYMENT_TRANSACTION"
                },
                "threeDSPurchaseInfo": {
                    "purchaseDate": "2028-10-31T17:07:14.91Z",
                    "threeDomainSecureTransactionType": "GOODS_SERVICES"
                }
            }
        }
    },
    "accountHolder": {
        "firstName": "John",
        "lastName": "Doe",
        "email": "example@example.com",
        "billingAddress": {
            "line1": "1234 Main Street",
            "city": "New York",
            "postalCode": "10111"
        },
        "phone": {
            "countryCode": "1",
            "phoneNumber": "1234567890"
        }
    },
    "merchant": {
        "merchantSoftware": {
            "companyName": "John's Gym",
            "productName": "Cart"
        }
    },
    "browserInfo": {
        "browserAcceptHeader": "application/json",
        "deviceIPAddress": "192.168.1.11",
        "browserLanguage": "en",
        "browserColorDepth": "8",
        "browserScreenHeight": "1",
        "browserScreenWidth": "1",
        "deviceLocalTimeZone": 1,
        "browserUserAgent": "Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:47.0) Gecko/20100101 Firefox/47.0",
        "challengeWindowSize": "FULL_SCREEN",
        "javaEnabled": true,
        "javaScriptEnabled": true
    }
}
```

**Step 3: Call the orchestrationFormSubmit function**

Use the <payment|verification>AuthenticationResult. `authenticationOrchestrationUrl` from the response to set the `orchestrationUrlFromResponse` variable in your JS. After the `orchestrationUrlFromResponse` is set, call `orchestrationFormSubmit`. At this point, J.P. Morgan's 3DS server performs authentication, and the Online Payments API automatically creates the payment or verification.

**Step 4: Listen for the POST HTTP request**

After the authentication process completes, the Online Payments APIs sends a `POST` HTTP request (also called the "postback") to the URL you sent in <payment|verification>AuthenticationRequest. `authenticationReturnUrl` on your `/payments` or `/verifications` request. The pastback call includes the `paymentrequestId` and a `responseStatus` ( `SUCCESS`, `ERROR`, or `DENIED`) of the associated payment or verification.

**Step 5: Request additional payment details**

To get the additional details associated with the transaction, perform a `GET` call to the `/payments` or `/verifications` endpoint (depending on the endpoint to which the transaction was originally posted). Authentication details, including any errors, can be found in the `<payment|verification>AuthenticationResult` attribute.

## Pass-through 3DS

We support pass-through 3DS if you use an independent 3DS authentication provider. With pass-through 3DS, you perform the full authentication flow via your authentication provider and include the 3DS results in the Online Payments API transaction.

The 3DS authentication response from your provider contains specific values. You must map these fields to the appropriate Online Payment API request fields without altering the field values. Within the `paymentMethodType.card.authentication` object, populate the following fields with pass-through 3DS authentication data:

- threeDS. `authenticationValue` \- 3DS Base64 cryptogram obtained prior to payment request.
- threeDS. `authenticationTransactionId` \- The directory server transaction ID.
- threeDS. `threeDSProgramProtocol` \- Value based on the version of 3DS used to complete the authentication.
- `electronicCommerceIndicator` \- The outcome of the authentication.

### Electronic Commerce Indicator (ECI) values for 3DS

Your 3DS provider sends you the ECI value, which you then pass in your Online Payments API request. "Fully authenticated" means the issuer has successfully authenticated the cardholder. "Attempted authentication" means the issuer did not attempt to authenticate, but the card brand did and was successful. "Not authenticated" means the authentication failed.

| Type | Visa | Mastercard/International Maestro | American Express | Discover/Diners | Japan Credit Bureau | UnionPay |
| --- | --- | --- | --- | --- | --- | --- |
| Fully-authenticated | 05 | 02 | 05 | 05 | 05 | 05 |
| Attempted authenticated | 06 | 01 | 06 | 06 | 06 | 06 |
| Not authenticated | 07 | 00 | 07 | 07 | 07 | 07 |

ECI values for 3DS


Tip

If you perform a network token transaction with 3DS and your authentication provider returns multiple ECI values, use the value specific to 3DS authentication.

Refer to the following example of a payment with pass-through 3DS:

Json
Copy

```json
{
    "amount": 14500,
    "currency": "USD",
    "paymentMethodType": {
        "card": {
            "accountNumber": "4110144110144115",
            "expiry": {
                "month": 5,
                "year": 2040
            },
            "authentication": {
                "electronicCommerceIndicator": "05",
                "threeDS": {
                    "authenticationValue": "AABAIcJIo5DIzAgVAkiAAAAAAA=",
                    "authenticationTransactionId": "01ade6ae340005c681c3a1890418",
                    "threeDSProgramProtocol": 2
                }
            }
        }
    },
    "accountHolder": {
        "firstName": "John",
        "lastName": "Doe",
        "email": "example@example.com",
        "billingAddress": {
            "line1": "1234 Main Street",
            "city": "New York",
            "postalCode": "10111"
        },
        "phone": {
            "countryCode": "1",
            "phoneNumber": "1234567890"
        }
    },
    "merchant": {
        "merchantSoftware": {
            "companyName": "John's Gym",
            "productName": "Cart"
        }
    }
}
```

## Related

[3DS API](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/3-d-secure/index)

[3DS API response codes](https://developer.payments.jpmorgan.com/api/commerce/optimization-protection/3-d-secure/error-codes)

[3DS API specification](https://developer.payments.jpmorgan.com/api/commerce/optimization-protection/3-d-secure/3-d-secure)

[Authorize and capture a payment](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/how-to/auth-and-capture-payment)

[Cards](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-methods/cards)

[Online payments response codes](https://developer.payments.jpmorgan.com/api/commerce/online-payments/online-payments/error-codes)

[Online Payments API specification](https://developer.payments.jpmorgan.com/api/commerce/online-payments/online-payments/online-payments)

[Request a 3DS authentication](https://developer.payments.jpmorgan.com/docs/commerce/optimization-protection/capabilities/3-d-secure/how-to/request-a-3ds-auth)

Is this page helpful?