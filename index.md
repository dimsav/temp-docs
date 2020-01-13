---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
title: Home
layout: home
---
# EveryPay Documentation Home
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

EveryPay provides a REST API, where all the responses are given in JSON format.

## Demo
 
For a demo of how to make a payment, visit the [Payment Demo]({% link payment-demo.md %}).


## Authentication

To be authenticated as an EveryPay API user, you need to use your private key, either from your sandbox account or your regular account. The system uses HTTP Basic authentication. Your key will serve as the username you specify during the process, leaving the username blank.

You need to use your private key in every payment request to our API. With the public key you can only create tokens for use in future transactions, for which your private key will be required.

Private key authentication example:

#### Authentication with CURL
```
curl https://api.everypay.gr/payments
  -u sk_PqSohnrYrRI1GUKOZvDkK5VVWAhnlU3R:
  // parameters -d key=value
```

#### Authentication with PHP
```php
// Set your private key
Everypay::setApiKey('sk_PqSohnrYrRI1GUKOZvDkK5VVWAhnlU3R');

$payment = Payment::create(array(
// parameters key =>  value
));
```

Note that in the case of API calls with curl, the semicolon is not part of the key, but is given as a separator between the username (key) and the password (blank).

###  Caution

* Please keep your private keys safe and do not share them with anyone. These private keys carry extremely confidential information about your account and your transactions.
* Due to the security of the transactions, all requests for your transactions must be made via https. Applications made in any other way will fail.
* The tokens, IDs, cards, payments, customers, etc., as well as the public and private keys shown in the examples, are random and have no real impact.

## Response codes

The JSON object of the service response contains, in the event of an error, some fields that characterize the transaction:

* the status field that matches the HTTP status code
* the code field, which is unique to the result of the request
* the message field that directly explains the state in which the transaction was found

Example of an error response:

```json
{
    "error": {
        "status": 400,
        "code": 20008,
        "message": "Card holder name is empty or too long (max. 255 chars)."
    }
}
```

# Your account

EveryPay provides a flexible administration platform. Some functionality of which, we cover in this section.

* Account creation – Company creation
* Public, Private and Shared keys (ΑPI keys)
* Test transactions

### Account creation – Company creation

Once you’ve registered with EveryPay, select the “Company” tab at your dashboard settings. There, you can fill in the company creation form, where you will need to enter:

* the company’s Tax Number
* a contact phone (make sure it is up to date for your convenience)
* and the bank account where you business can accept payments

A brief verification process will be followed by EveryPay, and once approved, transactions will be allowed for this company.

Your company now has a unique pair of public and private key and can accept card payments, once it has been activated and certified by EveryPay. Through the dashboard, you can deactivate your account at any time (irreversible process).
 
### Public, Private and Shared keys (ΑPI keys)   

Each company uses a pair of a public and a private key. In more specific cases, it uses a third public key called a shared key.

The Public Key is used to identify the company in calls through the everypay.js Javascript library or through the button function. The only function that can be performed with this key is the Token version.

The Private Key is used to perform all other types of API transactions, such as creating and listing payments, customers, and more. It is used within the code of your application, so, only your application should be aware of it.

The Shared Key is intended for use by potential partners of your company, who wish to execute transactions through your account for you.
This key can only execute payment and pre-approved transactions.

Through your dashboard environment you have the ability to enroll the keys at any time upon your request. Of course, you will also need to update your applications in order to use the new keys.

### Test transactions

The EveryPay Sandbox platform, where you can execute transactions in a virtual environment is located at https://sandbox-dashboard.everypay.gr. The platform is exactly the same as the production environment, so, you can easily switch to it, by changing the public and private keys in your application.
It is recommended that you test transactions in the sandbox environment, before making use of the EveryPay service in your application.

