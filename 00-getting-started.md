---
layout: page
title:  "Getting Started"
has_children: true
nav_order: 1
---

# Getting Started
{: .no_toc }
This documentation is designed to be used as a base to integrate the Travelsoft NDC API in a third-party system. The Travelsoft NDC API is based on SOAP Web Service technology, but it's also available as REST Web Services. The current NDC version exposed is 19.2.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Steps for Travelsoft NDC API Integration

- give IP addresses of your system to Travelsoft to update firewall of WS server
- build a WS client by using provided WSDL + XSD files
- get a test user from Travelsoft to make test calls
- integrate WS operations in your system (by following recommended flows)

If you have any questions, ask your contact at Travelsoft or contacts@travelsoft.fr if you don't yet have a referent contact.

## Environments

There are two environments:

- Test: (contact us)
- Production: (contact us)

## Authentication

To avoid to send username/password for each NDC request, a login method allows to authenticate a user and returns an access token in response to be used in all the NDC methods (airShopping, serviceList, etc). This login operation must be done once for a user, and the token returned can be used until the expiration date (provided in the login response with the token). Usually an access token is valid during one hour.

## Control header

A control node can be added in the SOAP header of each NDC request. This node contains extra data to control request:

- *Provider*: code of provider to request (put SWITCHALLINONE to receive all available companies)
- *ApiVersion*: version of Travelsoft NDC API to use (allows to maintain backward compatibility if necessary)

:information_source: The Travelsoft NDC API is designed as a gateway, so a provider code is mandatory in request by using control header to indicate the provider to request. The provider can be an airline system, an aggregator, another gateway API, etc. Only one provider code can be set for each request.

<details>
  <summary><b>Control Header Sample</b></summary>

{% highlight xml %}
<Control Provider="SWITCHALLINONE" ApiVersion="1.0" />
{% endhighlight %}

</details>

## NDC 19.2

Click [here](https://travelsoft-direct-connect.github.io/ndc-api-doc/getting_started/00-19.2.html) to get started on NDC 19.2.

## NDC 21.3

Click [here](https://travelsoft-direct-connect.github.io/ndc-api-doc/getting_started/01-21.3.html) to get started on NDC 21.3.