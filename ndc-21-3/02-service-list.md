---
layout: page
title:  "Service List"
parent: NDC 21.3
nav_order: 2
---

# ServiceList operation
{: .no_toc }
The service list method returns a list of available ancillaries for the selected flight offer in request.

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

# ServiceListRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| DistributionChain | Must contain agency ID as Seller | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| ServiceListCoreRequest | Must contain the flight offer selected | Mandatory |
| Paxs | List of passengers (same as AirShoppingRQ) | Mandatory |

# ServiceListRS

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PayloadAttributes | Same as requested + timestamp | Mandatory |
| Response | The response element detailed [below](#response) | Mandatory |

## Response
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Warnings | List of warnings returned by provider | Optional |
| DataLists | The response data lists (service definitions, etc) | Mandatory |
| ALaCarteOffer | The offer element detailed [below](#alacarteoffer) | Mandatory |

### ALaCarteOffer
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferID | The offer ID | Mandatory |
| OfferItem | List of offer items detailed [below](#offeritem) | Mandatory |

#### OfferItem
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferItemID | The offer item ID | Mandatory |
| Eligibility | Eligibility of this offer item (can be flight/PAX association) | Mandatory |
| Service | The service definition reference (contained id DataLists) | Mandatory |
| UnitPrice | Price of this offer item | Mandatory |

# Samples

<details>
  <summary><b>ServiceListRQ</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_ServiceListRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
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
		<ns2:Pax>
			<ns2:PaxID>PAX1</ns2:PaxID>
			<ns2:PTC>ADT</ns2:PTC>
		</ns2:Pax>
		<ns2:Pax>
			<ns2:PaxID>PAX2</ns2:PaxID>
			<ns2:PTC>ADT</ns2:PTC>
		</ns2:Pax>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
		<ns2:ServiceListCoreRequest>
			<ns2:OfferRequest>
				<ns2:Offer>
					<ns2:OfferID>e3c8b7dd-1ef0-4e88-834d-7e95686364af</ns2:OfferID>
					<ns2:OfferItem>
						<ns2:OfferItemID>6e78b002-e65d-46a3-86c2-91d357be72fe</ns2:OfferItemID>
						<ns2:OwnerCode>BA</ns2:OwnerCode>
					</ns2:OfferItem>
					<ns2:OwnerCode>BA</ns2:OwnerCode>
				</ns2:Offer>
			</ns2:OfferRequest>
		</ns2:ServiceListCoreRequest>
	</Request>
</IATA_ServiceListRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>ServiceListRS</b></summary>

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8'?>
<ns4:IATA_ServiceListRS xmlns="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns2="http://www.travelsoft.fr/orchestra/ndc/login" xmlns:ns3="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns4="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns5="http://www.w3.org/2000/09/xmldsig#">
	<ns4:Response>
		<ns3:ALaCarteOffer>
			<ns3:OfferID>07a2faaf-ef50-4acb-baa0-2aa565e57452</ns3:OfferID>
			<ns3:OfferItem>
				<ns3:Eligibility>
					<ns3:OfferFlightAssociations>
						<ns3:PaxSegmentReferences>
							<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentReferences>
					</ns3:OfferFlightAssociations>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				</ns3:Eligibility>
				<ns3:OfferItemID>eb5a15f1-e6a1-44ff-9faf-3f414a245da9</ns3:OfferItemID>
				<ns3:Service>
					<ns3:ServiceDefinitionRefID>SD1</ns3:ServiceDefinitionRefID>
					<ns3:ServiceID>SV151</ns3:ServiceID>
				</ns3:Service>
				<ns3:UnitPrice>
					<ns3:TotalAmount CurCode="EUR">62.50</ns3:TotalAmount>
				</ns3:UnitPrice>
			</ns3:OfferItem>
			<ns3:OfferItem>
				<ns3:Eligibility>
					<ns3:OfferFlightAssociations>
						<ns3:PaxSegmentReferences>
							<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentReferences>
					</ns3:OfferFlightAssociations>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				</ns3:Eligibility>
				<ns3:OfferItemID>cf48bb82-3244-468d-b60d-b5f37aa0d1c8</ns3:OfferItemID>
				<ns3:Service>
					<ns3:ServiceDefinitionRefID>SD2</ns3:ServiceDefinitionRefID>
					<ns3:ServiceID>SV152</ns3:ServiceID>
				</ns3:Service>
				<ns3:UnitPrice>
					<ns3:TotalAmount CurCode="EUR">162.50</ns3:TotalAmount>
				</ns3:UnitPrice>
			</ns3:OfferItem>
			<ns3:OfferItem>
				<ns3:Eligibility>
					<ns3:OfferFlightAssociations>
						<ns3:PaxSegmentReferences>
							<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentReferences>
					</ns3:OfferFlightAssociations>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				</ns3:Eligibility>
				<ns3:OfferItemID>d0408744-d61a-46f6-bc25-39286140ca72</ns3:OfferItemID>
				<ns3:Service>
					<ns3:ServiceDefinitionRefID>SD3</ns3:ServiceDefinitionRefID>
					<ns3:ServiceID>SV153</ns3:ServiceID>
				</ns3:Service>
				<ns3:UnitPrice>
					<ns3:TotalAmount CurCode="EUR">281.25</ns3:TotalAmount>
				</ns3:UnitPrice>
			</ns3:OfferItem>
			<ns3:OfferItem>
				<ns3:Eligibility>
					<ns3:OfferFlightAssociations>
						<ns3:PaxSegmentReferences>
							<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentReferences>
					</ns3:OfferFlightAssociations>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				</ns3:Eligibility>
				<ns3:OfferItemID>7cd05269-f944-4756-bfec-b9243e93a56a</ns3:OfferItemID>
				<ns3:Service>
					<ns3:ServiceDefinitionRefID>SD4</ns3:ServiceDefinitionRefID>
					<ns3:ServiceID>SV154</ns3:ServiceID>
				</ns3:Service>
				<ns3:UnitPrice>
					<ns3:TotalAmount CurCode="EUR">400.00</ns3:TotalAmount>
				</ns3:UnitPrice>
			</ns3:OfferItem>
		</ns3:ALaCarteOffer>
		<ns3:DataLists>
			<ns3:DatedMarketingSegmentList>
				<ns3:DatedMarketingSegment>
					<ns3:Arrival>
						<ns3:AircraftScheduledDateTime>2026-03-12T18:45:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>LHR</ns3:IATA_LocationCode>
						<ns3:TerminalName>5</ns3:TerminalName>
					</ns3:Arrival>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedMarketingSegmentId>DMS19</ns3:DatedMarketingSegmentId>
					<ns3:DatedOperatingSegmentRefId>DOS19</ns3:DatedOperatingSegmentRefId>
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
					<ns3:DatedMarketingSegmentId>DMS20</ns3:DatedMarketingSegmentId>
					<ns3:DatedOperatingSegmentRefId>DOS20</ns3:DatedOperatingSegmentRefId>
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
					<ns3:DatedOperatingLegID>DOL19</ns3:DatedOperatingLegID>
					<ns3:Dep/>
				</ns3:DatedOperatingLeg>
				<ns3:DatedOperatingLeg>
					<ns3:Arrival/>
					<ns3:CarrierAircraftType>
						<ns3:CarrierAircraftTypeCode>32A</ns3:CarrierAircraftTypeCode>
					</ns3:CarrierAircraftType>
					<ns3:DatedOperatingLegID>DOL20</ns3:DatedOperatingLegID>
					<ns3:Dep/>
				</ns3:DatedOperatingLeg>
			</ns3:DatedOperatingLegList>
			<ns3:DatedOperatingSegmentList>
				<ns3:DatedOperatingSegment>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedOperatingLegRefID>DOL19</ns3:DatedOperatingLegRefID>
					<ns3:DatedOperatingSegmentId>DOS19</ns3:DatedOperatingSegmentId>
					<ns3:Duration>P0Y0M0DT1H15M0S</ns3:Duration>
				</ns3:DatedOperatingSegment>
				<ns3:DatedOperatingSegment>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedOperatingLegRefID>DOL20</ns3:DatedOperatingLegRefID>
					<ns3:DatedOperatingSegmentId>DOS20</ns3:DatedOperatingSegmentId>
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
					<ns3:DatedMarketingSegmentRefId>DMS19</ns3:DatedMarketingSegmentRefId>
					<ns3:PaxSegmentID>SEG1</ns3:PaxSegmentID>
				</ns3:PaxSegment>
				<ns3:PaxSegment>
					<ns3:DatedMarketingSegmentRefId>DMS20</ns3:DatedMarketingSegmentRefId>
					<ns3:PaxSegmentID>SEG4</ns3:PaxSegmentID>
				</ns3:PaxSegment>
			</ns3:PaxSegmentList>
			<ns3:ServiceDefinitionList>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
						<ns3:TaxonomyFeature>
							<ns3:CodesetCode>OnePerPax</ns3:CodesetCode>
							<ns3:CodesetNameCode>ChoiceRestriction</ns3:CodesetNameCode>
						</ns3:TaxonomyFeature>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>1 Bagage supplémentaire</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>1 Bagage supplémentaire</ns3:Name>
					<ns3:ServiceDefinitionID>SD1</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
						<ns3:TaxonomyFeature>
							<ns3:CodesetCode>OnePerPax</ns3:CodesetCode>
							<ns3:CodesetNameCode>ChoiceRestriction</ns3:CodesetNameCode>
						</ns3:TaxonomyFeature>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>2 Bagages supplémentaires</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>2 Bagages supplémentaires</ns3:Name>
					<ns3:ServiceDefinitionID>SD2</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
						<ns3:TaxonomyFeature>
							<ns3:CodesetCode>OnePerPax</ns3:CodesetCode>
							<ns3:CodesetNameCode>ChoiceRestriction</ns3:CodesetNameCode>
						</ns3:TaxonomyFeature>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>3 Bagages supplémentaires</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>3 Bagages supplémentaires</ns3:Name>
					<ns3:ServiceDefinitionID>SD3</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
						<ns3:TaxonomyFeature>
							<ns3:CodesetCode>OnePerPax</ns3:CodesetCode>
							<ns3:CodesetNameCode>ChoiceRestriction</ns3:CodesetNameCode>
						</ns3:TaxonomyFeature>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>4 Bagages supplémentaires</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>4 Bagages supplémentaires</ns3:Name>
					<ns3:ServiceDefinitionID>SD4</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
						<ns3:TaxonomyFeature>
							<ns3:CodesetCode>OnePerPax</ns3:CodesetCode>
							<ns3:CodesetNameCode>ChoiceRestriction</ns3:CodesetNameCode>
						</ns3:TaxonomyFeature>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>5 Bagages supplémentaires</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>5 Bagages supplémentaires</ns3:Name>
					<ns3:ServiceDefinitionID>SD5</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
						<ns3:TaxonomyFeature>
							<ns3:CodesetCode>OnePerPax</ns3:CodesetCode>
							<ns3:CodesetNameCode>ChoiceRestriction</ns3:CodesetNameCode>
						</ns3:TaxonomyFeature>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>6 Bagages supplémentaires</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>6 Bagages supplémentaires</ns3:Name>
					<ns3:ServiceDefinitionID>SD6</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
						<ns3:TaxonomyFeature>
							<ns3:CodesetCode>OnePerPax</ns3:CodesetCode>
							<ns3:CodesetNameCode>ChoiceRestriction</ns3:CodesetNameCode>
						</ns3:TaxonomyFeature>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>7 Bagages supplémentaires</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>7 Bagages supplémentaires</ns3:Name>
					<ns3:ServiceDefinitionID>SD7</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
						<ns3:TaxonomyFeature>
							<ns3:CodesetCode>OnePerPax</ns3:CodesetCode>
							<ns3:CodesetNameCode>ChoiceRestriction</ns3:CodesetNameCode>
						</ns3:TaxonomyFeature>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>8 Bagages supplémentaires</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>8 Bagages supplémentaires</ns3:Name>
					<ns3:ServiceDefinitionID>SD8</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
						<ns3:TaxonomyFeature>
							<ns3:CodesetCode>OnePerPax</ns3:CodesetCode>
							<ns3:CodesetNameCode>ChoiceRestriction</ns3:CodesetNameCode>
						</ns3:TaxonomyFeature>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>9 Bagages supplémentaires</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>9 Bagages supplémentaires</ns3:Name>
					<ns3:ServiceDefinitionID>SD9</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
				<ns3:ServiceDefinition>
					<ns3:AirlineTaxonomy>
						<ns3:DescText>Checked Baggage</ns3:DescText>
						<ns3:TaxonomyCode>13EC</ns3:TaxonomyCode>
						<ns3:TaxonomyFeature>
							<ns3:CodesetCode>OnePerPax</ns3:CodesetCode>
							<ns3:CodesetNameCode>ChoiceRestriction</ns3:CodesetNameCode>
						</ns3:TaxonomyFeature>
					</ns3:AirlineTaxonomy>
					<ns3:Desc>
						<ns3:DescText>10 Bagages supplémentaires</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>10 Bagages supplémentaires</ns3:Name>
					<ns3:ServiceDefinitionID>SD10</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
			</ns3:ServiceDefinitionList>
		</ns3:DataLists>
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
		<ns3:Timestamp>2026-01-28T16:49:14.142+01:00</ns3:Timestamp>
		<ns3:VersionNumber>21.3</ns3:VersionNumber>
	</ns4:PayloadAttributes>
</ns4:IATA_ServiceListRS>
{% endhighlight %}

</details>
