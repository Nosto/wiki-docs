# How to Use the Blank Canvas to Create a Twitter Count Widget

{% hint style="warning" %}
You are reading the Classic Widget Documentation

Nextgen widgets are a new and improved way to display UGC content.&#x20;

Read the [Nextgen widget documentation here](../../widgets-nextgen/).
{% endhint %}

* [Introduction](blank-canvas-to-create-a-twitter-count-widget.md#introduction)
* [The common approach](blank-canvas-to-create-a-twitter-count-widget.md#common-approach)
* [Blank canvas approach](blank-canvas-to-create-a-twitter-count-widget.md#bc-approach)
* [Steps](blank-canvas-to-create-a-twitter-count-widget.md#steps)
* [Conclusion](blank-canvas-to-create-a-twitter-count-widget.md#conclusion)
* [Widget code for reference](blank-canvas-to-create-a-twitter-count-widget.md#code-ref)

## Introduction

Blank Canvas provides a fairly flexible way to display the information ingested in Visual UGC and allows us to create unique widgets. Sometimes the desired outcome is not to display the actual user generated content, but to use the ingested items count to unlock discounts (or any sort of information) when those numbers hit some sort of thresholds. This was the case of one of our clients: they wanted to create a progress bar and unlock five offers in a particular event.

[Back to Top](blank-canvas-to-create-a-twitter-count-widget.md#top)

## The Common Approach

Usually this is achieved by creating a custom application that uses the API to retrieve a summary from within a certain term or filter. This has a set of cons:

1. To host a custom app implies more work and higher costs.
2. It may imply using a database.
3. The developer uses only the API and doesn’t leverage the widgets editor.

[Back to Top](blank-canvas-to-create-a-twitter-count-widget.md#top)

## Blank Canvas approach

Advantages:

1. Faster development
2. No hosting needed

[Back to Top](blank-canvas-to-create-a-twitter-count-widget.md#top)

## Steps

1. The first step may look like a workaround but it is valid still and reflects the flexibility of the platform. By default, Nosto's UGC provides the UGC data as the pieces of information to render. This is not what we need for this particular case, so we can ignore/remove this callback all together: _Stackla.loadTilesByFilter(function (tiles) {…});_\
   Instead, we will use our own API call:

```

$(document).ready(function() {
    $.get('https://12345678.execute-api.ap-southeast-2.amazonaws.com/staging', function( resp ) {
        ...
    });
});
```

3.  That particular endpoint refers to Nosto's UGC API: _https://api.Stackla.com/api/filters/123456/summaries?aggregated\_by=default\&stack={your\_stack}\&access\_token={your\_access\_token}_

    It was proxied with AWS' API Gateway and cached with AWS' Cloud Front. This was done in order to prevent Nosto's UGC API to be hit more than 450 times during the 15 minutes time window. That method of proxing is reliable, easy to set up and cheap.
4. The returned summary needs to be parsed/manipulated and then we can invoke the following method:

```

Stackla.render({...});
```

Where the parameter is a valid JSON element.

7. The “layout” provided by the blank canvas framework will be rendered with the provided information. Please refer to _https://mustache.github.io/_ to understand about the templating language.
8. The CSS can now be edited to style the widget and make it responsive and nice-looking.
9. If there are some resources, we can use the media library provided by Stackla. By doing so, we avoid using a third party system to host the images, fonts or any file we may need.

[Back to Top](blank-canvas-to-create-a-twitter-count-widget.md#top)

## Conclusion

The development process was quick and straightforward, even though this was the first time we used the Blank Canvas Widget in this way.

```

Stackla.GoConnect.setText(placeholderName, newPlaceholderValue)
```

[Back to Top](blank-canvas-to-create-a-twitter-count-widget.md#top)

## Widget code for reference

### Javascript

```

var threshold1 = 1000, threshold2 = 2000, threshold3 = 5000;
var initialDegrees = 0;
var animTime = 1000;
var totalStages = 4;

Number.prototype.format = function (n, x) {
    var re = '\d(?=(\d{' + (x || 3) + '})+' + (n > 0 ? '\.' : '$') + ')';
    return this.toFixed(Math.max(0, ~~n)).replace(new RegExp(re, 'g'), '$&,');
};


$(document).ready(function () {
    $.get('https://abcdef123456.cloudfront.net', function (resp) {
        if (resp.errors.length === 0) {
            var nTweets = 0;
            if(typeof resp.data !== 'undefined' && typeof resp.data[0] !== 'undefined' && typeof resp.data[0].quantity !== 'undefined') {
                nTweets = resp.data[0].quantity;
            }

            //Mocked!
            //nTweets = 500;
            Stackla.render({
                extra: {
                    tweets: nTweets,
                    offer: [
                        {
                            num: 1,
                            enabled: nTweets >= threshold1,
                            threshold: threshold1.format(),
                            name: "Offer 1",
                            description: ["Promo 1","Description promo 1"],
                            clickthrough: "https://www.fake-host.com/offers/12345"
                        },
                        {
                            num: 2,
                            enabled: nTweets >= threshold2,
                            threshold: threshold2.format(),
                            name: "Offer 2",
                            description: ["Promo 2","Description promo 2"],
                            clickthrough: "https://www.fake-host.com/offers/23456"
                        },
                        {
                            num: 3,
                            enabled: nTweets >= threshold3,
                            threshold: threshold3.format(),
                            name: "Offer 3",
                            description: ["Promo 3","Description promo 3"],
                            clickthrough: "https://www.fake-host.com/offers/34567"
                        }
                    ]
                }
            });
            if (nTweets  0.33) {
            percComplete = 0.33 + 1 / (threshold2 - threshold1) * (tweets - threshold1) * 0.33;
            if (percComplete > 0.66) {
                percComplete = 0.66 + 1 / (threshold3 - threshold2) * (tweets - threshold2) * 0.66;
                if (percComplete > 1) {
                    percComplete = 1;
                }
            }
        }
        var timePerStage = parseInt(animTime / step);
        window.setTimeout(function () {
            animateRecursive(0, step, timePerStage);
        }, 0);
    }, 1000);
};
```

[Back to Top](blank-canvas-to-create-a-twitter-count-widget.md#top)

### CSS

```

body {
    border: 0;
    overflow: hidden;
}
.container {
    overflow: hidden;
    h1 {
        margin-top: 2px;
    }
    .offers-cont {
        margin: 0 auto;
    }
    .clearfix {
        overflow: auto;
    }
    .offer {
        float: left;
        width: 362px;
        height: 275px;
        background-color: #343433;
        color: #666565;
        margin: 5px;
        padding: 10px;
        text-align: center;
        .enabled {
            position: relative;
            width: 100%;
            height: 100%;
        }
        h1 {
            margin-top: 15px;
            margin-bottom: 10px;
        }
        .btn-cont {
            position: absolute;
            bottom: 30px;
            padding: 10px;
            border: 1px solid #FFF;
        }
        .desc {
            margin-top: 12px;
        }
        &.enabled {
            background-color: #db0626;
            color: #FFF;
            a, a:active, a:hover {
                color: #FFF;
                text-decoration: none;
                font-size: 1.3em;
                padding: 0;
                margin: 0;
            }
        }
    }
    .offer-1,
    .offer-2,
    .offer-3 {
        position: absolute;
        text-align: center;
        .separator {
            margin-left: auto;
            margin-right: auto;
        }
    }
    .offers-cont {
        margin-top: 60px;
    }
    .separator {
        background-color: #db0626;
        height: 4px;
        width: 55px;
        margin: 15px 0;
    }
    .stages {
        overflow: hidden;
        max-width: 700px;
        width: 100%;
        margin: 0 auto;
        transition: background-image 1.5s ease;
        background: url('https://web-assets.stackla.com/someclient.stackla.comhttp://sdp-media.dev/images/customimages/2016-11/1479441831/stage1.png') top center;
        background-size: contain;
        &.stage-2 {
            background-image: url('https://web-assets.stackla.com/someclient.stackla.comhttp://sdp-media.dev/images/customimages/2016-11/1479441822/stage2.png');
        }
        &.stage-3 {
            background-image: url('https://web-assets.stackla.com/someclient.stackla.comhttp://sdp-media.dev/images/customimages/2016-11/1479441818/stage3.png');
        }
        &.stage-4 {
            background-image: url('https://web-assets.stackla.com/someclient.stackla.comhttp://sdp-media.dev/images/customimages/2016-11/1479441810/stage4.png');
        }
    }
}
```

[Back to Top](blank-canvas-to-create-a-twitter-count-widget.md#top)

### Layout

```


<div class="track clearfix">
    <div class="container">
    {{#extra.offer}}
            <div class="offer-legend offer-{{num}}">
                <h1>{{name}}</h1>
                <div class="separator"></div>
                <div class="desc">{{threshold}} tweets to unlock</div>
            </div>
    {{/extra.offer}}
    <div class="stages">
            <div></div>
        </div>
        <div class="offers-cont clearfix">
    {{#extra.offer}}
    <div class="offer o{{num}} {{^enabled}}disabled{{/enabled}} {{#enabled}}enabled{{/enabled}}">
    {{^enabled}}
                    <div class="disabled">
                        <h1>{{name}}</h1>
                        <img src="https://s3-ap-southeast-2.amazonaws.com/client/assets/gray-lock.png" />
                        <div class="desc">Will be unlocked<br/>after {{threshold}} tweets</div>
                    </div>
    {{/enabled}}
    {{#enabled}}
                    <div class="enabled">
                        <h1>{{name}}</h1>
                        <div class="desc">
    {{#description}}
    {{.}}<br/>
    {{/description}}
    </div>
                        <div class="btn-cont">
                            <a href="{{clickthrough}}" target="_blank">Claim now</a>
                        </div>
                    </div>
    {{/enabled}}
    </div>
    {{/extra.offer}}
    </div>
    </div>
</div>
```

[Back to Top](blank-canvas-to-create-a-twitter-count-widget.md#top)
