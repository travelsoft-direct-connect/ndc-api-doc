# Getting Started

This documentation is designed to be used as a base to integrate the Orchestra NDC API in a third-party system. The Orchestra NDC API is based on SOAP Web Service technology. The current NDC version exposed is 19.2.

There are two environments:
- Test: <https://apgairlines-resa.recette.orchestra-platform.com/ndc/ws/soap/19.2/OrchestraNDCService?wsdl>
- Production: <https://apgairlines-resa.orchestra-platform.com/ndc/ws/soap/19.2/OrchestraNDCService?wsdl>

WSDL and XSD files can be downloaded directly [here](orchestraNDCService-20192.zip).

## Current available web operations

- *login* &rarr; get an access token
- *airShopping* &rarr; search flights
- *serviceList* &rarr; get ancillaries
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
- *Provider*: code of provider to request
- *ApiVersion*: version of Orchestra NDC API to use (allows to maintain backward compatibility if necessary)

<details>
  <summary><b>Control Header Sample</b></summary>

```xml
<Control Provider="AIRFRANCE" ApiVersion="1.0" />
 ```

</details>

## Shopping flow

>airShopping &rarr; serviceList &rarr; offerPrice &rarr; orderCreate

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

```xml
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
```

</details>

## Steps for Orchestra NDC API Integration

- give IP addresses of your system to Orchestra to update firewall of WS server
- build a WS client by using provided WSDL + XSD files
- get a test user from Orchestra to make test calls
- integrate WS operations in your system (by following recommended flows)

## All samples

Download all message samples [here](samples.zip).
