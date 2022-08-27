---
layout: post
title: 2. Project Structure
---
The IdP implementation (hereafter just IdP) is structured so that a customer can purchase IdP as a service (SaaS) or deploy IdP themselves to an environment of their liking.

IdPn is based of an open-Source [Shibboleth IdP](https://shibboleth.atlassian.net/wiki/spaces/IDP4/overview) software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)), which obviously can't be bought as it is [free as in beer](https://en.wiktionary.org/wiki/free_as_in_beer). However, for everyone that is benefiting from the Software, we recommend consideration to support the project by [joining the consortium](https://www.shibboleth.net/membership/).

WeAre Solutions has created modules that add in to the free IdP software accumulating the set of features it provides. In the core of the product development in WeAre Solutions is also the deployment model that makes it possible to provide the software as a service for customers.

WeAre Solutions doesn't charge anything of the core IdP software, but only the modules that it has implemented. Also, if a customer purchases IdP as a service, the customer pays for the costs that the deployment and management causes.

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

Above illustration presents two types of IdP:

* SaaS Customer
   * SaaS Customer purchaces a package of the solution including the logic and processes in deploying and managing IdP in completion or in limited setup
   * There is a recommended architecture in deploying the service to use, but IdP is possible to be deployed in variety of platforms. However, the complexity of the platform affects in costs of running the service.
* Library Customer
   * Library customer purchaces only some or just one module of IdP. They and deploy and manage the service deployment themselves.
   * Extensive knowledge and experience is needed to run IdP independently. However, consultation is available not only by the library vendor, but many other [commercial support partners](https://www.shibboleth.net/support/) as well.