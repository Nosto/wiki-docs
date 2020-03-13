This guide introduces you how to implement Nosto on a e-commerce store that does not yet have a dedicated plugin solution. The articles are designed as digestable step-by-step guides that walk you through how to establish the data exchange between the site and Nosto.

First its important to understand a few of the concepts before delving in to the documentation, to make sure that you as a integrator understand the different steps and tools required. 

* Nosto initializes by using a [snippet of Javascript](https://github.com/Nosto/docs-nosto-com/wiki/Add-Nosto-script) that starts the dialogue to your dedicated account. 
* Nosto depends on structured data that we extract from your site. At the bare minimum you need to go through all the steps listed under [Manual Implementation - Essentials](https://github.com/Nosto/docs-nosto-com/wiki/Manual-implementation).
* You can augment and extend the product tagging and the data collection process with the help of other articles found under this guide. 
* You can access your own account in the Nosto admin UI ([https://my.nosto.com/admin](my.nosto.com/admin)) where you can enable/configure/modify features. All templating and layouts are handled within your account.

In case you have already implemented Nosto and established continuous data exchange you can find more information related to troubleshooting or setting up features at [help.nosto.com](https://help.nosto.com/). 

**Implementation Checklist**

As a first step, please check if your e-commerce platform or software is already supported by a Nosto extension or integration. In case the platform is supported we warmly recommend to use an extension and reading the platform specific guide as this saves you some manual effort and time, since the implementation process differs a bit from the one described in this article series and practically automates all the steps.

Second, make sure that you have a Nosto account and if not, sign up on Nosto home page or contact Nosto sales.

Every Nosto account has a unique identifier known as accountID, which is required in the implementation. If you know that you have an account, but don’t know your accountID, log-in to your Nosto admin panel and learn where you can find it!

Third, if you first implement Nosto on a local device or on a development environment, bookmark this article about implementing Nosto on a test environment for later reading. No need to jump there yet, as it is also featured as the last article in this series.