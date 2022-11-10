---
layout: post
title: 10.11.2022 Deployment update
---
> ℹ️ Related versions
> * ip-idp-lib-auth-webauthn: 1.0-a
> * ip-idp-lib-auth-email: 1.0-a
> * ip-idp-lib-auth-oidcrp: 1.4
> * ip-idp-docker: 1.2.0
> * Shibboleth IdP: 4.2.1
> * Shibboleth IdP oidc-common: 2.1.0
> * Shibboleth IdP oidc-plugin: 3.2.1

Quite a lot of water has been flown in the River Vantaa since the last release notes. Not that many weeks is in between, but very much have happened in IdP development. This release could be defined as service release or as pure deployment improvement since the WebAuthn module was pre-released already before as a demo. Although there is not so much original development, with this release we have made quite a load of new features and functionalities available.

## WebAuthn module is now Alpha

In the end of the summer we did a pre-release of the new authentication methods: Fido2/WebAuthn. WebAuthn is not so new in the tech field, but in our respect it is very intriquing to make it available in Shibboleth IdP and under our implementation of the product for our customers. We wrote some details of the method already in previous release notes and the method has been [documented in the chapter](/pages/a-general/3-authenticationMethods/#ip-idp-lib-webauthn) of this book.

I don't want to repeat what has been written elsewhere, but as a reminder, I would like to point out again the [blog aticle](https://hanszandbelt.wordpress.com/2022/05/05/a-webauthn-apache-module/) from Hans Zandbelt. He put it very well in regard of the architecture of authentication products.

Also, if you like, please see a short demo video of WebAuthn in action in [LinkedIn](https://www.linkedin.com/posts/karppala_tunnistaminen-oidc-ugcPost-6994282797024587776-Qo2a).

## Email module to provide identification

In summer, we had only WebAuthn. As said, WebAuthn does quite little by itself. It is merely an authentication method. However, as a response from an IdP (the service) a RP (the client) wants to have some degree of identification of the end-user (unless the use case is to [authenticate opaquely](https://www.karilaalo.fi/notes/kayttajan-voi-tunnistaa-identifioimatta) - linked article is in Finnish).

In the lack of better options we decided to use email address of the user as the means of identification. In email authentication users prove that they are owners of the adress by entering one time code that is sent as a challenge to their email addresses. The email authentication is coupled in to WebAuthn registration flow so that once the user has proved the ownership to their email address, they can use WebAuthn with fingerprint or facial recognition (among other methods) going forward.

The email authentication module can also be used separately without coupling it with WebAuthn. It would stand a good ground in a case where application need verified email addresses. Application could do email address verification without implementing the procedure to each application separately only by connecting the application to IdP implementation using OIDC or SAML2.


> ℹ️ In the lack of better identification
>
> It must be understood that we are **very well aware** of the fact that there is multitude of stronger identification methods than the email address of the user. However, *stronger* is not always *better*. Email address is something that we can use as identification method without agreeing about anything to anyone. It is something that we can provide for our customers with absolutely zero negotiation with other parties.
>
> We are aware of Finnish Trust Network (FTN), Mobiilivarmenne and such, but those are either regulated strongly according to the law or we would need to either agree on reselling the service or agree on procedures how customers can get hold of the agreement using our Identity Provider implementation.
>
> What comes to suggested Mobiilivarmenne in other channels, despite of suggestions, none such discussions have emerged that would enable us to provide these services for our customers without further and further discussions. We like to do more than talk. This is why we decided to go forward with email until discussion evolve in something that can realise to actual service that customers can buy and use.

## Identification Card Authentication

Finnish citisens can apply for an identification card provided by the government and granted by the Police. As it happens this card has means for strong identification also digitally as it has a chip that can do cryptographical operations on the card chip (not crypto as in Web3, but cryptographics as in mathematics). This provides means of digital identification of the Finnish citisens. Read more at [official page](https://dvv.fi/en/activation-of-the-citizen-certificate).

With this release we made it possible to use Finnish Identification Card as authentication method. Applications don't need to implement client certificate authentication (MTLS - Mutual Transport Layer Security), but now they can connect to client certificate (X.509) authentication by connecting to our IdP implementation with OIDC.

We want to make it very clear that this is not something that we have originally done. The X.509 module is out-of-the-box [module](https://shibboleth.atlassian.net/wiki/spaces/IDP4/pages/1265631625/X509AuthnConfiguration) in the Shibboleth IdP upstream project. In this update we merely enhanced our deployment model (ip-idp-docker) so that it is easy for us to put up a separate instance doing the client certificate authentication.

The X.509 IdP server should be a separate instance. The MTLS handshake is done on the Transport Layer and the client certificate will be requested from the browser first thing during the TLS handshake. Hence, it would be inconvenient for users of other authentication means that they would be requested the certificate details each time of authentication.

While Identitication Card is not very convenient authentication method as it needs the card reader, we see that with WebAuthn this might be very interesting. We can do the identification part only once and maybe during every blue moon. After the initial identification user could throw the card reader in the cabinet and use only fingerprint to authenticate (until the time of next blue moon when a new identification will be required for reassurance).

Please, refer also to our [article in LinkedIn](https://www.linkedin.com/pulse/suomalainen-henkilökortti-verraton-tunnistusväline-kari-laalo) regarding the Identification Card (the article is in Finnish language).

## Service release aspects

Yes, this is also a service release. OIDC Plugin on upstream Shibboleth project develops really well and fast and in this release they have been updated as well. Also, Nashorn library providing Javascript interpretation was removed from Java going to version 15 (see [separate article](https://www.linkedin.com/pulse/nashorn-removed-kari-laalo) in English in LinkedIn). It was brough as a module to Shibboleth, so instead of using manually installed Nashorn library, we now use Shibboleth provided module.

The OIDC RP authentication library was also updated to provide new feature requested by a customer.

Also some other minor changes have been made to deployment and runtime scripts to facilitate future improvement.

This update still is backward compatible and requires nothing special from customers. However, as the update brings newer versions, customers are adviced to update if they have not agreed to a fully maintained service. Customers without a service agreement can discuss about the update with their contact person in WeAre or by [contacting](https://www.weare.fi/ota-yhteytta/) the support team.