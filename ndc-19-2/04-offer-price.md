---
layout: page
title:  "Offer Price"
parent: NDC 19.2
nav_order: 4
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
<Control Provider="SWITCHALLINONE" />
{% endhighlight %}

# OfferPriceRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Party | Must contain agency ID as sender | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Optional/Mandatory |
| --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| --- |
| DataLists | The request data lists detailed [below](#datalists)                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Mandatory |
| PricedOffer | List of selected offers to price with Shopping session ID.<br /><b>Note</b>: For certain airlines that provide free and mandatory seats, they are automatically pre-selected and returned in [OfferPriceRS](#offerpricers) as [OfferItems](#offerItem) with a SeatAssignment node in [ServiceAssociations](#serviceAssociations) and the price equal to 0. These OfferItems cannot be removed and, currently, cannot be changed with different seat selections. See the [Samples](#samples) section for examples. | Mandatory |

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
| DataLists | The response data lists (journeys, segments, service definitions, etc) [below](#datalists) | Mandatory |
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

### DataLists
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PaxList | List of passengers, detailed [below](#pax) | Mandatory |

#### Pax
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PTC              | PTC                                           | Mandatory          |
| PaxID            | Pax ID                                        | Mandatory          |
| IdentityDocs     | List of Pax Documents, detailed [below](#identitydoc). Note: If more than one IdentityDoc, only one of them will be required for OrderCreateRQ. | Optional           |

##### IdentityDoc
{: .no_toc }
When an IdentityDoc is sent in OfferPrice response, it indicates that this information is required for OrderCreateRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| IdentityDocID | When filled with 'XXXXXXX', it is mandatory to send this information in OrderCreateRQ for the IdentityDocTypeCode below | Optional |
| IdentityDocTypeCode | Document Code, possible values (PT, VS, ID, DL or FC)<br />PT = Passport<br />VS = Visa<br />ID = Identity card<br />DL = Driving License<br />FC = Fiscal code  | Optional |
| CitizenshipCountryCode | When filled with 'XX', it is mandatory to send this information in OrderCreateRQ | Optional |
| ResidenceCountryCode | When filled with 'XX', it is mandatory to send this information in OrderCreateRQ | Optional |

#### OfferItem
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferItemID | The offer item ID | Mandatory |
| FareDetail | Contains the PAX associations, the unit price in FarePriceType, and more information for each segment in FareComponent | Mandatory |
| PaymentTimeLimit | Contains zero duration (PT0S) if instant payment is required, otherwise indicate the payment time limit | Optional |
| Price | The total price of this offer item | Mandatory |
| Service | List of flight/serviceDefinition associations with PAX | Mandatory |

#### Service
{: .no_toc }

| Element             | Description                                                                                                                 | Optional/Mandatory |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------| --- |
| PaxRefID            | The ID of the passenger this offer item is associated to                                                                    | Mandatory |
| ServiceAssociations | References to the details of this Service. May include either Passenger Journeys, a Service Definition, or a Selected Seat. | Mandatory |
| ServiceID           | The unique identifier of the service                                                                                        | Mandatory |

#### ServiceAssociations
{: .no_toc }

| Element              | Description                                                                                                                      | Optional/Mandatory |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------| --- |
| SeatAssignment       | The Seat Location selected by the Passenger (via SeatAvailability) or assigned to the Passenger by Orchestra for a given segment | Mandatory |
| ServiceDefinitionRef | Reference to the specific definition of this service                                                                             | Mandatory |

#### SeatAssignment

| Element              | Description                                            | Optional/Mandatory |
|----------------------|--------------------------------------------------------| --- |
| DatedOperatingLegRefID       | Reference to the operating leg ID                      | Mandatory |
| Seat | Detailed regarding the seat selection (row and column) | Mandatory |

#### Seat

| Element  | Description                        | Optional/Mandatory |
|----------|------------------------------------| --- |
| ColumnID | The column the seat is situated on | Mandatory |
| RowNumber     | The row the seat is situated on    | Mandatory |


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
  <summary><b>OfferPriceRQ (with ancillaries)</b></summary>

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
			</PaxList>
		</DataLists>
		<PricedOffer>
			<SelectedOffer>
				<OfferRefID>8354fe3d-a1e4-46aa-b20d-1e6a5a9b7f12</OfferRefID>
				<OwnerCode>EY</OwnerCode>
				<SelectedOfferItem>
					<OfferItemRefID>9d8185d1-5d5b-4a45-971b-3ebe76c217ea</OfferItemRefID>
					<PaxRefID>PAX1</PaxRefID>
				</SelectedOfferItem>
				<ShoppingResponseRefID>a1302470-1316-4b22-9bef-065d216e6c6c</ShoppingResponseRefID>
			</SelectedOffer>
			<!-- Service Select Offer-->
			<SelectedOffer>
				<OfferRefID>3601c199-bb9f-461c-88d0-cae7d35b24d8</OfferRefID>
				<SelectedOfferItem>
					<OfferItemRefID>92228867-60d8-4858-afe1-b2b5eadabb57</OfferItemRefID>
				</SelectedOfferItem>
				<ShoppingResponseRefID>a1302470-1316-4b22-9bef-065d216e6c6c</ShoppingResponseRefID>
			</SelectedOffer>
			<!-- Seat Select Offer-->
			<SelectedOffer>
				<OfferRefID>ab8b0a48-5c24-4a7e-bef1-7c2de2a0ad43</OfferRefID>
				<SelectedOfferItem>
					<OfferItemRefID>a9a2dc7c-e040-4d0f-8204-c4ad7d0908df</OfferItemRefID>
				</SelectedOfferItem>
				<ShoppingResponseRefID>a1302470-1316-4b22-9bef-065d216e6c6c</ShoppingResponseRefID>
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

<details>
  <summary><b>OfferPriceRS (with mandatory free seat)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OfferPriceRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OfferPriceRS">
	<Response>
		<DataLists>
			<BaggageAllowanceList>
				<BaggageAllowance>
					<BaggageAllowanceID>BA3</BaggageAllowanceID>
					<PieceAllowance>
						<ApplicablePartyText>Traveler</ApplicablePartyText>
						<TotalQty>1</TotalQty>
					</PieceAllowance>
					<TypeCode>CarryOn</TypeCode>
				</BaggageAllowance>
				<BaggageAllowance>
					<BaggageAllowanceID>BA2</BaggageAllowanceID>
					<PieceAllowance>
						<ApplicablePartyText>Traveler</ApplicablePartyText>
						<TotalQty>1</TotalQty>
					</PieceAllowance>
					<TypeCode>Checked</TypeCode>
				</BaggageAllowance>
			</BaggageAllowanceList>
			<OriginDestList>
				<OriginDest>
					<DestCode>IAS</DestCode>
					<OriginCode>BVA</OriginCode>
					<OriginDestID>OD1</OriginDestID>
					<PaxJourneyRefID>PJ1</PaxJourneyRefID>
				</OriginDest>
			</OriginDestList>
			<PaxJourneyList>
				<PaxJourney>
					<Duration>P0Y0M0DT2H55M0S</Duration>
					<PaxJourneyID>PJ1</PaxJourneyID>
					<PaxSegmentRefID>SEG1</PaxSegmentRefID>
				</PaxJourney>
			</PaxJourneyList>
			<PaxList>
				<Pax>
					<PaxID>PAX1</PaxID>
					<PTC>ADT</PTC>
				</Pax>
			</PaxList>
			<PaxSegmentList>
				<PaxSegment>
					<Arrival>
						<AircraftScheduledDateTime>2026-08-31T12:30:00</AircraftScheduledDateTime>
						<IATA_LocationCode>IAS</IATA_LocationCode>
					</Arrival>
					<DatedOperatingLeg>
						<Arrival/>
						<CarrierAircraftType>
							<CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
							<CarrierAircraftTypeName>Airbus A320</CarrierAircraftTypeName>
						</CarrierAircraftType>
						<DatedOperatingLegID>DOL3</DatedOperatingLegID>
						<Dep/>
					</DatedOperatingLeg>
					<Dep>
						<AircraftScheduledDateTime>2026-08-31T08:35:00</AircraftScheduledDateTime>
						<IATA_LocationCode>BVA</IATA_LocationCode>
					</Dep>
					<Duration>P0Y0M0DT2H55M0S</Duration>
					<MarketingCarrierInfo>
						<CarrierDesigCode>W4</CarrierDesigCode>
						<MarketingCarrierFlightNumberText>3664</MarketingCarrierFlightNumberText>
					</MarketingCarrierInfo>
					<OperatingCarrierInfo>
						<CarrierDesigCode>W4</CarrierDesigCode>
					</OperatingCarrierInfo>
					<PaxSegmentID>SEG1</PaxSegmentID>
				</PaxSegment>
			</PaxSegmentList>
			<PriceClassList>
				<PriceClass>
					<CabinType>
						<CabinTypeName>ECONOMY</CabinTypeName>
					</CabinType>
					<Desc>
						<DescText>+1 bagage à main 40 x 30 x 20 cm (placé sous le siège)</DescText>
					</Desc>
					<Desc>
						<DescText>Bagage cabine 55 x 40 x 23 cm garanti en cabine (sauf raisons opérationnelles)</DescText>
					</Desc>
					<Desc>
						<DescText>32 kg checked-in bag included. Additional bags of the same weight can be purchased.</DescText>
					</Desc>
					<Desc>
						<DescText>Sélection de siège Premium avec les sièges offrant plus despace pour les jambes</DescText>
					</Desc>
					<Desc>
						<DescText>Option WIZZ Flex vous permettant de modifier la date de votre vol si nécessaire, sans frais supplémentaires</DescText>
					</Desc>
					<Desc>
						<DescText>Remboursement sur le compte WIZZ</DescText>
					</Desc>
					<Desc>
						<DescText>Embarquement prioritaire</DescText>
					</Desc>
					<Desc>
						<DescText>Free airport &amp; online check-in up to 30 days before flight departure.</DescText>
					</Desc>
					<Desc>
						<DescText>Enregistrement prioritaire</DescText>
					</Desc>
					<Desc>
						<DescText>Enregistrement automatique</DescText>
					</Desc>
					<Name>PLUS</Name>
					<PriceClassID>PC4</PriceClassID>
				</PriceClass>
			</PriceClassList>
		</DataLists>
		<PaymentFunctions>
			<PaymentSupportedMethod>
				<TypeCode>Cash</TypeCode>
			</PaymentSupportedMethod>
		</PaymentFunctions>
		<PricedOffer>
			<BaggageAllowance>
				<BaggageAllowanceRefID>BA3</BaggageAllowanceRefID>
				<BaggageFlightAssociations>
					<PaxSegmentRefID>SEG1</PaxSegmentRefID>
				</BaggageFlightAssociations>
				<PaxJourneyRefID>PJ1</PaxJourneyRefID>
				<PaxRefID>PAX1</PaxRefID>
			</BaggageAllowance>
			<BaggageAllowance>
				<BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
				<BaggageFlightAssociations>
					<PaxSegmentRefID>SEG1</PaxSegmentRefID>
				</BaggageFlightAssociations>
				<PaxJourneyRefID>PJ1</PaxJourneyRefID>
				<PaxRefID>PAX1</PaxRefID>
			</BaggageAllowance>
			<JourneyOverview>
				<JourneyPriceClass>
					<PaxJourneyRefID>PJ1</PaxJourneyRefID>
					<PriceClassRefID>PC4</PriceClassRefID>
				</JourneyPriceClass>
			</JourneyOverview>
			<OfferExpirationTimeLimitDateTime>2025-09-30T12:26:08.000</OfferExpirationTimeLimitDateTime>
			<OfferID>4a33c7ab-d5fe-4e15-8e4e-d13eab5b06d7</OfferID>
			<OfferItem>
				<FareDetail>
					<FareComponent>
						<CabinType>
							<CabinTypeCode>Z</CabinTypeCode>
							<CabinTypeName>ECONOMY</CabinTypeName>
						</CabinType>
						<FareBasisCode>Z</FareBasisCode>
						<PaxSegmentRefID>SEG1</PaxSegmentRefID>
						<PriceClassRefID>PC4</PriceClassRefID>
					</FareComponent>
					<FarePriceType>
						<FarePriceTypeCode>70J</FarePriceTypeCode>
						<Price>
							<BaseAmount CurCode="EUR">122.34</BaseAmount>
							<TotalAmount CurCode="EUR">122.34</TotalAmount>
						</Price>
					</FarePriceType>
					<PaxRefID>PAX1</PaxRefID>
				</FareDetail>
				<OfferItemID>83797443-e4ca-448a-a55a-deffcfadf8ef</OfferItemID>
				<PaymentTimeLimit>
					<PaymentTimeLimitDuration>PT0S</PaymentTimeLimitDuration>
				</PaymentTimeLimit>
				<Price>
					<BaseAmount CurCode="EUR">122.34</BaseAmount>
					<TotalAmount CurCode="EUR">122.34</TotalAmount>
				</Price>
				<Service>
					<PaxRefID>PAX1</PaxRefID>
					<ServiceAssociations>
						<PaxJourneyRefID>PJ1</PaxJourneyRefID>
					</ServiceAssociations>
					<ServiceID>SV178</ServiceID>
				</Service>
			</OfferItem>
			<OfferItem>
				<FareDetail>
					<FarePriceType>
						<FarePriceTypeCode>70J</FarePriceTypeCode>
						<Price>
							<BaseAmount CurCode="EUR">0.00</BaseAmount>
							<TotalAmount CurCode="EUR">0.00</TotalAmount>
						</Price>
					</FarePriceType>
					<PaxRefID>PAX1</PaxRefID>
				</FareDetail>
				<OfferItemID>869069b2-d80a-478e-9b8d-5e8d033b91a7</OfferItemID>
				<PaymentTimeLimit>
					<PaymentTimeLimitDuration>PT0S</PaymentTimeLimitDuration>
				</PaymentTimeLimit>
				<Price>
					<BaseAmount CurCode="EUR">0.00</BaseAmount>
					<TotalAmount CurCode="EUR">0.00</TotalAmount>
				</Price>
				<Service>
					<PaxRefID>PAX1</PaxRefID>
					<ServiceAssociations>
						<SeatAssignment>
							<DatedOperatingLegRefID>DOL3</DatedOperatingLegRefID>
							<Seat>
								<ColumnID>C</ColumnID>
								<RowNumber>2</RowNumber>
							</Seat>
						</SeatAssignment>
					</ServiceAssociations>
					<ServiceID>SV179</ServiceID>
				</Service>
			</OfferItem>
			<OfferItem>
				<FareDetail>
					<FareComponent>
						<CabinType>
							<CabinTypeCode>Z</CabinTypeCode>
							<CabinTypeName>ECONOMY</CabinTypeName>
						</CabinType>
						<FareBasisCode>Z</FareBasisCode>
						<PaxSegmentRefID>SEG1</PaxSegmentRefID>
						<PriceClassRefID>PC4</PriceClassRefID>
					</FareComponent>
					<FarePriceType>
						<FarePriceTypeCode>70J</FarePriceTypeCode>
						<Price>
							<BaseAmount>0</BaseAmount>
							<Fee>
								<Amount CurCode="EUR">1.22</Amount>
								<DescText>Service fees INR India</DescText>
								<DesigText>No commission</DesigText>
							</Fee>
							<TotalAmount CurCode="EUR">1.22</TotalAmount>
						</Price>
					</FarePriceType>
					<PaxRefID>PAX1</PaxRefID>
				</FareDetail>
				<OfferItemID>7ee6220e-5c25-4642-beef-f3539436a1e0</OfferItemID>
				<PaymentTimeLimit>
					<PaymentTimeLimitDuration>PT0S</PaymentTimeLimitDuration>
				</PaymentTimeLimit>
				<Price>
					<BaseAmount>0</BaseAmount>
					<TotalAmount CurCode="EUR">1.22</TotalAmount>
				</Price>
				<Service>
					<PaxRefID>PAX1</PaxRefID>
					<ServiceAssociations>
						<PaxJourneyRefID>PJ1</PaxJourneyRefID>
					</ServiceAssociations>
					<ServiceID>SV180</ServiceID>
				</Service>
			</OfferItem>
			<OwnerCode>W4</OwnerCode>
			<TotalPrice>
				<BaseAmount CurCode="EUR">122.34</BaseAmount>
				<Fee>
					<Amount CurCode="EUR">1.22</Amount>
					<DescText>Service fees INR India</DescText>
					<DesigText>No commission</DesigText>
				</Fee>
				<TotalAmount CurCode="EUR">123.56</TotalAmount>
			</TotalPrice>
		</PricedOffer>
		<ShoppingResponse>
			<ShoppingResponseRefID>0a2b9a1a-cc40-4c0d-8143-ead4d7c63889</ShoppingResponseRefID>
		</ShoppingResponse>
	</Response>
	<PayloadAttributes>
		<CorrelationID>a222c960-0d2c-4507-bd2c-59362825cc76</CorrelationID>
		<Timestamp>2026-07-31T12:36:40.907+02:00</Timestamp>
		<VersionNumber>19.2</VersionNumber>
	</PayloadAttributes>
</IATA_OfferPriceRS>
{% endhighlight %}

</details>

