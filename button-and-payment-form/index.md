---
layout: default
title: Button and Form
nav_order: 2
---
# Button and Form 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

EveryPay provides a script which is responsible for generating the frontend code required to let the customers 
fill the card form details securely.

For security and legal reasons, the customer's card details have to be inserted in a form provided
by EveryPay within an iFrame. There are two ways of integrating a card form into a website:

1. Using the Button
2. Using the Inline Form 

# Installation

To install the script, we first need to insert the following code into the html part of our e-commerce 
site. 

### Production

```html
<script src="https://button.everypay.gr/v2/js/everypay.js"></script>
<script>
    var everypay = Everypay('your-public-key', globalOptions)
</script>
```

### Sandbox

```html
<script src="https://sandbox-button.everypay.gr/v2/js/everypay.js"></script>
<script>
    var everypay = Everypay('your-public-key', globalOptions)
</script>
```

## Inserting the button

The button is the most popular way of adding a payment form. When the customer clicks the button,
the payment form will appear.

<div class="code-example">
<form method="post" action="#">
    <div id="button-container"></div>
</form>

<script src="https://sandbox-button.everypay.gr/v2/js/everypay.js"></script>

<script>
    var everypay = Everypay('pk_YZ0QiK99FANXID6GIlIWHGA5Gh5GGQXH', {"amount": 1200, 'locale': 'en'});

    everypay.button('#button-container');
</script> 
</div>

First, we need to add a container `#button-container` where the button will be inserted into. 
By default, the container needs to be inside an html `<form>` for the button to appear. 

```html
<form method="post" action="{url}">
    <div id="button-container"></div>
</form>
```

Then, we tell EveryPay to insert the button in the container like so.

```html
<script>
    everypay.button('#button-container', buttonOptions); 
</script>
```

- The first argument of the `everypay.button()` method is a string selector declaring where the button
should be inserted into. You are free to choose any selector you want as long as the element exists on the page.
- The second argument of the `everypay.button()` method is an object containing the options of the button. Example:
`buttonOptions = {amount: 100}`.

## Inserting an inline form

If we don't want to show the card form as a popup modal, we can display the form directly into our merchant site.
It will appear like so:

<div class="code-example">
    <form method="post" action="#">
        <div id="inline-container"></div> 
    </form>
    <script>
        everypay.form('#inline-container'); 
    </script> 
</div>

To insert the form, we need a container located inside an html `<form>`.

```html
<form method="post" action="#">
    <div id="inline-container"></div> 
</form>

<script>
    everypay.form('#button-container', options); 
</script>
```

To learn more about how to define the options property, see the options page.



# Options


## Global Options

Global options are settings applied globally to all subsequent `everypay.button()` calls.

If for instance you want to declare the `locale` "globally" for every button on the page, you 
can define it as a global option.

When `everypay.button(selector, options)` is executed, the options object will be merged with 
the globalOptions object.

#### Example

```html
<script>
    var everypay = Everypay('your-public-key', {'locale': 'el'})
    
    // This will display a button with greek text
    everypay.button('#button-1', {'amount': 100})
    
    // This will display a second button with english text
    everypay.button('#button-2', {'amount': 100, 'locale': 'en'})
</script>
```



## Options List

Here is the list of all available options. You may set these properties either as global or as 
button options.

### Language Options

| Key        | Description          | Default |
|:-------------|:------------------|:------|
| `locale` | The language of the texts displayed to customers. Can be `'en'` or `'el'`. | `'el'` |

### Payment Options

| Key        | Description          | Default |
|:-------------|:------------------|:------|
| `amount` (required) | An integer declaring the amount in cents that will be charged. |   |
| `max_installments` | An integer declaring the maximum number of installments. | 0 |
| `txn_type` | For advanced merchants only. Possible values `tds`, `no-tds`, `moto` |  |

### Button Options

