# Consuming UGC Webhooks via Zapier

## Overview

In this guide we explore the general concepts of consuming Nosto's UGC Webhooks via Zapier and the potential options.

## Key Concepts

### Zapier

To quote the [Zapier Documentation](https://zapier.com/developer/documentation), Zapier is a tool for primarily non-technical users to connect together web apps. An integration between two apps is called a Zap. A Zap is made up of a Trigger and an Action. Whenever the trigger happens in one app, Zapier will automatically perform the action in another app.

### Webhooks

Webhooks provide external applications notifications when specific events occur. You can read more about Nosto's UGC Webhooks in the [API documentation](../../../api-docs/webhooks.md).

## The Fun Part

In this guide we will do the following:

* Configure Trigger in Zapier
* Configure Nosto's UGC Webhooks
* Configure Action in Zapier
* Consume a Test Webhook

### Configure Zapier

Log in to your Zapier account.

From the menu bar choose "Create Zap".

The first step in creating a Zap is choosing a "trigger app" and an "action app".

The Zapier interface explains what these are quite well and below is a screenshot of what you should see. In this case, the "Trigger App" needs to be configured for Nosto's UGC, or more precisely, a Nosto's UGC Webook. For this we can choose the _Webhook by Zapier_ app, which will give us the _Catch Hook_ trigger.

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/image02.jpg)

On the other side is a menu for selecting the "Action App". There are many apps that can be integrated this way. Your use cases will vary, but for this example we can start with a simple _Email by Zapier_ app that will send an email after a webhook to the addressed person. We will focus on doing this when a tile is ingested, and we will assume that there is an existing GoConnect widget being used that will trigger the webhook itself.

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/image05-mod.png)

In the next section you should see a URL such as the one below for the trigger app. We will need it for the next section.

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/image00.png)

### Configure Nosto's UGC Webhooks

Log into your Stack admin portal and navigate to API > Webhooks section. Copy-and-paste the Zapier Webhook URL into the TILE\_INGESTED field as per the image below

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/imagexx-configurewebhook.png)

Go ahead and create a few sample entries via GoConnect. This will warm up the receiving end in Zapier so that we can see the data fields in Zapier in order to generate actions. If you don't have a site up and running, you can place your GoConnect embed code onto [codepen](http://codepen.io) or [jsbin](http://jsbin.com).

### Configure Action in Zapier

Back into Zapier. You should see a screen such as below to continue editing the Zap. You will notice that you can place specific conditions onto this form in order to skip webhooks that don't meet specific conditions.

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/image08.jpg)

In our case, it is a good idea to skip those that don't have an email address. Select the "Field" drop down and Zapier will attempt to find the available fields. It may not find any because we haven’t yet told Zapier what they are. In that case, you'll be presented with this pop-up:

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/image01.png)

There are 3 steps here that we have to complete. The first one is already done - we did this when we copied the end-point into the TILE\_INGESTED webhook field in Stackla. Click "Ok, I did this".

Step 2 asks you to "Go create a brand new Hook in Webhook by Zapier, then come back to Zapier. Don't close this popup!". If you already have the GoConnect set up that you want to use, go and create a tile. Make sure your GoConnect asks for a name and email address. If you don't have a GoConnect set up that you want to use to send to Mailchimp, you’ll need to create one and embed it somewhere so you can test it.

Once done, go back to Zapier and click "Ok, I did this".

Zapier will check to see if it can find the data you just submitted via GoConnect. When it finds it, it will present you with a confirmation. You can now close this dialog.

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/image09-mod.png)

Now we'll attempt to set up our filter again by clicking the "Field" dropdown. You should be presented with a bunch of options in the dropdown - this is the data that the Nosto's UGC Webhook TILE\_INGESTED sends when a tile is created via GoConnect.

We want to choose the field "External Data Email" as this is the field that contains the user’s email address.

The "condition" we want to choose is "exists" because we only want to trigger this task if an email is associated with the tile.

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/image03.jpg)

We are getting there. Now that we have configured webhooks and confirmed that they are arriving successfully, we can configure the actual email that will go out.

If you are using a different app, this is the part where you would skip over in favour of the app that you are actually configuring. For the mail app, however, here are some ways in which you can consume Nosto's UGC webhooks data as data fields.

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/imagexx-webhookdata.png)

When you're done click continue.

### Consume a Test Webhook

Click “Test Webhook by Zapier trigger” in the next step then click continue.

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/image04.jpg)

If everything is working and you're happy with your Zap, you can finally turn it on for wider use.

![](https://github.com/Stackla/docs/blob/master/docs/guides/consuming-stackla-webhooks-via-zapier/images/image13.png)

## Summary and Next Steps

This example can be further expanded with the following activities:

* Configuring emails to go out for successful GoConnect entries with a different mail service
* Saving Rights Managed contacts into your CRM

Last Updated on 10 July 2015 3:40:55 GMT
