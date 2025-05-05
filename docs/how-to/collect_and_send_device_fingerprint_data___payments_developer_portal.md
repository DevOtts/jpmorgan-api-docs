---
title: "collect and send device fingerprint data"
description: "Guide for payment processing and authorization"
category: "Payments"
---


# Collect and send device fingerprint data

The J.P. Morgan Device Fingerprint feature provides an easy way for you to collect data from your checkout page to aid in fraud detection. A device fingerprint, also known as a machine fingerprint, includes hardware, browser, geographic, and IP address information related to a remote computing device. Collect device telemetry from your checkout page to input into our fraud risk assessment process and help identify fraud patterns.

Enable device fingerprint collection with the following steps:

1. Add a script tag to install the device fingerprint JavaScript on your checkout page. Add the following to the body of your checkout page: `<script src="https://dfp-static.chase.com/static/dfp.js"></script>`
2. Verify the package by visiting your checkout page in a browser. Open Developer Tools -> Network. Confirm `prd-dfp.js` shows as loaded with a 200 response.
3. Follow these steps to check for the Session ID. The Session ID has a maximum of 128 characters and is valid for 24 hours. After 24 hours, the script reloads and generates a new Session ID.
1. For Chromium-based browsers (for example, Chrome, Edge, Opera, and Brave), in Developer Tools -> Application -> Local Storage, click on your website. For Safari and Firefox, in Developor Tools -> Storage -> Local Storage, click on your website.
2. The value for key dfp\_session\_id is the Session ID.
4. Include the `dfp_session_id` value in the risk.consumerDeviceInfo. `deviceFingerprintSessionId` field of your payment request.

| Field | Description |
| --- | --- |
| risk.consumerDeviceInfo. `deviceFingerprintSessionId` | Auto-generated fraud session ID.<br>- Each value is valid for 24 hours.<br>- Supports upper- and lower-case alphabetical and numeric characters. |

Device fingerprint required fields


The following example is a payment request with device fingerprint data.

**HTTP method**: `POST`

**Endpoint**: `/payments`

Json
Copy

```json
{
    "captureMethod": "NOW",
    "amount": 1234,
    "currency": "USD",
    "risk": {
        "consumerDeviceInfo": {
            "deviceFingerprintSessionId": "1e37134jASDh203"
        }
    },
    "merchant": {
        "merchantId": "991234567890",
        "merchantSoftware": {
            "companyName": "Payment Company",
            "productName": "Application Name",
            "version": "1.235",
            "softwareId": "string"
        },
        "merchantCategoryCode": "4819",
        "merchantLogoUrl": "string"
    },
    "paymentMethodType": {
        "card": {
            "accountNumberType": "PAN",
            "accountNumber": "string",
            "expiry": {},
            "walletProvider": "APPLE_PAY",
            "cardType": "VI",
            "cardTypeName": "VISA",
            "cvv": "string",
            "originalNetworkTransactionId": "1c4b1100-4017-11e9-b649-8de064224186",
            "isBillPayment": true,
            "maskedAccountNumber": "123456XXXXXX9876",
            "maskedCardNumber": "string",
            "cardTypeIndicators": {
                "cardProductTypes": []
            }
        }
    }
}
```

Is this page helpful?
