Use api.customer() function to send information about the current customer to Nosto. After the information is sent, current visit is updated with latest details.

Typical situation when manual update is needed if your site handles login with AJAX compared to regular implementation where login requires a page load and the following page includes customer tagging.


```js
nostojs(function(api) {
  api.customer({ email:"jane.doe@example.com", first_name:"Jane", last_name:"Doe", marketing_permission: true });
});
```