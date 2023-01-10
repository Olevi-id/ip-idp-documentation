---
layout: post
title: 1. Choosing an RP Product
---
As said in previous chapter, there are multitude of products and implementations available for OIDC. You should start your journey from the [list of certified products and implementations](https://openid.net/developers/certified/).

Also, if still planning on relying on SAML, there also is very wide range of different solutions.

Following sections that present RP solutions contain huge amount of details. We don't repeat all technical details here, so please follow the provided links if mentioned terms sound unfamiliar. Also, we don't currently give an example code snippets on how to configure recommended solutions. Most implementations provide the code snippets already.

We are planning in publishing a demonstration template implementations that you could use as starting point for your application if such can't easily be found yet.

First things first, there is an important announcement that we need to make:

## Do Not Plan on Writing Your Own

This is so important that we want to dedicate a complete sub-section for this. OIDC is fairly simple to implement. We see it often that developers take the product reference documentation and start coding their own implementation. Sometimes even IdP product vendors call their IdP as _authentication API_ where the service developer is thought to connect.

This is where things may fail severly. Even though OIDC, JSON and [JWT](https://www.rfc-editor.org/rfc/rfc7519) are fairly simple, the protocols and specifications define quite detailed **security** and **privacy controls**.

OIDC is simple in a sense that developers can get their implementations working quite easily and then they think that _it just works_ and move on to next problem. When _it just works_, it is not said that it implements the **security controls** that are baked deep in the specifications.

With this in mind, if you had initial thought that you will implement your own OIDC client, please give it another thought. There is multitude of thoroughly implemented products already. What makes you think that your own implementation would be at all better or **secure**?

One *better* step forward from the thought of writing your own is to use a ready library or the functionality already in the development framework or coding platform that you are using. However, even with those, you should make sure that you are using the authentication functionalities to their full extent when it comes to securing your application.

Now, sorry about the rant and let's move forward.

We will give few examples of possible products to use as Relying Party RP implementation.

## Apache HTTPD & mod_auth_openidc

[Hans Zanbelt](https://github.com/zandbelt) has written very good implementation of OIDC RP, the [mod_auhn_openidc](https://github.com/zmartzone/mod_auth_openidc). Hans Zanbelt is widely respected specialist in the field and provides support for the product through his [company](https://zmartzone.eu).

mod_auth_openidc plugs in to [Apache HTTPD](https://httpd.apache.org) Web server that [can function](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html) fairly efficiently as authenticating [reverse](https://en.wikipedia.org/wiki/Reverse_proxy) HTTP Proxy.

The mod_auth_openidc project already has very good configuration documentation and example configuration snippets that you can use to get your project started.

You can find one sample of using the module with Olevi IdP [here](https://github.com/klaalo/ip-auth-proxy-template).

## Spring Framework and Spring Security

If Java is your language, [Spring Framework](https://spring.io/projects/spring-framework) is very likely your platform. [Spring Security](https://spring.io/projects/spring-security) adds [OAuth2 and OIDC support](https://docs.spring.io/spring-security/site/docs/5.2.0.RELEASE/reference/htmlsingle/#oauth2) to applications that are written in Java with Spring.

If you are only just starting your application, [Spring Boot](https://spring.io/projects/spring-boot) is very quick way of putting up your software project. There is even an [initialiser web site](https://start.spring.io) that helps you define your project specification files ([Maven](https://maven.apache.org) POM).

Spring project provides [code samples](https://github.com/spring-projects/spring-security/tree/5.2.0.RELEASE/samples/boot/oauth2login) that you can start with to integrate your application to external authentication.

## .NET and C#

**TO BE REFINED**

[.NET](https://dotnet.microsoft.com/) seems to be Free, Cross-Platform, Open Source developer platform that developer can benefit to build apps for Andoid, iOS, macOS and Windows from a single codebase. [C Sharp](https://en.wikipedia.org/wiki/C_Sharp_(programming_language)), also known as C# is a general-purpose, multi-paradigm programming language to write applications in .NET environment and apparently also in others.

The simple guides an [tutorials](https://www.w3schools.com/cs/index.php) available show that the platform is vivid and strongly living.

As we don't curently have good references to C# libraries or implementations, we just save you the bourden of internet searches and list some references:

* [OpenID Connect Client Library for native Applications](https://github.com/IdentityModel/IdentityModel.OidcClient)

That seems to be a short list. However, ASP.NET Core [documents](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/social/social-without-identity?source=recommendations&view=aspnetcore-6.0) an ability to provide authentication based on commonly known authentication providers available in the Internet. If this is possible, it should be quite easy to implement OIDC Client with ASP.NET Core services as well.


**TO BE REFINED**

This section contains citations from background materials that the links point to. As might be evident, this section is initially written by a Java [Boomer](https://en.wikipedia.org/wiki/OK_boomer) and we hope that an actual C# developer would refine this section to be more accurate and useful.

Now, by looking the .NET platform, it seems to be locked in and not very well standards based. By locked in, it is referred that the ASP.NET documentation doesn't describe a way to openly connect to identity providers *in general*, but only to those recommended by the platform.

This section need to be refined by a developer that has better understanding of the platform.

**TO BE REFINED**
