---
title: Webhooks
layout: default
nav_order: 5
---
# Webhooks
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}
---

## What is Webhooks

Webhooks is a feature provided to the merchant, to be notified at a provided url about events, such as payments or refunds. The merchant must submit a url in the relevant form of their EveryPay dashboard. The response of the EveryPay API will be submitted via PUT method to that url.

The events that a marketer can choose for webhooks are:

* New payment
* Refund

For example, after a successful payment, the API will send – by PUT method – to the url the trader declared a JSON string, with the following information:

```json
{
    "token": "pmt_ETF9EaZURr3l6mC8n6TzClBS",
    "date_created": "2015-11-09T19:03:58+0200",
    "description": "Order #A-777",
    "currency": "EUR",
    "status": "Captured",
    "amount": 10480,
    "refund_amount": 0,
    "fee_amount": 272,
    "payee_email": null,
    "payee_phone": null,
    "refunded": false,
    "refunds": [],
    "installments_count": 0,
    "installments": [],
    "card": {
        "expiration_month": "08",
        "expiration_year": "2016",
        "last_four": "0003",
        "type": "Visa",
        "holder_name": "John Doe",
        "supports_installments": false,
        "max_installments": 0
    }
}
```

From the above answer, the merchant can see all the useful information regarding this new payment, such as the token, the date of the transaction, the amount, any refunds, installments, and non-sensitive card details used for the transaction (the last 4 digits, the cardholder name, etc).

## Installation

The merchant opens their account settings page from the Dashboard interface (https://dashboard.everypay.gr) – after they have logged in with their credentials. Select Webhooks from the account settings page.

![Webhooks setup]({{ site.url }}/{{ site.baseurl }}/assets/img/webhook-example-01.png)

There they can add as many webhooks as they want, from the corresponding button, where they submit the url and the type of events for that webhook.

![Webhooks url setup]({{ site.url }}/{{ site.baseurl }}/assets/img/webhook-example-03.png)

The merchant can manage their webhooks from the list at any time, as shown below.

![Manage webhooks]({{ site.url }}/{{ site.baseurl }}/assets/img/webhook-example-04.png)

## Example code

The url provided by the merchant for the webhooks, could simply be run by a server that accepts the PUT request with a JSON string, store it as text or decompose it into its individual parts, and handle them as the logic requires.
An example of such code in PHP.

```php
//retrieve data from PUT request
$input = @file_get_contents("php://input");

//either just save entire string
file_put_contents("/tmp/newEvent.txt", $input);

//or retrieve the token and use it as an id somewhere
$payload = json_decode($input);
file_put_contents("/tmp/newEvent".$payload->token.".txt", $input);
```