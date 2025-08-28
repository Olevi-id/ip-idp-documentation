---
layout: post
title: 5. Preferred Architecture
---
In typical setup Olevi IdP is run in [Amazon Web Services Cloud Platform](https://aws.amazon.com). However, as anyone can run the IdP, there are multitude of possibilities to run it. This topic is frequently discussed, so we want to be upfront in what is our plan in providing an IdP instance.

We run the service in [an Fargate](https://aws.amazon.com/fargate/) instance. An Application Load Balancer ([ALB](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/)) sits in front of the service.

The Fargate instane is a [Docker](https://www.docker.com) Container. It relies on Docker Images that are served from AWS Elastic Container Registry ([ECR](https://aws.amazon.com/ecr/)). The container images are maintained using comprehensive devOps automation implemented using [Bitbucket Pipelines](https://bitbucket.org/product/features/pipelines).

The actual Fargate services, their configuration and the configuration of the actual IdP service are also managed using devOps automation in Bitbucket Pipelines. To give a view on how to differentiate levels on configuration to put up a functional IdP instance (or a cluster), the devOps maintenance can be seen two folded:
* Docker image build time baseline devOps automation and configuration scripts
* Runtime configuration of a specific instance and a scripting machinery to maintain it

See also [Project Structure](../2-projectStructure/) on how actual Docker image and deployment have been separated.

![Preferred Architecture](../../../assets/img/idconcept-aws-arch-simplified.svg)

There is quite a lot of details in maintaining the cloud infrastructre. It's not possible to be fully complete in this short chapter. Our intention is to document the IdP service, not running it in cloud environment. That would make another book. So, we only shortly mention that the infrastructure aspects of the platform are handled using [Terraform](https://www.hashicorp.com/products/terraform).

## Variations to Acrhitecture

As illustrated above, the IdP service usually runs in isolated network that is protected with (at least) an Application Load Balancer (ALB). However, there is (at least one) exceptions to this. Instance implementing X.509 client certificate authentication (MTLS - Mutual Transport Layer Security) is good example. Such would be used to authenticate Finnish Citisens with their Identity Cards. In MTLS the traffic between client browser and the IdP instance need to be protected with encryption from end to end. Hence ALB can't be in between.

Currently, in this special case we will expose the IdP instance to a public network with the public interface of the Fargate instance open to internet. In these cases the X.509 authentication would be **the only** available functionality of the IdP.

## Single Node or Cluster of Nodes

Clustering of a service is not simple. This topic is more thoroughly and very well discussed in [Shibboleth Documentation](https://shibboleth.atlassian.net/wiki/spaces/IDP4/pages/1265631729/Clustering).

In our case when authentication is mostly done using OIDC, making the IdP service stateless actually is comparatively simple. However, there is still important details to be taken care off, so we feel that the issue should not be taken lightly.

With further argumentation in next sections we are simply saying: although it is possible, we don't run the IdP as a cluster of nodes by default. Thorough discussion and planning should be given to following aspects before demanding clustering of the IdP service.

### Capacity planning

Shibboleth IdP software is very efficient. It can run countless of transactions with very little capacity without much of a burden. So in light of environment friendliness, our opinion is that no extra capasity should be waisted on clustering where it is not needed. AWS Fargate scales very well horisontally, so we don't see the need to cluster the service even on high traffic instances.

Further, while Shibboleth uses [Spring Web Flow](https://spring.io/projects/spring-webflow), and taking into account other aspects on how the product is built (e.g. [attribute-resolver](https://shibboleth.atlassian.net/wiki/spaces/IDP4/pages/1265631549/AttributeResolverConfiguration) architecture), functions requiring processing capacity can be easily outsourced from the IdP instance.

Actually, our opinion is that the IdP should be left to the task that it is designed for and other task should be left for those architectural components that handle them best. Saying this in other words: IdP is for authentication. Other aspects of IAM, like provisioning of identities or difficult aspects of authentication and/or access policy determination should be left to other products.

IdM (Identity Management) platforms are a perfect tools to provision user accounts to services. Policy Enforcement Points (PeP) or API Gateways are ideal to grant or revoke access to services. Policy Agents (with their big brother engines) are perfect tools to determine whether given user is authenticated to perform an operation or which operations they are entitled to. Identity Governance (IGA) is a toolset and methodology to handle and maintain aspects of identities, authentication and all things related.

None of aforementioned tasks should be done on the IdP, but the IdP should be left with its one task: the authentication.

> Sidenote: there are exceptions to this rule and things are not always black and white.

### Service Level (SLA)

Another aspect in discussions related to clustering is availability. We run the service in AWS. Availability is very important aspect for Amazon to start with. The cloud platform in itself quaranties [very good availability](https://aws.amazon.com/ecs/sla/) in the first place.

The Fargate instance running the IdP service is also defined in a manner that maintains good availability. The service has health monitoring built inside. Also, updating the instance (regular software updates and maintenance) will be done in a manner that makes sure that no instance is put in to service until it is ready and healthy.

Of course, devOps procedures can fail in all that, but clustering can't protect from procedural errors made in devOps maintenance practices.

Forcing clustering to gain even better availability what the cloud platform can offer might be counterproductive. Clustering brings complexity that can cause the service to fail more often than it would without added complexity.

If risk management is important, we would suggest on planning fault tolerancy and site reliability (SRE - Site Reliability Engineering) in completion instead of forcing it to a single component.

## From a Shared IdP Instance to Dedicated Customer Account

When run as managed service, there are still options on how the AWS account is founded. Two general options are easy to scope:

* running in solely dedicated AWS account for given customer
* running in shared AWS account

I addition to run the service in managed AWS account it is possible to run the service in customer's own already created AWS account. However, this requires access rights for support partner technicians and other aspects that need to be agreed regarding responsibilities and where demarcation line for the SLA will be drawn.

### Dedicated AWS Account

It is possible to create an AWS account that is solely allocated for the customer. Nothing in the account is shared with any other customer.

Having a dedicated account makes it possible that support partner can host other services for this customer as well. Platform expenses for the services in the account can be estimated and billed to more exact terms.

However, as all components are dedicated to the customer, customer pays for all expenses regarding hosting the services running in this AWS account. There is no other customers sharing the costs.

E.g. load balancer set up for the IdP service is dedicated for the customer and the costs are not shared. Possibly only one IdP instance is running behind the load balancer, so the load balancer cost compared to instance cost can cause 50 % of platform costs for that account if the load balancer can't be shared with some other service built for that customer in this AWS account.

### Shared AWS account

Support partners can have pre-defined AWS-accounts where IdP service can be deployed among with other IdP services for other clients. The IdP service runs in isolated container. Nothing in customer's container is shared with any other clients.

However, as the AWS account is shared, the load balancer (and possibly other frontend protecting services like Web Application Firewalls etc) serving a bunch of IdP services can be shared. The costs for frontend services can be shared between clients and the share of relative load balancer costs to a single IdP instance is smaller.

In shared environment it is more difficult to estimate and account for the costs of shared resources, so a single fixed fee for shared platform costs will be billed.

### Which Scenario Should I Choose

We recommend starting with shared AWS account. In general, expenses are lower in shared environment. If customer has strict policy for not running services in shared environments, then dedicated AWS account is needed.

If other services are planned to be hosted in managed AWS account, then it might be beneficial to set up a dedicated account for customer hosting all planned services in one account.

When customer's use of services grow, it is possible to quite simply migrate from shared account to dedicated one.