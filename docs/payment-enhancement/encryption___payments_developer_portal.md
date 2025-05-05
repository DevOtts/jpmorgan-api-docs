---
title: "encryption"
description: "Guide for payment processing and authorization"
category: "Payments"
---


# Encryption

Page encryption is an effective method of reducing Payment Card Industry (PCI) scope by protecting sensitive information, which can then safely pass through your application and in payment authorization requests.

For security reasons, do not store encrypted data. Only use encrypted data to process transactions in real time. Authorization responses containing transaction reference IDs or tokens may be stored on your application for future transactions or servicing.

## How page encryption works

To secure cardholder payment data entered by a registered or guest shopper, page encryption secures the sensitive data within the browser session. The HTML code of your payment page must include the following JavaScript (JS) files:

- **getkey.js** — Retrieves a one-time dynamic key that is used for encryption.

  - **Test environment URL:** https://safetechpageencryptionvar.chasepaymentech.com/pie/v1/YYYYYYYYYYYY/getkey.js
  - **Production environment URL:** https://safetechpageencryption.chasepaymentech.com/pie/v1/YYYYYYYYYYYY/getkey.js
- **encryption.js** — Uses the key to encrypt card information as it is entered.

  - **Test environment URL:** https://safetechpageencryptionvar.chasepaymentech.com/pie/v1/encryption.js
  - **Production environment URL:** https://safetechpageencryption.chasepaymentech.com/pie/v1/encryption.js

You must replace `YYYYYYYYYYYY` with the ID you recieve from your J.P. Morgan implementation manager.

Attention

The test and production URLs are similar. Confirm the correct URL is in place, especially when testing or switching environments.

## Implement page encryption

Complete the following steps to implement the page encryption service:

**Step 1**: Add **getkey.js** to the HTML code of the payment page. Your ID is provided by an implementation manager after page encryption is enabled for your account. This value must be used in the encryption/decryption request.

**Step 2**: Add **encryption.js** to the HTML code of the payment page. This action encrypts the account number and card verification value (CVV). The embedded JS provides the following two features:

- `ValidatePANChecksum` — Performs a MOD 10 checksum on the account number entered. Major card types include a checksum as the last digit to ensure correct entry of the account number. The function returns true if the checksum is valid. It is good practice to validate the checksum to avoid unnecessary API calls.
- `ProtectPANandCVV` — Performs the encryption on the specified account number value and card security value. Submit three parameters: **credit card**, **CVV** (optional), and **embed** flag. **Note**: Set the boolean parameter **embed** to **false**. This routine returns an array of three values. **NULL** is returned if an error occurs during encryption.

Javascript
Copy

```javascript
var result = ProtectPANandCVV(ccno, cvv, false);
if(result != null) {
	document.getElementById("cryptCard").value = result[0];
	document.getElementById("cryptCvv").value = result[1];
	if (result.length > 2) {
		document.getElementById("integrityCheckVal").value = result[2];
	} else {
		alert("Error: ProtectPANandCVV call returned null. You may have entered an invalid card number.");
	}
}
```

The following sample HTML contains **getkey.js** and **encryption.js**:

Plaintext
Copy

