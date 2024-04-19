---
layout: post
title: 19.4.2024 Olevi Docker packaging update to version 2.0.0
---

Released 19.4.2024

> ℹ️ Related versions:
> * Shibboleth IdP: 5.1.2
> * Shibboleth IdP oidc-common: 3.1.0
> * Shibboleth IdP oidc-config: 2.1.0
> * Shibboleth IdP oidc-op: 4.1.0
> * ip-idp-docker: 2.0
> * ip-idp-lib-auth-oidcrp: 2.0
> * ip-idp-lib-auth-email: 2.0
> * ip-idp-lib-auth-webauthn: 2.0



Winter passed since our previous published release notes. To remind, we don't publish notes on each release. Clients receive information on releases in our internal communication channels using our usual co-operation procedures. So many releases have flown by during the winter. A lot has happened.

Winter has been very busy for Olevi. There has been production go-lives and interesting new co-operation announcement with [Vorna ID](https://vornaid.com). 

We are very glad in being able to make this new release. In addition to it giving us even more flexibility in releasing new features, it frees us time to actually implement new features. More on these below in the roadmap for future that we took as new practise in previous notes.

In respect to the roadmap in previous notes, quite many things came true in our planning. There was few mentions like:

* Maintain [Zero Knowledge](https://en.wikipedia.org/wiki/Zero-knowledge_proof)
* Research need and possibilities to store users’ Webauthn credentials in backend database

Previous musings have realised as innovation project. We are working on a new Olevi Identifier product launch with possible support from [Business Finland](https://www.businessfinland.fi/suomalaisille-asiakkaille/palvelut/rahoitus/tutkimus-ja-kehitysrahoitus/innovaatioseteli). Currently we are at innovation phase, but we already have some concrete deliveries. We will announce more information on these while plans are in such state that they are ready to be published.

But let's go to sections of our actual release.

## Shibboleth version 5

As you know, Olevi is based of Open Source upstream project: Shibboleth IdP. Olevi modules plug in to Shibboleth IdP. This release uses updated [Shibboleth major version 5](https://shibboleth.atlassian.net/wiki/spaces/IDP5/overview). There is some configuration changes that need to be fixed in updating between upstream major versions. See Shibboleth [release notes](https://shibboleth.atlassian.net/wiki/spaces/IDP5/pages/3199500367/ReleaseNotes) for details.

## Modules are plugins

In early stages we assumed term _module_ to define packages of funtionalities that Olevi adds to Shibboleth IdP. However, Shibboleth considers term _module_ as collection of pieces of configuration. See Shibboleth definitions of terms [module](https://shibboleth.atlassian.net/wiki/spaces/IDP5/pages/3199512708/Modules) and [plugin](https://shibboleth.atlassian.net/wiki/spaces/IDP5/pages/3199512741/PluginDevelopment).

In this update Olevi packages that we referred as modules are now actual Shibboleth IdP Plugins. We know this might be confusing, but especially in contractual context we still continue using term _module_ to refer to Olevi functionality package that is subject to license. To make things more clear, when referring to this kind of usage of the term, we try to add distinction in discussing Olevi modules and Shibboleth Plugins.

Migration to Shibboleth Plugins was done to streamline Plugin delivery to clients. Previously there was Maven reporitory where modules were downloaded from. After migration to Olevi version 2 no external Maven repositories are necessary.

## Name changes

There was still references to WeAre in Olevi code. Some might still be found in documentation and different places, but in this update we changed the naming to `fi.olevi`. This brings another set of necessary changes in configuration as _Java_ package names change. But we have offered technical release notes to clients that make it easy to search and replace the changed names.

## Most changes under the hood

As usual in any software implementation requiring computer to run on, they tend to evolve. Support for previous Shibboleth major version 4 will end eventually. For this reason we recommend clients to start update process quickly.

Most of the changes in the update are under the hood. Functional changes have been provided for clients in response to feature requests. All essential features implemented to previous major version of Olevi are in place in new major version as well.

We strognly recommend to test the update rigorously in client environments before taking it live in production. Even though the new release has been in use in Olevi instances already, there is multitude of different use cases that are best tested in actual environments.

## Roadmap for the future

Picked from previous plan:

* Additional improvements to Groups feature and functionalities
    * Possibility for time based expiry and renewall requests of group memberships
* Olevi Identity innovation project

Wishes from our clients:

* Possible connections to Azure AD guest management
    * Still to be better formulated and defined

Olevi near future plans:

* Vorna ID integration
* [Join](https://www.shibboleth.net/membership/) Shibboleth Consortium
* Grow client base
* Investigate new co-operation opportunities

As with any other technology company, we do also have some plans on the table that we are not ready to disclose publicly yet. Stay tuned to our channels, and especially to our [LinkedIn page](https://www.linkedin.com/company/olevi-id/) for more up to date updates and notes.

Again we feel the need to remind that Olevi is not ready. Not even after this release. Olevi will get better by each release. It happens best by appreciated feedback from our clients. But anyone can contact us with their ideas, suggestions, questions or anything. The contact form can be found on [olevi.fi](https://www.olevi.fi) front page.

You can also schedule a meeting with our distinguished developer [Karidea](https://calendly.com/karidea) directly to their calendar at the time that suits you best.

Also, you can try Olevi any time you like at our online [demo](https://test-idp.olevi.fi/). Using the demo you can start developing your software that uses Olevi free of charge.