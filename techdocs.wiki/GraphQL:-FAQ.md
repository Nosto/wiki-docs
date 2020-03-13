### How can I prevent my test GraphQL mutations from modifying the data of my live account?

If you would like to test against your production Nosto account, we recommend adding a header `X-Nosto-Ignore` to all requests. This would still yield correct results without skewing the intelligence engine.