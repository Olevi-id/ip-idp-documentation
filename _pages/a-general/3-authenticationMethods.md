---
layout: post
title: 3. Authentication Methods
---
The implementation contains currently 3 authentication module libraries provided by Olevi. In addition to these, [modules](https://shibboleth.atlassian.net/wiki/spaces/IDP4/pages/1265631601/AuthenticationConfiguration) made available through Shibboleth project can be used as well. Shibboleth project is well document. Please follow previous link regarding basic modules in Shibboleth.

Each library has a separate integration manual for deployers that are available on [request](https://www.weare.fi/en/contact-page/).

## ip-idp-lib-auth-oidcrp

The OIDCRP authentication module provides proxy authentication to an external OIDC OP (Identity Provider). This is also called _authentication brokering_. External OIDC OP can be anything implementing standard protocol exchange, but following most evident variations have been tested:
* [Google Identity](https://developers.google.com/identity/)
* [Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/auth-oidc)
* [FTN Strong Authentication](https://www.kyberturvallisuuskeskus.fi/en/our-activities/regulation-and-supervision/electronic-identification)
    * [Request](https://www.weare.fi/en/contact-page/) demonstration for connected brokers

In brokering authentication, we use terms _northbound_ and _southbound_ derived from SDN routing. We apologise for possibly not quite following use of the original term, so it may need to be clarified.

![Id Brokering](../../../assets/img/idp-brokering.svg)

In illustration above, the applications relying on authentication are at the bottom (south). The actual IdPs providing the authentication methods are at the top (north). The IdP instance handling the brokering is in between.

### Supported Protocols

Currently the implementation supports OIDC and SAML2 protocols for RPs relying on authentication (south). As Shibboleth IdP is used as base product, other protocols available in Shibboleth could be used as well, but our view is that OIDC or SAML2 will suffice in most cases. Also, Shibboleth project currently supports SAML2 and OIDC and other methods are provided mainly as extensions (e.g. [CAS](https://shibboleth.atlassian.net/wiki/spaces/IDP4/pages/1265631627/CasProtocolConfiguration)).

The _ip-idp-lib-authn-oidcrp_ itself implements OIDC RP in connecting to authentication methods (north). That is the main functionality of the library.

### Feature Matrix

The OIDC RP library implementation supports only subset of features specified in OIDC Core. The authentication code flow is the mainstream on OPs, so main aspects of it is covered.

In connecting to strong authentication methods, full message level encryption is usually needed, so authentication request signing and id_token encryption is supported as well.

The OIDC RP authentication method can be configured in different needs by specifying parameters to relevant bean the library provides. Please, find more detailed guidance for configuration in integration manual. In most usual cases customers don't need to dive into details of message exchange aspects when the service is acquired as MSP.

## ip-idp-lib-auth-email

The email authentication module does two simple things:

1. validate the holder of a given email address
2. cather the emaill address to be resolved as an attribute

The module has been implemented mainly as an identification method for WebAuthn module, but it can bes used independently as well although we feel that it is a bit awkward in user experience. One can validate their ownership of an email address once, but after that it will be an annoyance.

In addition to using the module purely for authentication, it makes a good identification method where only validation of email address ownership is needed. Many applications provide email validation already, but the email authentication module makes it possible for application designers and developers to outsource the email validation fro the application using OIDC authentication which is readily available in many platforms and frameworks.

The module is quite simple and there is not much else to say. Please refer to integration manual for further details if implementing you own IdP instance.

## ip-idp-lib-webauthn

The WebAuthn module brings hard token authentication to a reach of an application relying on authentication using OIDC or SAML2. [WebAuthn](https://w3c.github.io/webauthn/) is a subset of Fido2 specification held by [FIDO Alliance](https://fidoalliance.org/fido2/).

Fido2 spectrum of specifications cover quite a lot of different aspects of providing secure means of PKI encryption with different kinds of devices and use cases. WebAuthn is a narrower approach to the spectrum providing user a means of proving their being using processes inside their browsers in connection to external token devices or secure enclaves in their devie itself.

WebAuthn doesn't take a stand in identification. A concept of an user need to be otherwise managed or handled. The WebAuthn process is thought to have 2 separate ceremonies:

1. registration
2. authentication

The act of registration precedes the act of authentication. Once the registraion is completed, authentication can happen given that the details of registration ceremony have been stored and used again to validate the data provided in authentication ceremony. The authentication ceremony response data prove that the acting user is the same person that was involved in registration.

### Distributed User Database

Our view is that the IdP should only be a passing point for the user and a holder of the session tokens while they last. After that only transient audit log information should remain from the user's existence on the IdP itself. This is also the reason that we don't store user's authentication data on the IdP.

But as mentioned, the registration data need to be stored in order to authenticate the user again. We will store it in users' own browsers in their local storage. This forms a distributed database where users hold their own information needed to authenticate again.

The user object is serialised and encrypted while on hold with the user. User can access it, but can't read it. User can remove it, but then another registration ceremony procedure is needed to authenticate again.

So by this, a pair of the authentication token and the user data object is formed. Authentication can happen only by holding complete pair. If either part of the pair is missing, a new registration ceremony procedure is needed.

### Benefits of WebAuthn

Benefits of the WebAuthn module are obvious and impressive. There are quite a lot of information already available online so we do spare readers from full glory, and only mention few as a list:

* Passwordless experience
    * Users don't need to remember difficult passwords or passphrases to get authenticated
* Improved User Experience
    * Not only the abcense of passwords, but the speed and convenience of the authentication ceremony makes the authentication process really smooth
* Improved security
    * Authentication ceremony can only be repeated with physical devices that user holds and in many implementations the device is locked so that it can't be used unless unlocked with biometric sensor 

### Means of Identification

So, given the concept of WebAuthn, a means of identification is needed. Although this was mentioned before, we feel the need to have a separate chapter on this detail to emphasize the issue.

Currently, email authentication module is used as a means of identification in default implementation of the WebAuthn module authentication flow. However, there are other methods available, e.g. strong customer authentication methods available in many european countries (e.g. [FTN](https://www.kyberturvallisuuskeskus.fi/en/our-activities/regulation-and-supervision/electronic-identification) in Finland).

One could connect OIDC RP authentication module with WebAuthn registration so that maybe an expensive or difficult identification method could be used as initial identification and after that WebAuthn would be used.

The authentication [flow definition mechanism](https://docs.spring.io/spring-webflow/docs/current-SNAPSHOT/reference/) that Shibboleth uses is very powerful. Virtually anything can be implemented by only a means of configuration.

The WebAuthn module holds one example of an authentication flow definition. It can be overwritten with anything else the deployer desires. However, quite deep knowledge is needed and usually this kind of aspects are handled when the user acquires the authentication module as part of their MSP delivery.

### Fitting of the Module in the Architecture

We anticipate that very typical question will be: "why not use WebAuthn as actual Identity Provider for the application directly, why have the IdP in between". We will discuss the benefits of an IdP in latter chapter more thoroughly.

But what one should notice regarding the WebAuthn profile, is that it is merely a describtion on how the authentication device should be used. I.e. WebAuthn profile describes the use of an authentication method. Identity provider sits in between of authentication methods and applications relying on authentication to provide other aspects of authentication as well:

* trust management
* signle sign on (SSO)
* attribute resolvation
    * possibly enriching authentication with extra data
* attribute filtering
    * making sure that only those SPs entitled to recieve the data actually receives it
* policy enforcement
    * if the SP can't handle policy enforcement (PEP), IdP can help in that as well
* managing the rainbow of different Levels of Assurance (see: [assurance level](https://www.rfc-editor.org/rfc/rfc4949))

Also, as explained in this chapter, in order to benefit from WebAuthn at all, a prior knowledge on _who the user is_ need to exist in prior to using WebAuthn. This is where the IdP is needed. In some cases implementors and deployers of the software want to handle also those difficult identification and authentication aspects in their own code and it is fine. Sometimes deploeyrs like the simplicity of an external IdP.

Hans Zandbelt has put this very well in [his blog](https://hanszandbelt.wordpress.com/2022/05/05/a-webauthn-apache-module/), so we like to refer to his posting here as well.

### Further details

As with other modules, further details of the module implementation can be found in integration manual. However, again, those aspects should rarely be taken in own hands, but leave them to an expericed IAM-specialist. We strongly suggest in acquiring the module as MSP, however, it is available as a library as well.