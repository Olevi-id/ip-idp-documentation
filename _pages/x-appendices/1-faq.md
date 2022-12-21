---
layout: post
title: 1. Frequently Asked Questions
---

## How Olevi Compares to AWS Cognito

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

Olevi is developed based on open source product ([Shibboleth IdP](https://www.shibboleth.net)). Productalisation of Olevi is Finnish work by Finnish developers. [WeAre Solutions](https://www.weare.fi) providing Olevi service is middle sized Finnish company employing Finnish developers and paying taxes to Finland. WeAre Solutions is proud to co-operate with educational institutions and creating trainee positions where from many developers have grown all the way e.g. to lead their own company or developer team.

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
    * We recommend to [contact WeAre Solutions](https://www.weare.fi/en/aws-select-consulting-partner/)
* have a budget to adapt to special needs
    * connectors to sync user pools from external user directories
    * adaption of federated authentication to Cognito feature set
    * possibly triggered hooks to react to events during authentication flow
    * Lambda fucntions to help with authorisation decisions
        * or connectors to external authorisation service
* adapt to changes in everything in previous bullet