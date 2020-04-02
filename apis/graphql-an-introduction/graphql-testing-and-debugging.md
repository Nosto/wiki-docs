# Testing and Debugging

The GraphQL endpoints provide functionality to make it easier to test against a real account.

## Ignoring Test Requests

Every operation made against the GraphQL endpoints cause your website's data to be mutated. When doing performance testing or an equivalent, it is often necessary to exclude test traffic so as to not pollute your live account.

Using the header `X-Nosto-Ignore: True` will cause any traffic from being recorded. Queries and Mutations will work normally but any API calls containing this header will not be archived or accrue towards the statistics.

An example of how this header can be leveraged can be found on our [Nosto's GraphQL Android Example](https://github.com/Nosto/example-android/wiki/Adding-Debugging-Support) app.

## Debugging Requests

Every operation made against our GraphQL endpoint returns a unique request identifier contained in an `X-Request-Id` response header.

While we ensure that the APIs are as robust as possible if you do encounter an HTTP 5XX response from the endpoint, simply log the request identifier along with the error as the unique request identifier allows our engineers to troubleshoot the issue swiftly.

