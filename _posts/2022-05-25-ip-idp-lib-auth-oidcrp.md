---
layout: post
title: 25.5.2022 OIDC RP Library update
---
> ℹ️ Related versions
> * ip-idp-lib-auth-oidcrp: 1.3
> * ip-idp-docker: 1.0.4

This release adds many new features that were added based on feature requests from customers. Although many features have been added, in configuration wise the release is backwards compatible so that old customers can continue using the updated version as drop in replacement of the older versions. No new deprecation warnings have been added to the documentation or to the code.

As before, licensed library customers can download the JavaDoc from the repositories using regular procedures using Maven. The official technical product documentation is provided as readme.md and handed to clients by request as PDF printout.

## First Release Notes for the library

These are the first release notes that we write up for the library product itself. The product still is heavily tied to WeAre Solutions Identity Provider implementation all together, but as the product is growing quite vividly at the moment, we thought that this is also a good channel to provide our customers with the information where we are going. Also, we will now try if this is a good approach to address both types of clients that we have:
* those that use the library as part of their own implementation and deployment process
* those that rely on product deployment that WeAre is serving as part of their DigiOffice palette

Release Notes for this release are probably a bit longer than in future, but that might only be because we haven’t written this info down before. Let’s see what future brings. The releases happen quite variable rate mostly based on the customers' needs for changes and improvements. We can’t anticipate yet what the update rate will be, but currently there are quite good vibes from customers.

## Important note on new Shibboleth Identity Provider releases

Although the library has been implemented backwards compatible when it comes to library beans configuration, some changes will be needed in taking the library in use with latest Shibboleth Identity Provider product release versions (currently 4.1.6).

This change affected the library already on release 1.2 when the project structure changed (see blow).

Please make particular attention in following note that is also added in library changeLog

> ❗️ **Note on oauth2-oidc-sdk**

> After release of this library to version 1.2, you should note that recent version of oauth2-oic-sdk from Nimbus is needed. Shibboleth OIDC Common v1.1.0 includes version 9.5.3 from oauth2-oidc-sdk which is too old. The old release from Nimbus uses protected classes and the oidcrp library accesses token endpoints of the upstream OP using access token acquired during authentication. When using Nimbus with protected classes, the library can't access the token with how the library is currently implemented.

If you are running your own deployment, you need to make changes accordingly. We anticipate that Shibboleth guys will take more recent version of the Nimbus library in use, but as this is not a flaw, we are prepared to just wait that the changes happen organically.

## New features

The library grows gradually and so does the support for OIDC RP specs. With this release, the most important new feature is that:

**Decrypted upstream id_tokens supported**

Supported decryption methods are:
* A128CBC-HS256
* A192CBC-HS384
* A256CBC-HS512
* A128GCM
* A192GCM
* A256GCM
* A128CBC+HS256
* A256CBC+HS512
* XC20P

Supported JWE algorithms are:
* RSA1_5
* RSA-OAEP
* RSA-OAEP-256

The implementation is supposed to be as plain and simple as possible and hence is heavily leaning on to Nimbus Open Source OAuth2/OIDC library. So the list of supported algorithms are basically the set of algorithms that are made available by Nimbus library. The list is only added here because it was discussed with clients during the updates and it is anticipated that the discussion will continue.

Make note, that this relates only to id_token received from the upstream OP. The decryption support for downstream clients are configured in basic Shibboleth IdP configuration (OIDC Plugin or SAML2 settings).

## Private_key_jwt client authentication to upstream OP

In addition to id_token encryption, we added a feature to support private_key_jwt client authentication to the upstream OP token endpoint. This is actually very neat authentication method in comparison to client_auth_basic in a sense that no secrets need to be exchanged between the providers. If security is essential, surely the identity of the other provider should be ensured while exchanging the public key, but the messaging channel doesn’t need to be secured in regard to possible eavesdropping.

## Relay authentication request claims from downstream RP

Feature to relay authentication requests to upstream OP was added. It can now be configured which claims in the authentication request should be relayed in authentication request to upstream op. If downstream authentication request is not signed, the values are taken from the HTTP request attributes, if they are available.

## Minor changes to support some OPs

Some upstream OPs require special configurations to take in account their peculiarities. We added possibility to:
* send custom non-standard aud value in client_assertion when getting id_token from upstream OP
* add acr_values to upstream request (standard OIDC Core feature)
* disable authentication request signing even when sigJWK is set

The library is set up so that if it is provided with the signature key, the authentication requests will be signed. The same key is used on token endpoint to sign client_assertion. However, according to integration guide of an OP, the authorisation request should not be signed even though private_key_jwt client authentication is used. This forced us to add this option.

All of these mentioned configuration options are optional and need only be used with OPs that require this kind of special attention.

## Noteworthy feature updates from previous release

As we mentioned before in our blog article (in Finnish), Microsoft has interesting approach in interpreting OIDC Core regarding iss claim in id_token they release from Azure AD platform in a case where guest user from another tenant authenticates with application client that is registered in the actual tenant. The tenant id of the another tenant is released.

Because of this in earlier release we added a feature to accept any iss-claim value in the id_token. This also is an optional setting which of course is not set by default.

We discussed quite a lot if we should have whitelisted set of acceptable iss values, but decided to go with this approach as there are other aspects securing the setup. However in some special cases a separate interceptor or flow action step probably should be considered.

## Project setup
Going to release version 1.2 we changed the project setup according to illustration below.

![Project structure of the illustrated](../../../assets/img/maven-project-layout.svg)
> ❕ Make note
>
> this is not the original historical illustration in the release notes but illustration updates as the implementation evolves

As said, there are customers using the library directly as part of their own implementation and customers that rely on WeAre DigiOffice setup. The parent project was initiated to manage software dependencies of the current library and libraries to come more easily. Also the devOps deployment project that WeAre uses is based on this parent project. In their own deployment, clients can rely on WeAre parent project setup as well or create their own based on their own needs, which is probably the most relevant in the case.

## Other relevant changes

Not related to OIDC RP library directly, but in conjunction to this release the WeAre Identity Provider devOps deploy automation pipeline has changed and improved significantly. This is mentioned in reference to the library as the library is so integral part of WeAre IdP DigiOffice solution. See also our notes on roadmap of the devOps deploy in general.

## Roadmap

We have plenty of ideas for improvement going forward. Most intriguing idea is to bring new authentication methods that we are not ready to disclose in detail yet. There is also plan to improve the administration, management and monitoring of the IdP instance.

Currently the management of the instance is based on DigiOffice agreement and service palette of ours, but automation based on self-service instead of devOps specialist initiation of changes is highly wished feature.

Currently the deployment is based on a house made method that we haven’t refined to be released as product. At least not yet, that is. Currently we don’t support using our devOps automation in customer premises or customer’s own cloud, but we are interested in going forward with that field of sight as well.
