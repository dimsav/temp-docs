---
title: Payment Demo
layout: default
nav_order: 2
---

# Payment Demo

The first step to make a payment is to know our API keys. 
- We will need the public key (starting with `pk_`) and the secret key (starting with `sk_`).

Then let's add a card form into our website.

## The button and the card form

Add the following code into your website's html.

<div class="code-example">
<form method="post" action="#">
    <div id="button-container"></div>
</form>

<script src="https://sandbox-button.everypay.gr/v2/js/everypay.js"></script>
<script>
    var everypay = Everypay('pk_YZ0QiK99FANXID6GIlIWHGA5Gh5GGQXH');

    everypay.button('#button-container', {"amount": 1200, 'locale': 'en'});
</script> 
</div>

```html
<form method="post" action="{your-endpoint}">
    <h1>Checkout</h1>
    <div id="button-container"></div>
</form>

<script src="https://sandbox-button.everypay.gr/v2/js/everypay.js"></script>
<script>
    var everypay = Everypay('{your-public-key}');

    everypay.button('#button-container', {"amount": 1200});
</script> 
```

- Make sure to replace `{your-endpoint}` with a POST endpoint on your site. 
- Also, replace `{your-public-key}` with your key.

When the card details are entered into the card form, the script will append the card token
into the form as hidden input named `everypay_token` and will submit the form on your backend.  

## Charging the card

Now, on your site's backend, you need to handle the post request created by the form.

This code below is written in PHP using Laravel and [EveryPay's PHP SDK](https://github.com/everypay/everypay-php). 

You can install the sdk using composer:
 
```bash
composer require everypay/everypay-php
```

Then, in our backend, we handle the form and make a request to the payments API.   
```php
// Controller 
use EveryPay\EveryPay;
use EveryPay\Payment;

public function postAction(){

    $tokenFromForm = request('everypay_token');


    Everypay::setApiKey('{your-private-key}');
    Everypay::$isTest = true; // only for sandbox
    
    $paymentRequest = Payment::create([
        'amount' => 1200,
        'token' => $tokenFromForm
    ]);
    
    if (isset($paymentRequest->error)) {
        echo 'Error '.$paymentRequest->error->code.': '.$re->error->message;
    } else {
        echo 'Payment was successful!';
    }   
}
```

Congratulations! You've just completed a test card payment.
