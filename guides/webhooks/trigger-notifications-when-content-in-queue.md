# Trigger notifications when content is in the moderation queue

## Overview

When there are a number of items such as Tweets or Instagram photos in the moderation queue to be addressed, Nosto's UGC can be utilised to trigger a process. Often this process will be an internal business process, which may have interfaces either to your applications or 3rd party services.

In this guide, we will explore a case where content has either been newly ingested into the Stack and configured to be queued, or existing content has been queued for moderation. A separate application is concerned with receiving this notification and in turn triggering an email when there are more than 5 notifications in the last minute. The significance of such an event is open to your imagination, but in some cases it may mean that there is a higher than usual rate of content arriving and should be looked at manually.

More specifically, we will ingest `#mongodb`, but we will get it to all Queue until we manually approve for publishing.

## Key concepts

### Tile

In your Stack, the name Tile is given to a piece of content that has been saved and made available for moderation. It could be a Tweet, an Instagram photo or a YouTube video, amongst others. It could also be a user-generated post to your photo competition.

### Webhooks

In this example we will utilise Nosto's [Webhooks](../../api-docs/webhooks.md), namely the [TILE\_INGESTED](../../api-docs/webhooks.md#webhook-tile-ingested) and [TILE\_STATUS\_UPDATE](../../api-docs/webhooks.md#webhook-tile-status-update) notifications. As the names may suggest, the former is triggered when a new piece of content is ingested (e.g. a Tweet comes in) and the latter when an existing piece of content's status is modified (e.g. a Tweet previously queued for moderation has been approved).

### Example Application

As an intermediate application is required for this guide, we will create a simple Node.js application to accept Webhook notifications and trigger emails.

## The Fun Part

### Getting Started

In this example we will need to do 3 main things, namely:

1. Create an application to receive webhooks and handle notifications
2. Create the relevant webhook
3. Start ingesting some content to be notified of

### Create the Example Application

To simplify the example, we will create a simple Node.js application that will receive webhook notifications and buffer the last 5 results. If the oldest record is in the last 1 minute, it will trigger an email notificiation.

