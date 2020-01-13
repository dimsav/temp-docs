---
title: Test Cards
layout: default
nav_order: 6
---

# Test Cards

You can use the following card numbers to execute test transaction scenarios

## Success test cards

| Card Number      | Card Type | Use case |
|:-----------------|:----------|:------|
| 4888137257116730 | Visa      | Predefined scenarios |
| 4916915111701144 | Visa      | Predefined scenarios |
| 4853451426120387 | Visa  | Predefined scenarios |
| 5206140271967502 | MasterCard  | Predefined scenarios |
| 5217925525906273 | MasterCard  | Successful transaction |
| 4556390755719395 | Visa  | Successful transaction |
| 4908440000000003 | Visa  | Successful transaction with installments |

## Failure test cards

Below are scripts that return specific errors, so you can control the behavior of your application in similar cases.

| Card Number      | Status | Error Code | Description      |
|:-----------------|:-------|:-----------|:-----------------|
| 4888137257116730 | 500    | 99999      | Unexpected error |
| 4853451426120387 | 502    | 40013      | Payment failed in remote gateway |
| 4916915111701144 | 200    |       | It will generate a successful payment, which in case of refund will fail with error code 502 |
| 5206140271967502 | 200    |       | It will generate a successful payment, which in case of refund will fail with error code 500 |

## What you must test in your application

On the web server of your application, check the following:

* All the values ​​you receive during the payment steps (e.g. yourshop.com/payment-step2) are desirable and expected
* The various cases of errors are handled accordingly by your application