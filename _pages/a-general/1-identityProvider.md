---
layout: post
title: 1. Identity Provider Implementation
---
Identity Provider (IdP) is an authentication server providing Relying Parties (RP) i.e. services or applications a mean of authenticating users. With an IdP a Service Provider can outsource authentication processes to an external entity instead of creating and maintaining those processes with the business application.

## Protocols

There are currently two major protocols to exchange authentication information between IdPs and Service Providers (SP) or Relying Parties (RP):

* [SAML2](http://saml.xml.org/saml-specificationss)
* OpenID Connect ([OIDC](https://openid.net/specs/openid-connect-core-1_0.html))
    * OIDC is based on: [OAuth2](https://www.rfc-editor.org/rfc/rfc6749)

WeAre Solutions IdP supports both of the above and a most of the functionality described in OAuth2 when providing means of authentication of the users to your RP.

The connection between IdPs and RPs using these protocols may get a bit confusing when _Identity Brokering_ comes in to a picture. We discuss _northbound_ and _southbound_ authentication more thoroughly in chapter [3. Authentication Methods](../3-authenticationMethods).



There exists also others, older or otherwise obsolete or narrow margin protocols. To mention few:

* [Kerberos](https://en.wikipedia.org/wiki/Kerberos_(protocol))
* Central Authentication Service ([CAS](https://en.wikipedia.org/wiki/Central_Authentication_Service))
* [Radius](https://en.wikipedia.org/wiki/RADIUS)

These or other protocols are not in the scope of WeAre Solutions IdP service, but WeAre is very powerfull copany and can provide a multitude of services. Abcense of these protocols in this implenentation out of the box doesn't mean they could not be covered with some other implementation or adjustement to this implementation.

### Strong Specifications

Major protocols are defined by specifications that are written in strong consencus and have strong community support. From mentioned protocols, SAML2 is more mature and widely adopted, where as OIDC can be considered as new challenger in the field.

While OIDC can be considered as new, it has heavy foothold as keyplayers in modern cloud services have adopted it in use.

Links to specifications are provided above.

## Single Sign On (SSO)

In the center of the two major protocols is the Single Sign On (SSO) capability. SSO refers to a feature where a user authenticates only once on the IdP and after succesful authentication and valid session on the IdP, new sessions can be created on each RP connected and trusted by the IdP.

## Authentication Methods

The IdP implements the processes for authentication and protocol exchange between itself and the RPs. The IdP is equipped with different kind of authentication methods.

Username and password are known as traditional authentication methods, although currently considered bad practice.

Different authentication methods are mor thoroughly discussed in chapter [3. Authentication Methods](../3-authenticationMethods).

## See more

See more at Wikipedia article: [Identity_Provider](https://en.wikipedia.org/wiki/Identity_provider)