| Key        | Description          | Default |
|:-------------|:------------------|:------|
| `button_text` | The text displayed inside the button   | `'Pay with card'` when english is selected.  |
| `button_background_color` | The value of the css property `background-color` | `'#2b6cb0'`   |
| `button_hover_background_color` | The value of the css property `hover:background-color` | `'#2c5282'` |
| `button_text_color` | The color of the text | `'#ffffff'` |
| `button_hover_text_color` | The color of the text on hover | `'#ffffff'` |
| `button_border_color` | The color of the border | Defaults to the color of the background |
| `button_hover_border_color` | The color of the border on hover | Defaults to the color of the background on hover |
| `button_full_width` | A boolean indicating if the button should be a full width element | `false` |
| `button_radius` | The value of the css property `border-radius` | `'4px'` |
| `button_font_family` | The value of the css property `font-family` | |
| `button_font_weight` | The value of the css property `font-weight` | |
| `button_font_size` | The value of the css property `font-size` | `'15px''` |

### Form Options

These settings apply both to the modal and the inline form.

| Key        | Description          | Default |
|:-------------|:------------------|:------|
| `form_button_text` | The text displayed in the button below the card form  | `'Pay 12,00 €'` when english is selected.  |
| `form_button_background_color` | The color of the submit button below the card form  | `'#2b6cb0'` |
| `form_button_text_color` | The text color of the submit button below the card form  | `'#ffffff'` |

### Inline Form Options

| Key        | Description          | Default |
|:-------------|:------------------|:------|
| `form_button_hide` | Boolean to indicate whether the button below the card form should be hidden. This can be useful when the inline form is submitted programmatically.  | `false` |

## Token Received Event handler

It is possible to define a function that is executed by EveryPay's script when the card form generates
a valid card token.

When a card token is generated, the default behavior of EveryPay's script is to append the token as a
hidden input and submit the html form in the merchant's site. 
To provide a better user experience, we might want to change this default behavior and do something custom. 

A typical example would be to attempt to make the payment with an ajax request and if the
payment fails, to show an error inside the card form and let the user enter a different
card. 

```html
<script>
    everypay.button('#button-container', {
        "amount": 1200,
        "on_token_received": function(token, showError, hideForm) {
            // We got a card token. Let's try to make the payment...
            if (paymentWasSuccessful) {
                hideForm() // close modal
                // show success message
            } else {
                showError('Invalid card') // show error on card form
            }
        }
    }); 
</script>
```

The three arguments provided by EveryPay are:
1. `token`: Contains the card token as a string.
2. `showError`: This is a function that displays an error inside the card form
3. `hideForm`: This closes the card form modal

**showError(string)**

The function `showError()` accepts an error argument that can have two types:

A string containing the error message to display. This will place the input string right
below the card input. If `showError('Invalid card')` is executed, then the message will
appear like so:

![Invalid card]({{ site.url }}/{{ site.baseurl }}/assets/img/screen-1.png)

```html
<script>
everypay.button('#button-container', {
    "amount": 1200,
    "on_token_received": function(token, showError, hideForm) {
    // failed ajax request
    showError('Invalid card')
    }
}); 
</script>
```

**showError(object)**

The function `showError()` can also accept an error object returned by the EveryPay API 
payment request.

```html
<script>
everypay.button('#button-container', {
    "amount": 1200,
    "on_token_received": function(token, showError, hideForm) {
        paymentResponse = ajaxPaymentToMerchantBackend(token)
        // var paymentResponse = {"error": {
        //      "status": 401,
        //      "message": "Client should contact issuing bank",
        //      "code": 40060
        //   }};
        if (paymentResponse.error) {
            showError(paymentResponse)    
        } else {
            window.location = 'success-url'
        }
    }
}); 
</script>
```

![Invalid card]({{ site.url }}/{{ site.baseurl }}/assets/img/screen-2.png)

The showError will return a user friendly message, depending on the error code we received from the 
payment API call.