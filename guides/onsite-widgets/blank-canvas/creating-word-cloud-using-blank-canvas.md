# Creating a Word Cloud using Blank Canvas

{% hint style="warning" %}
You are reading the **Classic Widget Documentation**

Classic widgets are a previous widget version for onsite widgets created before September 23rd.

From September 23rd 2025, all new widgets created will be NextGen.

Please check your widget version on the **Widget List page** to see if it is **Classic** or **NextGen** widget.

You can read the [Nextgen widget documentation here](../../widgets-nextgen/).
{% endhint %}

* [Overview](creating-word-cloud-using-blank-canvas.md#overview)
* [Layout](creating-word-cloud-using-blank-canvas.md#layout)
* [CSS](creating-word-cloud-using-blank-canvas.md#css)
* [Javascript](creating-word-cloud-using-blank-canvas.md#javascript)
* [Final Result](creating-word-cloud-using-blank-canvas.md#final)

## Overview

Nosto's UGC Blank Canvas Widget allows users to very quickly and easily design their own Widget templates to suit their organisation's requirements.

In this guide we are going to create a fixed-dimension Word Cloud, leveraging [jQCloud](http://mistic100.github.io/jQCloud) which will also auto-refresh after a set period.

In the guide we are going to move through each of the Mustache Partials to build the final Widget.

[Back to Top](creating-word-cloud-using-blank-canvas.md#top)

## Layout

For the Layout Partial we will be simply defining the relevant classes where we will want to render the Word Cloud. The class names we will be used as inline with what our version of [jQCloud](http://mistic100.github.io/jQCloud) (2.0.3) recommends.

```
<div class="wall first">
	<div class="tagwrap">
		<div class="wall__tags"></div>
	</div>
</div>
```

For this particular Blank Canvas there is no need to actually reference the Tiles partial as we won't be actually loading any individual pieces of UGC.

[Back to Top](creating-word-cloud-using-blank-canvas.md#top)

## CSS

Following the definition of the Layout Partial, we can now start to define the styling of our Word Cloud. In here the most important items we have defined relates to the various sizes and colours of the elements in the Word Cloud (Elements: `div.jqcloud span.jqcloud-word.w1 - div.jqcloud span.jqcloud-word.w10`) and the overarching Styling for the Wall.

```
@import url('https://fonts.googleapis.com/css?family=Open+Sans:300,400,700');

/**
* Tag Colours & Weights
*/
div.jqcloud span.jqcloud-word.w1 { color:#888 }
div.jqcloud span.jqcloud-word.w2 { color:#444; font-weight: 700; }
div.jqcloud span.jqcloud-word.w3 { color:#666; font-size: 35px; }
div.jqcloud span.jqcloud-word.w4 { color:#000; font-size: 38px; }
div.jqcloud span.jqcloud-word.w5 { color:#888; font-size: 40px; }
div.jqcloud span.jqcloud-word.w6 { color:#e63650; font-size: 42px;}
div.jqcloud span.jqcloud-word.w7 { color:#444; font-size: 44px; }
div.jqcloud span.jqcloud-word.w8 { color:#222; font-size: 46px; }
div.jqcloud span.jqcloud-word.w9 { color:#000; font-size: 48px; }
div.jqcloud span.jqcloud-word.w10 { color:#e63650; font-weight: 700; font-size: 50px;}

/**
* Styling for Wall
*/
.wall {
	display: inline-block;
	position: absolute;
	top: 0;
	background: #fff;
}
.wall.first {
   left: 20px;
}
.tagwrap {
	background: #fff;
}
body, html {
 	margin: 0;
	padding: 0;
	height: 700px;
	background: #fff;
}

.jqcloud {
	font-size: 20px;	
}

.jqcloud-word{
	text-transform: uppercase;
	font-family: 'Open Sans', sans-serif;
	font-weight: 300;
}

.jqcloud-word::before {
	content: '#'
}
```

[Back to Top](creating-word-cloud-using-blank-canvas.md#top)

## Javascript

Finally comes the most important part, loading the [jQCloud](http://mistic100.github.io/jQCloud) Javascript Framework. For this example, we have used jQCloud 2.0.3, however if you use earlier or later versions the behaviour maybe slightly different. You will need to download the jQuery JS file, host it and then update the below code to reference it in order for the below Javascript to work.

Now in our Javascript we are defining how frequently we would like our Widget to refresh, the size of our Widget and the delay for loading each element. For this same, we've set it to refresh twice after 1000ms and have set the Widget size to 850px wide by 700px tall.

```
var refreshTime =  1 * 1000;
	refreshed = false; 

 Stackla.loadJS([
	 "jqcloud.js"
 ]).then(function () {
	 
	 // Load tiles data from selected filter
	var refreshCloud = function () {
		Stackla.loadTilesByFilter(function (tiles) {
			if (!refreshed) Stackla.render({tiles: tiles});
			var results = [];
			var hashtags = tiles.forEach(function(post){	
				var retrievedHashtags = getHashTags(post.caption);
				if(retrievedHashtags.length){
					results.concat(retrievedHashtags);
					retrievedHashtags.forEach(function(tag){
						results.push(tag);
					});
				}

			}, {
				limit:2
			});
			var words = processResults(results);
			
			if (!refreshed) {
				setTimeout(function () { 
					$('.wall.first .wall__tags, .wall__tags_clone').jQCloud(words, {
					width: 850,
					height: 700,
					delay: 2,
					encodeURI:true,
					afterCloudRender: function () {}
				});
				}, 1000);
				refreshed = true;
			}
		});
	};
	 
	 // get the cloud
	 refreshCloud();
	 
	 // set an interval to refresh
	 setInterval(refreshCloud, refreshTime);
 });

function getHashTags(inputText) {  
    var regex = /(?:^|\s)(?:#)([a-zA-Z\d]+)/gm;
    var matches = [];
    var match;

    while ((match = regex.exec(inputText))) {
        matches.push(match[1]);
    }
    return matches;
}
function findWithAttr(array, attr, value) {
    for(var i = 0; i < array.length; i += 1) {
        if(array[i][attr].toLowerCase() === value.toLowerCase()) {
            return i;
        }
    }
    return -1;
}

function processResults(results){
	var words = [];
	results.forEach(function(tag){
		var wordIndex = findWithAttr(words, "text", tag);
		if(wordIndex < 0){
			// if there's no match
			var word = {
				text : ''+tag,
				weight : 1
			};
			words.push(word);
		}else{
			words[wordIndex].weight +=1;
		}
	});
	return words;
}
var that = this;
```

[Back to Top](creating-word-cloud-using-blank-canvas.md#top)

## Final Result

The Final Result will look something like below. It is important to note, that depending on the Filter you are using, how much content you have associated with it, and the variation of words will all have an impact on the end design.

jQCloud will ignore words which are only ever mentioned once, and if it cannot differentiate the occurrence of certain words, it may skip some of the size classes we defined in the CSS.

```html
<script type="text/javascript">(function (d, id) 
{ var t, el = d.scripts[d.scripts.length - 1].previousElementSibling; 
    if (el) el.dataset.initTimestamp = (new Date()).getTime();
    if (d.getElementById(id)) return; t = d.createElement('script'); 
    t.src = '//assetscdn.stackla.com/media/js/widget/fluid-embed.js'; 
    t.id = id; (d.getElementsByTagName('head')[0] || d.getElementsByTagName('body')[0]).appendChild(t); }
(document, 'stackla-widget-js'));
</script>
```
