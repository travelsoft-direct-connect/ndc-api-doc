---
layout: page
title:  "Order Change"
nav_order: 10
---

# OrderChange operation
{: .no_toc }
The order change method allows to make additional updates after order creation in the airline system. For example, it must be used to
- add payment to issue tickets
- accept disruption (schedule change)

---------------------------------------

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Release notes

| Version | Notes |
| --- | --- |
| 1.0 | Initial version. |

## Mandatory HTTP header

- *AuthToken*: token value retrieved from login response

# OrderChangeRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Party | Must contain agency ID as sender | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| DataLists | The request data lists detailed [below](#datalists) | Mandatory |
| Order | The order to change, detailed [below](#order) | Mandatory |
| PaymentFunctions | Must contain an element 'PaymentProcessingDetails' with Cash method to issue tickets | Mandatory for ticket issue |
| ChangeOrder | Must contain an element 'AcceptChange' with order item references to accept disruption | Mandatory for disruption acceptance |

### DataLists
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| ContactInfoList | List of contacts | Optional |
| PaxList | List of passengers | Optional |

### Order
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderID | ID of the order to change | Mandatory |

# OrderChange - OrderViewRS

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PayloadAttributes | Same as requested + timestamp | Mandatory |
| Response | The response element detailed [below](#response) | Mandatory |
| PaymentFunctions | Payment information used to issue tickets | Mandatory if ticket issued |

## Response
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Warnings | List of warnings returned by provider | Optional |
| DataLists | The response data lists (journeys, segments, service definitions, etc) | Mandatory |
| Order | The order element detailed [below](#order) | Mandatory |
| TicketDocInfo | List of tickets issued for each passengers, detailed [below](#ticketdocinfo) | Mandatory if ticket issued |

### Order
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderID | The order ID (to use for servicing) | Mandatory |
| BookingRefs | List of booking references | Mandatory |
| StatusCode | The order status {::nomarkdown}<ul><li>OPENED: order confirmed</li><li>CLOSED: order cancelled</li></ul> {:/} | Mandatory |
| TotalPrice | The total price of the whole order | Mandatory |
| OrderItems | List of order items detailed [below](#orderitem) | Mandatory |

#### OrderItem
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderItemID | ID of the order item | Mandatory |
| FareDetail | Contains the PAX associations, the unit price in FarePriceType, and more information for each segment in FareComponent | Mandatory |
| Price | The total price of this offer item | Mandatory |
| Services | List of flight/serviceDefinition associations with PAX and StatusCode: {::nomarkdown}<ul><li>SB: issuance in progress (waiting to be confirmed, OrderRetrieveRQ has to be called periodically until status is updated)</li><li>T: tickets issued</li></ul> {:/} | Mandatory |

### TicketDocInfo
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| BookingRef | The booking reference linked | Mandatory |
| PaxRefID | The passenger reference linked to the tickets | Mandatory |
| Ticket | List of tickets for the given booking reference and passenger reference, detailed [below](#ticket) | Mandatory |

#### Ticket
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Coupon | The coupon information | Mandatory |
| ReportingTypeCode | BSP | Mandatory |
| TicketDocTypeCode | The ticket type: T (ticket), J (EMD-Associated), Y (EMD-Standalone) | Mandatory |
| TicketNumber | The ticket number | Mandatory |

# Samples

<details>
  <summary><b>OrderChangeRQ - Ticket Issue</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderChangeRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderChangeRQ">
    <Party>
        <Sender>
            <TravelAgency>
                <AgencyID>agency1234</AgencyID>
            </TravelAgency>
        </Sender>
    </Party>
    <PayloadAttributes>
        <CorrelationID>dbc121be-c27b-4b63-9eab-5e73dfb8b6c4</CorrelationID>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <Request>
        <DataLists>
            <ContactInfoList>
                <ContactInfo>
                    <ContactInfoID>CONT1</ContactInfoID>
                    <EmailAddress>
                        <EmailAddressText>florian.garnier@orchestra.eu</EmailAddressText>
                    </EmailAddress>
                    <Phone>
                        <AreaCodeNumber></AreaCodeNumber>
                        <CountryDialingCode>33</CountryDialingCode>
                        <PhoneNumber>0622761972</PhoneNumber>
                    </Phone>
                    <PostalAddress>
                        <CityName>Paris</CityName>
                        <CountryCode>FR</CountryCode>
                        <PostalCode>75002</PostalCode>
                        <StreetText>38 avenue de l'opera</StreetText>
                    </PostalAddress>
                </ContactInfo>
            </ContactInfoList>
            <PaxList>
                <Pax>
                    <Birthdate>1986-02-02</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <Individual>
                        <Birthdate>1986-02-02</Birthdate>
                        <GivenName>Florian</GivenName>
                        <IndividualID>IND1</IndividualID>
                        <Surname>Garnier</Surname>
                        <TitleName>MR</TitleName>
                    </Individual>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <Birthdate>1986-03-03</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <Individual>
                        <Birthdate>1986-03-03</Birthdate>
                        <GivenName>Floria</GivenName>
                        <IndividualID>IND2</IndividualID>
                        <Surname>Garnier</Surname>
                        <TitleName>MRS</TitleName>
                    </Individual>
                    <PaxID>PAX2</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
            </PaxList>
        </DataLists>
        <Order>
            <OrderID>544755</OrderID>
            <OwnerCode>BA</OwnerCode>
        </Order>
        <PaymentFunctions>
            <PaymentProcessingDetails>
                <Amount CurCode="EUR">690.32000000000000000000</Amount>
                <OrderAssociation>
                    <OrderItemRefID>32764ce2-5548-4498-b36f-58a10c3906f9</OrderItemRefID>
                    <OrderItemRefID>1dbb3411-1bc3-4e6c-896e-b90cc7a585b6</OrderItemRefID>
                    <OrderItemRefID>6863b890-9fae-4dee-928c-5387050fda95</OrderItemRefID>
                    <OrderItemRefID>55fe8531-08db-4b23-afe3-00f9c467c1bd</OrderItemRefID>
                </OrderAssociation>
                <PaymentMethod>
                    <Cash/>
                </PaymentMethod>
                <TypeCode>CA</TypeCode>
            </PaymentProcessingDetails>
        </PaymentFunctions>
    </Request>
</IATA_OrderChangeRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderViewRS - Ticket Issue</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderViewRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderViewRS">
    <Response>
        <DataLists>
            <ContactInfoList>
                <ContactInfo>
                    <ContactInfoID>CONT1</ContactInfoID>
                    <EmailAddress>
                        <EmailAddressText>florian.garnier@orchestra.eu</EmailAddressText>
                    </EmailAddress>
                    <PostalAddress>
                        <CityName>Paris</CityName>
                        <CountryCode>FR</CountryCode>
                        <PostalCode>75002</PostalCode>
                        <StreetText>38 avenue de l'opera</StreetText>
                    </PostalAddress>
                </ContactInfo>
            </ContactInfoList>
            <PaxList>
                <Pax>
                    <Birthdate>1986-02-02+01:00</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <Birthdate>1986-03-03+01:00</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <PaxID>PAX2</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
            </PaxList>
        </DataLists>
        <Order>
            <BookingRef>
                <BookingEntity>
                    <Carrier>
                        <AirlineDesigCode>BA</AirlineDesigCode>
                    </Carrier>
                </BookingEntity>
                <BookingID>T72IXL</BookingID>
            </BookingRef>
            <OrderID>544755</OrderID>
        </Order>
        <TicketDocInfo>
            <BookingRef>
                <BookingEntity>
                    <Carrier>
                        <AirlineDesigCode>BA</AirlineDesigCode>
                    </Carrier>
                </BookingEntity>
                <BookingID>T72IXL</BookingID>
            </BookingRef>
            <PaxRefID>PAX3</PaxRefID>
            <PaymentInfoRefID>PAY1</PaymentInfoRefID>
            <Ticket>
                <Coupon>
                    <CouponNumber>1</CouponNumber>
                    <CouponStatusCode>I</CouponStatusCode>
                </Coupon>
                <ReportingTypeCode>BSP</ReportingTypeCode>
                <TicketDocTypeCode>T</TicketDocTypeCode>
                <TicketNumber>125</TicketNumber>
            </Ticket>
            <Ticket>
                <Coupon>
                    <CouponNumber>1</CouponNumber>
                    <CouponStatusCode>I</CouponStatusCode>
                </Coupon>
                <ReportingTypeCode>BSP</ReportingTypeCode>
                <TicketDocTypeCode>T</TicketDocTypeCode>
                <TicketNumber>2113793590</TicketNumber>
            </Ticket>
        </TicketDocInfo>
        <TicketDocInfo>
            <BookingRef>
                <BookingEntity>
                    <Carrier>
                        <AirlineDesigCode>BA</AirlineDesigCode>
                    </Carrier>
                </BookingEntity>
                <BookingID>T72IXL</BookingID>
            </BookingRef>
            <PaxRefID>PAX4</PaxRefID>
            <PaymentInfoRefID>PAY1</PaymentInfoRefID>
            <Ticket>
                <Coupon>
                    <CouponNumber>1</CouponNumber>
                    <CouponStatusCode>I</CouponStatusCode>
                </Coupon>
                <ReportingTypeCode>BSP</ReportingTypeCode>
                <TicketDocTypeCode>T</TicketDocTypeCode>
                <TicketNumber>125</TicketNumber>
            </Ticket>
            <Ticket>
                <Coupon>
                    <CouponNumber>1</CouponNumber>
                    <CouponStatusCode>I</CouponStatusCode>
                </Coupon>
                <ReportingTypeCode>BSP</ReportingTypeCode>
                <TicketDocTypeCode>T</TicketDocTypeCode>
                <TicketNumber>2113793591</TicketNumber>
            </Ticket>
        </TicketDocInfo>
    </Response>
    <PayloadAttributes>
        <CorrelationID>dbc121be-c27b-4b63-9eab-5e73dfb8b6c4</CorrelationID>
        <Timestamp>2021-02-04T09:40:28.309+01:00</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <PaymentFunctions>
        <PaymentProcessingSummary>
            <Amount CurCode="EUR">690.32</Amount>
            <PaymentID>PAY1</PaymentID>
            <PaymentMethod>
                <Cash/>
            </PaymentMethod>
            <TypeCode>Cash</TypeCode>
        </PaymentProcessingSummary>
    </PaymentFunctions>
</IATA_OrderViewRS>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderChangeRQ - Accept disruption</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderChangeRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderChangeRQ">
    <Party>
        <Sender>
            <TravelAgency>
                <AgencyID>agency1234</AgencyID>
            </TravelAgency>
        </Sender>
    </Party>
    <PayloadAttributes>
        <CorrelationID>dbc121be-c27b-4b63-9eab-5e73dfb8b6c4</CorrelationID>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <Request>
        <ChangeOrder>
            <AcceptChange>
                <OrderItemRefID>32764ce2-5548-4498-b36f-58a10c3906f9</OrderItemRefID>
                <OrderItemRefID>1dbb3411-1bc3-4e6c-896e-b90cc7a585b6</OrderItemRefID>
            </AcceptChange>
        </ChangeOrder>
        <Order>
            <OrderID>544755</OrderID>
            <OwnerCode>BA</OwnerCode>
        </Order>
    </Request>
</IATA_OrderChangeRQ>
{% endhighlight %}

</details>
