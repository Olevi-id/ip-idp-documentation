---
layout: post
title: 1. Frequently Asked Questions
---

## How Olevi Compares to AWS Cognito?

[Amazon Cognito](https://aws.amazon.com/cognito/) is very capable and frictionless customer identity and access management product that scales

> Amazon Cognito provides an identity store that scales to millions of users, supports social and enterprise identity federation, and offers advanced security features to protect your consumers and business. Built on open identity standards, Amazon Cognito supports various compliance regulations and integrates with frontend and backend development resources.

Both Cognito and Olevi work in very similar space and provide very similar functionality. They both provide a service that Service Provides (SPs) can rely on to get users authenticated. Cognito expresses in their documentation that it does also authorisation. In a sense this is true as Cognito implements [User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) that can be reflected on while making decisions on authorisation.

Olevi does not claim taking responsibility of authorisation, but it is handed over to other architectural components. Basic idea is that Relying Parties have Policy Enforcement Points (PEP) that make decisions on authorisation according to the needs of the application based on information relayed in authentication. In our thinking the service itself and developers implementing the service know best what kind of authorisations are needed. On the other hand, Olevi can connect to external auhtorisation or policy server that can best handle and hand over the information about authorisation.

As said many times in different occasions, we believe that each tool should be used to what it does best. Olevi does authentication best. Although authentication is the most important task for Olevi, it **can do authorisation** as well, if needed. Shibboleth has comprehensive attribute-resolver which can not only connect to external services, do basic decision scripting. We don't emphasize this capability as we think that there are better tools for this task.

### Pools vs Statelessness

Olevi strives to be stateless in a sense to authentication. Olevi strives to know of the user as little as possible. Olevi strives not to have user directory, but instead it handles personal data needed to process authentication on the fly during the authentication flow and session validity. After those have passed, personal data will be forgotten.

Cognito can connect to [Identity Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html) and do federated authentication. However, how Cognito is built and how authorisation decisions are made, it usually is necessity to store user data in User Pool. Quite often services relying on Cognito use User Pools synchronised to Customer Management System (CRM) or Human Resources (HR) system as master data source of the user information. These systems are linked and synchroniced.

Olevi can connect to multitude of user directories (mainly using LDAP, but also with customised or out of the box connectors), but Olevi in itself doesn't store user data (on authentication component). The directories are external and only their APIs are accessed during authentication flow.

### Processing of Personal Data

While using Cognito you can choose where (geographically) personal data is stored, it is in the cloud. By choosing Olevi, personal data footprint in the cloud is lesser when personal data is distributed to persons themselves (using email+WebAuthn). Or you can choose to host user directory in more isolated environment while processing only authentication in the cloud. In tightiest setting, you can host the complete setup on-premise. 

### SaaS vs Everything as a Code

Cognito is a SaaS service bought from the cloud. It does exactly and entirely what it claims. Customisation of Cognito UI is possible, but under the hood functionality of Cognito is what it is. It can't be modified.

Cognito customisation still is possible with customised UI or by programming connectors or connecting to external authentication services. In this sense we think that Olevi can be continuation of Cognito. We anticipate that going forward we will be responding requests from organisations using Cognito, if we can support their specific use case.

The strong authentication in Finland (FTN - Finnish Trust Network) is concrete case example of this. It is very possible to connect Cognito to FTN Broker without anything specific in between. However, if anything special in the flow would be needed to be added or to comply with very serious requirements for cryptographics in FTN, there may very well be situations where external support will be needed in a form of adapting these different components to talk with each other.

Olevi is a product based on open source upstream product where everything is implemented as a code. If needed, every aspect of Olevi can be modified and suited to customers' specific needs directly in Olevi implementation rather that developing separate connectors or modifiers that suit business needs to SaaS feature supply.

### AWS vs Host Anywhere

Cognito is best suited to protect services with authentication and authorisation in Amazon Cloud. It surely can be used to provide authentication also outside af AWS hosted services, but that is not what it is built for.

While Olevi is run in AWS by default, it can be run anywhere. It can be run in any cloud or even on customer's own premises. 

While Olevi is run in AWS it can provide authentication to any service in the Internet.

### Pricing

Cognito is basicly free of charge until 50 000 monthly active users (mau). Users pricing gap for Olevi is not harcoded but based on risk assesment and customer's business's scope. In theory customers serving millions of users can use Olevi with significantly lower costs than any commercial SaaS service.

For small user pools Cognito is very probably less expensive than Olevi in product pricing. However, prepare to pay for consultation and adapting Cognito to your specific use case or business need.

### Non-functional Aspects

Olevi is developed based on open source product ([Shibboleth IdP](https://www.shibboleth.net)). Productalisation of Olevi is Finnish work by Finnish developers. 

Cusstomers investing in Olevi invest in Finnish innovation and product development. Cognito is run by Amazon, which is United States based company.

### When to Choose Cognito

Choose cognito when:
* your payload (the services) run on AWS
    * and you plan to keep it that way until forseeable future

and you have: 
* very well specified and documented use cases
    * use cases are evaluated across Cognito feature matrix
* homogenic user base
    * or you can easily group your users in separate pools

and:
* you connect to or provide small set and very simple authentication methods
   * username/password (not recommended)
   * federated authentication to social media providers or
   * federated authentication to well established and ready identity provider
   * standard MFA mechanisms

When choosing Cognito, prepare to:
* know the product
* agree and order external consultation support from well established and specialiced organisation
* have a budget to adapt to special needs
    * connectors to sync user pools from external user directories
    * adaption of federated authentication to Cognito feature set
    * possibly triggered hooks to react to events during authentication flow
    * Lambda fucntions to help with authorisation decisions
        * or connectors to external authorisation service
* adapt to changes in everything in previous bullet

## Is Olevi Strong Authentication Provider?


No. Olevi is a tool. One tool can't provide Strong Authentication. Being a Trust Service is much more than one tool. It is about policies, processes, practices, controls and much more.

As a concept, Strong Authentication in Finland is connected to [Finnish Trust Network](https://www.kyberturvallisuuskeskus.fi/en/our-activities/regulation-and-supervision/electronic-identification), where mostly banks offer their customer credentials as means to strong authentication and Telco operators offer [Mobiilivarmenne](https://mobiilivarmenne.fi/eng/) method that is a certificate on end users SIM-card.

Another variations of the question is:

> Is Olevi compliant to FTN?

This question is relevant especially in Spring 2023, when rest of the requirements in new [M72B regulation](https://www.kyberturvallisuuskeskus.fi/sites/default/files/media/file/M72B_2022_MÄÄRÄYS_72B_tunnistus-_ja_luottamuspalvelut_ENG_julkaistu.pdf) by Traficom take effect.

Then, another variation of the question can be articulated as:

> Is Olevi compliant to Strong Authentication?

or 

> Is Olevi compliant to PSD2?

The answer is the same. No. Olevi can't be compliant by itself. These regulations are about much more than just one tool.

### Real world analogy

Let's take a real world analogy to explain. You buy green energy from your energy company to your home. The company promises in an agreement that the energy you buy is produced according to green energy principles without waisting excess natural resources and without burning fossil fuels.

You plug your portable power bank in your home to charge it with this green energy. Then you go about your business and charge your mobile phone with the power bank.

Is your power bank a green energy provider?

No, it is not. To make your power bank green, there are much more to consider than just energy source. You need to consider e.g. how the power bank was manufactured or how it will be disposed when the time comes. Do you use it until the end of its lifecycle and then recycle it appropriately?

### Olevi FTN compliance

Olevi is being used as part of FTN and it is being used to connect to FTN. In technical terms Olevi provides necessary features for an organisation to be compliant provider or client in FTN.

As regulations change and if there is demand from customers, Olevi will implement new features to support changing regulation. Currently and quite often the support is already built in. If in doubt, [contact](https://www.olevi.fi/) for details.

### Could we use Olevi technologies to provide trust services

Yes you can, if your organisation has the means and capabilities to provide those services in general. If you are interested in using Olevi as part of your services, [contact](https://www.olevi.fi/).

## What is an Instance?

Definition of term _instance_ comes up as a question often while discussing Olevi pricing. Olevi charges fees on licenses. License fees are based on instances.

In Olevi terms, instance is one IdP service. Possibly not always, but practically most often amount of instances equal to separate URL addresses that customer has as IdP services. For example, your test instance has separate URL from your production instance, so you have two instances.

In OIDC terms one OpenId Connect Provider (an issuer) is one instance. In OIDC _iss_ is an unique identifier for an OpenId Connect Provider (an IdP).

Olevi scales vertically in terms of performance. Number of nodes is a separate thing from instances. An Olevi instance is one instance regardless of the number of nodes needed to run the instance. To say this in other words, Olevi license model doesn't charge fees based on CPU cores or other performance related aspects.

Based on your deployment model you either pay for performance directly to your platform provider yourself or you pay to your support service provider if they host the service for you as a service.

> #### Defition of an instance
>
> Olevi instance is singular logical entity of an IdP service that most often equals to unique URL or OIDC _iss_ of the IdP service.

## Do I Pay for Open Source while Purchasing Olevi

No, you don't. Olevi is based on open source software Shibboleth IdP. When you agree to license terms and make an agreement to purchase Olevi, you agree to the price that includes only Olevi Software that Olevi has created. The price you pay for the software ensures the development and continuity of the software and products and services that are based on the software.

While you pay for the software Olevi has created, Olevi gains some profit in doing its business. That profit goes to owners of Olevi company that actually are the developers of the product. When you purhace Olevi software you ensure that the developers can pay for the food and shelter they need to be able to provide their expertiese.

While Olevi gets income by selling the software, there is some expenses to run the company. Olevi is still a young company and income flow is limitied but Olevi has committed to join [Shibboleth Consortium](https://www.shibboleth.net) that provides Shibboleth software. Shibboleth Consortium funds development of Shibboleth Software. The member fee of Shibboleth Software will be an expense to Olevi company.

License fee for Olevi covers the purchase of Olevi Software. However with a small portion the customer of Olevi is also supporting the development of Shibboleth software. 

Despite the fact that Olevi will apply to be a member in Shibboleth Consortium, in addition to buying Olevi license we engourage our clients and our resellers to become members to Shibboleth Consortium directly as themselves as well.