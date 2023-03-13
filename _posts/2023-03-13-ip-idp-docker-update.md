---
layout: post
title: 13.3.2023 Deployment update to version 1.3.0
---
> ℹ️ Related versions
> * ip-idp-lib-auth-webauthn: 1.0
> * ip-idp-lib-auth-email: 1.0
> * ip-idp-lib-auth-oidcrp: 1.5.6
> * ip-idp-docker: 1.3.0
> * Shibboleth IdP: 4.3.0
> * Shibboleth IdP oidc-common: 2.1.0
> * Shibboleth IdP oidc-plugin: 3.3.0

## Release status

* ✓ 8.3.2023: peer review
* ✓ 13.3.2023: release published to customers

## General thoughts

This is another release that grew a bit big. Quite many enhancements happened to hit at the same time. There has been small releases in between since previous release notes and it is necessary to remind that we don't currently write release notes from each release specifically. This is why release notes tend to collect quite a lot of enhancements that have been gathered during given development cycle, so to say.

We don't have fixed release windows nor fixed development cycles, but it seems that things gather and we bundle them up into a form of release notes given time and opportunity.

Let's go to actual major changes since previous release. As usual, our release notes will include world embracing thoughts of various things that have been thought during the development cycle. If we get feedback on the matter, we can consider adding TLDR; section in future.

## Shibboleth versions update

Shibboleth IdP software was updated to version 4.3.0. Shibboleth consortium announced that this will be last version in series 4 unless security or maintenance updates will become necessary. It is interesting to see what major version 5 will bring. One can anticipate changes by following discussions in Shibboleth's users mailing list and Project Roadmap in wiki etc, but unfortunately we currently have limited time to use on that.

Also, OIDC-plugin were updated and there were some major enhancements as well. E.g. openid-configuration comes now out of the box instead of serving it from Web Server Container's root context. We changed to use this new functionality as well. It is fare to note that openid-configuration may be a bit accurate from IdP instance to instance as the template is seldomly updated to match actual configuration of the instance. This is usually related on how much a given customer wants to put thought and time in to the issue. Openid-configuration is not often the full basis in discovery between parties, so there is not always enough reason to use time to make it accurate.

Also, OIDC-plugin has had quite a lot of changes in code base related on how action steps in authentication flow function. There has been name changes in classes and behavioural changes in abstract classes at least regarding to context navigation functions. This is not directly related to Olevi, but as customers in some cases share common code with Shibboleth upstream, in some cases one need to react to these kind of changes in the upstream.

Our intention is to keep as fresh as possible in connection to upstream code instead of leaving things behind and resolve them later. This forces us also to monitor changes in the upstream and although we have limited time to participate in actual discussions in channels, we kind of do interaction with the project via the upstream code. Until now the interaction has been unidirectional as Shibboleth Project's code is so awesome we think we are able to read developers' intentions by reading the code. We don't want to bourdon the project with our questions until absolutely necessary.

## Authentication modules release update

We finally stamped the new authentication modules with release version 1.0. Someone would say that they are now generally available, but in reality modules have been in testing use already for a quite long time now. This is related to email and webauthn modules as they are closely interconnected. Currently the webauthn module relies on identification done by email. Going future we do want to study general purpose methods of using other identity sources as well.

There is not much drama related to 1.0 version tagging. It was only a practical decision and doesn't mean they are completely ready without lack of features or so. It doesn't mean there is no room for improvement and future development. We will discuss this in later chapter.

Also OIDCRP-module was updated related to new feature requests from customers. Added features are more thoroughly presented to customer requesting features and in integration documentation of the modules itself. Customers using these modules according to their agreement will receive this info according to day to day communication.

## Major management Interface release

Most important changes in our opinion in this release is the management interface feature renewall. Previously the management interface was only a method to test authentication using Olevi IdP.

In this relase the management interface has added features to make it possible to manage customers' services on a SaaS instance. Yes, this release makes it possible to publish a SaaS instance of Olevi. The SaaS instance of the Olevi IdP will read its client metadata provided by the management interface making it possible for clients to do integration with Olevi using self service.

To integrate a service to Olevi customer needs to:
* register a customership with SaaS instance vendor (authentication, push of a button)
* register a subscription (push of a button)
    * when subscription has been accepted by a reseller (WeAre Solutions currently pushes another button)
* register a Relying Party (RP) client (fill in redirect uris, push a button)

Olevi SaaS instance will refresh client metadata and after that the customer can start using the client.

We built this around a subscription model that requires interaction with reseller. One could have done an UI based on credit card payment and hourly pricing. Quite commonly receipts and contracts are the basis to any business in Finland and western countries, so we decided to go this route. Still we feel this makes treshold to start using Olevi even lower and makes it possible to use Olevi even quite low costs instead of creating an IdP instance for each customer separately.

Please tell us how you find this new functionality and which kind of changes to it you would like to see that would serve you better.

## Roadmap for next releases

We will here shed some light to our next plans. Make note that this roadmap plan is not written in stone and we don't set milestones or schedules for feature releases. Customer interaction tells us best which we will focus to. If you want some specific feature to be prioritised or find something that is not on the list, please, contact your reseller, currently WeAre Solutions.

### Monitor Shibboleth v5 release

As mentioned, Shibboleth Consortium announced they will be releasing major version 5 in future. It will be very interesting to see what it will bring. Especially we are interested in OIDC Proxying feature that the Consortium has been developing. WeAre provided OIDC RP module brings some special functionalities we don't expect Shibboleth Proxy implementation to cover, so when the new functionality will be released we need to see how to integrate customer requested features to the new proxy implementation.

We don't want to have our own proxy module just for some reason. We believe strongly in Shibboleth code and we want to use that where ever possible. It may be possible to bring customer requested features e.g. as context check predicates or similar more limited add on functionalities or even plugin modules as per currently facilitated by Shibboleth.

Before doing any major shifts we will see what the new update brings and react only then. It may be that during summer we have better time to study new version if it is not released before that.

### WebAuthn library updates

We didn't invent WebAuthn module out of a blue ourselves. It also is based on Open Source library publicly available. Next major work task for WebAuthn module is to take updated release of the Java WebAuthn library to use. It has some backwards incompatible changes we need to reflect to, so this will be one big development cycle going forward.

We don't yet know, if it will be done as a separate release and what else will fit in to that cycle. We will see.

### Authentication flow improvements

Especially for WebAuthn flow we have recognised some improvements that can be done to the flow. Also, it will be interesting to research other identification methods parallel to email address. One might have noticed and it might have been mentioned that we were approached by [hightrust.id](hightrust.id) and [sinuna.fi](sinuna.fi) and these methods have been added to [demo instance](https://demo.olevi.fi/). However, these or similar installations have not yet been added as identification methods to WebAuthn.

Also, we have connected multiple FTN Strong Authentication Broker providers to Olevi in various use cases. It will be interesting to study how to connect them with WebAuthn better (we had one initial POC of that). Time will see what customers see important in this field.

### Management interface

In current thinking, entitlement management self service will be the next major development cycle for the management interface. Currently we have a method to express user roles as attributes using attribute resolver. However, we want to make this easier and support self service for users in entitlement / role management as well.

Idea is that anyone (in context of an instance being SaaS or MSP) could create and become an owner of an entitlement or a role and then manage members in these. Users can request access to entitlemen or role from its owner that can either accept or deny membership.

## Postfix

This is it this time. Thank you for your interest. As always, if anything was unclear or not answered, please contact your closest reseller for [answers](https://www.weare.fi/en/contact-page/).