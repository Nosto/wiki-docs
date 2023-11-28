# Using external session identifiers

Nosto uses a cookie to set to identify browsers between their visits to the site. The purpose and usage of this cookie is very similar to how web analytics tools, such as Google Analytics, work.

Each session is identified using a randomly generated 24-character secure string and persisted in a `2c.cId` cookie.

For privacy reasons, if you have the need to disallow any third-party tracking cookies, this feature can be used to achieve that.

{% hint style="info" %}
This feature will not remove previously set `2c.cId` cookies and will only affect sessions from the time of implementation onward.
{% endhint %}

\`\`

## Changing the way sessions are handled

You can opt-out of tracking by enabling using the `setCustomerIdentifierService`method.

Assuming that your platform has a method called `platform.getSID()` that returns the current session identifier as offered by your platform - you would use:

```javascript
nostojs((api) => {
  api.visit.setCustomerIdentifierService({
    getCustomerId: () => {
      return platform.getSID()
    },

    setCustomerId: id => {
      // Do nothing as you do not want to overwrite
      // the session identifier.
    }
  })
});
```

To showcase, the flexibility offered by this API - the following example simply uses `LocalStorage` \(instead of cookies\) to persist the session identifier.

```javascript
nostojs((api) => {
  api.visit.setCustomerIdentifierService({
    getCustomerId: () => {
      // Fetch the customer from to the LocalStorage
      // or return null if there is none i.e. a new
      // session
      return localStorage.getItem('my_nosto_id')
    },

    setCustomerId: id => {
      // Save the returned customer identifer to the
      // LocalStorage. 
      localStorage.setItem('my_nosto_id', id);
    }
  })
});
```

