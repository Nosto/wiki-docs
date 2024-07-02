# GraphQL

GraphQL is a query language for APIs. What does this mean for you? Unlike regular SOAP or REST APIs, GraphQL gives you the ultimate flexibility in being able to specify in your API requests specifically what data you need and get back exactly that.

As a query language, it provides you with a lot of flexibility that most normal APIs will not. Without needing to recreate endpoints, you can provide developers with the same functionality as a bulk endpoint. Your queries will be cleaner and easier to understand by combining multiple queries into one request.

## What’s the difference between GraphQL API and the regular API?

The regular API is very well structured and specifically defined. The endpoints have their set requests and responses and that’s what you get whether or not that matches your usage pattern. GraphQL lets you control all of this so that the way you consume the data matches exactly what you need.

This is both a pro and a con. If your use case does not require all of the data, GraphQL can speed up your requests as we do less work on the server-side to fulfill those requests. Conversely, if you need all of the data in one request, your requests could slow down as we do more work to fulfill these requests.

## Should I use GraphQL over the regular API?

If you would like to access our intelligence engine, you must use GraphQL. The legacy REST APIs are primarily used for sending orders, products and exchange rates.

## Limitations

Geo Segments:

Geo segments rely on the ability to determine the geographical location of the end customer.
This typically requires a client-side script that can access the user's IP address. In case of a GraphQL only implementation, this should still be possible if the GraphQL calls are made directly from the end user to Nosto. If the calls are being proxied to a backend server and subsequently forwarded to Nosto, this will essentially hide the end-user's IP from the request and instead, send the backend server IP to Nosto, making it impossible to determine the geolocation of the end user.