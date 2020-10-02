---
layout: page
title:  "Offer Price"
nav_order: 5
---

# OfferPrice operation
{: .no_toc }
The offer price method is used to confirm price and availability of selected offers (flight + ancillaries) and returns one packaged priced offer.

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

## Control header

The provider to request must be sent in the control header. For example:

{% highlight xml %}
<Control Provider="VUELING" />
{% endhighlight %}

# OfferPriceRQ

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
| PricedOffer | List of selected offers to price with Shopping session ID | Mandatory |

### DataLists
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Paxs | List of passengers (same as AirShoppingRQ) | Mandatory |

# OfferPriceRS

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PayloadAttributes | Same as requested + timestamp | Mandatory |
| Response | The response element detailed [below](#response) | Mandatory |

## Response
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Warnings | List of warnings returned by provider | Optional |
| ShoppingResponse | The Shopping session ID to use for next requests | Mandatory |
| DataLists | The response data lists (journeys, segments, service definitions, etc) | Mandatory |
| PricedOffer | The priced offer element detailed [below](#pricedoffer) | Mandatory |

### PricedOffer
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferID | ID of the offer priced | Mandatory |
| JourneyOverview | Overview of contained journeys with price class links | Mandatory |
| BaggageAllowance | The baggage allowance for each pax/segment | Mandatory |
| TotalPrice | The total price of this offer | Mandatory |
| OfferItems | List of offer items detailed [below](#offeritem) | Mandatory |

#### OfferItem
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferItemID | The offer item ID | Mandatory |
| FareDetail | Contains the PAX associations, the unit price in FarePriceType, and more information for each segment in FareComponent | Mandatory |
| Price | The total price of this offer item | Mandatory |
| Services | List of flight/serviceDefinition associations with PAX | Mandatory |

# Samples

<details>
  <summary><b>OfferPriceRQ</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OfferPriceRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OfferPriceRQ">
    <PayloadAttributes>
        <CorrelationID>a222c960-0d2c-4507-bd2c-59362825cc76</CorrelationID>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <Request>
        <DataLists>
            <PaxList>
                <Pax>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <PaxID>PAX2</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
            </PaxList>
        </DataLists>
        <PricedOffer>
            <SelectedOffer>
                <OfferRefID>23bedc85-dd6a-482b-ac29-2f0c608ed478</OfferRefID>
                <OwnerCode>AF</OwnerCode>
                <SelectedOfferItem>
                    <OfferItemRefID>e99b73dc-16a1-4b9b-a8bc-f26b6299f5bb</OfferItemRefID>
                </SelectedOfferItem>
                <ShoppingResponseRefID>2d62d243-8837-4e4d-a91c-45550a2fd6fa</ShoppingResponseRefID>
            </SelectedOffer>
            <SelectedOffer>
                <OfferRefID>2076a058-e502-44ab-94b1-80b3e1ef8bcd</OfferRefID>
                <OwnerCode>AF</OwnerCode>
                <SelectedOfferItem>
                    <OfferItemRefID>353e5fce-965f-4fc5-ad68-c4b430b87ad4</OfferItemRefID>
                </SelectedOfferItem>
                <SelectedOfferItem>
                    <OfferItemRefID>463d2c99-fe21-40b4-9cf4-feb29da73f4b</OfferItemRefID>
                </SelectedOfferItem>
                <ShoppingResponseRefID>2d62d243-8837-4e4d-a91c-45550a2fd6fa</ShoppingResponseRefID>
            </SelectedOffer>
        </PricedOffer>
    </Request>
</IATA_OfferPriceRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OfferPriceRS</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OfferPriceRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OfferPriceRS">
    <Response>
        <DataLists>
            <BaggageAllowanceList>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA1</BaggageAllowanceID>
                    <PieceAllowance>
                        <ApplicablePartyText>Traveler</ApplicablePartyText>
                        <TotalQty>0</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
            </BaggageAllowanceList>
            <OriginDestList>
                <OriginDest>
                    <DestCode>JNB</DestCode>
                    <OriginCode>CDG</OriginCode>
                    <OriginDestID>OD1</OriginDestID>
                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>CDG</DestCode>
                    <OriginCode>JNB</OriginCode>
                    <OriginDestID>OD2</OriginDestID>
                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT14H10M0S</Duration>
                    <PaxJourneyID>PJ5</PaxJourneyID>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT10H45M0S</Duration>
                    <PaxJourneyID>PJ2</PaxJourneyID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </PaxJourney>
            </PaxJourneyList>
            <PaxList>
                <Pax>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <PaxID>PAX2</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
            </PaxList>
            <PaxSegmentList>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T08:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>321</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T07:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2002</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG5</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T21:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>JNB</IATA_LocationCode>
                        <TerminalName>B</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>772</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T10:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>0112</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG4</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-23T19:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2E</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>77W</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-23T08:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>JNB</IATA_LocationCode>
                        <TerminalName>B</TerminalName>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>0995</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
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
                        <DescText>1 hand baggage item and 1 accessory only (12 kg total*)</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Cancellation is not possible</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Change fee + fare difference</DescText>
                    </Desc>
                    <Desc>
                        <DescText>No refund if you missed your flight</DescText>
                    </Desc>
                    <Desc>
                        <DescText>* This applies to flights operated by KLM or Air France. For other airlines, please check the airline's website for baggage rules</DescText>
                    </Desc>
                    <Name>Light</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
            </PriceClassList>
            <ServiceDefinitionList>
                <ServiceDefinition>
                    <ServiceTaxonomy>
                        <DescText>Checked Baggage</DescText>
                        <TaxonomyCode>13EC</TaxonomyCode>
                    </ServiceTaxonomy>
                    <Desc>
                        <DescText>1 luggage item</DescText>
                    </Desc>
                    <Name>1 luggage item</Name>
                    <ServiceDefinitionID>SD1</ServiceDefinitionID>
                </ServiceDefinition>
            </ServiceDefinitionList>
        </DataLists>
        <PaymentFunctions>
            <PaymentSupportedMethod>
                <TypeCode>Cash</TypeCode>
            </PaymentSupportedMethod>
        </PaymentFunctions>
        <PricedOffer>
            <BaggageAllowance>
                <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                <BaggageFlightAssociations>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </BaggageFlightAssociations>
                <PaxRefID>PAX1</PaxRefID>
                <PaxRefID>PAX2</PaxRefID>
            </BaggageAllowance>
            <JourneyOverview>
                <JourneyPriceClass>
                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    <PriceClassRefID>PC1</PriceClassRefID>
                </JourneyPriceClass>
                <JourneyPriceClass>
                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    <PriceClassRefID>PC1</PriceClassRefID>
                </JourneyPriceClass>
            </JourneyOverview>
            <OfferID>601a1ef9-4ac3-471f-98a4-b635e2991d1f</OfferID>
            <OfferItem>
                <FareDetail>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>X</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <FareBasisCode>XL5VUIL1</FareBasisCode>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FarePriceType>
                        <Price>
                            <BaseAmount CurCode="EUR">151.00000000000000000000</BaseAmount>
                            <TaxSummary>
                                <Tax>
                                    <Amount CurCode="EUR">200.00000000000000000000</Amount>
                                    <TaxCode>TAX_GROUP_1_PAX_1_2</TaxCode>
                                    <TaxName>Fuel surcharge (YQ/YR) - PAX1,2</TaxName>
                                </Tax>
                                <Tax>
                                    <Amount CurCode="EUR">77.66000000000000000000</Amount>
                                    <TaxCode>GENERAL_TAXES_PAX_1_2</TaxCode>
                                    <TaxName>Taxes - PAX1,2</TaxName>
                                </Tax>
                                <TotalTaxAmount CurCode="EUR">277.66000000000000000000</TotalTaxAmount>
                            </TaxSummary>
                            <TotalAmount CurCode="EUR">428.66000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                    <PaxRefID>PAX2</PaxRefID>
                </FareDetail>
                <OfferItemID>8e092c73-ce79-40b5-ba63-9046b0f001f5</OfferItemID>
                <Price>
                    <BaseAmount CurCode="EUR">302.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">857.32000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </ServiceAssociations>
                    <ServiceID>SV388</ServiceID>
                </Service>
            </OfferItem>
            <OfferItem>
                <FareDetail>
                    <FarePriceType>
                        <Price>
                            <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                            <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                </FareDetail>
                <OfferItemID>3f4f71eb-c800-4dae-adca-4f61f51a7de2</OfferItemID>
                <Price>
                    <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <ServiceDefinitionRef>
                            <ServiceDefinitionRefID>SD1</ServiceDefinitionRefID>
                        </ServiceDefinitionRef>
                    </ServiceAssociations>
                    <ServiceID>SV389</ServiceID>
                </Service>
            </OfferItem>
            <OfferItem>
                <FareDetail>
                    <FarePriceType>
                        <Price>
                            <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                            <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                </FareDetail>
                <OfferItemID>d59bb99f-d764-457e-9946-08ead0428113</OfferItemID>
                <Price>
                    <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <ServiceDefinitionRef>
                            <ServiceDefinitionRefID>SD1</ServiceDefinitionRefID>
                        </ServiceDefinitionRef>
                    </ServiceAssociations>
                    <ServiceID>SV390</ServiceID>
                </Service>
            </OfferItem>
            <TotalPrice>
                <BaseAmount CurCode="EUR">402.00000000000000000000</BaseAmount>
                <TaxSummary>
                    <Tax>
                        <Amount CurCode="EUR">400.00000000000000000000</Amount>
                        <TaxCode>TAX_GROUP_1_PAX_1_2</TaxCode>
                        <TaxName>Fuel surcharge (YQ/YR) - PAX1,2</TaxName>
                    </Tax>
                    <Tax>
                        <Amount CurCode="EUR">155.32000000000000000000</Amount>
                        <TaxCode>GENERAL_TAXES_PAX_1_2</TaxCode>
                        <TaxName>Taxes - PAX1,2</TaxName>
                    </Tax>
                    <TotalTaxAmount CurCode="EUR">555.32000000000000000000</TotalTaxAmount>
                </TaxSummary>
                <TotalAmount CurCode="EUR">957.32000000000000000000</TotalAmount>
            </TotalPrice>
        </PricedOffer>
        <ShoppingResponse>
            <ShoppingResponseRefID>2d62d243-8837-4e4d-a91c-45550a2fd6fa</ShoppingResponseRefID>
        </ShoppingResponse>
    </Response>
    <PayloadAttributes>
        <CorrelationID>a222c960-0d2c-4507-bd2c-59362825cc76</CorrelationID>
        <Timestamp>2020-09-30T17:41:43.912</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_OfferPriceRS>
{% endhighlight %}

</details>
