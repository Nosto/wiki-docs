# Automation Rules API

## Overview

Nosto's UGC offers a full suite of API endpoints for Automation Rules allowing customers with API access to make all appropriate requests namely GET, POST, PUT, and DELETE.

Just like all other REST API calls, Rate Limits, Authentication and Authorisation processes, and Content Type and Accept Headers should all be followed when making your API calls. This information is defined [here](https://github.com/Stackla/docs/blob/master/docs/api-docs/rest/README.md) within the Developer Portal

## API Calls

### Create Rule

Resource URL

POST https://api.stackla.com/api/tilerules/?access\_token=\[Access Token]\&stack=\[Stack Name]

The Request Body must include **Rule Name** (`name`), [**Trigger**](https://github.com/Stackla/docs/blob/master/automation/trigger/README.md) (`trigger`), [**Action**](https://github.com/Stackla/docs/blob/master/automation/actions/README.md) (`action`), whether the Rule is active (`enabled`), and whether it is for Tiles or Assets (`type`).

```
{
    "name": "Rule Name",
    "trigger": { "$or": [ ] },
    "action": [ ],
    "enabled": 1,
    "type": 
}
```

Included below is an example of a filled-out rule-based.

```
{
    "name": "Rule Name",
    "trigger": {
        "$or": [
            {
                "image_tags": {
                    "$eq": "animal"
                }
            },
            {
                "image_tags": {
                    "$eq": "human"
                }
            }
                     
        ]
    },
    "action": [
        {
            "type":"$addToSet",
            "field":"tags",
            "value":"1"
        },
        {
            "type":"$addToSet",
            "field":"tags",
            "value":"2"
        },
        {
            "type":"$set",
            "field":"location",
            "value":{
                "name":"Crows Nest NSW",
                "latitude":-33.8255695,
                "longitude":151.1949095
            }
        },
        {
            "type":"$set",
            "field":"stackla_sentiment_score",
            "value":1
        },
       {
            "type":"$set",
            "field":"status",
            "value":"published"
        }
    ],
   "enabled": 1,
   "type": tile,
}
```

### Fetch All Rules

Fetch all Automation Rules associated with your Stack

GET https://api.stackla.com/api/tilerules/?access\_token=\[Access Token]\&stack=\[Stack Name]

### Fetch Specific Rule

Fetch all conditions specified for a Specific Automation Rule on your Stack

GET https://api.stackla.com/api/tilerules/\[Rule ID]?access\_token=\[Access Token]\&stack=\[Stack Name]

### Update Specific Rule

Update a Specific Automation Rule on your Stack

PUT https://api.stackla.com/api/tilerules/\[Rule ID]?access\_token=\[Access Token]\&stack=\[Stack Name]

### Delete Specific Rule

Remove a Specific Automation Rule on your Stack

DELETE https://api.stackla.com/api/tilerules/\[Rule ID]?access\_token=\[Access Token]\&stack=\[Stack Name]

### Retrospectively apply a Rule

Apply an Automation Rule to Tile or Asset records that already exist in your Stack

PUT https://api.stackla.com/api/tilerules/\[Rule ID]/apply?access\_token=\[Access Token]\&stack=\[Stack Name]

[Back to Top](api.md#top)
