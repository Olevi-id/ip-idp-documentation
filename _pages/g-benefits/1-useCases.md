---
layout: post
title: 1. Use Cases
---
In chapter [authentication methods](../../a-general/3-authenticationMethods/) we presented only briefly some use cases for the IdP. Let's dive a bit deeper in different scenarios one might have. This is not exhaustive list and given examples are only examples.

Olevi IdP is very versatile product and can adapt to many different scenarios that we (the developers) have not even invented yet. If you have a problem, please, [let us know](https://www.weare.fi/en/contact-page/) so that we can study if it can be solved or alleviated using authentication.

## Authentication Method Consolidation

With Id Brokering one can connect multiple Identity Providers to single broker entity and connect services to that broker.

![Id Brokering](../../../assets/img/idp-brokering.svg)

Typical example is where two companies have merged but user migration is still unfinished. Company A) uses Google Workspaces and company B) uses Azure AD. Both companies have vast amount of users that need to migrate to use common platform. It may have been decided that merged company uses only Azure AD going forward, but company A) has vast amount of files and documents on Google Workspaces so that migration takes time.

Also, company A) has connections to multiple external services using [Google Identity Platform](https://cloud.google.com/identity-platform/) so that users in company A) can use Google Credentials to access thir party services.

During migration period and even after migration preparing for the future it might help that instead of connectin third party service authentication to either identity platforms (Google or Azure AD), an Identity Provider will be taken in to use so that users from both platforms can access same services using their old credentials in their own company platform before the merger.

![Authentication Merger Consolidation](../../../assets/img/useCase-idpMergerConsolidation.svg)

More difficult option to handle the merger case is to connect both platforms directly to both platforms. Many application providing Single Sign On (SSO) can connect only to one IdP. This is not lost cause still, as one can either expedite migration of group of those users that need to access the service most or try to find methods to federate another platform using second one.

Also, in worst case scenario where all applications need to be connected to both platforms, authentication provider selection need to be implemented to each service separately. In broker case the authentication method selection can be done on the broker IdP instead of implementing it differently on each service.

![Authentication Merger Without Consolidation](../../../assets/img/useCase-idpMergerWithoutConsolidation.svg)

## Authentication Flow Actions

In modern service architectures many such things that have previously required pre-provisioning or user data synchronisation before user can access the service, can be achieved with only authentication and during authentication. Olevi, powered by Shibboleth IdP, can do Actions during authentication flow to fulfill tasks that have traditionally been handled using Identity Management Systems (IdM) or syncing and replication services. There are two main possibilities:

* authentication flow [Context-Check intecept](https://shibboleth.atlassian.net/wiki/spaces/IDP4/pages/1265631716/ContextCheckInterceptConfiguration) mechanism
* completely customised authentication flows using [Spring Web Flow (SWF)](https://shibboleth.atlassian.net/wiki/spaces/IDP4/pages/1265631859/GeneralArchitecture#Use-of-Spring-and-Web-Flow)

Many modern authentication providers provide similar functionality through web-hooks that trigger external functions to add extra functionality. While this can be done using Olevi as well, being a code product, also more complicated functionalities can be programmed to be processed during authentication.

However, as we have stated elswhere in this document, one should still keep tools to what they are purposed to. Olevi is mostly a tool for authentication. It is fine line between overusing your tool for something it is not. While planning authentication flows, deep understanding in overall architectural big picture need to be maintained for not to step over that fine line. Stepping over the line might not harm immediately, but hinder future development or migration to other products.

### Enrhichement of Authentication Data

_Attribute resolvation_ is out of the box action in authentication flow provided by Shibboleth IdP software. It is very powerful tool that Olevi can use to enrich the authentication data with attributes that might not available from authentication method.

For example, Olevi can connect to [FTN](https://www.kyberturvallisuuskeskus.fi/en/our-activities/regulation-and-supervision/electronic-identification) (Finnish Trust Network) broker for authentication, but FTN provides only limited set of attributes for the user. SSN (Social Security Number) is mandatory identifier for a user received from FTN.

However, SSN might not be very useful user detail for many services as traditionally SSN has been protected very carefully in application databases. Applications tend to use separate userIds for users instead of SSN. With customised attribute resolvers Olevi can connect to external user databases to exhange user identifier received from authenticatioon to actual user identifiers in organisation's database. SSN can be exchanged to actual userId or other surrogate identifier used for the users in the application.

In addition to exchanging identifiers, Olevi can fetch extra data from user database.

User databases can be in various sizes and forms. Communication to user database is most simply done using e.g. HTTP/Rest or Olevi can connect to LDAP directory. But being a code product, there is no limitation on how Olevi can connect to external databases.

Also there is no limit in number of external databases Olevi can connect.

![Enrichement of User Attributes](../../../assets/img/idp-attribute-enrichement.svg)

### Just In Time (JIT) User Provisioning

One example of authentication flow action step might be creating and updating user profile to a service that requires pre-provisioned user profiles.

![Jit provisioning](../../../assets/img/useCase-jitProvisioning.svg)

## Protocol Adaptor

JIT provisioning brings us an excellent real world use case example. [As per documentation](https://help.salesforce.com/s/articleView?id=sf.sso_jit_about.htm&type=5), Salesforce tells us that JIT Provisioning is possible for SSO-users that are able to connect their IdP to Salesforce using SAML2. What if an organisation has only OpenId Connect (OIDC) provider, or a strategical ground rule that only OIDC will be used in connecting to services?

SalesForce also [states](https://help.salesforce.com/s/articleView?id=sf.sso_provider_openid_connect.htm&type=5&language=en_US) that OIDC providers can be connected, but that requires expensive license. Also, JIT user provisioning is currently documented only for SAML2 connections, not for OIDC.

Olevi can act as a protocol adapter between organisations actual Identity Provider and the service adapting one protocol to another.

![Protocol Adaptor](../../../assets/img/useCase-protocolAdaptor.svg)

In use case that SalesForce has documented, organisation can save moving from one license edition to another by connecting authentication with SAML2 instead of OIDC that requires different license edition.

Also, it might be that the service that organisation relies on supports only one kind of protocol. It might be that services migrate to modern architectures and support only OIDC where organisation has only SAML2 capable Identity Provider in use.

## Tested integrations

This is initial and non-exhaustive list of integrations that have been tested using Olevi. Not all tested and tried integrations have been mentioned in detail.

### Identity Providers

* Azure AD
* Google Identity
* Telia Tunnistus (FTN)
* Nets eIdent (FTN)
* [Hightrust](https://www.hightrust.id)
* [Sinuna](https://sinuna.fi) is decomissioned

### Relying Parties (Cloud, SaaS)

* AWS Web Console
* Atlassian Access
* Citrix Netscaler
* Samltest
* [Entra ID](https://learn.microsoft.com/en-us/entra/external-id/direct-federation)

### Relying Party Products

* Apache mod\_auth\_openidc
* Spring Security

