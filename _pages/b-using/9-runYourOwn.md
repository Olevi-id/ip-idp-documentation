---
layout: post
title: 9. Running Your Own IdP Instance
---
Anyone can run an Identity Provider by using [Shibboleth IdP](https://shibboleth.atlassian.net/wiki/spaces/IDP4/overview) software that is produced as Open Source product.

Olevi customers that have license agreement to use the software can run Olevi IdP.

We understand that some customers want to maintain their own setup or try fitting an IdP to their setup before committing to long term procurement. For these customers we have made it easier to run Shibboleth software. Shibboleth project doesn't yet provide supported Docker Image of the software, but we have made one available for you.

Our Docker image is based on old and abandonded [Unicon project](https://github.com/Unicon/shibboleth-idp-dockerized) that was later maintained by [CSC](https://github.com/CSCfi/shibboleth-idp-dockerized). While CSC guys have done amazing job with the image, it seems that they have not updated it publicly lately. There are other images as well. At least Ian Young's [version](https://github.com/iay/shibboleth-idp-docker) seems interesting.

If you want to try and see concretely, what Shibboleth is all about, you can find our image project in [GitHub](https://github.com/Olevi-id/shibboleth-idp-dockerized) or download it from [DockerHub](https://hub.docker.com/r/klaalo/shibboleth-idp). Both sites have some basic instructions on how to launch the service.

## For Experienced Users

While we do make the Shibboleth Docker Image available for anyone to try, we feel that it is necessary to mention that experienced knowledge on IT and authentication is needed to understand the IdP. Possibly needless to mention, but for sure understanding on Linux Platforms, Docker and basic devOps practices are needed to run the service.