```plaintext
<!DOCTYPE html>
<html lang="en">
<head>
    <title>PIE Encryption | Payments </title>
    <meta charset="UTF-8">
    <link href="styles/reset-style.css" rel="stylesheet">
    <link href="styles/simple.css" rel="stylesheet">
    <link th:href="@{/css/bootstrap.min.css}" rel="stylesheet" type="text/css"></link>
    <link th:href="@{/css/layout.css}" rel="stylesheet" type="text/css"></link>
    <!-- begin PIE scripts
    <script type="text/javascript" src="https://safetechpageencryption.chasepaymentech.com/pie/v1/encryption.js"></script>
    <script type="text/javascript" src="https://safetechpageencryption.chasepaymentech.com/pie/v1/123456789012/getkey.js"></script>
    -->
    <script type="text/javascript" src="https://safetechpageencryptionvar.chasepaymentech.com/pie/v1/encryption.js"></script>
    <script type="text/javascript" src="https://safetechpageencryptionvar.chasepaymentech.com/pie/v1/100000000005/getkey.js"></script>
    <script type="text/javascript">
        // This function checks whether getkey.js is loaded.
        function is_pie_key_download_error() {
            if (( typeof(PIE) == 'undefined') || (typeof(PIE.K) == 'undefined')
                || (typeof(PIE.L) == 'undefined') || (typeof(PIE.E) == 'undefined')
                || (typeof(PIE.key_id) == 'undefined') || (typeof(PIE.phase) == 'undefined'))
            {
                return true;
            }
            return false;
        }
        // This function checks whether encryption.js is loaded.
        function is_pie_encryption_download_error() {
            if ((typeof ValidatePANChecksum != 'function')
                || (typeof ProtectPANandCVV != 'function')) {
                return true;
            }
            return false;
        }
        function doEncryption() {
            if(is_pie_encryption_download_error()) {
                alert("Failed to load encryption.js file");
                return;
            }
            if(is_pie_key_download_error()) {
                alert("Failed to load getkey.js file");
                return;
            }
            var ccno = document.getElementById("cardNo").value;
            var cvv = document.getElementById("cvv").value;
            var embed = document.getElementById("embed").checked;
            if(embed) {
// Check MOD 10 digit, since PIE embedded encryption
// requires that the MOD 10 checksum is valid.
                if(!ValidatePANChecksum(ccno)) {
                    alert("PAN has invalid checksum.");
                    return false;
                }
            }
            var result = ProtectPANandCVV(ccno, cvv, !embed);
            if(result != null) {
                document.getElementById("cryptCard").value = result[0];
                document.getElementById("cryptCvv").value = result[1];
                if (result.length > 2)
                    document.getElementById("integrityCheckVal").value = result[2];
            } else {
                alert("Error: ProtectPANandCVV call returned null. You may have entered an invalid card number.");
            }
            document.getElementById("keyId").value = PIE.key_id;
            document.getElementById("phase").value = PIE.phase;
            return false;
        }
    </script>
    <!-- end PIE scripts -->
</head>
<body>
<div class="container">
    <form class="form-horizontal" id="pieForm" method="post" action="" onsubmit="return false;">
        <h1>Page Encryption Example</h1>
        <div class="demo-card-information">
            <div class="merchant-details">
                <h2>PCI Information</h2>
                <div class="row">
                    <label id="cardLabel" for="cardNo">Card Number</label>
                    <input id="cardNo" name="cardNo" type="text" class="form-control input-md" />
                    <!--<input id="cardNo" name="cardNo" type="text" class="form-control input-md" onblur="doEncryption()"/>-->
                </div>
                <div class="row">
                    <label id="cvvLabel" for="cvv">CVV</label>
                    <input type="text" id="cvv" name="cvv" class="form-control input-md"/>
                </div>
                <div class="row">
                    <label id="embedLabel" for="embed">Embed key</label>
                    <input type="checkbox" id="embed" name="embed" value="Y" />
                </div>
                <div class="row">
                    <div id="leftBut" class="butFloat">
                        <button id="encrypt1" onclick="return doEncryption()" class="btn btn-primary btn-login"><span>Encrypt Card</span></button>
                    </div>
                    <div id="rightBut" class="butFloat">
                        <button class="btn btn-primary btn-login" id="encrypt" onclick="window.location.href=window.location.href"><span>Reload Page</span></button>
                    </div>
                </div>
            </div>
        </div>
        <div class="demo-card-results">
            <div class="merchant-details">
                <h3>Encrypted card details... (Hidden from END User)</h3>
                <div class="row">
                    <label id="cryptCardLabel" for="cryptCard">Encrypted Card</label>
                    <input id="cryptCard" name="cryptCard" type="text" class="form-control input-md" readonly />
                </div>
                <div class="row">
                    <label id="cryptCvvLabel" for="cryptCvv">Encrypted CVV</label>
                    <input id="cryptCvv" name="cryptCvv" type="text" class="form-control input-md" readonly />
                </div>
                <div class="row">
                    <label id="integrityCheckValLabel" for="integrityCheckVal">Integrity Check</label>
                    <input id="integrityCheckVal" name="integrityCheckVal" class="form-control input-md" type="text" readonly />
                </div>
            </div>
        </div>
    </form>
</div>
</body>
</html>
```

**Step 3**: Send a transaction with encrypted card data by mapping the following encrypted variables to your transaction request:

| Result | Description | Payments API |
| --- | --- | --- |
| 0 | Encrypted card number | paymentMethodType.card.accountNumber |
| 1 | Encrypted CVV | paymentMethodType.card.cvv |
| 2 | Integrity check value | paymentMethodType.card.encryptionIntegrityCheck |

Encrypted variables for a Payments API request


For encrypted card data within the payment request payload, refer to the following example:

Json
Copy

```json
{
    "paymentMethodType": {
        "card": {
            "accountNumberType": "SAFETECH_PAGE_ENCRYPTION",
            "accountNumber": "401200jpLWkYXHd1112",
            "expiry": {
                "month": 5,
                "year": 2025
            },
            "cvv": "5D4C3B",
            "encryptionIntegrityCheck": "ABCDFKEJGJTHFHG"
        }
    }
}
```

Here is an example of card data in the payment response:

Json
Copy

```json
{
    "paymentMethodType": {
        "card": {
            "maskedAccountNumber": "401200XXXXXX1112",
            "paymentTokens": [\
                {\
                    "tokenProvider": "SAFETECH",\
                    "tokenNumber": "4012000803101112",\
                    "responseStatus": "SUCCESS"\
                }\
            ]
        }
    }
}
```

## Related

[Safetech tokenization](https://developer.payments.jpmorgan.com/docs/commerce/online-payments/capabilities/online-payments/payment-enhancements/tokenization#safetech-tokenization)

Is this page helpful?
