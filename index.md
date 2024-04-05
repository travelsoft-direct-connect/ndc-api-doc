---
layout: page
title:  "Getting Started"
nav_order: 0
---

# Getting Started
{: .no_toc }
This documentation is designed to be used as a base to integrate the Orchestra NDC API in a third-party system. The Orchestra NDC API is based on SOAP Web Service technology, but it's also available as REST Web Services. The current NDC version exposed is 19.2.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Steps for Orchestra NDC API Integration

- give IP addresses of your system to Orchestra to update firewall of WS server
- build a WS client by using provided WSDL + XSD files
- get a test user from Orchestra to make test calls
- integrate WS operations in your system (by following recommended flows)

If you have any questions, ask your contact at Orchestra or contact@orchestra.eu if you don't yet have a referent contact.

## Environments

There are two environments:

- Test: (contact us)
- Production: (contact us)

WSDL and XSD files can be downloaded directly [here](orchestraNDCService-20192.zip).

## Current available web operations

- *login* &rarr; get an access token
- *airShopping* &rarr; search flights
- *serviceList* &rarr; get ancillaries
- *seatAvailability* &rarr; get seat map
- *offerPrice* &rarr; quote an offer selection
- *orderCreate* &rarr; make a booking
- *orderRetrieve* &rarr; get a booking
- *orderChange* &rarr; issue tickets
- *orderReshop* &rarr; get cancel fee
- *orderCancel* &rarr; cancel a booking

## Authentication

To avoid to send username/password for each NDC request, a login method allows to authenticate a user and returns an access token in response to be used in all the NDC methods (airShopping, serviceList, etc). This login operation must be done once for a user, and the token returned can be used until the expiration date (provided in the login response with the token). Usually an access token is valid during 24h.

## Control header

A control node can be added in the SOAP header of each NDC request. This node contains extra data to control request:

- *Provider*: code of provider to request (put SWITCHALLINONE to receive all available companies)
- *ApiVersion*: version of Orchestra NDC API to use (allows to maintain backward compatibility if necessary)

:information_source: The Orchestra NDC API is designed as a gateway, so a provider code is mandatory in request by using control header to indicate the provider to request. The provider can be an airline system, an aggregator, another gateway API, etc. Only one provider code can be set for each request.

<details>
  <summary><b>Control Header Sample</b></summary>

{% highlight xml %}
<Control Provider="SWITCHALLINONE" ApiVersion="1.0" />
{% endhighlight %}

</details>

## Endpoints

Depending on your preferences, the NDC API can be consumed as SOAP WS or REST WS. In both cases, the *Content-Type* header must be *text/xml;charset=UTF-8*

### SOAP endpoint

Note: NDC messages are contained in SOAP envelope for request and response. Control header must be defined in the SOAP header as XML element.

- https://\_\_\_\_\_\_\_\_/ndc/ws/soap/19.2/OrchestraNDCService

<details>
  <summary><b>SOAP request sample</b></summary>
<pre>
POST https://.../ndc/ws/soap/19.2/OrchestraNDCService HTTP/1.1
Content-Type: text/xml;charset=UTF-8
SOAPAction: "http://www.travelsoft.fr/orchestra/ndc/19.2/airShopping"
AuthToken: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
</pre>
{% highlight xml %}
<?xml version='1.0' encoding='UTF-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
   <soapenv:Header xmlns:head="http://www.travelsoft.fr/orchestra/ndc/headers">
      <head:Control Provider="SWITCHALLINONE" />
   </soapenv:Header>
   <soapenv:Body>
        <IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRQ">
            ...
        </IATA_AirShoppingRQ>
    </soapenv:Body>
</soapenv:Envelope>
{% endhighlight %}

</details>

<details>
  <summary><b>SOAP response sample</b></summary>

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8'?>
<S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
    <S:Body>
        <ns5:IATA_AirShoppingRS xmlns:ns5="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRS">
            ...
        </ns5:IATA_AirShoppingRS>
    </S:Body>
</S:Envelope>
{% endhighlight %}

</details>


### REST endpoints

Note: NDC messages can be sent directly (without envelope, not like SOAP endpoint). Control header must be defined in the HTTP headers (*Orx-Control-Provider*, *Orx-Control-ApiVersion*, *Orx-Control-EnvironmentTarget*, etc).

- https://\_\_\_\_\_\_\_\_/ndc/ws/rest/19.2/Login
- https://\_\_\_\_\_\_\_\_/ndc/ws/rest/19.2/AirShopping
- https://\_\_\_\_\_\_\_\_/ndc/ws/rest/19.2/ServiceList
- https://\_\_\_\_\_\_\_\_/ndc/ws/rest/19.2/SeatAvailability
- https://\_\_\_\_\_\_\_\_/ndc/ws/rest/19.2/OfferPrice
- https://\_\_\_\_\_\_\_\_/ndc/ws/rest/19.2/OrderCreate
- https://\_\_\_\_\_\_\_\_/ndc/ws/rest/19.2/OrderRetrieve
- https://\_\_\_\_\_\_\_\_/ndc/ws/rest/19.2/OrderReshop
- https://\_\_\_\_\_\_\_\_/ndc/ws/rest/19.2/OrderChange
- https://\_\_\_\_\_\_\_\_/ndc/ws/rest/19.2/OrderCancel

<details>
  <summary><b>REST request sample</b></summary>
<pre>
POST https://.../ndc/ws/rest/19.2/AirShopping HTTP/1.1
Content-Type: text/xml;charset=UTF-8
Orx-Control-Provider: SWITCHALLINONE
AuthToken: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
</pre>
{% highlight xml %}
<?xml version='1.0' encoding='UTF-8'?>
<IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRQ">
  ...
</IATA_AirShoppingRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>REST response sample</b></summary>

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8'?>
<ns5:IATA_AirShoppingRS xmlns:ns5="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRS">
  ...
</ns5:IATA_AirShoppingRS>
{% endhighlight %}

</details>

## Shopping flow

>airShopping (&rarr; offerPrice) &rarr; serviceList &rarr; seatAvailability &rarr; offerPrice &rarr; orderCreate

Note: the airShopping transaction initiates a session which is maintained on Orchestra side by using the ShoppingResponseID as session ID.

## Servicing flows

- Get order data for refresh:

>orderRetrieve

- Ticket issue:

>orderChange

- Cancellation:

>orderReshop &rarr; orderCancel

## Error management

Always in NDC responses (see an example below), except if the request is malformed, a SOAP fault will be returned.

<details>
  <summary><b>AirShoppingRS - Error Example</b></summary>

  {% highlight xml %}
  <IATA_AirShoppingRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRS">
    <Error>
      <Code>911</Code>
      <DescText>Unable to process - system error</DescText>
      <LangCode>en</LangCode>
      <OwnerName>ORCHESTRA</OwnerName>
    </Error>
    <PayloadAttributes>
      <CorrelationID>a222c960-0d2c-4507-bd2c-59362825cc76</CorrelationID>
      <Timestamp>2020-10-01T10:51:29.072</Timestamp>
      <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
  </IATA_AirShoppingRS>
  {% endhighlight %}
</details>

## All samples

Download all message samples [here](samples.zip).
