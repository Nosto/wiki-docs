In this article, you will learn how to collect email addresses from other sources. If you have implemented the customer tagging, Nosto will automatically collect the information for logged-in users. For all other scenarios, you will need to use our Javascript API to push the customer information to us:

## Collecting email addresses from a form field

If your website has a form field that accepts email address, you can [collect customer information using our JS API](https://github.com/Nosto/techdocs/wiki/Sending-customer-information). A more simplified way would be to add the class `nosto_email_capture` to any email field on the page whose addresses need to be captured. Having done that, include the following snippet into your site.

The script below binds every `input` field with the class `nosto_email_capture` and submits the collected information to Nosto when the parent form is submitted.

```javascript
jQuery("form input.nosto_email_capture").each(function(idx, input) {
    if (!input.form) {
        return;
    }
    input.form.onsubmit = function(event) {
        var email = input.value;
        if (email) {
            // Push customer information to current visit
            nostojs(function(api) {
                console.log("Collecting email " + email);
                api.customer({email: email});
            });
        } else {
            console.log("No email specified");
        }
    }
});
```