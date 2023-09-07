---
layout: post
title: 6.9.2023 Olevi Docker packaging update to version 1.5.0
---
> ℹ️ Related versions
> * ip-idp-lib-auth-webauthn: 1.2
> * ip-idp-lib-auth-email: 1.2
> * ip-idp-lib-auth-oidcrp: 1.6
> * ip-idp-docker: 1.5.0
> * Shibboleth IdP: 4.3.1
> * Shibboleth IdP oidc-common: 2.2.0
> * Shibboleth IdP oidc-config: 1.0.1
> * Shibboleth IdP oidc-plugin: 3.4.0

## General thoughts

Quite a lot of time passed since previous release notes. Minor updates have been done, but as we have told before, we don't write release notes on every update.

Latest updates are mostly maintenance, while some bugs have been fixed and quite many features have been added. Added features are requests from clients that add minor functionalities that have been documented in integration manuals.

Shibboleth upstream product has also done some updates fixing some flaws and preparing to major upcoming Shibboleth v5 release. That happens quite soon in coming weeks probably. Shibboleth doesn't set fixed release dates. They release when they are ready. With this update Olevi is getting better prepared to upcoming changes in major Shibboleth version update.

Please keep up with the updates to make yourself ready for next releases. We don't do backports, so only way to be ready for the updates and stay safe, is to use latest software.

Shibboleth v5 will bring breaking changes, so v5 will require fiddling with the configuration. Be prepared already during and by this update.

## Configuration changes

As a preparedness to upcoming major version update, some configurations need to be updated in flow files. See details in Shibboleth [documentation](https://shibboleth.atlassian.net/wiki/spaces/IDP5/pages/3199500962/Moving+to+Suppliers+for+accessing+HttpServlet+Objects) related to changing from _httpServletRequest_ to _httpServletRequestSupplier_ in bean parameters. The old setter has been deprecated and now is the time to fix your configuration. The support for deprecated setter will be removed in next release.

Those clients that have support contract are safe and their support partner will handle the change.

## OIDC plugin support

OIDC Plugin added new dependency as dependency to OIDC Config plugin was added. Olevi takes care of this in the packaging.

## Module updates

Module updates are mainly maintenance updates. Version increases are also related to organisational reorganisation of Olevi that we tell more on next chapter.

## Olevi organisational reorganisation

During the summer Olevi was reorganised to its own entity. This was done to clarify the development of Olevi Software. The changes don't affect customers. Customers probably saw no change at all, especially if they buy service and delivery aspetcs in addition to software packages.

These version updates that are seen in this release note relate in addition to maintenance and upkeeping of the product, to source code reorganisation to new management platform.

Customers that have bought complete packages and that are receiving the source code packages, still receive their updates from traditional channels.

We have gained new customers and have more use cases to cover and hence we reorganised the storage of Olevi Software assets that are in the form of source code. New pipelines were created that support better different kinds of customers and delivering the code and features to customers more easy.

Also test instances were installed to new platform. We are concentrating more on delivering our services from European platforms and from Finland where possible.

We will not get ready in near future, probably never. There is plenty to improve. We find constantly new ways to develop. We receive constantly high hopes and wishes from our customers. We deliver best to our ability while maintaining the stability of features currently in use.

More changes will be seen and we research constantly better, easier and faster ways to make it possible to deliver new features. While traditional delivery channels are still available for some time, we might get ready to find new delivery channels going forward. When that happens, some changes will be needed from customers, depending on their deployment. Again, your support partner will handle most of the changes, if support is part of your package.

We are very grateful for our loyal customers. It is an honor to remain at your service. Also an honor to gain trust in new customers that keep us able to do what we do.

## Olevi Groups feature

Major addition in features is the Olevi Groups feature. Groups feature make it possible to manage access management aspect in authentication based on group membership of the user. The feature adds workflows to create and manage groups in addition to request group membership as self service.

More thorough explanation of the feature is added in [documentation](/pages/h-mgmForms/0-management-interface). Previously we discussed this by initial name "Entitlement Management", but Olevi Groups is what came out. That was the exact demand from customers.

We continue to listen to your hopes and wishes. Please, let us know of your additinal wishes or those that we weren't able to satisfy with Groups feature.

## OIDC RP module now available from Shibboleth

We want to be clear that OIDC RP functionality is now available from Shibboleth upstream as well. See details in their [documentation](https://shibboleth.atlassian.net/wiki/spaces/IDPPLUGINS/pages/3013804089/OIDCRelyingPartyAuthnConfiguration).

In concrete terms this means that brokering functionality is now available from upstream Shibboleth Software. This includes both SAML2 (added already before) and OIDC (new addition) brokering.

However, we have not been able to research new Shibboleth OIDC RP plugin yet in how well it supports custom functionalities added in module provided by Olevi.

Our aim is to be as close to standard Shibboleth way of doing things as possible. When time permit, we will investigate how we can shift toward Shibboleth provided OIDC RP functionalities. We don't have answers yet, but also in this category, requests from our customers tell us the direction to go.

Olevi OIDC RP module is not going to vanish any near future and customers can be confident that their service will continue. We keep on supporting and updating our module. Still, those customers that want to shift to use Shibboleth provided module have that option as well.

## Roadmap for the future

We have been requested for more clear roadmap for future. As said before, we adapt to customer requests and the road ahead is as clear as the requests from you. However, what we are seeing in near proximity is:

* Update to Shibboleth v5 major version
* Additional improvements to Groups feature and functionalities
    * Possibility for time based expiry and renewall requests of group memberships
* Application template for Spring Boot to connect to Olevi
    * Even faster initiation of new application development
* Clarify and streamline entrypoint and initialisation scripts
    * Even faster deployment of the product for new IT-experts not still familiar with the product
* Define Olevi Identity better
    * Maintain [Zero Knowledge](https://en.wikipedia.org/wiki/Zero-knowledge_proof)
    * As little centralised identity storage as possible

Including previous plans:
* WebAuthn library updates and improvements
    * Libraries we use develop fast and we want to keep up to date with latest updates
    * Research need and possibilities to store users' Webauthn credentials in backend database
        * Support roaming users and Passkeys better
        * Maintain Zero Knowledge as much as possible
* Authentication flow improvements
    * Less clicks and better accessibility
    * Include identity proofing in stored credentials object

Wishes from our clients:

* Possible connections to Azure AD guest management
    * Still to be better formulated and defined

### Contradiction in Goals

Seasoned reader might find contradictory goals in maintainig zero knowledge and adding identity storage in centralised database. We want to make clear that Olevi still tries to know as little as possible of the user. Olevi handles personal identity data as little as possible in quantity and as in period of time holding the data.

As for centralised storage of the data, that might be technical necessity. The public key of the user might be needed in posession of Olevi to make it possible for the users to use their Passkeys in different devices. Apple started to duplicate the keys from the device to the cloud, so to keep up with the added functionalities, we might be forced to go this road as well. While doing this, we want to ensure that the data is encrypted in the manner that we can't restore the data in plain text while the user is not active in the processing.

We will need to plan very carefully what this means. While we would like to store personal data as little as possible, it is difficult to maintain this goal and gain best user experience. This is constant weighing between benefits of: 

* to maintain privacy
* improving UX

More researh will be needed.

## Ending Words

Spring and Summer have been vivid and time passed fast. There are many ideas in our sleeve not ready to be mentioned here. We have also met many possible partners that could integrate to Olevi Software easily.

Again, if you have any questions, suggestions or ideas, feel free to [contact](https://www.weare.fi/en/contact-page/) us through our reseller WeAre Solutions.