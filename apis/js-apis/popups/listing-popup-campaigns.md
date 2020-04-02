# Listing Popup Campaigns

You can use the JS API to list all the popup campaigns. This may be handy in situations when you would like to fetch all campaigns and iterate them to decide whether they should be shown or not.

```javascript
nostojs(function(api) {
  console.log(api.popupCampaigns());
});
```

## Fields

| Field | Type | Reason |
| :--- | :--- | :--- |
| id | String | The identifier of the popup campaign |
| name | String | The name of the popup campaign |
| type | String | The trigger-type of the popup campaign |

