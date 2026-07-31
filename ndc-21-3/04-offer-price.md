---
layout: page
title:  "Offer Price"
parent: NDC 21.3
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
| DistributionChain | Must contain agency ID as Seller | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Optional/Mandatory |
| --- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| --- |
| DataLists | The request data lists detailed [below](#datalists)                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Mandatory |
| PricedOffer | List of selected offers to price with Shopping session ID. <br /><b>Note</b>: For certain airlines that provide free and mandatory seats, they are automatically pre-selected and returned in [OfferPriceRS](#offerpricers) as [OfferItems](#offerItem) with a SeatAssignment node in [OfferServiceAssociation](#offerServiceAssociation) and the price equal to 0. These OfferItems cannot be removed and, currently, cannot be changed with different seat selections. See the [Samples](#samples) section for examples. | Mandatory |

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
| IdentityDoc      | Pax Document, detailed [below](#identitydoc)  | Optional           |

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
| OfferServiceAssociation | References to the details of this Service. May include either Passenger Journeys, a Service Definition, or a Selected Seat. | Mandatory |
| ServiceID           | The unique identifier of the service                                                                                        | Mandatory |

#### OfferServiceAssociation
{: .no_toc }

| Element              | Description                                                                                                                      | Optional/Mandatory |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------| --- |
| SeatAssignment       | The Seat Location selected by the Passenger (via SeatAvailability) or assigned to the Passenger by Orchestra for a given segment | Mandatory |
| ServiceDefinitionRef | Reference to the specific definition of this service                                                                             | Mandatory |

#### SeatAssignment

| Element              | Description                                            | Optional/Mandatory |
|----------------------|--------------------------------------------------------| --- |
| Seat | Detailed regarding the seat selection (row and column) | Mandatory |
| SeatAssignmentAssociations       | References to the operating leg or the segment            | Mandatory |

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
<IATA_OfferPriceRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
	<DistributionChain>
		<ns2:DistributionChainLink>
			<ns2:Ordinal>1</ns2:Ordinal>
			<ns2:OrgRole>Seller</ns2:OrgRole>
			<ns2:ParticipatingOrg>
				<ns2:OrgID>agency1234</ns2:OrgID>
			</ns2:ParticipatingOrg>
		</ns2:DistributionChainLink>
	</DistributionChain>
	<PayloadAttributes>
		<ns2:CorrelationID>97a37f8d-96bd-33ac-a15e-dfc529bdfc32</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:DataLists>
			<ns2:PaxList>
				<ns2:Pax>
					<ns2:PaxID>PAX1</ns2:PaxID>
					<ns2:PTC>ADT</ns2:PTC>
				</ns2:Pax>
				<ns2:Pax>
					<ns2:PaxID>PAX2</ns2:PaxID>
					<ns2:PTC>ADT</ns2:PTC>
				</ns2:Pax>
			</ns2:PaxList>
		</ns2:DataLists>
		<ns2:PricedOffer>
			<ns2:SelectedOfferList>
				<ns2:SelectedOffer>
					<ns2:OfferRefID>e3c8b7dd-1ef0-4e88-834d-7e95686364af</ns2:OfferRefID>
					<ns2:OwnerCode>BA</ns2:OwnerCode>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>6e78b002-e65d-46a3-86c2-91d357be72fe</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:PaxRefID>PAX2</ns2:PaxRefID>
					</ns2:SelectedOfferItem>
				</ns2:SelectedOffer>
				<ns2:SelectedOffer>
					<ns2:OfferRefID>07a2faaf-ef50-4acb-baa0-2aa565e57452</ns2:OfferRefID>
					<ns2:OwnerCode>BA</ns2:OwnerCode>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>eb5a15f1-e6a1-44ff-9faf-3f414a245da9</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:SelectedALaCarteOfferItem>
							<ns2:Qty>1</ns2:Qty>
						</ns2:SelectedALaCarteOfferItem>
					</ns2:SelectedOfferItem>
				</ns2:SelectedOffer>
			</ns2:SelectedOfferList>
		</ns2:PricedOffer>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_OfferPriceRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OfferPriceRS</b></summary>

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8'?>
<ns4:IATA_OfferPriceRS xmlns="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns2="http://www.travelsoft.fr/orchestra/ndc/login" xmlns:ns3="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns4="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns5="http://www.w3.org/2000/09/xmldsig#">
	<ns4:Response>
		<ns3:DataLists>
			<ns3:BaggageAllowanceList>
				<ns3:BaggageAllowance>
					<ns3:BaggageAllowanceID>BA5</ns3:BaggageAllowanceID>
					<ns3:PieceAllowance>
						<ns3:TotalQty>1</ns3:TotalQty>
					</ns3:PieceAllowance>
					<ns3:TypeCode>CarryOn</ns3:TypeCode>
				</ns3:BaggageAllowance>
				<ns3:BaggageAllowance>
					<ns3:BaggageAllowanceID>BA1</ns3:BaggageAllowanceID>
					<ns3:PieceAllowance>
						<ns3:TotalQty>0</ns3:TotalQty>
					</ns3:PieceAllowance>
					<ns3:TypeCode>Checked</ns3:TypeCode>
				</ns3:BaggageAllowance>
			</ns3:BaggageAllowanceList>
			<ns3:DatedMarketingSegmentList>
				<ns3:DatedMarketingSegment>
					<ns3:Arrival>
						<ns3:AircraftScheduledDateTime>2026-03-12T18:45:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>LHR</ns3:IATA_LocationCode>
						<ns3:TerminalName>5</ns3:TerminalName>
					</ns3:Arrival>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedMarketingSegmentId>DMS23</ns3:DatedMarketingSegmentId>
					<ns3:DatedOperatingSegmentRefId>DOS23</ns3:DatedOperatingSegmentRefId>
					<ns3:Dep>
						<ns3:AircraftScheduledDateTime>2026-03-12T18:30:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>CDG</ns3:IATA_LocationCode>
						<ns3:TerminalName>2C</ns3:TerminalName>
					</ns3:Dep>
					<ns3:MarketingCarrierFlightNumberText>315</ns3:MarketingCarrierFlightNumberText>
				</ns3:DatedMarketingSegment>
				<ns3:DatedMarketingSegment>
					<ns3:Arrival>
						<ns3:AircraftScheduledDateTime>2026-03-19T22:25:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>CDG</ns3:IATA_LocationCode>
						<ns3:TerminalName>2C</ns3:TerminalName>
					</ns3:Arrival>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedMarketingSegmentId>DMS24</ns3:DatedMarketingSegmentId>
					<ns3:DatedOperatingSegmentRefId>DOS24</ns3:DatedOperatingSegmentRefId>
					<ns3:Dep>
						<ns3:AircraftScheduledDateTime>2026-03-19T20:05:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>LHR</ns3:IATA_LocationCode>
						<ns3:TerminalName>5</ns3:TerminalName>
					</ns3:Dep>
					<ns3:MarketingCarrierFlightNumberText>322</ns3:MarketingCarrierFlightNumberText>
				</ns3:DatedMarketingSegment>
			</ns3:DatedMarketingSegmentList>
			<ns3:DatedOperatingLegList>
				<ns3:DatedOperatingLeg>
					<ns3:Arrival/>
					<ns3:CarrierAircraftType>
						<ns3:CarrierAircraftTypeCode>32N</ns3:CarrierAircraftTypeCode>
					</ns3:CarrierAircraftType>
					<ns3:DatedOperatingLegID>DOL23</ns3:DatedOperatingLegID>
					<ns3:Dep/>
				</ns3:DatedOperatingLeg>
				<ns3:DatedOperatingLeg>
					<ns3:Arrival/>
					<ns3:CarrierAircraftType>
						<ns3:CarrierAircraftTypeCode>32A</ns3:CarrierAircraftTypeCode>
					</ns3:CarrierAircraftType>
					<ns3:DatedOperatingLegID>DOL24</ns3:DatedOperatingLegID>
					<ns3:Dep/>
				</ns3:DatedOperatingLeg>
			</ns3:DatedOperatingLegList>
			<ns3:DatedOperatingSegmentList>
				<ns3:DatedOperatingSegment>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedOperatingLegRefID>DOL23</ns3:DatedOperatingLegRefID>
					<ns3:DatedOperatingSegmentId>DOS23</ns3:DatedOperatingSegmentId>
					<ns3:Duration>P0Y0M0DT1H15M0S</ns3:Duration>
				</ns3:DatedOperatingSegment>
				<ns3:DatedOperatingSegment>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedOperatingLegRefID>DOL24</ns3:DatedOperatingLegRefID>
					<ns3:DatedOperatingSegmentId>DOS24</ns3:DatedOperatingSegmentId>
					<ns3:Duration>P0Y0M0DT1H20M0S</ns3:Duration>
				</ns3:DatedOperatingSegment>
			</ns3:DatedOperatingSegmentList>
			<ns3:OriginDestList>
				<ns3:OriginDest>
					<ns3:DestCode>LHR</ns3:DestCode>
					<ns3:OriginCode>CDG</ns3:OriginCode>
					<ns3:OriginDestID>OD1</ns3:OriginDestID>
					<ns3:PaxJourneyRefID>PJ1</ns3:PaxJourneyRefID>
				</ns3:OriginDest>
				<ns3:OriginDest>
					<ns3:DestCode>CDG</ns3:DestCode>
					<ns3:OriginCode>LHR</ns3:OriginCode>
					<ns3:OriginDestID>OD2</ns3:OriginDestID>
					<ns3:PaxJourneyRefID>PJ4</ns3:PaxJourneyRefID>
				</ns3:OriginDest>
			</ns3:OriginDestList>
			<ns3:PaxJourneyList>
				<ns3:PaxJourney>
					<ns3:Duration>P0Y0M0DT1H15M0S</ns3:Duration>
					<ns3:PaxJourneyID>PJ1</ns3:PaxJourneyID>
					<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
				</ns3:PaxJourney>
				<ns3:PaxJourney>
					<ns3:Duration>P0Y0M0DT1H20M0S</ns3:Duration>
					<ns3:PaxJourneyID>PJ4</ns3:PaxJourneyID>
					<ns3:PaxSegmentRefID>SEG4</ns3:PaxSegmentRefID>
				</ns3:PaxJourney>
			</ns3:PaxJourneyList>
			<ns3:PaxList>
				<ns3:Pax>
					<ns3:PaxID>PAX1</ns3:PaxID>
					<ns3:PTC>ADT</ns3:PTC>
				</ns3:Pax>
				<ns3:Pax>
					<ns3:PaxID>PAX2</ns3:PaxID>
					<ns3:PTC>ADT</ns3:PTC>
				</ns3:Pax>
			</ns3:PaxList>
			<ns3:PaxSegmentList>
				<ns3:PaxSegment>
					<ns3:DatedMarketingSegmentRefId>DMS23</ns3:DatedMarketingSegmentRefId>
					<ns3:PaxSegmentID>SEG1</ns3:PaxSegmentID>
				</ns3:PaxSegment>
				<ns3:PaxSegment>
					<ns3:DatedMarketingSegmentRefId>DMS24</ns3:DatedMarketingSegmentRefId>
					<ns3:PaxSegmentID>SEG4</ns3:PaxSegmentID>
				</ns3:PaxSegment>
			</ns3:PaxSegmentList>
			<ns3:PriceClassList>
				<ns3:PriceClass>
					<ns3:CabinType>
						<ns3:CabinTypeName>ECONOMY</ns3:CabinTypeName>
					</ns3:CabinType>
					<ns3:Desc>
						<ns3:DescText>INC - SNACK</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>INC - CABIN BAG UPTO 56 X 45 X 25CM</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>INC - HANDBAG UPTO 40 X 30 X 15CM</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>CHA - CHANGE BEFORE DEPARTURE</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>CHA - CHANGE AFTER DEPARTURE</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>CHA - SAME DAY FLT CHNG P2P ONLY</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>CHA - SEAT CHOICE</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>CHA - 1ST BAG MAX 23KG 51LB 208LCM</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>CHA - 2ND BAG MAX 23KG 51LB 208LCM</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>NOF - REFUND BEFORE DEPARTURE</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>NOF - REFUND AFTER DEPARTURE</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>NOF - LOUNGE ACCESS</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>NOF - PRIORITY SECURITY</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>NOF - DEDICATED CHECK IN ZONE</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>BASIC</ns3:Name>
					<ns3:PriceClassID>PC5</ns3:PriceClassID>
				</ns3:PriceClass>
			</ns3:PriceClassList>
			<ns3:ServiceDefinitionList>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>1 Bagage supplémentaire</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>1 Bagage supplémentaire</ns3:Name>
					<ns3:ServiceDefinitionID>SD1</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
			</ns3:ServiceDefinitionList>
		</ns3:DataLists>
		<ns3:PaymentFunctions>
			<ns3:PaymentSupportedMethod>
				<ns3:PaymentTypeCode>Cash</ns3:PaymentTypeCode>
			</ns3:PaymentSupportedMethod>
		</ns3:PaymentFunctions>
		<ns3:PricedOffer>
			<ns3:BaggageAssociations>
				<ns3:BaggageAllowanceRefID>BA5</ns3:BaggageAllowanceRefID>
				<ns3:OfferFlightAssociations>
					<ns3:PaxSegmentReferences>
						<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						<ns3:PaxSegmentRefID>SEG4</ns3:PaxSegmentRefID>
					</ns3:PaxSegmentReferences>
					<ns3:PaxJourneyRef>
						<ns3:PaxJourneyRefID>PJ1</ns3:PaxJourneyRefID>
					</ns3:PaxJourneyRef>
				</ns3:OfferFlightAssociations>
				<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				<ns3:PaxRefID>PAX2</ns3:PaxRefID>
			</ns3:BaggageAssociations>
			<ns3:BaggageAssociations>
				<ns3:BaggageAllowanceRefID>BA1</ns3:BaggageAllowanceRefID>
				<ns3:OfferFlightAssociations>
					<ns3:PaxSegmentReferences>
						<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						<ns3:PaxSegmentRefID>SEG4</ns3:PaxSegmentRefID>
					</ns3:PaxSegmentReferences>
					<ns3:PaxJourneyRef>
						<ns3:PaxJourneyRefID>PJ1</ns3:PaxJourneyRefID>
					</ns3:PaxJourneyRef>
				</ns3:OfferFlightAssociations>
				<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				<ns3:PaxRefID>PAX2</ns3:PaxRefID>
			</ns3:BaggageAssociations>
			<ns3:JourneyOverview>
				<ns3:JourneyPriceClass>
					<ns3:PaxJourneyRefID>PJ1</ns3:PaxJourneyRefID>
					<ns3:PriceClassRefID>PC5</ns3:PriceClassRefID>
				</ns3:JourneyPriceClass>
				<ns3:JourneyPriceClass>
					<ns3:PaxJourneyRefID>PJ4</ns3:PaxJourneyRefID>
					<ns3:PriceClassRefID>PC5</ns3:PriceClassRefID>
				</ns3:JourneyPriceClass>
			</ns3:JourneyOverview>
			<ns3:OfferExpirationTimeLimitDateTime>2026-01-28T17:19:05.000</ns3:OfferExpirationTimeLimitDateTime>
			<ns3:OfferID>009bd994-285d-4c00-8140-6afeb185fe38</ns3:OfferID>
			<ns3:OfferItem>
				<ns3:FareDetail>
					<ns3:FareComponent>
						<ns3:CabinType>
							<ns3:CabinTypeCode>Q</ns3:CabinTypeCode>
							<ns3:CabinTypeName>ECONOMY</ns3:CabinTypeName>
						</ns3:CabinType>
						<ns3:FareTypeCode>70J</ns3:FareTypeCode>
						<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						<ns3:PaxSegmentRefID>SEG4</ns3:PaxSegmentRefID>
						<ns3:PriceClassRefID>PC5</ns3:PriceClassRefID>
					</ns3:FareComponent>
					<ns3:FarePriceType>
						<ns3:Price>
							<ns3:BaseAmount CurCode="EUR">41.25</ns3:BaseAmount>
							<ns3:TaxSummary>
								<ns3:Tax>
									<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
									<ns3:TaxCode>GENERAL_TAXES_PAX_1_2</ns3:TaxCode>
									<ns3:TaxName>Taxes - PAX1,2</ns3:TaxName>
								</ns3:Tax>
								<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
							</ns3:TaxSummary>
							<ns3:TotalAmount CurCode="EUR">154.61</ns3:TotalAmount>
						</ns3:Price>
					</ns3:FarePriceType>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
				</ns3:FareDetail>
				<ns3:OfferItemID>1f6a66e2-eb8f-4eb3-b05f-dfe58a8f9191</ns3:OfferItemID>
				<ns3:PaymentTimeLimit>
					<ns3:PaymentTimeLimitDuration>
						<ns3:PaymentTimeLimitDuration>PT0S</ns3:PaymentTimeLimitDuration>
					</ns3:PaymentTimeLimitDuration>
				</ns3:PaymentTimeLimit>
				<ns3:Price>
					<ns3:BaseAmount CurCode="EUR">82.50</ns3:BaseAmount>
					<ns3:TaxSummary>
						<ns3:Tax>
							<ns3:Amount CurCode="EUR">226.72</ns3:Amount>
							<ns3:TaxCode>GENERAL_TAXES_PAX_1_2</ns3:TaxCode>
							<ns3:TaxName>Taxes - PAX1,2</ns3:TaxName>
						</ns3:Tax>
						<ns3:TotalTaxAmount CurCode="EUR">226.72</ns3:TotalTaxAmount>
					</ns3:TaxSummary>
					<ns3:TotalAmount CurCode="EUR">309.22</ns3:TotalAmount>
				</ns3:Price>
				<ns3:Service>
					<ns3:OfferServiceAssociation>
						<ns3:PaxJourneyRef>
							<ns3:PaxJourneyRefID>PJ1</ns3:PaxJourneyRefID>
							<ns3:PaxJourneyRefID>PJ4</ns3:PaxJourneyRefID>
						</ns3:PaxJourneyRef>
					</ns3:OfferServiceAssociation>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
					<ns3:ServiceID>SV725</ns3:ServiceID>
				</ns3:Service>
			</ns3:OfferItem>
			<ns3:OfferItem>
				<ns3:FareDetail>
					<ns3:FarePriceType>
						<ns3:Price>
							<ns3:BaseAmount CurCode="EUR">62.50</ns3:BaseAmount>
							<ns3:TotalAmount CurCode="EUR">62.50</ns3:TotalAmount>
						</ns3:Price>
					</ns3:FarePriceType>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				</ns3:FareDetail>
				<ns3:OfferItemID>1da39e1d-3689-4953-b3ec-bbc3123fa7c2</ns3:OfferItemID>
				<ns3:PaymentTimeLimit>
					<ns3:PaymentTimeLimitDuration>
						<ns3:PaymentTimeLimitDuration>PT0S</ns3:PaymentTimeLimitDuration>
					</ns3:PaymentTimeLimitDuration>
				</ns3:PaymentTimeLimit>
				<ns3:Price>
					<ns3:BaseAmount CurCode="EUR">62.50</ns3:BaseAmount>
					<ns3:TotalAmount CurCode="EUR">62.50</ns3:TotalAmount>
				</ns3:Price>
				<ns3:Service>
					<ns3:OfferServiceAssociation>
						<ns3:ServiceDefinitionRef>
							<ns3:OfferFlightAssociations>
								<ns3:PaxSegmentReferences>
									<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
								</ns3:PaxSegmentReferences>
							</ns3:OfferFlightAssociations>
							<ns3:ServiceDefinitionRefID>SD1</ns3:ServiceDefinitionRefID>
						</ns3:ServiceDefinitionRef>
					</ns3:OfferServiceAssociation>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:ServiceID>SV726</ns3:ServiceID>
				</ns3:Service>
			</ns3:OfferItem>
			<ns3:OwnerCode>BA</ns3:OwnerCode>
			<ns3:TotalPrice>
				<ns3:BaseAmount CurCode="EUR">145.00</ns3:BaseAmount>
				<ns3:TaxSummary>
					<ns3:Tax>
						<ns3:Amount CurCode="EUR">226.72</ns3:Amount>
						<ns3:TaxCode>GENERAL_TAXES_PAX_1_2</ns3:TaxCode>
						<ns3:TaxName>Taxes - PAX1,2</ns3:TaxName>
					</ns3:Tax>
					<ns3:TotalTaxAmount CurCode="EUR">226.73</ns3:TotalTaxAmount>
				</ns3:TaxSummary>
				<ns3:TotalAmount CurCode="EUR">371.73</ns3:TotalAmount>
			</ns3:TotalPrice>
		</ns3:PricedOffer>
		<ns3:Warning>
			<ns3:Code>SUPPLIER_WARN</ns3:Code>
			<ns3:DescText>Allowed forms of payment for these offer(s) - Card or Previously issued valid E-Voucher</ns3:DescText>
			<ns3:OwnerName>BRITISH_AIRWAYS</ns3:OwnerName>
		</ns3:Warning>
		<ns3:Warning>
			<ns3:Code>SUPPLIER_WARN</ns3:Code>
			<ns3:DescText>All services may not be delivered as the requested fare component may include a codeshare flight or an interline itinerary.</ns3:DescText>
			<ns3:OwnerName>BRITISH_AIRWAYS</ns3:OwnerName>
		</ns3:Warning>
	</ns4:Response>
	<ns4:PayloadAttributes>
		<ns3:CorrelationID>97a37f8d-96bd-33ac-a15e-dfc529bdfc32</ns3:CorrelationID>
		<ns3:Timestamp>2026-01-28T17:01:05.277+01:00</ns3:Timestamp>
		<ns3:VersionNumber>21.3</ns3:VersionNumber>
	</ns4:PayloadAttributes>
</ns4:IATA_OfferPriceRS>
{% endhighlight %}

</details>

<details>
  <summary><b>OfferPriceRS (with mandatory free seat)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns2:IATA_OfferPriceRS xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
	<ns2:Response>
		<DataLists>
			<BaggageAllowanceList>
				<BaggageAllowance>
					<BaggageAllowanceID>BA3</BaggageAllowanceID>
					<PieceAllowance>
						<TotalQty>1</TotalQty>
					</PieceAllowance>
					<TypeCode>CarryOn</TypeCode>
				</BaggageAllowance>
				<BaggageAllowance>
					<BaggageAllowanceID>BA2</BaggageAllowanceID>
					<PieceAllowance>
						<TotalQty>1</TotalQty>
					</PieceAllowance>
					<TypeCode>Checked</TypeCode>
				</BaggageAllowance>
			</BaggageAllowanceList>
			<DatedMarketingSegmentList>
				<DatedMarketingSegment>
					<Arrival>
						<AircraftScheduledDateTime>2026-08-31T12:30:00</AircraftScheduledDateTime>
						<IATA_LocationCode>IAS</IATA_LocationCode>
					</Arrival>
					<CarrierDesigCode>W4</CarrierDesigCode>
					<DatedMarketingSegmentId>DMS6</DatedMarketingSegmentId>
					<DatedOperatingSegmentRefId>DOS6</DatedOperatingSegmentRefId>
					<Dep>
						<AircraftScheduledDateTime>2026-08-31T08:35:00</AircraftScheduledDateTime>
						<IATA_LocationCode>BVA</IATA_LocationCode>
					</Dep>
					<MarketingCarrierFlightNumberText>3664</MarketingCarrierFlightNumberText>
				</DatedMarketingSegment>
			</DatedMarketingSegmentList>
			<DatedOperatingLegList>
				<DatedOperatingLeg>
					<Arrival/>
					<CarrierAircraftType>
						<CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
						<CarrierAircraftTypeName>Airbus A320</CarrierAircraftTypeName>
					</CarrierAircraftType>
					<DatedOperatingLegID>DOL6</DatedOperatingLegID>
					<Dep/>
				</DatedOperatingLeg>
			</DatedOperatingLegList>
			<DatedOperatingSegmentList>
				<DatedOperatingSegment>
					<CarrierDesigCode>W4</CarrierDesigCode>
					<DatedOperatingLegRefID>DOL6</DatedOperatingLegRefID>
					<DatedOperatingSegmentId>DOS6</DatedOperatingSegmentId>
					<Duration>P0Y0M0DT2H55M0S</Duration>
				</DatedOperatingSegment>
			</DatedOperatingSegmentList>
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
					<DatedMarketingSegmentRefId>DMS6</DatedMarketingSegmentRefId>
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
				<PaymentTypeCode>Cash</PaymentTypeCode>
			</PaymentSupportedMethod>
		</PaymentFunctions>
		<PricedOffer>
			<BaggageAssociations>
				<BaggageAllowanceRefID>BA3</BaggageAllowanceRefID>
				<OfferFlightAssociations>
					<PaxSegmentReferences>
						<PaxSegmentRefID>SEG1</PaxSegmentRefID>
					</PaxSegmentReferences>
					<PaxJourneyRef>
						<PaxJourneyRefID>PJ1</PaxJourneyRefID>
					</PaxJourneyRef>
				</OfferFlightAssociations>
				<PaxRefID>PAX1</PaxRefID>
			</BaggageAssociations>
			<BaggageAssociations>
				<BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
				<OfferFlightAssociations>
					<PaxSegmentReferences>
						<PaxSegmentRefID>SEG1</PaxSegmentRefID>
					</PaxSegmentReferences>
					<PaxJourneyRef>
						<PaxJourneyRefID>PJ1</PaxJourneyRefID>
					</PaxJourneyRef>
				</OfferFlightAssociations>
				<PaxRefID>PAX1</PaxRefID>
			</BaggageAssociations>
			<JourneyOverview>
				<JourneyPriceClass>
					<PaxJourneyRefID>PJ1</PaxJourneyRefID>
					<PriceClassRefID>PC4</PriceClassRefID>
				</JourneyPriceClass>
			</JourneyOverview>
			<OfferExpirationTimeLimitDateTime>2025-09-30T12:26:08.000</OfferExpirationTimeLimitDateTime>
			<OfferID>3ae74012-0661-49be-a69d-451a1016ccb8</OfferID>
			<OfferItem>
				<FareDetail>
					<FareComponent>
						<CabinType>
							<CabinTypeCode>Z</CabinTypeCode>
							<CabinTypeName>ECONOMY</CabinTypeName>
						</CabinType>
						<FareBasisCode>Z</FareBasisCode>
						<FareTypeCode>70J</FareTypeCode>
						<PaxSegmentRefID>SEG1</PaxSegmentRefID>
						<PriceClassRefID>PC4</PriceClassRefID>
					</FareComponent>
					<FarePriceType>
						<Price>
							<BaseAmount CurCode="EUR">122.34</BaseAmount>
							<TotalAmount CurCode="EUR">122.34</TotalAmount>
						</Price>
					</FarePriceType>
					<PaxRefID>PAX1</PaxRefID>
				</FareDetail>
				<OfferItemID>7b545879-242b-4d71-abc3-26cb1bdf6eda</OfferItemID>
				<PaymentTimeLimit>
					<PaymentTimeLimitDuration>
						<PaymentTimeLimitDuration>PT0S</PaymentTimeLimitDuration>
					</PaymentTimeLimitDuration>
				</PaymentTimeLimit>
				<Price>
					<BaseAmount CurCode="EUR">122.34</BaseAmount>
					<TotalAmount CurCode="EUR">122.34</TotalAmount>
				</Price>
				<Service>
					<OfferServiceAssociation>
						<PaxJourneyRef>
							<PaxJourneyRefID>PJ1</PaxJourneyRefID>
						</PaxJourneyRef>
					</OfferServiceAssociation>
					<PaxRefID>PAX1</PaxRefID>
					<ServiceID>SV41</ServiceID>
				</Service>
			</OfferItem>
			<OfferItem>
				<FareDetail>
					<FarePriceType>
						<Price>
							<BaseAmount CurCode="EUR">0.00</BaseAmount>
							<TotalAmount CurCode="EUR">0.00</TotalAmount>
						</Price>
					</FarePriceType>
					<PaxRefID>PAX1</PaxRefID>
				</FareDetail>
				<OfferItemID>08297d09-b668-4fb8-ad1b-1e3cbf3c9cd2</OfferItemID>
				<PaymentTimeLimit>
					<PaymentTimeLimitDuration>
						<PaymentTimeLimitDuration>PT0S</PaymentTimeLimitDuration>
					</PaymentTimeLimitDuration>
				</PaymentTimeLimit>
				<Price>
					<BaseAmount CurCode="EUR">0.00</BaseAmount>
					<TotalAmount CurCode="EUR">0.00</TotalAmount>
				</Price>
				<Service>
					<OfferServiceAssociation>
						<SeatAssignment>
							<Seat>
								<ColumnID>C</ColumnID>
								<RowNumber>2</RowNumber>
							</Seat>
							<SeatAssignmentAssociations>
								<DatedOperatingLegRef>
									<DatedOperatingLegRefID>DOL6</DatedOperatingLegRefID>
								</DatedOperatingLegRef>
							</SeatAssignmentAssociations>
						</SeatAssignment>
					</OfferServiceAssociation>
					<PaxRefID>PAX1</PaxRefID>
					<ServiceID>SV42</ServiceID>
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
						<FareTypeCode>70J</FareTypeCode>
						<PaxSegmentRefID>SEG1</PaxSegmentRefID>
						<PriceClassRefID>PC4</PriceClassRefID>
					</FareComponent>
					<FarePriceType>
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
				<OfferItemID>f98da77b-3ae1-4142-acf7-f07ad1bff394</OfferItemID>
				<PaymentTimeLimit>
					<PaymentTimeLimitDuration>
						<PaymentTimeLimitDuration>PT0S</PaymentTimeLimitDuration>
					</PaymentTimeLimitDuration>
				</PaymentTimeLimit>
				<Price>
					<BaseAmount>0</BaseAmount>
					<Fee>
						<Amount CurCode="EUR">1.22</Amount>
						<DescText>Service fees INR India</DescText>
						<DesigText>No commission</DesigText>
					</Fee>
					<TotalAmount CurCode="EUR">1.22</TotalAmount>
				</Price>
				<Service>
					<OfferServiceAssociation>
						<PaxJourneyRef>
							<PaxJourneyRefID>PJ1</PaxJourneyRefID>
						</PaxJourneyRef>
					</OfferServiceAssociation>
					<PaxRefID>PAX1</PaxRefID>
					<ServiceID>SV43</ServiceID>
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
	</ns2:Response>
	<ns2:PayloadAttributes>
		<CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</CorrelationID>
		<Timestamp>2026-07-31T12:21:02.306+02:00</Timestamp>
		<VersionNumber>21.3</VersionNumber>
	</ns2:PayloadAttributes>
</ns2:IATA_OfferPriceRS>
{% endhighlight %}

</details>
