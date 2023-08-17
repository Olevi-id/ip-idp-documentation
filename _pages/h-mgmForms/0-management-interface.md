---
layout: post
title: h) Management Interface
---
Olevi Identity Provider, that is based on open source software product Shibboleth IdP, is very versatile tool and can do many things. Verstaility brings complexity. One major motivation for us with Olevi is to make difficult things more approachable. We understand very well that our mumbling on very complex authentication practices is unclear and we work very hard to make it better comprehensible.

This is why we created a Management Interface tool for Olevi. We call it _mgmForms_. This is a name we started to work with initially, so it may very well be that it will change to something else while it evolves.

Initially the tool was only a test interface for the Identity Provider. It allowed us and clients to test authentication process and see the results. Over time we needed more features and now it does a lot more.

MgmForms has been implemented with [Spring Boot](https://spring.io/projects/spring-boot). Spring Framework has created a simple [initialiser](https://start.spring.io/;) tool, that makes it simple for anyone to start their own application development. Olevi is ready to be plugged in as an Identity Provider for Spring and hence we are using this combination ourselves as well.

## Olevi SaaS

It is a deliberate decision that we don't advertise it loudly, but Olevi can also be purchased as SaaS. The most typical use case is to purchase Olevi installed as a MSP deployment or to be run on customer's own platform. However, the Management Interface makes it possible to request a Relying Party (RP) registration using self service workflow and to start using Olevi Identity Provider for authentication without specialiced installations or difficult negotiations for an agreement.

The workflow to register Olevi client is quite intuitive and we will not provide step-by-step instructions. We expect clients to be experienced in Authentication and Authorisation Infrastructure (AAI). However, Olevi has close connections to support providers that can help in integrations to AAI even if client is not be that experienced in the field.

Feel free to [contact](https://www.weare.fi/en/contact-page/) if further advice should be needed.

## Olevi Groups

Most anticipated and requested feature when starting to implement Olevi was **Groups**. While one would think that organisations know their users and users' rights and entitlements, reality is that they don't. Simple task of managing admin access to an application can be cumbersome even in most sophisticated organisations. If not in general, in many and so numerous corner cases.

When we tell that authentication is simple using Olevi, the first question always is that how can we separate authorised users from others. Based on basic principles of AAI, RBAC (Role Based Access Control) is given. It is obvious thought that users bring their attributes with them during authentication and access management can be based on the details that we know about the user based on authentication. Reality is different.

Nowadays users bring only their identity while authenticated. Organisations have little understanding in roles and entitlements of their users. This is where Olevi Groups come in.

Using Olevi Management Interface, anyone (in selected scope of installation or SaaS) can create a group and to call and accept members to join it.

Olevi Groups has only few prerequicities:

* group admin and members have to register Olevi Identity
* group admins and members need to be informed of processing of their personal data

Personal data processing will be done in the scope of Management Interface. Olevi provides test and production instances for general use. In addition to general instances, clients can purchase their own instance based on Olevi [license terms](https://www.olevi.fi/Olevi-License-Terms.md) by agreeing with a reseller.

### Create a Group

Anhyone with access to Olevi Management Interface can create a Group. Groups are created in the scope of the Management Interface. E.g. general Olevi test instance has its own scope while production instance has another. If clients have purchased their own instances, clients can decide the scopes of groups.

Inside each scope, the Groups are common. In general Olevi instances anyone with registered Olevi ID can request membership to any Group. It is up to Group admins wether they accept or deny requests. Certain restrictions apply to limit possible bad actors trying to harass Group admins.

### Manage a Group

While created, the creator of the Group becomes Group admin. Group admins can accept or deny Group memberships. People get access to a Group only after admin has accepted the request. Group admin can also remove member from the Group at any time.

Group admins see list of members in the Group and their identifiers and some personal data of the members. Group admins and members become informed of their personal data processing from a Privacy Policy that the Management Interface client has published or in case of general Olevi instances, from [Olevi Pricvcy Policy](https://www.olevi.fi/tietosuoja) document.

Olevi Privacy Policy has been published with CC license so that it is easy for any customer to start planning their personal data processing. Read further details from the Privacy Policy document.

### Request Membership

After Group admin has created the Group, people can request access to it. On Group page, admins can copy a link to membership request page of the Group and send it to people they want to invite as members to the Group. The link is open for anyone that has registered an Olevi Id in the scope of the instance.

### Manage Access

Once Group has been created and members have been granted memberships to Groups, members are granted with Group claim while authenticating using their Olevi ID and Olevi Identity Provider. Groups that users have been accepted as members are listed in _groups_ claim that is released from userInfo endpoint of the Identity Provider.

Relying Parties (applications) can then make decision on access to specific features based on user's _groups_ claim. _groups_ claim is an array of groups the user has been accepted as a member like so:

    "groups": [
          "heyYouGroupIsThis",
          "heiHooTassaToinen",
          "UusiRyhmaTyhja",
          "worldAdmin",
          "reSeller",
          "testiRyhmaUusi",
          "toinenTestiRyhmaUusi"
        ]

## We don't send emails

Olevi Groups doesn't send transactional emails. Also this is an intentional decision we made (which can be challenged as any other opinion that we have).

Only transactional emails regarding Olevi Groups are the one time pin codes (OTP) that will be sent to verify that users have access to email addresses they claim to have.

There is enough of phishing already and we don't want to add to that.

We don't inform when

* new Group has been created
* user has requested an access
* admin has accepted or denied access to a Group

Our principle is that people talk to each other. It is not our task to decide what channel or what kind of notification method should be used for each user. We think that Group admins know their users and Group members know their admins. People in Groups talk and discuss. It is not our job to make discussion possible. That is a task for other products.

We think that people should talk more. We are not experts in communication.

Our advice is that if you feel that

* you should belong to a Group
    * contact the Group admin
* your people should be in a Group
    * invite them
* you want to accept your people to a Group and just did it
    * let them know

To make it clear: we don't communicate about Groups access. It's your job.