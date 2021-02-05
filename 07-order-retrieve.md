---
layout: page
title:  "Order Retrieve"
nav_order: 7
---

# OrderRetrieve operation
{: .no_toc }
The order retrieve method allows to get booking information after order creation. The order ID must be sent in request to be able to retrieve information from the airline system.

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

# OrderRetrieveRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Party | Must contain agency ID as sender | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderFilterCriteria | The order filter criteria [below](#orderfiltercriteria) | Mandatory |

### OrderFilterCriteria
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Order | Must contain the ID of order to retrieve | Mandatory |

# OrderRetrieve - OrderViewRS

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PayloadAttributes | Same as requested + timestamp | Mandatory |
| Response | The response element detailed [below](#response) | Mandatory |

## Response
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Warnings | List of warnings returned by provider | Optional |
| DataLists | The response data lists (journeys, segments, service definitions, etc) | Mandatory |
| Order | The order element detailed [below](#order) | Mandatory |

### Order
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderID | The order ID (same as requested) | Mandatory |
| BookingRefs | List of booking references | Mandatory |
| TotalPrice | The total price of the whole order | Mandatory |
| OrderItems | List of order items detailed [below](#orderitem) | Mandatory |

#### OrderItem
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderItemID | ID of the order item | Mandatory |
| FareDetail | Contains the PAX associations, the unit price in FarePriceType, and more information for each segment in FareComponent | Mandatory |
| Price | The total price of this offer item | Mandatory |
| Services | List of flight/serviceDefinition associations with PAX | Mandatory |

# Samples

<details>
  <summary><b>OrderRetrieveRQ</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderRetrieveRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderRetrieveRQ">
    <Party>
        <Sender>
            <TravelAgency>
                <AgencyID>agency1234</AgencyID>
            </TravelAgency>
        </Sender>
    </Party>
    <PayloadAttributes>
        <CorrelationID>c3421ac5-96cd-3aed-b40b-aca63b056173</CorrelationID>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <Request>
        <OrderFilterCriteria>
            <Order>
                <OrderID>544759</OrderID>
                <OwnerCode>BA</OwnerCode>
            </Order>
        </OrderFilterCriteria>
    </Request>
</IATA_OrderRetrieveRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderViewRS</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderViewRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderViewRS">
    <Response>
        <DataLists>
            <OriginDestList>
                <OriginDest>
                    <DestCode>BCN</DestCode>
                    <OriginCode>LHR</OriginCode>
                    <OriginDestID>OD1</OriginDestID>
                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>LHR</DestCode>
                    <OriginCode>BCN</OriginCode>
                    <OriginDestID>OD2</OriginDestID>
                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H5M0S</Duration>
                    <PaxJourneyID>PJ1</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H15M0S</Duration>
                    <PaxJourneyID>PJ3</PaxJourneyID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </PaxJourney>
            </PaxJourneyList>
            <PaxList>
                <Pax>
                    <Birthdate>1986-01-01</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <Birthdate>1986-02-02</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <PaxID>PAX2</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
            </PaxList>
            <PaxSegmentList>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2021-06-01T22:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2021-06-01T19:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>3</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT2H5M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>BA</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>0482</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>BA</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG1</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2021-06-10T21:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>3</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2021-06-10T20:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT2H15M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>BA</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>0487</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>BA</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG3</PaxSegmentID>
                </PaxSegment>
            </PaxSegmentList>
            <PriceClassList>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>Bagage à main uniquement</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Sièges attribués ou payez pour choisir votre siège quand vous le souhaitez</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Dernier à embarquer</DescText>
                    </Desc>
                    <Name>BASIC</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
            </PriceClassList>
        </DataLists>
        <Order>
            <BookingRef>
                <BookingEntity>
                    <Carrier>
                        <AirlineDesigCode>BA</AirlineDesigCode>
                    </Carrier>
                </BookingEntity>
                <BookingID>T79BPA</BookingID>
            </BookingRef>
            <BookingRef>
                <BookingEntity>
                    <Org>
                        <OrgID>ORCHESTRA</OrgID>
                    </Org>
                </BookingEntity>
                <BookingID>BA-T79BPA</BookingID>
            </BookingRef>
            <OrderID>544759</OrderID>
            <OrderItem>
                <FareDetail>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>O</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>L</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FarePriceType>
                        <Price>
                            <BaseAmount CurCode="EUR">162.00000000000000000000</BaseAmount>
                            <TotalAmount CurCode="EUR">162.00000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX2</PaxRefID>
                </FareDetail>
                <OrderItemID>8c51dabc-00a2-4377-8f9d-5e022c61c4de</OrderItemID>
                <PaymentTimeLimitDateTime>2021-02-05T00:59:00.000</PaymentTimeLimitDateTime>
                <Price>
                    <BaseAmount CurCode="EUR">162.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">162.00000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV373</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV374</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
            </OrderItem>
            <OrderItem>
                <FareDetail>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>O</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>L</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FarePriceType>
                        <Price>
                            <BaseAmount CurCode="EUR">162.00000000000000000000</BaseAmount>
                            <TotalAmount CurCode="EUR">162.00000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                </FareDetail>
                <OrderItemID>526094de-889e-4914-92f3-bf19e3f62b14</OrderItemID>
                <PaymentTimeLimitDateTime>2021-02-05T00:59:00.000</PaymentTimeLimitDateTime>
                <Price>
                    <BaseAmount CurCode="EUR">162.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">162.00000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV375</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV376</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
            </OrderItem>
            <OrderItem>
                <FareDetail>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>O</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>L</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FarePriceType>
                        <Price>
                            <BaseAmount>0</BaseAmount>
                            <TaxSummary>
                                <Tax>
                                    <Amount CurCode="EUR">61.96000000000000000000</Amount>
                                    <TaxCode>GENERAL_TAXES_PAX_2</TaxCode>
                                    <TaxName>Taxes - PAX2</TaxName>
                                </Tax>
                                <TotalTaxAmount CurCode="EUR">61.96000000000000000000</TotalTaxAmount>
                            </TaxSummary>
                            <TotalAmount CurCode="EUR">61.96000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX2</PaxRefID>
                </FareDetail>
                <OrderItemID>edae5606-df2e-41cc-8d8d-d6959c7078d7</OrderItemID>
                <PaymentTimeLimitDateTime>2021-02-05T00:59:00.000</PaymentTimeLimitDateTime>
                <Price>
                    <BaseAmount>0</BaseAmount>
                    <TotalAmount CurCode="EUR">61.96000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV377</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV378</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
            </OrderItem>
            <OrderItem>
                <FareDetail>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>O</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>L</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FarePriceType>
                        <Price>
                            <BaseAmount>0</BaseAmount>
                            <TaxSummary>
                                <Tax>
                                    <Amount CurCode="EUR">61.96000000000000000000</Amount>
                                    <TaxCode>GENERAL_TAXES_PAX_1</TaxCode>
                                    <TaxName>Taxes - PAX1</TaxName>
                                </Tax>
                                <TotalTaxAmount CurCode="EUR">61.96000000000000000000</TotalTaxAmount>
                            </TaxSummary>
                            <TotalAmount CurCode="EUR">61.96000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                </FareDetail>
                <OrderItemID>fe426dfe-70f2-42be-b9b0-8245a4d203f7</OrderItemID>
                <PaymentTimeLimitDateTime>2021-02-05T00:59:00.000</PaymentTimeLimitDateTime>
                <Price>
                    <BaseAmount>0</BaseAmount>
                    <TotalAmount CurCode="EUR">61.96000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV379</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV380</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
            </OrderItem>
            <OwnerCode>BA</OwnerCode>
            <StatusCode>OPENED</StatusCode>
            <TotalPrice>
                <BaseAmount CurCode="EUR">324.00000000000000000000</BaseAmount>
                <TaxSummary>
                    <Tax>
                        <Amount CurCode="EUR">61.96000000000000000000</Amount>
                        <TaxCode>GENERAL_TAXES_PAX_2</TaxCode>
                        <TaxName>Taxes - PAX2</TaxName>
                    </Tax>
                    <Tax>
                        <Amount CurCode="EUR">61.96000000000000000000</Amount>
                        <TaxCode>GENERAL_TAXES_PAX_1</TaxCode>
                        <TaxName>Taxes - PAX1</TaxName>
                    </Tax>
                    <TotalTaxAmount CurCode="EUR">123.92000000000000000000</TotalTaxAmount>
                </TaxSummary>
                <TotalAmount CurCode="EUR">447.92000000000000000000</TotalAmount>
            </TotalPrice>
        </Order>
    </Response>
    <PayloadAttributes>
        <CorrelationID>c3421ac5-96cd-3aed-b40b-aca63b056173</CorrelationID>
        <Timestamp>2021-02-04T10:27:17.922+01:00</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_OrderViewRS>
{% endhighlight %}

</details>
