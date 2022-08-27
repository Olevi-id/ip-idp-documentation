---
layout: post
title: b) Using the IdP
---
Using the IdP requires following things from the customer:
1. agreement
2. registration
3. metadata exchange
4. RP installation (PEP)

The purchase and deployment doesn't always happen exactly in this order and there may be separate phases in deployment where particular details are clarified back and forth more thoroughly during the deployment. IAM (Identity and Access Management) may contain complex details and it is usually better to split large problems in to smaller pieaces and resolve them one piece at a time.

## Agreement

As mentioned in chapter [2. Project Structure](../a-general/2-projectStructure.md), there is two ways of purchasing the IdP service:
* as a Service (SaaS)
* deploy your own (Library Customer)

Currently, to purchase your instance and/or library license, contact [WeAre Sales](https://www.weare.fi/en/contact-page/).

## Registration

While negotiating of your purchase, registration of your Relying Party (RP) will be mentioned. You will enter necessary technical details of your environment, which form your client entity on the IdP instance. In exchange you will receive details on how to fetch technical details of the IdP instance to add in your deployment.

## Metadata Exchange

After administrative agreement and Registration are ready, the IdP and your RP can begin technical Metadata Exchange to form technical trust between entities.

![metadata exchange between IdP and RP](../../../assets/img/idp-metadata-exchange.svg)

Metadata exchange happen very similarly whether your client will be using OIDC or SAML. As OIDC is currently the most prominent protocol, only OIDC metadata exhcange is currently described in this document. Please [request](https://www.weare.fi/en/contact-page/) for details of [SAML protocol metadata exchange](https://www.oasis-open.org/committees/download.php/35391/sstc-saml-metadata-errata-2.0-wd-04-diff.pdf) if that is required in your case.

### OIDC Metadata

#### Your RP metadata on the IdP instance

You provide the IdP with a set of redirect_uri addresses, which will be stored to your client metadata on the IdP. You will be informed tha following kind of metadata has been stored of your client entity on the IdP:

    {
        "scope":"openid phone",
        "redirect_uris": [
        "https://rp.rexample/return",
        "https://rp.rexample/client/login/oauth2/code/weareStaging",
        "http://localhost:8080/return",
        "https://localhost:8080/client/login/oauth2/code/weareStaging"
        ],
        "client_id":"your_client_xyz",
        "client_secret":"keep-this-safe-and-confidential",
        "response_types":["code"],
        "grant_types":["authorization_code"]
    }

Within your client metadata on the IdP _client_id_ and _client_secret_ are set. These are unique identifier of your RP and a secret your client need to use in order to authenticate to the IdP for it to provide your client with personal data of the users in protected manner. These details you need to configure on your RP. The OIDC Core spcification has also other means of [client authentication](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) and other behavioural aspects of your RP that might need be set differently of your client

#### IdP metadata on your client RP

IdP instance that you will be using publishes its metadata for your RP to consume so that your RP can form trusted and confidential connection with the IdP. As [defined in the specification](https://openid.net/specs/openid-connect-discovery-1_0.html), you can find the IdP metadata from a well-known endpoint. For example, in WeAre Staging IdP the metadata URL is following:

[https://weare-staging.idp.youridentityplatform.biz/.well-known/openid-configuration](https://weare-staging.idp.youridentityplatform.biz/.well-known/openid-configuration)

You will configure this endpoint URL in your RP instance. Most often regarding the metadata exchange, you don't need to do anything else but enter your _client_id_, _client_secret_ and the IdP metadata URL to your RP configuration and you are good to go.

## RP Installation

Finally, you need an RP that protects you application. Your RP connects to the IdP requesting for user authentication.

It is highly probable that you don't even need to do actual installation or deployment as OIDC is so widely accepted that the application, product, API Gateway or some other achitectural component already implements OIDC support in your environment.

However, in next chapters we provide some recommendations and examples of a products or software thet you can use as the RP implementattion if you don't already have one available.

Also, before taking any advice, you should see the [list of certified products and implementations](https://openid.net/developers/certified/).