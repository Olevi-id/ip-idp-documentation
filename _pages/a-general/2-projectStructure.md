---
layout: post
title: 2. Project Structure
---
The IdP implementation (hereafter just IdP) is structured so that a customer can purchase IdP as a managed service (MSP) or deploy IdP themselves to an environment of their liking.

IdPn is based of an open-Source [Shibboleth IdP](https://shibboleth.atlassian.net/wiki/spaces/IDP4/overview) software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)), which obviously can't be bought as it is [free as in beer](https://en.wiktionary.org/wiki/free_as_in_beer). However, for everyone that is benefiting from the Software, we recommend consideration to support the project by [joining the consortium](https://www.shibboleth.net/membership/).

Olevi has created modules that add in to the free IdP software accumulating the set of features it provides. In the core of the product development in Olevi is also the deployment model that makes it possible to provide the software as a service for customers.

Olevi doesn't charge anything of the core IdP software, but only the modules that it has implemented. Also, if a customer purchases IdP as a service, the customer pays for the costs that the deployment and management causes.

## The Structure

IdP is based of [Java](https://aws.amazon.com/corretto/) \| [Maven](https://maven.apache.org) project management tool purposed to be deployed in an ([Docker](https://www.docker.com)) Container.

There are 3 main layers in the project structure below of project parent setup:

1. Authentication Modules
1. Container Image Build
1. Deployment Automation
   * Service Center

The levels contain sub-projects implemented with respective technologies. The Deployment Automation, also known as [devOps](https://en.wikipedia.org/wiki/DevOps), is an integral part of the project in a case where customer purchases a service.

The service offering can be ammended with a Service Center option that provides support, monitoring and maintenance of the service product.

![Three layered structure of the illustrated](../../../assets/img/maven-project-layout.svg)

## Types of Customers

Above illustration presents two ways to set up the IdP product agreement:

* Managed Service (MSP) Customer
   * MSP Customer purchaces a package of the solution including the logic and processes in deploying and managing IdP in completion or in limited setup
   * There is a recommended architecture in deploying the service to use, but IdP is possible to be deployed in variety of platforms. However, the complexity of the platform affects in costs of running the service.
* Library Customer
   * Library customer purchace only a module or multiple modules to add functionality to their own IdP. They install, manage and hold every maintenance aspect of the IdP service themselves.
   * Extensive knowledge and experience is needed to run IdP independently. However, consultation is available not only by the library vendor, but many other [commercial support partners](https://www.shibboleth.net/support/) as well.

## Purchasing Olevi

You order your Olevi instance from your reseller. Currently, contact [WeAre Solutions](https://www.weare.fi/en/contact-page/).

The cost structure is two layered:

1. Continuous charges
1. Project work to set Olevi up

Your reseller makes you an offer on continuous charges and defines a project plan with you to put up your Olevi instances in quantity and quality that you choose. It is good to understand that planning your project is work accomplished by distinguished experts that do just that as their occupation.

This practically means that probably the initial figures for the cost of putting up an Olevi and related services is a rough estimate. To find out more exact number for the price of your project probably requires work that your reseller will need to charge from you as a consulting fee.

For some examples of pricing, please refer to Olevi [marketing pages](https://www.olevi.fi/) (in Finnish).

### Continuous Charges

Continuous charges to purchase Olevi is most often three layered:

| Component | Description |
| --------- | ----------- |
| License fees | Olevi charges license fees that you pay to your reseller. This part of the cost is used to develop Olevi technologies and to provide the added functionalities that you can't still get from the base OSS product or that are unique to Olevi |
| Support Service (SLA) | Any service need taking care of to function properly over time. Your support provider charges a fee for the support that they provide based on the service level that you agreed. In tricky situations your reseller can refer to Olevi for 3rd tier support. |
| Platform costs | A platform is needed to run Olevi. Depending on your chosen deployment model, it may be that you pay for the platform directly to you platform provider, or one can be hosted for you by your reseller or support partner |

### Project Costs

In addition to continuous charges Olevi need to be put up for you to run. Olevi deployment is farely simple, but things get trickier when:
* the IdP is connected to
   * Relying Parties (RP) i.e. services
   * authentication methods
* customisations or modifications will be done to suit your needs in
   * authentication flows
   * user interface (UI)
   * user experience (UX)

Setting up an Olevi instance is project work that you define and agree with your reseller or support partner.