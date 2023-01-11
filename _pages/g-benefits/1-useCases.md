---
layout: post
title: 1. Use Cases
---
In chapter [authentication methods](../../a-general/3-authenticationMethods/) we presented only briefly some use case for the IdP. Let's dive a bit deeper in different scenarious one might have. This is not exhaustive list and given examples are only examples.

Olevi IdP is very versatile product and can adapt to many different scenarious that we the developers have not even invented yet. If you have a problem, please, [let us know](https://www.weare.fi/en/contact-page/) so that we can study if it can be solved or alleviated using authentication.

## Authentication Method Consolidation

With Id Brokering one can connect multiple Identity Providers to single broker entity and connect services to that broker.

![Id Brokering](../../../assets/img/idp-brokering.svg)

Typical example is where two companies have merged but user migration is still unfinished. Company A) uses Google Workspaces amnd company B) uses Azure AD. Both companies have vast amount of users that need to migrate to use common platform. It may have been decided that merged company uses only Azure AD going forward, but comany A) has vast amount of files and documents on Google Workspaces so that migration takes time.

Also, company A) has connections to multiple external services using [Google Identity Platform](https://cloud.google.com/identity-platform/) so that users in company A) can use Google Credentials to access thir party services.

During migration period and even after migration preparing for the future it might help that instead of connectin third party service authentication to either identity platforms (Google or Azure AD), an Identity Provider will be taken in to use so that users from both platforms can access same services using their old credentials in their own company platform before the merger.

![Authentication Merger Consolidation](../../../assets/img/useCase-idpMergerConsolidation.svg)

More difficult option to handle the merger case is to connect both platforms directly to both platforms. Many application providing Single Sign On (SSO) can connect only to one IdP. This is not lost cause still, as one can either expedite migration of group of those users that need to access the service most or try to find methods to federate another platform using second one.

Also, in worst case scenario where all applications need to be connected to both platforms, authentication provider selection need to be implemented to each service separately. In broker case the authentication method selection can be done on the broker IdP instead of implementing it differently on each service.

![Authentication Merger Without Consolidation](../../../assets/img/useCase-idpMergerWithoutConsolidation.svg)

## Authentication Flow Action

