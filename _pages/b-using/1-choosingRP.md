---
layout: post
title: 1. Choosing an RP Product
---
As said in previous chapter, there are multitude of products and implementaations available for OIDC. You should start your journey from the [list of certified products and implementations](https://openid.net/developers/certified/).

Also, if still planning on relying on SAML, there also is very wide range of different solutions.

First things first, there is an important announcement that we need to make:

## Do Not Plan on Writing Your Own

This is so important that we want to dedicate a complete sub-section for this. OIDC is fairly simple to implement. We see it very often that developers take the product reference documentation and start coding their own implementation. Sometimes even IdP product vendors call their IdP as _authentication API_ where the service developer is thought to connect.

This is where things may fail very severly. Even though OIDC, JSON and [JWT](https://www.rfc-editor.org/rfc/rfc7519) are fairly simple, the protocols and specifications define quite detailed **security** and **privacy controls**.

OIDC is simple in a sense that developers can get their implementations working quite easily and then they think that _it just works_ and move on to next problem. When _it just works_, it is not said that it implements the **security controls** that are bakes deep in the specifications.

With this in mind, if you had initial thought that you will implement your own OIDC client, please give it another thought. There is multitude of thoroughly implemented products already. What makes you think that your own implementation would be at all better or **secure**?

One *better* step forward from the thought of writing your own is to use a ready library or the functionality already in the dvelopment framework or coding platform that you are using. However, even with those, you should make sure that you are using the authentication functionalities to their full extent when it comes to securing your application.

Now, sorry about the rant and let's move forward.

We will give few examples of possible products to use as Relying Party RP implementation.

## Apache HTTPD & mod_authn_openidc

Hans Zanbelt has written very good implementation of OIDC RP, the [mod_auhn_openidc](https://github.com/zmartzone/mod_auth_openidc). [Hans Zanbelt](https://github.com/zandbelt) is widely respected specialist it the field and provides support for the product through his [company](https://zmartzone.eu).

mod_authn_openics plugs in to [Apache HTTPD](https://httpd.apache.org) Web server that [can function](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html) fairly efficiently as authentication [reverse](https://en.wikipedia.org/wiki/Reverse_proxy) HTTP Proxy.

Make note that previous paragraphs contein huge amount of details. We don't repeat all technical details here, but please follow the provided links.