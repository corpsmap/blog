---
layout: post
current: post
cover: assets/images/apache-small.jpg
navigation: True
title: Apache HTTPD local reverse proxy
date: 2024-02-20 10:00:00
tags: [Local Dev]
class: post-template
subclass: "post"
author: will
---

You might be asking yourself "what is a local reverse proxy and why would I need one?", if so, great! That's what we're going to cover today.

This is the first in a series of posts I'm using to document my local development setup using Docker-Compose to orchestrate all the services you would need to run a local development environment that emulates how you might be deploying your site in CWBI. This includes a number of base services that will allow you to get work done without having to be on the network with access to any of our shared resources.

You can use the following setup in a couple of different ways, lately I've been creating mono-repos with the full stack of things you would need included for each application I'm working on. You could also stand up a single instance of the shared resources like HTTPD and use them across all the apps you're building.

I decided on the mono-repo approach so that it is easy to hand a project off to a new team member and get them up and running quickly.

Let's do a quick overview of the local dev setup, then dive into HTTPD. We'll add more posts to cover the other components soon.

### The Stack

**Basic Components**

- Apache HTTPD - Reverse Proxy, TLS endpoint
- Keycloak - Authentication / Authorization
- Postgres - Database

**Optional** Depending on what I'm building I might include these as well:

- Flyway - Can be used for more advanced things, but typically I use it to manage DB schema updates
- PG-Tileserv - Vector tiles out of the box for any PostGIS data in a DB
- PG-Featureserv - OGC Features API out of the box for any PostGIS data in a DB

Let's get into those in more detail later, for now we'll deal with Apache HTTPD.

## HTTPD

Apache HTTPD is the workhorse of the internet, it's the ol tried and true web server that the CWBI program uses to implement the WAF (Web Application Firewall) layer of our environment. That means that all requests over HTTP come in through Apache and are forwarded to the respective service based on the routing rules laid out by the team.

This layer also acts as our TLS endpoint, that means that all traffic to HTTPD is required to be HTTPS, then the forwarded requests are handled using basic un-encrypted HTTP, but since that traffic lives only within our environment we think it works well enough. Some might argue that we should be HTTPS everywhere, but that imposes some interesting hurdles when trying to generate and rotate the certificates required to make that work.

Apache also handles mutual authentication, that's a long way of saying it'll read your CAC (Common Access Card) or PIV (Personal Identity Verification) certificates as part of the request negotiation if you tell it to. This is how we're doing CAC/PIV based login to Keycloak. In production we specify which routes should request mutual auth, but in development I usually just set it globally.

In order to do mutual auth locally, we need a couple things though, we need to run in HTTPS mode, which means we need a valid HTTPS certificate that our browser can recognize so we don't break the many browser based rules around HTTPS. Luckily we have a teammate that created his own CA that we can leverage for a local HTTPS certificate.

## Setup

I typically setup a project folder that contains my source for the application as well as folders for each of the services managed by Docker Compose. It will work as a single development infrastructure stack independent of the source code of your app as well.

So given a folder structure like:

```
/my-project
  /api
  /httpd
  /keycloak
  /flyway
  /ui
  /docker-compose.yml
```

Let's set up the contents of the `/httpd` folder, then configure our `docker-compose.yml` for apache.

### Dockerfile

The dockerfile basically just takes the off-the-shelf Apache HTTPD container and copies some of our customized files in to configure the setup.

```dockerfile
FROM httpd:2.4
COPY httpd.conf /usr/local/apache2/conf/httpd.conf
COPY httpd-ssl.conf /usr/local/apache2/conf/extra/httpd-ssl.conf
COPY dev2-crrel-mil.crt /usr/local/apache2/conf/dev2-crrel-mil.crt
COPY dev2-crrel-mil.key /usr/local/apache2/conf/dev2-crrel-mil.key
COPY CRREL-CA1.crt /usr/local/apache2/conf/CRREL-CA1.crt
COPY trustore.crt /usr/local/apache2/conf/trustore.crt
```

```dockerfile
FROM httpd:2.4
```

Start with the base public httpd image, version 2.4.

```dockerfile
COPY httpd.conf /usr/local/apache2/conf/httpd.conf
COPY httpd-ssl.conf /usr/local/apache2/conf/extra/httpd-ssl.conf
```

Copy our customized config files into the container, we'll get into the weeds on these next.

```dockerfile
COPY dev2-crrel-mil.crt /usr/local/apache2/conf/dev2-crrel-mil.crt
COPY dev2-crrel-mil.key /usr/local/apache2/conf/dev2-crrel-mil.key
COPY CRREL-CA1.crt /usr/local/apache2/conf/CRREL-CA1.crt
COPY trustore.crt /usr/local/apache2/conf/trustore.crt
```

Copy our local dev certificate and CA information, this lets us run HTTPS locally. **These certificates should never be used on a server.**

### httpd.conf

This is the main configuration file for for HTTPD. You shouldn't really change anything here unless you know what you're doing.

### httpd-ssl.config

All of the TLS config is here, this is also where we'll define our reverse proxy endpoints pointing to our other services.

[Continue to the next post in the series ->](/local-dev-keycloak)
