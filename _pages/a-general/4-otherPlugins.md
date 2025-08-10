---
layout: post
title: 4. Other Plugins
---

The main method of operation of Olevi is to complement software readily available from [Shibboleth Consortium](https://www.shibboleth.net). Shibboleth IdP software has defined a neat plugin mechanism to complete the feature set available from consortium. Olevi plugs in to the great Shibboleth IdP software using this mechanism.

## WebAuthn

Olevi deprecated its own WebAuthn plugin Summer 2025. Below we give a little background information to the change. Storage Principal plugin chapter below explains how customers can find familiar features previously provided by Olevi WebAuthn plugin.

Olevi released the WebAuthn functionality a bit in advance from similar Shibboleth Consortiun [plugin](https://shibboleth.atlassian.net/wiki/spaces/IDPPLUGINS/pages/3878256667). Our customers were able to start providing WebAuthn authentication to users before Shibboleth plugin became officially available.

Now that the Shibboleth plugin has been released, it makes no sense to develop similar plugin by Olevi. Consortium has made great work in developing their plugin.


### Roaming WebAuthn Keys

While starting to implement WebAuthn in Olevi, our initial thought was not to create a database on Olevi IdP, but to distribute WebAuthn (Fido) principals to users' browsers' local storage. To be very clear, this is also possible on Shibboleth Consortium plugin.

However, regardless of what Olevi or Shibboleth Consortium developers have thought, Apple decided things on our and users' behalf. While it can be argumented that Fido was not meant to be roaming from one device to another, Apple decided that it can be done and did it.

Instead of making Fido tied to a single device, Apple synchronices Fido keys between devices using iCloud. Apple also introduced a term _Passkeys_ to denote their view on the specification.

Other implementations have emerged as well. E.g. see [Proton Pass](https://proton.me/pass).

It became evident that we can't swim against the current, so we need to adjust to roaming Fido keys from one device to another. This renders user's local storage useless. Meaning that IdP server side storage is needed.

While user's storage still can be used both on Olevi and Consortium WebAuthn plugin, it makes no sense to user. Olevi's WebAuthn plugin started from the perspective that user's personal data was kept on hold of users themselves. Apple has since made this idea obsolete and we have no option but to store user's personal data server side.

We do however have big ideas on how to keep users personal data very safe using encryption so that the data can only be accessed while the user is present.

All together, this does mean that server side storage is necessary for customers actually planning to use WebAuthn going forward. This is our recommendation from now on.

### Differences in the Plugins

While Consortium and Olevi plugins function obviously very similarly, there are some differences. Most important difference is that Consortium plugin doesn't provide means to store user's attribute data between authentication events.

We added a little extra so that our customers can keep the same feature set and functionalities that they have accustomed to usign Olevi's WebAuthn plugin. We created a separate plugin that stores user's data in between authentication events. We named it accordingly: _storage principal_.

## ip-idp-lib-intercept-storage-principal

Olevi Storage Principal is an Shibboleth Intercept Plugin that is able to collect and store users' personal data in between authentication events.

The purpose is to make day to day operations of the end user easier. Our customers rely on identity verification methods, which sometimes are a bit cumbersome for the users. We made it possible to store identity verificiation data and other attribute data on IdP side so that identity verification doesn't need to be done again on each authentication event.

Customers can choose and define by configuration how often or in which situation they would like users to do identity verification. Step up Authentication is the most evident example. For example: user could browse acounting data with basic authentication methods, but if they want to accept a bill for payment, they need to have their identity verified. If that is not the case based on attributes received in authentication, user could be sent back to the IdP with a request to perform identity verification.

### Server Side Storage

Server Side Storage is obvious necessity for Storage Principal functionality. Olevi's plugin uses standard Shibboleth [Storage](https://shibboleth.atlassian.net/wiki/spaces/IDP5/pages/3199509576/StorageConfiguration) service. [JDBC](https://shibboleth.atlassian.net/wiki/spaces/IDPPLUGINS/pages/2989096970) storage using [PosgresQL](https://www.postgresql.org) is tested and recommended.

The server side storage wasn't difficult choise after discussion related to WebAuthn plugin. Customers have to set up a database in any case, so we can easily plug in to same server side storage here.

It need to be noted, however, that we feel that the database data doesn't need to be strongly backed up. The data is sensitive in a sense that we want to make sure that it is not tampered with or accessed by anyone else but the user.

As the data is very easy to be reformed, there isn't very high need to back the data up. It customer were to loose the database, they can easily start from scratch. Users would need to do their registration of Fido keys and verification methods again, but we recommend periodic renewal of user data anyhow.

### Supported Authentication Methods

Currently two authentication methods are supported out of the box:

* Olevi Email Authentication
* CandourId
    * Based on [CSC plugin](https://github.com/CSCfi/shibboleth-idp-plugin-candour-id)
    * Plugs in to [CandourId](https://candour.fi) Identity Verification

Other methods can be added based on customer demand. It probably will very soon become necessary to store authentication details received from authentication methods provided by [Finnish Trust Network (FTN)](https://kyberturvallisuuskeskus.fi/en/our-activities/regulation-and-supervision/electronic-identification).

### User Issued Attributes

We have also made it possible for the users complete their own details by themselves.

In our daily operation our customers have made a note that names provided for users by banks are quite inaccurate. This is why we now make it possible for users to enter their own names.

We have learned that although user issued attributes are rarely trustworthy, users tend to like their names to be how they like them to be called. This means that users themselves currently are the best source for accurate name data.

### Olevi View in the Box

We have provided an example of interception view to implement Storage Principal functionality. As anything else in Shibboleth and Olevi, this is easily modifiable by customers and for them. The flow and view presented by Olevi is only one example and it can be easily modified and expanded to customers needs.

### Plans for the Future

We have been asked for at least following new features that we plan to ammend to this feature set:

* Phone number verification using SMS

### Let Us Know Your Needs

As always, we hope that customers interested in this functionality let us know what they hope for going forward. You can find contact details on [Olevi web pages](https://www.olevi.fi). Please click "Yhteydenotto" button in navigation bar.