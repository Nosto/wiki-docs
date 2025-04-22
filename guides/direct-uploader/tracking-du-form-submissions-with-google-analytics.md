# How to Track Direct Uploader form submissions with Google Analytics (Universal & GA 4)

* [Overview](tracking-du-form-submissions-with-google-analytics.md#overview)
* [Key Concepts](tracking-du-form-submissions-with-google-analytics.md#key-concepts)
* [The Fun Part](tracking-du-form-submissions-with-google-analytics.md#the-fun-part)
  * [Getting Started](tracking-du-form-submissions-with-google-analytics.md#getting-started)
  * [Getting to know the Global Widget Events API](tracking-du-form-submissions-with-google-analytics.md#getting-to-know-the-global-widget-events-api)
  * [Sending data to Google Analytics](tracking-du-form-submissions-with-google-analytics.md#sending-data-to-google-analytics)

## Overview

Nosto's UGC provides a Google Analytics plugin that allows you to track common user interactions via Google Event tracking.

The data sent to Google Analytics is pre-set and currently limited to interactions with Onsite Widgets and Email Campaigns.

In this guide, we will showcase how you can use the Direct Uploader custom code editor to track in Google Analytics when someone submits a form.

[Back to Top](tracking-du-form-submissions-with-google-analytics.md#top)

## Key Concepts

### Google Analytics Events

[Events](https://support.google.com/analytics/answer/9356037?hl=en\&sjid=5023917053232407279-AP) are user interactions with content that can be tracked independently from a web page or screen load.

Data related to these interactions can be sent to Google Analytics.

[Back to Top](tracking-du-form-submissions-with-google-analytics.md#top)

## The Fun Part

### Getting Started

In this example we will need to do the following:

* Create a Direct Uploader form
* Use Google Tag Manager and Google Analytics event tracking methods to send some of this data to Google Analytics.

This guide can be used for both Universal Analytics and Google Analytics 4.

[Back to Top](tracking-du-form-submissions-with-google-analytics.md#top)

### Sending data to Google Analytics

Before we look into this further, we'll need a way to establish how this data is being sent to Google Analytics via Google Tag Manager. As the gtag() calls do not use the current Nosto's UGC Google Analytics plugin Tracking ID or data, we'll need to include this in the Direct Uploader Javascript or the parent page with the tracking ID or Google Tag ID / Measurement ID. In the example below we are including firstname, lastname, and email as custom parameters for the event

```
window.Stackla.GoConnectCallbacks = {
 onBeforeSave: function (formData, errorMessagesFromServer) {
 if(typeof(formData['custom-data-programs']) !== 'undefined' && formData['custom-data-programs'] == 'Select from the drop-down menu') {
    errorMessagesFromServer['custom-data-programs'] = 'Please select a program';
    return false;
 } else {
   gtag('event', 'nosto_ugc_upload_form_submission',
{
'app_name': 'Nosto - UGC',
'firstname': $("[name='firstname']").val(),
'lastname': $("[name='lastname']").val(),
'email': $("[name='email']").val(),
}
);
 return true;
 }    }
};var head = document.getElementsByTagName('head')[0];var script = document.createElement('script');script.setAttribute('async', '');script.setAttribute('src', 'https://www.googletagmanager.com/gtag/js?id=X-XXXXX');head.appendChild(script);window.dataLayer = window.dataLayer || [];
function gtag(){dataLayer.push(arguments);}
gtag('js', new Date());
gtag('config', 'X-XXXXX'); 
```

Please change the X-XXXXX in the code above to Tracking Id for Universal Analytics or Google Tag Id / Measurement ID for Google Analytics 4. You can find more details [here](https://support.google.com/analytics/answer/9539598?hl=en).

[Back to Top](tracking-du-form-submissions-with-google-analytics.md#top)
