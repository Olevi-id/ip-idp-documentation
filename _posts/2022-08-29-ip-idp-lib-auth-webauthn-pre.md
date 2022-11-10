---
layout: post
title: 29.8.2022 WebAuthn Module pre-release
---
> ℹ️ Related versions
> * ip-idp-lib-auth-webauthn: pre-release
> * ip-idp-docker: 1.1.4

Fido2 WebAuthn authentication module has been implemented for WeAre Solutions Oy Identity Provider. Many compatible authentication tokens are already available for users:
* TouchId (Safari) on MacBooks, iPads and iPhones
* FaceId on iPhones
* Yubikey
* Spear.fi Fido2 token
* Many other devices
* Many other token vendors

## The End of Password Era

WebAuthn provides smooth authentication and removes need for passwords. Benefits exceed traditional methods brightly:
* Increased security with device based authentication
* Enhanced convenience in user experience
* Improved efficiency in time used in authentication procedures

## Token Registration Ceremony

To enable authentication, users register their Fido2 token to be used with the IdP. In registration user’s token is paired with their browser.
Token is comparable to a physical key in real life. If the key were to be lost, no one can use it without encrypted registration object stored in user’s browser. To recover from the lost key or changed device, user simply performs registration ceremony again.

The registration will be initiated automatically when user without prior registration tries to authenticate. User identification is performed first, after which the WebAuthn registration happens.

By default the IdP is set so that user can also bypass the registration and use one time authentication. If user bypasses the registration, they need to perform the identification process again until registration is done. When registration is done, users can use only the Fido2 token to authenticate.

## User Identification

At first, email address is used to identify the user. The ownership of the email address is verified with a one time code that is sent to the email address user entered.

Other identification methods are researched actively. Suggestions will be warmly accepted.

## Distributed User Database

Personal data of the users is handled on the IdP only during authentication flow and session validity. Beyond session validity and mandatory audit logging, no personal data of the users is stored on the IdP. The user database is distributed by nature. User’s browser and authentication token form inseparable pair.

## Independent

As the personal data is stored in users' browsers, there are no external services to rely on in authentication. No other party is involved in authentication flow, unless specifically configured to fetch extra data from 3rd party.

## Demonstration Service

In addition to WebAuthn module, the service (RP - Relying Party) to test and demonstrate the IdP implementation was renewed. Previously separate service has been added as optional but integral context of the IdP solution. 
Going forward, the RP will evolve to management interface of the IdP. Currently RP has profile page feature that is easily fitted to separate needs.

## Policy Enforcement Point (PEP)

The achitectural component used to protect actual business application with authentication is often called Policy Enforcement Point (PEP). PEP is an RP - Relying Party connecting to the IdP. The  decision to allow or deny user access is based on the authentication information relayed from the IdP.

Most applications today already have a feature to protect the application with authentication and offer Single Sign On (SSO) capabilities. However, if such support is absent, WeAre Solutions solutions to cover these situations as well.

## Few Words on WeAre Identity Provider Solution

WeAre Solutions Identity Provider is OIDC/SAML2 compatible implementation, based on Shibboleth IdP open source software. During the span of few years, the solution has evolved based on discussions and experience on clients' needs. The solution is modular, container based and driven by thoroughly developed and constantly improving devOps automation.

The IdP Solution, as its open source upstream big brother software product, is a reference implementation and a laboratory to research authentication mechanisms needed in commercial enterprises today. At WeAre our intention is to find the best match to client’s need based on our thorough experience. Whether the product is from our own keyboard or from a friend’s or partner’s, we value the effort equally. There is a tool for each task.

## Try it yourself

Try our live demonstration deployment at: [demo.olevi.fi](https://demo.olevi.fi/)