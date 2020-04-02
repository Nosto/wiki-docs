# Popup Callbacks

The JS API can be used to register callbacks to hook into the popup events. To register listener to a callback, use `api.listen(callbackId, callbackFunction)` function.

## Email Given Callback

Whenever customer has input their email address into a Nosto behavioral pop-up that asked whether they want to subscribe to an email newsletter.

```javascript
nostojs(function(api){
  api.listen("emailgiven", function(emailSubscriptionEvent) {
  console.log(emailSubscriptionEvent.email);
  console.log(emailSubscriptionEvent.newsletter);
  });
});
```

#### Fields

| Field | Type | Reason |
| :--- | :--- | :--- |
| email | String | The email address in the user input. |
| newsletter | String | Whether the user gave their consent to subscribing to a newsletter. E.g. either the pop­up prompting for the email address input was worded similarly to “Please enter your email address to subscribe to our newsletter:” or there was an explicit checkbox that the user checked to give their consent. |

## Popup Opened Callback

```javascript
api.listen("popupOpened", function(popupEvent) {
    if (popupEvent.error) {
      console.error(popupEvent.error);
    } else {
      console.log(popupEvent.campaignId);
      console.log(popupEvent.type);
    }
  });
```

#### Fields

| Field | Type | Reason |
| :--- | :--- | :--- |
| campaignId | String | The identifier of the popup campaign |
| type | String | The trigger-type of the popup campaign |
| error | String | Description of the error in the case the pop-up failed to open. |

## Popup Ribbon Callback

The customer can minimize a Nosto behavioral pop-up into a ribbon to be shown at the edge of the viewport. The pop-up is also changed to be shown in its minimized ribbon form after a page load is done after a pop-up has been shown.

This callback will be called whenever the ribbon is rendered onto screen after a page load.

```javascript
  api.listen("popupRibbonShown", function(ribbonEvent) {
    console.log(ribbonEvent.campaignId);
  });
```

#### Fields

| Field | Type | Reason |
| :--- | :--- | :--- |
| campaignId | String | The identifier of the popup campaign |

## Popup Minimised Callback

The customer can minimize a Nosto behavioral pop-up into a ribbon to be shown at the edge of the viewport. This callback will be called when the customer clicks the minimize button on the pop-up.

```javascript
  api.listen("popupMinimized", function(popupEvent) {
    console.log(popupEvent.campaignId);
  });
```

#### Fields

| Field | Type | Reason |
| :--- | :--- | :--- |
| campaignId | String | The identifier of the popup campaign |

## Popup Maximised Callback

The customer can minimize a Nosto behavioral pop-up into a ribbon to be shown at the edge of the viewport. When they click on this ribbon, the pop-up will be maximized again to be shown in full size. This callback will be called when the customer clicks the ribbon to maximize the pop-up.

```javascript
  api.listen("popupMaximized", function(popupEvent) {
    console.log(popupEvent.campaignId);
  });
```

#### Fields

| Field | Type | Reason |
| :--- | :--- | :--- |
| campaignId | String | The identifier of the popup campaign |

## Popup Closed Callback

The customer can click a “close permanently” button or link in a Nosto behavioral pop-up to dismiss the pop-up permanently. This callback is called when the customer clicks on that button or link.

```javascript
  api.listen("popupClosed", function(popupEvent) {
    console.log(popupEvent.campaignId);
  });
```

#### Fields

| Field | Type | Reason |
| :--- | :--- | :--- |
| campaignId | String | The identifier of the popup campaign |

## Coupon Given Callback

The callback will be called when a customer clicks a button inside a Nosto behavioral pop-up to get their discount coupon code.

```javascript
  api.listen("couponGiven", function(couponEvent) {
    if (couponEvent.error) {
      console.error(couponEvent.error);
    } else {
      console.log(couponEvent.campaignId);
      console.log(couponEvent.couponCode);
      console.log(couponEvent.origin);
    }
  });
```

#### Fields

| Field | Type | Reason |
| :--- | :--- | :--- |
| campaignId | String | The identifier of the popup campaign. |
| couponCode | String | The trigger-type of the popup campaign. |
| origin | String | Where the coupon code originated from. Always `popup`. |
| error | String | Description of the error in the case the pop-up failed to open. |

### Cart Abandonment Callback

The callback will be called when a customer clicks a button inside a Nosto abandoned cart pop-up to get an abandoned cart email.

```javascript
  api.listen("sendabandonedcartemail", function(sendMailEvent) {
    if (!sendMailEvent.sent) {
      console.error(sendMailEvent.message);
    } else {
      console.log(sendMailEvent.campaignId);
      console.log(sendMailEvent.email);
    }
  });
```

#### Fields

| Field | Type | Reason |
| :--- | :--- | :--- |
| sent | boolean | A boolean indicating whether the email was sent |
| campaignId | String | The identifier of the popup campaign |
| email | String | The email address to which the email was sent |
| message | String | Any error messages relating to the email sending |

