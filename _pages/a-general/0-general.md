---
layout: post
title: General
---
Subsequent pages contain general information of the Implementation.

## Documentation Practices

In this documentation we try not to repeat detailed information that is already detailed elsewhere. In aspects, where better detailed information can be found in standard specifications or widely trusted party, link to that documentation is included. We try to make links as specific as possible. Where referred documentation facilitates it, exact chapter or another anchor in the text is included in the text. If this is not possible, we try to add textual reference in connection to the link.

Internet is dynamic and evolving thing and while semantic web has not really realised, links fail sometimes. If you find broken linke, please help us to fix them with [Pull Requests](https://github.com/klaalo/ip-idp-documentation/pulls) or [contact us](https://www.weare.fi/en/contact-page/).

## Clarification of Used Terms

We try to follow mainly terms defined in [OIDC Specification](https://openid.net/specs/openid-connect-core-1_0.html#Terminology). However, there is some variance.

### IdP

We refer to the IdP service an instance of an IdP entity and in some situations your own deployment as the _IdP_, although in OpenId Connect terms the entity performing the authentication of the users is referred with term _OpenId Provider (OP)_. The term IdP dates back to older times when SAML2 was the prominent protocol for authentication. For concistency we still use the old term in this documentation although it most often refers to _OIDC OP_.

### RP or Rp

With the term _Relying Party (RP)_ we refer to the _Service Provider (SP)_ that is protecting the access to your application or service. RP is used as a term in both OIDC and SAML worlds, although in SAML the service is most often referred as the _SP_.

In illustrations _RP_ is written with lowercase _p_ (Rp). This is because of a technical limitation in the [PlantUML](https://plantuml.com) renderers that we use. They produce horrific results when both letters in the acronym are uppercase.

![Rp connecting to an IdP](../../../assets/img/rp-idp.svg)

In some situations the terms _RP_, _SP_, or _PEP_ might be used interchangeably. The term _Policy Enforcement Point (PEP)_ also refers to a technical entity, product or implementation that protects the access to application by predefined policies. As the purpose of all these are somewhat similar, these terms appear in similar use although according to a case in hand with a slightly different context. 

