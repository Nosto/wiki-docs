# Enabling & Disabling Popups

You can use the JS API to conditionally enable or disable a popup.

## Enabling a Popup

The following snippet enabled the specified popup.

```javascript
nostojs(function(api) {
  api.enablePopup("popupCampaignId");
});
```

### Parameters

| Field | Type | Required | Default | Reason |
| :--- | :--- | :--- | :--- | :--- |
| id | String | ✔ |  | The identifier of the popup campaign |

## Disabling a Popup

The following snippet disables the specified popup.

```javascript
nostojs(function(api) {
  api.disablePopup("popupCampaignId");
});
```

### Parameters

| Field | Type | Required | Default | Reason |
| :--- | :--- | :--- | :--- | :--- |
| id | String | ✔ |  | The identifier of the popup campaign |

