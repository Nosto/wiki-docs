# Samples

## Overview

Nosto's UGC Automation Rules can be extended far beyond just the Triggers and Actions offered in the Admin User Interface Criteria Builder to cater to a range of bespoke use cases for our clients. The following guide provides some sample code to assist clients who are keen to take advantage of this functionality

**Note:** It is recommended that you are familiar with Automation Rules [Triggers](https://github.com/Stackla/docs/blob/master/automation/triggers/README.md) and [Actions](https://github.com/Stackla/docs/blob/master/automation/actions/README.md) before attempting any of the following custom use-cases

## Trigger Samples

Coming Soon

## Action Samples

### Apply Shopspot

Apply a Shopspot Product Tag (`tag`) to a pre-defined area (`coords`) on an Image Tile at the point of aggregation

.

```

         "action": [
	 {
	      "type": "$set",
	      "field": "hotspots",
	      "value": [
		      	{
		      		"type": "point",
		      		"coords": [50,50],
		      		"tag": {"id": 123456}
		      	}	      
	      	]
         }
```

[Back to Top](samples.md#top)
