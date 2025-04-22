# Single Sign On (SSO)

## Overview

Nosto's UGC supports Secure Assertion Markup Language (SAML), which allows you to provide single sign-on (SSO) for your Nosto's UGC account using enterprise identity providers such as Active Directory and LDAP.

By using SAML, a user is automatically verified with the identity provider when they sign in. The user can then access the Nosto Platform without being prompted to enter separate login credentials.

Key benefits of Nosto's UGC SSO offering include:

* Clients can enforce their respective password standards
* Access to Nosto's UGC is routed through the client's Identity Provider (IdP)
* Clients can revoke access to any system, including Nosto's UGC, by locking the account and/or changing the password
* Clients can implement their own Two-Factor Authentication (2FA) or Multi-Factor Authentication (MFA) standards
* End Users can leverage the same credentials they use for other systems
* Clients can implement their policies around Password resets
* Staff are discouraged from sharing their accounts

### Configuration Details

* **Protocol:** SAML 2.0
* **Relying Party Name:** Stackla
* **Supported SAML Profiles:** IdP Initiated SSO
* **Claim Type:** NameIDPolicy: urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress
* **Hash Algorithm:** SHA-256
* **Supported SAML Profiles:** SP-Initiated SSO
* **Issuance Authorization Rules:** Permit All Users

[Back to top](sso.md#section-limitations)

## Instructions

Nosto's UGC Single Sign On (SSO) offering is designed to work with organizations own Identity Provider services, such as Active Directory and LDAP, as well as online SAML services, such as Okta, Google, and SalesForce.

Setup guides for the service are available below.

* [Getting Started with Single Sign-On](http://help.nosto.com/en/articles/5793581-getting-started-with-single-sign-on-for-ugc)
* [How to configure SAML 2.0 for Nosto's UGC on Okta](http://saml-doc.okta.com/SAML_Docs/How-to-Configure-SAML-2.0-for-Stackla.html)
* [Setup Nosto's UGC as a Google SAML App](http://help.nosto.com/en/articles/5793573-sso-how-to-setup-nosto-s-ugc-as-a-google-saml-app)
* [Setup Nosto's UGC as a SalesForce Connected App](http://help.nosto.com/en/articles/5793572-sso-how-to-setup-nosto-s-ugc-as-a-salesforce-connected-app)

[Back to top](sso.md#section-limitations)
