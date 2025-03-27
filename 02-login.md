---
layout: page
title:  "Login"
nav_order: 2
---

# Login operation
{: .no_toc }
The login operation authenticates a user and returns a token to access all others methods. A token has an expiration date and can be used for all requests of the user (agency/agent) until it expires. Usually a token is valid during 1 hour after its creation.

**You should also receive in this response somes cookies (JSESSIONID + ROUTE_ID), you must send them back to all the next requests in you workflow!**

__Important__:

It could occur that tokens are removed because of a server restart, so it's important to check all NDC error responses to detect this case of token lost. If NDC error code is `368` (description: "`Not authorized`", owner: "`ORCHESTRA`"), that means token is expired or lost, so a login request must be executed again to get a new token.

Even if the token has not yet expired, it must be renewed before performing certain critical operations, such as AirShopping (flight search), OrderRetrieve (refreshing an order, ticket issue or cancellation). This ensures the latest authentication context and prevents potential authorization issues.

---------------------------------------

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Request Sample

{% highlight xml %}
<LoginRQ Username="agency1234" Password="XXXX" xmlns="http://www.travelsoft.fr/orchestra/ndc/login"/>
{% endhighlight %}

## Response Sample

{% highlight xml %}
<LoginRS AuthStatus="Success" xmlns="http://www.travelsoft.fr/orchestra/ndc/login">
 <AuthToken ExpirationDate="2020-10-02T09:19:34.747+02:00">
    <Value>SFi/r6fi4kUA3Zw1zE+cJQtPrV+QWXlbXi2z6c5g89/Z8mD5QW/na7E7PvEexbuP</Value>
 </AuthToken>
</LoginRS>
{% endhighlight %}
