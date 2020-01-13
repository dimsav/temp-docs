---
title: Self Hosted Form
layout: default
nav_order: 7
---

# Self Hosted Card Form

EveryPay accepts payment requests with the user’s card and the merchant’s public key and detects if this card is registered with 3D Secure. If yes, then it performs the authentication process, where the user is asked to enter the 3D Secure code that is available, otherwise it proceeds without that verification.

In any case, if the card details are correct and the 3D Secure verification process is either not needed or successfully completed, a token card will be returned to the merchant in the callback url stated in the original form.

## How to use

The merchant’s application must contain a form with the following information:

```html
<form action="https://button.everypay.gr/tds" method="post" id="payment-form">
    <div class="form-row">
        <label>Αριθμός κάρτας</label>
        <input type="text" maxlength="16" autocomplete="off" name="card_number"/>
    </div>
    <div class="form-row">
        <label>Όνομα κατόχου</label>
        <input type="text" maxlength="30" autocomplete="off" name="holder_name"/>
    </div>
    <div class="form-row">
        <label>CVV</label>
        <input type="text" maxlength="3" autocomplete="off" name="cvv"/>
    </div>
    <div class="form-row">
        <label>Ημερομηνία Λήξης</label>
        <select name="expiration_month"/>
            <option value="1">1</option>
            <option value="2">2</option>
            ...
            <option value="12">12</option>
        </select>
        <span> / </span>
        <select name="expiration_year"/>
            <option value="2015">2015</option>
            <option value="2016">2016</option>
            <option value="2017">2017</option>
            <option value="2018">2018</option>
            <option value="2019">2019</option>
        </select>
    </div>
    <input type="hidden" name="public_key" value="{PUBLIC_KEY}"/>
    <input type="hidden" name="amount" value="{AMOUNT}"/>
    <input type="hidden" name="callback_url" value="{CALLBACK_URL}"/>
	<input type="hidden" name="md" value="ORDER189500">
    <button>Πληρωμή</button>
</form>
```

## Caution:

* Replace {PUBLIC_KEY} with your public key.
* Replace {AMOUNT} with the payment in cents of 12000 for 120.00
* Replace {CALLBACK_URL} with the url, where the new token card will be redirected to for subsequent payment. It may be a url of your site.

## After submitting the above form

* EveryPay performs the process of verifying the card with the 3D Secure service.
* At the end of a successful authentication process, the card token (e.g. ctn_QEbq4jdOkAhNebqDL0vynxFc) is allowed and the parameters that were given in the md field are allowed. The end-user does not know if this card had to be verified by a 3D Secure procedure – from the card service of the respective organization.
* You now have to make a call to the API (https://api.everypay.gr/payments) with the newly recovered card token (valid for 15 minutes), the payment amount, and with the merchant’s private key.