To run this application, you will also need to install [npm](https://www.npmjs.org) to use [Express](http://expressjs.com) (with body-parser), and [Nodemailer](http://www.nodemailer.com). The following 4 files should all be in the same path.

#### package.json

This file contains the npm package data.

```

{
    "name": "notifier-2000",
    "description": "The future of notifications.",
    "version": "0.0.1",
    "private": true,
    "dependencies": {
        "express": "3.x",
        "body-parser": "~1.5.x",
        "nodemailer": "1.0"
    }
}
```

To install the necessary packages, run this on the command line, in the same path as the above package file.

$ npm install

#### app.js

This is the main application. Essentially the application will listen on port 1337 and wait for a POST of type application/json. When a valid POST is received, it will buffer the timestamps of webhooks and email the alert via the MyNotifierQueue and MyNotifier classes, respectively (see below).

```

var express = require('express');
var bodyParser = require('body-parser')
var myNotifier = require('./my-notifier.js');
var myNotifierQueue = require('./my-notifier-queue.js');

var app = express();
var router = express.Router();
app.use(bodyParser.json());

app.post('/tilewebhook', function(req, res) {
    if(typeof req.body['action'] !== 'undefined' &&
        (req.body['action'] === 'TILE_INGESTED' || req.body['TILE_STATUS_UPDATE']) &&
        (req.body['status'] === 'queued')
        ) {
        // Received a notification of the correct action
        notifierQueue.push((new Date()).getTime());
        if(notifierQueue.exceededThreshhold()) {
            console.log('Reached threshhold -- sending email!');
            notifier.notify();
            notifierQueue.reset();
            notifierQueue.lastNotificationSent = (new Date().getTime());
        }
    }
    res.send('Thank you for your notification.\n');
});

notifierQueue = new myNotifierQueue({from:'thedude@stackla.com', to:'anotherdude@stackla.com'});
notifier = new myNotifier();

// Get the party started
app.listen(1337);
console.log('In the zone and waiting for action.');
```

#### my-notifier-queue.js

This code contains the helper class for capturing notifications and buffering them in a queue. The items are really just timestamps, with the queue always being a fixed lenght. If the last item in the queue's timestamp exceeds the threshhold (or better yet is less than the threshhold), then we've captured the maximum number of notifications within an allowed period.

```

var MyNotifierQueue = function(config) {
    this.notifications = [];
    this.thNotifications = 5; // maximum times to receive notification
    this.thDuration = 60; // duration of threshhold measure, in seconds
    this.lastNotificationSent = 0;
}
MyNotifierQueue.prototype.push = function(timestamp) {
    if(typeof this.notifications[0] === 'number' && this.lastNotificationSent > 0 && (new Date().getTime()) - this.lastNotificationSent < this.thDuration * 1000) return;
    this.notifications.splice(4);
    this.notifications.unshift(timestamp);
}
MyNotifierQueue.prototype.exceededThreshhold = function() {
    if(typeof this.notifications[this.thNotifications-1] === 'number') {
        return ((new Date()).getTime() - this.notifications[this.thNotifications-1]) < this.thDuration * 1000;
    }
    return false;
}
MyNotifierQueue.prototype.reset = function() {
    this.notifications = [];
}

module.exports = MyNotifierQueue;
```

#### my-notifier.js

When the notification needs to be sent, this helper class will utilise nodemailer to send the notification.

```

var nodemailer = require('nodemailer');

var MyNotifier = function(config) {
    var config = config || {};
    // You will need to change the following to your default information
    this.from = config.from || 'TheSenderDude <thedude@stackla.com>';
    this.to = config.to || 'SomeOtherDude <someotherdude@stackla.com>';
    this.subject = config.subject || 'Items queued for moderation!';
    this.text = config.text || 'Hey Dude, Moderate yo Stack!';
}
MyNotifier.prototype.notify = function() {
    // You will need to change the following to your information
    var transporter = nodemailer.createTransport({
        service: 'Gmail',
        auth: {
            user: 'thedude@stackla.com',
            pass: 'WhatIsAPassword?'
        }
    });

    var mailOptions = {
        from: this.from, // sender address
        to: this.to, // list of receivers
        subject: this.subject, // Subject line
        text: this.text, // plaintext body
        html: '<b>' + this.text + '</b>' // html body
    };

    transporter.sendMail(mailOptions, function(error, info){
        if(error){
            console.log(error);
        }else{
            console.log('Message sent: ' + info.response);
        }
    });
}

module.exports = MyNotifier;
```

To start listening for webhooks, run the following command:

```
$ node app.js
```

### Configure Webhooks

With the hard part over (and let's pretend that doing the above just works), the next step is to configure the webhook URL. This is done in your Stack's Admin Portal ([https://my.stackla.com/](https://my.stackla.com)), in the _API_ > _Webhooks_ area.

In this example, we configured a service to expect to receive webhook notifications at `http://localhost:1337/tilewebhook`, but of course you will need to deploy this to a location accessible over the web, preferably via SSL.

For example:

https://example.com/tilewebhook

To test the URL, modify the following command line with your URL and verify that you get a response as below:

$ curl -H "Content-Type: application/json" -d '{"status":"queued","action":"TILE\_INGESTED"}' http://example.com/tilewebhook Thank you for your notification.

### Begin Ingesting Content

What happens when you want to grab all of the great content on Twitter hashtagged with #mongodb, but want to handpick the content to publish? In Nosto's UGC it's very easy: simply set the content to queue until you're read to publish. Create a new Aggregation Term for the Twitter Hashtag `#mongodb`. In the Moderation Tab, configure it to _Queue_ all posts, as per the screenshot below.

## Summary and Next Steps

At this point we have explored 3 concepts: creating an application that will receive data from Nosto's UGC; configuring Nosto's UGC to provide the data; configuring Nosto's UGC to acquire Social Media content and queue it.

The previous example can further be expanded to use more of the data provided by the Webhook and other Nosto's UGC API methods. Following are some examples.

* By inspecting the Tags in the payload, you can make a decision on who will be notified of the content;
* Giving more focus to the time, you can choose whether to hold notifications until the morning;
* By listening for Published status rather than Queued, you can send a notification to the person who originated the content instead -- i.e. let contestants know that their photo is available for voting!
