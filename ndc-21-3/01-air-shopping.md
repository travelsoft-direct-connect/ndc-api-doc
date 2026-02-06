---
layout: page
title:  "Air Shopping"
parent: NDC 21.3
nav_order: 1
---

# AirShopping operation
{: .no_toc }

The air shopping operation allows to initiate a shopping session and returns a list of flight offers depending on criteria given in request.

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
- *Accept-Encoding*: set to gzip to enable GZIP compression for faster responses

## Control header

The provider to request must be sent in the control header. For example:

{% highlight xml %}
<Control Provider="SWITCHALLINONE" />
{% endhighlight %}

# AirShoppingRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| DistributionChain | Must contain agency ID as Seller | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| FlightRequest | List of origin/destination criteria (one OD for one-way, two ODs for round-trip or open-jaw) | Mandatory |
| FlightRelatedCriteria | The flight criteria detailed [below](#flightrelatedcriteria) | Mandatory |
| PaxList | List of passengers with type (PTC): {::nomarkdown}<ul><li>ADT for adults</li><li>CHD for children + birth date or age </li><li>INF for infants</li></ul>{:/} Note that loyalty program account can be set if given as input with passenger name, see [below](#loyaltyprogramaccount) | Mandatory |
| ResponseParameters | {::nomarkdown}<ul><li>Currency requested (EUR by default)</li><li>Language requested (ignored if not supported by the provider)</li></ul>{:/} | Optional |
| OfferCriteria | The offer criteria containing flight criteria, as preferred transport class (ECONOMY, BUSINESS, etc), see [below](#offercriteria) | Mandatory |

### LoyaltyProgramAccount
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| AccountNumber | FQTV account number | Mandatory |
| LoyaltyProgram | FQTV program detailed [below](#loyaltyprogram) | Mandatory |

#### LoyaltyProgram
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| ProgramCode | FQTV program code | Mandatory |
| ProgramName | FQTV program name | Optional |

### OfferCriteria
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| FareCriteria | Used to request specific fare codes [below](#farecriteria) | Optional |
| SpecialNeedsCriteria | Used to limit search results [below](#specialneedscriteria) | Optional |

### FlightRelatedCriteria
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| FlightCriteria | The flight criteria detailed [below](#flightcriteria) | Mandatory |
| CarrierCriteria | Used to request specific carriers [below](#carriercriteria) | Optional |

#### FareCriteria
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| FareTypeCode | Indicates the fare codes to request, can be: {::nomarkdown}<ul><li>70J (public)</li><li>70E (negotiated)</li><li>70F (corporate)</li><li>758-SEAMEN (seamen)</li><li>758-IT (TourOperator)</li><li>758-VFR (visit friends and relatives)</li><li>SPANISH_RESIDENT (Spanish resident discounted fares)</li></ul> {:/} Note: if FareTypeCode is omitted, public fares will be requested | Mandatory |

#### FlightCriteria
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| FlightCharacteristicsCriteria | Allows to request non-stop flights by setting NonStop in CharacteristicsCode with PrefLevelCode=Preferred (or PrefLevelCode=Exclude to request flights with stops only) | Optional |

#### SpecialNeedsCriteria
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Qty | Indicates the desired max results | Mandatory |
| FreeText | Indicates that we are limiting offers (possible values: offerLimit) | Mandatory |
| SpecialServiceCode | Indicates how we are limiting results (possible values: CHEAPEST, FIRST, SLICE) <br> Default is CHEAPEST <br> Note: Slice takes randomly some results at the beginning, middle and end of the response | Optional |

### CarrierCriteria
{: .no_toc }

| Carrier | Indicates the carriers to request by setting AirlineDesigCode with the IATA airline code | Mandatory |

# AirShoppingRS

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PayloadAttributes | Same as requested + timestamp | Mandatory |
| Response | The response element detailed [below](#response) | Mandatory |

## Response
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Warnings | List of warnings returned by provider | Optional |
| DataLists | The response data lists (journeys, segments, etc) | Mandatory |
| OffersGroup | List of flight offers detailed [below](#offersgroup). For a round-trip search, offers can be returned as {::nomarkdown}<ul><li>combination mode (round-trip offers => only one offer to select), Offer/MatchTypeCode="Full"</li><li>flat mode (outbound offers + inbound offers => two offers to select), Offer/MatchTypeCode="Partial"</li></ul>{:/}| Mandatory |

### OffersGroup
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferID | The offer ID | Mandatory |
| OwnerCode | The airline owner code | Mandatory |
| JourneyOverview | Overview of contained journeys with price class links | Mandatory |
| MatchTypeCode | "Full" if combination mode (or oneway search), "Partial" if flat mode (outbound offers + inbound offers) | Mandatory |
| BaggageAllowance | The baggage allowance for each pax/segment | Mandatory |
| TotalPrice | The total price of this offer | Mandatory |
| OfferItems | List of offer items detailed [below](#offeritem). For a round-trip offer, offer items can be divided by journey (if prices are detailed by journey) or not (when prices are for the whole round-trip). | Mandatory |

#### OfferItem
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferItemID | The offer item ID | Mandatory |
| FareDetail | Contains the PAX associations, the unit price in FarePriceType, and more information for each segment in FareComponent | Mandatory |
| Price | The total price of this offer item | Mandatory |
| Services | List of flight associations with PAX | Mandatory |
| PaymentTimeLimit | Contains the payment time limit (PaymentTimeLimitDateTime) | Optional |


# Samples

<details>
  <summary><b>AirShoppingRQ - OneWay</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
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
		<ns2:CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:FlightRequest>
			<ns2:FlightRequestOriginDestinationsCriteria>
				<ns2:OriginDestCriteria>
                    <ns2:CabinType>
                        <ns2:CabinTypeCode>ECONOMY</ns2:CabinTypeCode>
                    </ns2:CabinType>
					<ns2:DestArrivalCriteria>
						<ns2:IATA_LocationCode>PAR</ns2:IATA_LocationCode>
					</ns2:DestArrivalCriteria>
					<ns2:OriginDepCriteria>
						<ns2:Date>2026-03-12</ns2:Date>
						<ns2:IATA_LocationCode>LON</ns2:IATA_LocationCode>
					</ns2:OriginDepCriteria>
					<ns2:OriginDestID>OD1</ns2:OriginDestID>
				</ns2:OriginDestCriteria>
			</ns2:FlightRequestOriginDestinationsCriteria>
		</ns2:FlightRequest>
		<ns2:PaxList>
            <ns2:Pax>
                <ns2:PaxID>PAX1</ns2:PaxID>
                <ns2:PTC>ADT</ns2:PTC>
            </ns2:Pax>
            <ns2:Pax>
                <ns2:PaxID>PAX2</ns2:PaxID>
                <ns2:PTC>ADT</ns2:PTC>
            </ns2:Pax>
            <ns2:Pax>
                <ns2:PaxID>PAX3</ns2:PaxID>
                <ns2:Birthdate>2016-07-03</ns2:Birthdate>
                <ns2:PTC>CHD</ns2:PTC>
            </ns2:Pax>
            <ns2:Pax>
                <ns2:PaxID>PAX4</ns2:PaxID>
                <ns2:PTC>INF</ns2:PTC>
            </ns2:Pax>
		</ns2:PaxList>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_AirShoppingRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShoppingRQ - RoundTrip</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
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
		<ns2:CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:FlightRequest>
			<ns2:FlightRequestOriginDestinationsCriteria>
				<ns2:OriginDestCriteria>
                    <ns2:CabinType>
                        <ns2:CabinTypeCode>ECONOMY</ns2:CabinTypeCode>
                    </ns2:CabinType>
					<ns2:DestArrivalCriteria>
						<ns2:IATA_LocationCode>PAR</ns2:IATA_LocationCode>
					</ns2:DestArrivalCriteria>
					<ns2:OriginDepCriteria>
						<ns2:Date>2026-03-12</ns2:Date>
						<ns2:IATA_LocationCode>LON</ns2:IATA_LocationCode>
					</ns2:OriginDepCriteria>
					<ns2:OriginDestID>OD1</ns2:OriginDestID>
				</ns2:OriginDestCriteria>
				<ns2:OriginDestCriteria>
					<ns2:DestArrivalCriteria>
						<ns2:IATA_LocationCode>LON</ns2:IATA_LocationCode>
					</ns2:DestArrivalCriteria>
					<ns2:OriginDepCriteria>
						<ns2:Date>2026-03-19</ns2:Date>
						<ns2:IATA_LocationCode>PAR</ns2:IATA_LocationCode>
					</ns2:OriginDepCriteria>
					<ns2:OriginDestID>OD2</ns2:OriginDestID>
				</ns2:OriginDestCriteria>
			</ns2:FlightRequestOriginDestinationsCriteria>
		</ns2:FlightRequest>
		<ns2:PaxList>
            <ns2:Pax>
                <ns2:PaxID>PAX1</ns2:PaxID>
                <ns2:PTC>ADT</ns2:PTC>
            </ns2:Pax>
            <ns2:Pax>
                <ns2:PaxID>PAX2</ns2:PaxID>
                <ns2:PTC>ADT</ns2:PTC>
            </ns2:Pax>
            <ns2:Pax>
                <ns2:PaxID>PAX3</ns2:PaxID>
                <ns2:Birthdate>2016-07-03</ns2:Birthdate>
                <ns2:PTC>CHD</ns2:PTC>
            </ns2:Pax>
            <ns2:Pax>
                <ns2:PaxID>PAX4</ns2:PaxID>
                <ns2:PTC>INF</ns2:PTC>
            </ns2:Pax>
		</ns2:PaxList>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_AirShoppingRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShoppingRQ - Special fares (Public + InclusiveTour)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
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
		<ns2:CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:FlightRequest>
			<ns2:FlightRequestOriginDestinationsCriteria>
				<ns2:OriginDestCriteria>
                    <ns2:CabinType>
                        <ns2:CabinTypeCode>ECONOMY</ns2:CabinTypeCode>
                    </ns2:CabinType>
					<ns2:DestArrivalCriteria>
						<ns2:IATA_LocationCode>PAR</ns2:IATA_LocationCode>
					</ns2:DestArrivalCriteria>
					<ns2:OriginDepCriteria>
						<ns2:Date>2026-03-12</ns2:Date>
						<ns2:IATA_LocationCode>LON</ns2:IATA_LocationCode>
					</ns2:OriginDepCriteria>
					<ns2:OriginDestID>OD1</ns2:OriginDestID>
				</ns2:OriginDestCriteria>
			</ns2:FlightRequestOriginDestinationsCriteria>
		</ns2:FlightRequest>
		<ns2:OfferCriteria>
            <ns2:FareCriteria>
                <ns2:FareTypeCode>70J</ns2:FareTypeCode>
				<ns2:FareTypeCode>758-IT</ns2:FareTypeCode>
            </ns2:FareCriteria>
        </ns2:OfferCriteria>
		<ns2:PaxList>
            <ns2:Pax>
                <ns2:PaxID>PAX1</ns2:PaxID>
                <ns2:PTC>ADT</ns2:PTC>
            </ns2:Pax>
            <ns2:Pax>
                <ns2:PaxID>PAX2</ns2:PaxID>
                <ns2:PTC>ADT</ns2:PTC>
            </ns2:Pax>
            <ns2:Pax>
                <ns2:PaxID>PAX3</ns2:PaxID>
                <ns2:Birthdate>2016-07-03</ns2:Birthdate>
                <ns2:PTC>CHD</ns2:PTC>
            </ns2:Pax>
            <ns2:Pax>
                <ns2:PaxID>PAX4</ns2:PaxID>
                <ns2:PTC>INF</ns2:PTC>
            </ns2:Pax>
		</ns2:PaxList>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_AirShoppingRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShoppingRQ - LoyaltyProgram</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
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
		<ns2:CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:FlightRequest>
			<ns2:FlightRequestOriginDestinationsCriteria>
				<ns2:OriginDestCriteria>
                    <ns2:CabinType>
                        <ns2:CabinTypeCode>ECONOMY</ns2:CabinTypeCode>
                    </ns2:CabinType>
					<ns2:DestArrivalCriteria>
						<ns2:IATA_LocationCode>PAR</ns2:IATA_LocationCode>
					</ns2:DestArrivalCriteria>
					<ns2:OriginDepCriteria>
						<ns2:Date>2026-03-12</ns2:Date>
						<ns2:IATA_LocationCode>LON</ns2:IATA_LocationCode>
					</ns2:OriginDepCriteria>
					<ns2:OriginDestID>OD1</ns2:OriginDestID>
				</ns2:OriginDestCriteria>
			</ns2:FlightRequestOriginDestinationsCriteria>
		</ns2:FlightRequest>
		<ns2:PaxList>
            <ns2:Pax>
                <ns2:PaxID>PAX1</ns2:PaxID>
                <ns2:PTC>ADT</ns2:PTC>
                <ns2:LoyaltyProgramAccount>
                    <ns2:AccountNumber>2083488723</ns2:AccountNumber>
                    <ns2:LoyaltyProgram>
                        <ns2:ProgramCode>FB</ns2:ProgramCode>
                    </ns2:LoyaltyProgram>
                </ns2:LoyaltyProgramAccount>
            </ns2:Pax>
		</ns2:PaxList>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_AirShoppingRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShoppingRQ - Search limit</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
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
		<ns2:CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:FlightRequest>
			<ns2:FlightRequestOriginDestinationsCriteria>
				<ns2:OriginDestCriteria>
                    <ns2:CabinType>
                        <ns2:CabinTypeCode>ECONOMY</ns2:CabinTypeCode>
                    </ns2:CabinType>
					<ns2:DestArrivalCriteria>
						<ns2:IATA_LocationCode>PAR</ns2:IATA_LocationCode>
					</ns2:DestArrivalCriteria>
					<ns2:OriginDepCriteria>
						<ns2:Date>2026-03-12</ns2:Date>
						<ns2:IATA_LocationCode>LON</ns2:IATA_LocationCode>
					</ns2:OriginDepCriteria>
					<ns2:OriginDestID>OD1</ns2:OriginDestID>
				</ns2:OriginDestCriteria>
			</ns2:FlightRequestOriginDestinationsCriteria>
		</ns2:FlightRequest>
		<ns2:OfferCriteria>
            <ns2:SpecialNeedsCriteria>
                <ns2:FreeText>offerLimit</ns2:FreeText>
                <ns2:SpecialServiceCode>CHEAPEST|FIRST|SLICE</ns2:SpecialServiceCode>
                <ns2:Qty>200</Qty>
           </ns2:SpecialNeedsCriteria>
        </ns2:OfferCriteria>
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
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_AirShoppingRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShoppingRQ - Search specific carriers</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
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
		<ns2:CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:FlightRequest>
			<ns2:FlightRequestOriginDestinationsCriteria>
				<ns2:OriginDestCriteria>
                    <ns2:CabinType>
                        <ns2:CabinTypeCode>ECONOMY</ns2:CabinTypeCode>
                    </ns2:CabinType>
					<ns2:DestArrivalCriteria>
						<ns2:IATA_LocationCode>PAR</ns2:IATA_LocationCode>
					</ns2:DestArrivalCriteria>
					<ns2:OriginDepCriteria>
						<ns2:Date>2026-03-12</ns2:Date>
						<ns2:IATA_LocationCode>LON</ns2:IATA_LocationCode>
					</ns2:OriginDepCriteria>
					<ns2:OriginDestID>OD1</ns2:OriginDestID>
				</ns2:OriginDestCriteria>
			</ns2:FlightRequestOriginDestinationsCriteria>
		</ns2:FlightRequest>
		<ns2:FlightRelatedCriteria>
		    <ns2:CarrierCriteria>
			    <ns2:Carrier>
                    <ns2:AirlineDesigCode>VY</ns2:AirlineDesigCode>
                </ns2:Carrier>
			</ns2:CarrierCriteria>
			<ns2:CarrierCriteria>
			    <ns2:Carrier>
                    <ns2:AirlineDesigCode>KL</ns2:AirlineDesigCode>
                </ns2:Carrier>
			</ns2:CarrierCriteria>
		</ns2:FlightRelatedCriteria>
		<ns2:OfferCriteria>
            <ns2:SpecialNeedsCriteria>
                <ns2:FreeText>offerLimit</ns2:FreeText>
                <ns2:SpecialServiceCode>CHEAPEST|FIRST|SLICE</ns2:SpecialServiceCode>
                <ns2:Qty>200</Qty>
           </ns2:SpecialNeedsCriteria>
        </ns2:OfferCriteria>
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
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_AirShoppingRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShoppingRQ - Search Spanish resident discounted fares</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
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
		<ns2:CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:FlightRequest>
			<ns2:FlightRequestOriginDestinationsCriteria>
				<ns2:OriginDestCriteria>
                    <ns2:CabinType>
                        <ns2:CabinTypeCode>ECONOMY</ns2:CabinTypeCode>
                    </ns2:CabinType>
					<ns2:DestArrivalCriteria>
						<ns2:IATA_LocationCode>MAD</ns2:IATA_LocationCode>
					</ns2:DestArrivalCriteria>
					<ns2:OriginDepCriteria>
						<ns2:Date>2026-03-12</ns2:Date>
						<ns2:IATA_LocationCode>TCI</ns2:IATA_LocationCode>
					</ns2:OriginDepCriteria>
					<ns2:OriginDestID>OD1</ns2:OriginDestID>
				</ns2:OriginDestCriteria>
				<ns2:OriginDestCriteria>
					<ns2:DestArrivalCriteria>
						<ns2:IATA_LocationCode>TCI</ns2:IATA_LocationCode>
					</ns2:DestArrivalCriteria>
					<ns2:OriginDepCriteria>
						<ns2:Date>2026-03-19</ns2:Date>
						<ns2:IATA_LocationCode>BCN</ns2:IATA_LocationCode>
					</ns2:OriginDepCriteria>
					<ns2:OriginDestID>OD2</ns2:OriginDestID>
				</ns2:OriginDestCriteria>
			</ns2:FlightRequestOriginDestinationsCriteria>
		</ns2:FlightRequest>
		<ns2:OfferCriteria>
            <ns2:FareCriteria>
                <ns2:FareTypeCode>SPANISH_RESIDENT</ns2:FareTypeCode>
            </ns2:FareCriteria>
        </ns2:OfferCriteria>
		<ns2:PaxList>
            <ns2:Pax>
                <ns2:PaxID>PAX1</ns2:PaxID>
                <ns2:PTC>ADT</ns2:PTC>
            </ns2:Pax>
            <ns2:Pax>
                <ns2:PaxID>PAX3</ns2:PaxID>
                <ns2:AgeMeasure>8</ns2:AgeMeasure>
                <ns2:PTC>CHD</ns2:PTC>
            </ns2:Pax>
		</ns2:PaxList>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_AirShoppingRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShopping RS - Error</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns2:IATA_AirShoppingRS xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
    <ns2:Error>
        <Code>102</Code>
        <DescText>Invalid/Missing Departure Date</DescText>
        <LangCode>en</LangCode>
        <OwnerName>ORCHESTRA</OwnerName>
        <StatusText>Past date</StatusText>
    </ns2:Error>
    <ns2:PayloadAttributes>
        <CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</CorrelationID>
        <Timestamp>2026-01-28T16:40:31.889+01:00</Timestamp>
        <VersionNumber>21.3</VersionNumber>
    </ns2:PayloadAttributes>
</ns2:IATA_AirShoppingRS>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShopping RS - OneWay</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns2:IATA_AirShoppingRS xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
    <ns2:Response>
        <DataLists>
            <BaggageAllowanceList>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA1</BaggageAllowanceID>
                    <PieceAllowance>
                        <TotalQty>0</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA2</BaggageAllowanceID>
                    <PieceAllowance>
                        <TotalQty>1</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA3</BaggageAllowanceID>
                    <PieceAllowance>
                        <TotalQty>1</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
            </BaggageAllowanceList>
            <DatedMarketingSegmentList>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T10:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS1</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS1</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T08:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>304</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T22:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS2</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS2</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T20:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>322</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T13:50:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS3</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS3</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T11:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>308</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T15:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS4</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS4</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T12:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>310</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T17:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS5</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS5</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T15:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>314</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T19:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS6</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS6</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T17:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>318</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T17:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ORY</IATA_LocationCode>
                        <TerminalName>3</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS7</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS7</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T15:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>8130</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
            </DatedMarketingSegmentList>
            <DatedOperatingLegList>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL1</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>319</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL2</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL3</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>321</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL4</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>32N</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL5</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL6</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL7</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
            </DatedOperatingLegList>
            <DatedOperatingSegmentList>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL1</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS1</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL2</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS2</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL3</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS3</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL4</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS4</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL5</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS5</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL6</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS6</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>VY</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL7</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS7</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                </DatedOperatingSegment>
            </DatedOperatingSegmentList>
            <OriginDestList>
                <OriginDest>
                    <DestCode>CDG</DestCode>
                    <OriginCode>LHR</OriginCode>
                    <OriginDestID>OD1</OriginDestID>
                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>ORY</DestCode>
                    <OriginCode>LHR</OriginCode>
                    <OriginDestID>OD2</OriginDestID>
                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                    <PaxJourneyID>PJ1</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                    <PaxJourneyID>PJ2</PaxJourneyID>
                    <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ3</PaxJourneyID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ4</PaxJourneyID>
                    <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ5</PaxJourneyID>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ6</PaxJourneyID>
                    <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ7</PaxJourneyID>
                    <PaxSegmentRefID>SEG7</PaxSegmentRefID>
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
                <Pax>
                    <Birthdate>2016-07-03</Birthdate>
                    <PaxID>PAX3</PaxID>
                    <PTC>CHD</PTC>
                </Pax>
                <Pax>
                    <PaxID>PAX4</PaxID>
                    <PTC>INF</PTC>
                </Pax>
            </PaxList>
            <PaxSegmentList>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS1</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG1</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS2</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG2</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS3</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG3</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS4</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG4</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS5</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG5</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS6</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG6</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS7</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG7</PaxSegmentID>
                </PaxSegment>
            </PaxSegmentList>
            <PriceClassList>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>CHA - CHANGE BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - CHANGE AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - REFUND BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - REFUND AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - SAME DAY FLT CHNG P2P ONLY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - SEAT CHOICE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - 1ST BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - 2ND BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - LOUNGE ACCESS</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SNACK</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - PRIORITY SECURITY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - DEDICATED CHECK IN ZONE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - CABIN BAG UPTO 56 X 45 X 25CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - HANDBAG UPTO 40 X 30 X 15CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Time/date changes permitted at any time before each flight departure for a change fee of 70.0EUR or an upgrade fee of 70.0EUR plus any difference in fare. All sectors may be repriced. Changes subject to availability. Fees apply per ticket</DescText>
                    </Desc>
                    <Desc>
                        <DescText>There are no refunds except for any Government &amp; airport taxes</DescText>
                    </Desc>
                    <Name>BASIC</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>CHA - CHANGE BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - CHANGE AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - REFUND BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - REFUND AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - SEAT CHOICE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - 2ND BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - LOUNGE ACCESS</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SNACK</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - PRIORITY SECURITY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - DEDICATED CHECK IN ZONE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - CABIN BAG UPTO 56 X 45 X 25CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - HANDBAG UPTO 40 X 30 X 15CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SAME DAY FLT CHNG P2P ONLY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - 1ST BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>There are no refunds except for any Government &amp; airport taxes</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Time/date changes permitted at any time before each flight departure for a change fee of 58.0EUR or an upgrade fee of 58.0EUR plus any difference in fare. All sectors may be repriced. Changes subject to availability. Fees apply per ticket</DescText>
                    </Desc>
                    <Name>PLUS</Name>
                    <PriceClassID>PC2</PriceClassID>
                </PriceClass>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>INC - CHANGE BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - CHANGE AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - REFUND BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - REFUND AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SAME DAY FLT CHNG P2P ONLY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SEAT CHOICE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - 1ST BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - 2ND BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - LOUNGE ACCESS</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SNACK</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - PRIORITY SECURITY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - DEDICATED CHECK IN ZONE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - CABIN BAG UPTO 56 X 45 X 25CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - HANDBAG UPTO 40 X 30 X 15CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Time/date changes permitted at any time for the difference in fare. Changes subject to availability</DescText>
                    </Desc>
                    <Desc>
                        <DescText>If you cancel a refund is permitted, subject to recalculation of the fare for any journey flown. There are no cancellation fees.</DescText>
                    </Desc>
                    <Name>PLUS FLEX</Name>
                    <PriceClassID>PC3</PriceClassID>
                </PriceClass>
            </PriceClassList>
        </DataLists>
        <OffersGroup>
            <CarrierOffers>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>acec2703-5da7-48e1-9b22-6197f8ea610a</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">42.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">100.19</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>75daf9f5-de07-480d-84fb-c5a445544c8d</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">85.00</BaseAmount>
                            <TotalAmount CurCode="EUR">200.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV1</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">42.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">81.46</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>ef0a04e9-e26b-41ee-a615-1ed131092472</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">42.50</BaseAmount>
                            <TotalAmount CurCode="EUR">81.46</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV2</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">5.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">43.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>2834edb4-71d1-419f-bc99-65d7504a2999</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">5.00</BaseAmount>
                            <TotalAmount CurCode="EUR">43.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV3</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">132.50</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">325.80</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>812edcd3-3d50-4005-acb9-aeebd1eee4a7</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">42.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">100.19</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>fcd1c9c6-6538-42a3-b227-d24ff1b2aac4</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">85.00</BaseAmount>
                            <TotalAmount CurCode="EUR">200.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV4</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">42.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">81.46</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>28050bcf-9a98-4558-8396-75568d8e886a</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">42.50</BaseAmount>
                            <TotalAmount CurCode="EUR">81.46</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV5</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">5.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">43.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>748e1864-7297-46ee-b814-db6b7d429b38</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">5.00</BaseAmount>
                            <TotalAmount CurCode="EUR">43.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV6</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">132.50</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">325.80</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>716d0a1e-617a-4578-a4d2-a1c930b7c71e</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">45.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">102.69</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>71731252-65c3-4133-9297-004220f760c5</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">90.00</BaseAmount>
                            <TotalAmount CurCode="EUR">205.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV7</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">45.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">83.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>696c1015-2ed9-40cd-9646-b9d7538bc8ae</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">45.00</BaseAmount>
                            <TotalAmount CurCode="EUR">83.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV8</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">5.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">43.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>37dcc242-416d-4619-8047-d856b0532fd0</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">5.00</BaseAmount>
                            <TotalAmount CurCode="EUR">43.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV9</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">140.00</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">333.30</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>35f99909-b501-4878-8ad3-6fbaeb59f6c3</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">45.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">102.69</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>6e733319-f92f-4bd5-a980-c0208b536cb5</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">90.00</BaseAmount>
                            <TotalAmount CurCode="EUR">205.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV10</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">45.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">83.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>4b7bf301-5078-4944-87e5-2fa87f752bb4</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">45.00</BaseAmount>
                            <TotalAmount CurCode="EUR">83.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV11</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">5.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">43.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>d6f02e6a-77f1-4b0b-bc36-70d384287819</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">5.00</BaseAmount>
                            <TotalAmount CurCode="EUR">43.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV12</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">140.00</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">333.30</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>e9bf657c-8fd9-4aa2-9df5-b80d0a77bd36</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">45.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">102.69</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>fd5d4cd2-97a7-435e-89cc-78c404bc4348</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">90.00</BaseAmount>
                            <TotalAmount CurCode="EUR">205.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV13</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">45.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">83.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>011b3d83-ae6d-434d-bffb-47474e383a6c</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">45.00</BaseAmount>
                            <TotalAmount CurCode="EUR">83.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV14</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">5.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">43.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>60e1482b-4b2e-4eb9-8aea-ba6d13515165</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">5.00</BaseAmount>
                            <TotalAmount CurCode="EUR">43.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV15</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">140.00</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">333.30</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>002c5134-26c5-48e3-8675-9c38f747e366</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">45.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">102.69</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>f2e27435-f28b-4ccd-974a-4055877e22f1</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">90.00</BaseAmount>
                            <TotalAmount CurCode="EUR">205.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV16</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">45.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">83.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>6a2d5cd3-bb8a-4571-9fe5-e3ac276cb5e6</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">45.00</BaseAmount>
                            <TotalAmount CurCode="EUR">83.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV17</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">5.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">43.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>4e4814ea-243a-477b-b26a-1fcc09229589</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">5.00</BaseAmount>
                            <TotalAmount CurCode="EUR">43.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV18</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">140.00</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">333.30</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>f84df174-6c09-4f22-91ab-8b8cb61af243</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">62.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">120.19</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>93329890-be15-4816-9af5-eddb4be8ee82</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">125.00</BaseAmount>
                            <TotalAmount CurCode="EUR">240.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV19</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">62.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">101.46</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>fd8d61cc-4262-48fc-b9a4-b0fb3128df15</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">62.50</BaseAmount>
                            <TotalAmount CurCode="EUR">101.46</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV20</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">13.75</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">52.71</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>2f78be77-4526-4aaf-89bf-8b9c32360780</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">13.75</BaseAmount>
                            <TotalAmount CurCode="EUR">52.71</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV21</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">201.25</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">394.55</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            <PriceClassRefID>PC3</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>50911df2-1cd9-4d55-a378-011dc4f8b406</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">647.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">705.19</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>2dfe2500-b128-42c0-8c52-bc0780032969</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">1295.00</BaseAmount>
                            <TotalAmount CurCode="EUR">1410.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV22</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">647.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">686.46</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>283f7ad3-2c51-4ea1-8ed0-d83e8c060d62</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">647.50</BaseAmount>
                            <TotalAmount CurCode="EUR">686.46</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV23</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">130.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">168.96</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>d3765c8b-d25b-4ab3-b833-2c6eed6cf953</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">130.00</BaseAmount>
                            <TotalAmount CurCode="EUR">168.96</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV24</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">2072.50</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">2265.80</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
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
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC3</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>777d9ae5-33df-4ae3-8fff-598229f67cb5</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">842.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">900.19</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>6f73bf5d-53e5-4ff0-acf9-784ed97b0793</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">1685.00</BaseAmount>
                            <TotalAmount CurCode="EUR">1800.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV25</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">842.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">881.46</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>70ec427c-e3b9-4a7d-b00f-2bbc9c1147f5</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">842.50</BaseAmount>
                            <TotalAmount CurCode="EUR">881.46</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV26</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">83.75</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">122.71</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>e44b51fa-8797-4bb8-bcac-4d48a26e5a77</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">83.75</BaseAmount>
                            <TotalAmount CurCode="EUR">122.71</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV27</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">2611.25</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">2804.55</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA3</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC3</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>7905f9e6-245c-45b4-9f93-e83d9c325726</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">842.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">900.19</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>082aad8d-df62-469b-932b-706fb075c317</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">1685.00</BaseAmount>
                            <TotalAmount CurCode="EUR">1800.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV28</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">842.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">881.46</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>cd919050-8847-401e-9639-3a8df15b23e7</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">842.50</BaseAmount>
                            <TotalAmount CurCode="EUR">881.46</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV29</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">83.75</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">122.71</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>cb4e1f2f-b4c9-4f62-9b45-31d03127ea24</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">83.75</BaseAmount>
                            <TotalAmount CurCode="EUR">122.71</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV30</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">2611.25</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">2804.55</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA3</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            <PriceClassRefID>PC3</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:11:34.304</OfferExpirationTimeLimitDateTime>
                    <OfferID>637aec4b-e4f9-4dba-a831-7b36399300a2</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">842.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">57.69</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">900.19</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>1314d62f-18ac-4a52-af42-5a131b8fe0d2</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">1685.00</BaseAmount>
                            <TotalAmount CurCode="EUR">1800.38</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV31</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">842.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">881.46</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>5ee68967-92ee-48f1-af0b-e0af725c2022</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">842.50</BaseAmount>
                            <TotalAmount CurCode="EUR">881.46</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV32</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">83.75</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">122.71</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>20132c71-4c92-497f-82f8-5a11364e2d5e</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">83.75</BaseAmount>
                            <TotalAmount CurCode="EUR">122.71</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV33</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">2611.25</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">2804.55</TotalAmount>
                    </TotalPrice>
                </Offer>
				<Offer>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">842.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">881.46</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>2e0fe4a5-9963-402b-91d2-389e7c519a75</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">842.50</BaseAmount>
                            <TotalAmount CurCode="EUR">881.46</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV41</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">83.75</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">38.96</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">122.71</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>aa4b5559-884d-4a63-bbe1-8b94e87e5f34</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">83.75</BaseAmount>
                            <TotalAmount CurCode="EUR">122.71</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV42</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">2611.25</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">193.30</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">2804.55</TotalAmount>
                    </TotalPrice>
                </Offer>
            </CarrierOffers>
        </OffersGroup>
        <Warning>
            <Code>SUPPLIER_WARN</Code>
            <DescText>All services may not be delivered as the requested fare component may include a codeshare flight or an interline itinerary.</DescText>
            <OwnerName>BRITISH_AIRWAYS</OwnerName>
        </Warning>
    </ns2:Response>
    <ns2:PayloadAttributes>
        <CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</CorrelationID>
        <Timestamp>2026-01-28T16:41:34.616+01:00</Timestamp>
        <VersionNumber>21.3</VersionNumber>
    </ns2:PayloadAttributes>
</ns2:IATA_AirShoppingRS>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShopping RS - RoundTrip (mode combination)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns2:IATA_AirShoppingRS xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
    <ns2:Response>
        <DataLists>
            <BaggageAllowanceList>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA1</BaggageAllowanceID>
                    <PieceAllowance>
                        <TotalQty>0</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA2</BaggageAllowanceID>
                    <PieceAllowance>
                        <TotalQty>1</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA3</BaggageAllowanceID>
                    <PieceAllowance>
                        <TotalQty>1</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
            </BaggageAllowanceList>
            <DatedMarketingSegmentList>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T10:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS1</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS1</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T08:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>304</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-19T18:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS2</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS2</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-19T18:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>315</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T22:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS3</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS3</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T20:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>322</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-19T15:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS4</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS4</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-19T14:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>309</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-19T16:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS5</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS5</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-19T16:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>311</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-19T21:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS6</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS6</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-19T20:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>319</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-19T10:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS7</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS7</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-19T10:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>303</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-19T12:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS8</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS8</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-19T11:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>305</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T13:50:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS9</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS9</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T11:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>308</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T15:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS10</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS10</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T12:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>310</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T17:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS11</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS11</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T15:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>314</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T19:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS12</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS12</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T17:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>318</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-19T07:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>5</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS13</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS13</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-19T07:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2C</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>323</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T17:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ORY</IATA_LocationCode>
                        <TerminalName>3</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS14</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS14</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T15:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>8130</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-19T14:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>LHR</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS15</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS15</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-19T14:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ORY</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>8131</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
            </DatedMarketingSegmentList>
            <DatedOperatingLegList>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL1</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL2</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>319</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL3</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>32N</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL4</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>321</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL5</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL6</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL7</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL8</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL9</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>321</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL10</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>32N</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL11</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL12</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>319</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL13</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL14</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
                <DatedOperatingLeg>
                    <Arrival/>
                    <CarrierAircraftType>
                        <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                    </CarrierAircraftType>
                    <DatedOperatingLegID>DOL15</DatedOperatingLegID>
                    <Dep/>
                </DatedOperatingLeg>
            </DatedOperatingLegList>
            <DatedOperatingSegmentList>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL1</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS1</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL2</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS2</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H15M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL3</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS3</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL4</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS4</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL5</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS5</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL6</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS6</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL7</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS7</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL8</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS8</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL9</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS9</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL10</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS10</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL11</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS11</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL12</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS12</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>BA</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL13</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS13</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>VY</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL14</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS14</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>VY</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL15</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS15</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                </DatedOperatingSegment>
            </DatedOperatingSegmentList>
            <OriginDestList>
                <OriginDest>
                    <DestCode>CDG</DestCode>
                    <OriginCode>LHR</OriginCode>
                    <OriginDestID>OD1</OriginDestID>
                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ11</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ12</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>LHR</DestCode>
                    <OriginCode>CDG</OriginCode>
                    <OriginDestID>OD2</OriginDestID>
                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ13</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>ORY</DestCode>
                    <OriginCode>LHR</OriginCode>
                    <OriginDestID>OD3</OriginDestID>
                    <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>LHR</DestCode>
                    <OriginCode>ORY</OriginCode>
                    <OriginDestID>OD4</OriginDestID>
                    <PaxJourneyRefID>PJ15</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                    <PaxJourneyID>PJ1</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H15M0S</Duration>
                    <PaxJourneyID>PJ2</PaxJourneyID>
                    <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                    <PaxJourneyID>PJ3</PaxJourneyID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                    <PaxJourneyID>PJ4</PaxJourneyID>
                    <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                    <PaxJourneyID>PJ5</PaxJourneyID>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H20M0S</Duration>
                    <PaxJourneyID>PJ6</PaxJourneyID>
                    <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ7</PaxJourneyID>
                    <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ8</PaxJourneyID>
                    <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ9</PaxJourneyID>
                    <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ10</PaxJourneyID>
                    <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ11</PaxJourneyID>
                    <PaxSegmentRefID>SEG11</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ12</PaxJourneyID>
                    <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ13</PaxJourneyID>
                    <PaxSegmentRefID>SEG13</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ14</PaxJourneyID>
                    <PaxSegmentRefID>SEG14</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ15</PaxJourneyID>
                    <PaxSegmentRefID>SEG15</PaxSegmentRefID>
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
                <Pax>
                    <Birthdate>2016-07-03</Birthdate>
                    <PaxID>PAX3</PaxID>
                    <PTC>CHD</PTC>
                </Pax>
                <Pax>
                    <PaxID>PAX4</PaxID>
                    <PTC>INF</PTC>
                </Pax>
            </PaxList>
            <PaxSegmentList>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS1</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG1</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS2</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG2</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS3</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG3</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS4</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG4</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS5</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG5</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS6</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG6</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS7</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG7</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS8</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG8</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS9</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG9</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS10</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG10</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS11</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG11</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS12</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG12</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS13</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG13</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS14</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG14</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <DatedMarketingSegmentRefId>DMS15</DatedMarketingSegmentRefId>
                    <PaxSegmentID>SEG15</PaxSegmentID>
                </PaxSegment>
            </PaxSegmentList>
            <PriceClassList>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>CHA - CHANGE BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - CHANGE AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - REFUND BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - REFUND AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - SAME DAY FLT CHNG P2P ONLY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - SEAT CHOICE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - 1ST BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - 2ND BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - LOUNGE ACCESS</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SNACK</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - PRIORITY SECURITY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - DEDICATED CHECK IN ZONE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - CABIN BAG UPTO 56 X 45 X 25CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - HANDBAG UPTO 40 X 30 X 15CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Time/date changes permitted at any time before each flight departure for a change fee of 70.0EUR or an upgrade fee of 70.0EUR plus any difference in fare. All sectors may be repriced. Changes subject to availability. Fees apply per ticket</DescText>
                    </Desc>
                    <Desc>
                        <DescText>There are no refunds except for any Government &amp; airport taxes</DescText>
                    </Desc>
                    <Name>BASIC</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>CHA - CHANGE BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - CHANGE AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - REFUND BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - REFUND AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - SEAT CHOICE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - 2ND BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - LOUNGE ACCESS</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SNACK</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - PRIORITY SECURITY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - DEDICATED CHECK IN ZONE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - CABIN BAG UPTO 56 X 45 X 25CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - HANDBAG UPTO 40 X 30 X 15CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SAME DAY FLT CHNG P2P ONLY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - 1ST BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>There are no refunds except for any Government &amp; airport taxes</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Time/date changes permitted at any time before each flight departure for a change fee of 58.0EUR or an upgrade fee of 58.0EUR plus any difference in fare. All sectors may be repriced. Changes subject to availability. Fees apply per ticket</DescText>
                    </Desc>
                    <Name>PLUS</Name>
                    <PriceClassID>PC2</PriceClassID>
                </PriceClass>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>INC - CHANGE BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - CHANGE AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - REFUND BEFORE DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - REFUND AFTER DEPARTURE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SAME DAY FLT CHNG P2P ONLY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SEAT CHOICE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - 1ST BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>CHA - 2ND BAG MAX 23KG 51LB 208LCM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - LOUNGE ACCESS</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - SNACK</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - PRIORITY SECURITY</DescText>
                    </Desc>
                    <Desc>
                        <DescText>NOF - DEDICATED CHECK IN ZONE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - CABIN BAG UPTO 56 X 45 X 25CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>INC - HANDBAG UPTO 40 X 30 X 15CM</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Time/date changes permitted at any time for the difference in fare. Changes subject to availability</DescText>
                    </Desc>
                    <Desc>
                        <DescText>If you cancel a refund is permitted, subject to recalculation of the fare for any journey flown. There are no cancellation fees.</DescText>
                    </Desc>
                    <Name>PLUS FLEX</Name>
                    <PriceClassID>PC3</PriceClassID>
                </PriceClass>
            </PriceClassList>
        </DataLists>
        <OffersGroup>
            <CarrierOffers>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:13:14.459</OfferExpirationTimeLimitDateTime>
                    <OfferID>ccec58d8-3da4-4413-82c6-039ddf5188ee</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Q</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">41.25</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">113.55</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">154.80</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>a7b1f39f-2ce8-43de-93fd-b64aab955f65</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">82.50</BaseAmount>
                            <TotalAmount CurCode="EUR">309.60</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV1</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Q</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">41.25</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">94.83</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">136.08</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>0efd7ac8-198f-413d-b645-c1ba41b41502</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">41.25</BaseAmount>
                            <TotalAmount CurCode="EUR">136.08</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV2</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Q</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">41.25</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">94.83</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">136.08</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX3</PaxRefID>
                        </FareDetail>
                        <OfferItemID>45ec7611-f66d-40c0-ac96-09b1af5c3b5d</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">41.25</BaseAmount>
                            <TotalAmount CurCode="EUR">136.08</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX3</PaxRefID>
                            <ServiceID>SV17</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Q</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">5.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">44.01</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">49.01</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>d569898d-e016-45bd-98e7-f4f44bb2ec2e</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">5.00</BaseAmount>
                            <TotalAmount CurCode="EUR">49.01</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV18</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">128.75</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">365.94</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">494.69</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAssociations>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <OfferFlightAssociations>
                            <PaxSegmentReferences>
                                <PaxSegmentRefID>SEG14</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG15</PaxSegmentRefID>
                            </PaxSegmentReferences>
                            <PaxJourneyRef>
                                <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                            </PaxJourneyRef>
                        </OfferFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                        <PaxRefID>PAX3</PaxRefID>
                        <PaxRefID>PAX4</PaxRefID>
                    </BaggageAssociations>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ15</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferExpirationTimeLimitDateTime>2026-01-28T17:13:14.459</OfferExpirationTimeLimitDateTime>
                    <OfferID>930db3a9-b087-4538-8689-e36a7c04a3b7</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>O</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG14</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG15</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">122.50</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">111.83</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">234.33</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>9818951e-30b7-4c3c-96bc-febdfffe4612</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">245.00</BaseAmount>
                            <TotalAmount CurCode="EUR">468.66</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ15</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceID>SV127</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>Y</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareTypeCode>70J</FareTypeCode>
                                <PaxSegmentRefID>SEG14</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG15</PaxSegmentRefID>
                                <PriceClassRefID>PC3</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">271.25</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">44.01</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">315.26</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX4</PaxRefID>
                        </FareDetail>
                        <OfferItemID>fb344d6d-36a7-43dc-b2f8-5625e13e43e0</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDate>
                                <PaymentTimeLimitDateTime>2026-02-01T00:59:59.000</PaymentTimeLimitDateTime>
                            </PaymentTimeLimitDate>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">271.25</BaseAmount>
                            <TotalAmount CurCode="EUR">315.26</TotalAmount>
                        </Price>
                        <Service>
                            <OfferServiceAssociation>
                                <PaxJourneyRef>
                                    <PaxJourneyRefID>PJ15</PaxJourneyRefID>
                                </PaxJourneyRef>
                            </OfferServiceAssociation>
                            <PaxRefID>PAX4</PaxRefID>
                            <ServiceID>SV258</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>BA</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">4328.75</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">360.77</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">4689.52</TotalAmount>
                    </TotalPrice>
                </Offer>
            </CarrierOffers>
        </OffersGroup>
        <Warning>
            <Code>SUPPLIER_WARN</Code>
            <DescText>All services may not be delivered as the requested fare component may include a codeshare flight or an interline itinerary.</DescText>
            <OwnerName>BRITISH_AIRWAYS</OwnerName>
        </Warning>
    </ns2:Response>
    <ns2:PayloadAttributes>
        <CorrelationID>e0511f74-7d4b-3763-b80c-08b1ec1803d2</CorrelationID>
        <Timestamp>2026-01-28T16:43:15.358+01:00</Timestamp>
        <VersionNumber>21.3</VersionNumber>
    </ns2:PayloadAttributes>
</ns2:IATA_AirShoppingRS>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShopping RS - RoundTrip (mode flat)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRS">
    <Response>
        <DataLists>
            <BaggageAllowanceList>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA1</BaggageAllowanceID>
                    <PieceAllowance>
                        <ApplicablePartyText>Traveler</ApplicablePartyText>
                        <TotalQty>1</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
            </BaggageAllowanceList>
            <OriginDestList>
                <OriginDest>
                    <DestCode>ADA</DestCode>
                    <OriginCode>SAW</OriginCode>
                    <OriginDestID>OD1</OriginDestID>
                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>SAW</DestCode>
                    <OriginCode>ADA</OriginCode>
                    <OriginDestID>OD2</OriginDestID>
                    <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ11</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ12</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ13</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ1</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ2</PaxJourneyID>
                    <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ3</PaxJourneyID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ4</PaxJourneyID>
                    <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ5</PaxJourneyID>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ6</PaxJourneyID>
                    <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ7</PaxJourneyID>
                    <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ8</PaxJourneyID>
                    <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ9</PaxJourneyID>
                    <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ10</PaxJourneyID>
                    <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ11</PaxJourneyID>
                    <PaxSegmentRefID>SEG11</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ12</PaxJourneyID>
                    <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ13</PaxJourneyID>
                    <PaxSegmentRefID>SEG13</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ14</PaxJourneyID>
                    <PaxSegmentRefID>SEG14</PaxSegmentRefID>
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
                        <AircraftScheduledDateTime>2020-10-06T01:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T00:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2090</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG1</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T07:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T06:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2080</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG2</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T10:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T08:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2082</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG3</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T12:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T11:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2086</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG4</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T18:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T16:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2092</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG5</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T21:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T20:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2096</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG6</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-07T00:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T23:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2098</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG7</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T07:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T05:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2099</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG8</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T09:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T08:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2081</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG9</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T12:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T11:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2083</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG10</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T14:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T13:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2087</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG11</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T18:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T16:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2089</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG12</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T20:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T18:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2093</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG13</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-13T00:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T22:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2097</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG14</PaxSegmentID>
                </PaxSegment>
            </PaxSegmentList>
            <PriceClassList>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Name>ECONOMY</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
            </PriceClassList>
        </DataLists>
        <OffersGroup>
            <CarrierOffers>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>1d6f03a6-3568-487d-a603-8a26fc69ddf2</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>2bff1296-3e3b-4e1a-bf07-25d3bf755c58</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV1</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>eedef893-2755-444d-8306-64508ebc0cf6</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>34e18eb0-4c17-491d-ac67-4d10e1583ab6</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV2</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>edd88984-9beb-4912-a91c-096efc8ce241</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>7709256e-c894-49d0-818b-73b7ac3c3d1a</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV3</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>a0a443bb-16ea-4e03-8e83-a0e687e503a1</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>b2743ed4-9e87-4ceb-ac80-aed1ae88fd56</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV4</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>ab0a3725-2e9e-4ae4-9fcf-5ff4dd786eb1</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>0754ec29-14dd-421f-a141-b3f45d0d6365</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV5</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>75c6f2fb-69f1-4b37-a22c-e71de9b22a00</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>8fb5a37c-5912-4353-b60d-63c35cf601c1</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV6</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>f6c6df39-126d-441a-b7ea-0f581e6a8acd</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>2a28dbfc-1d5b-4d24-9ea2-af0a5ca0535d</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV7</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>8203e42a-fbab-4a42-9b69-78adb1d67e86</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>e232a1c6-529e-43f8-863b-b8c7a018995f</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV8</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>8feb63a6-4072-4c77-9023-d84bdfeda6e6</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>d7c9e00e-aa95-4dfb-bee5-fb75087a0cbe</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV9</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>de99749d-bd46-4d4f-918e-21f76c6f98e2</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>99fca37d-4fd6-4672-81fd-8421d54f0395</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV10</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG11</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ11</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>5dfac5fb-a4e9-4710-a21c-b0e72e02ea14</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG11</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>af0db36f-4bf8-4322-9912-251e82f0046e</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ11</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV11</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ12</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>0c301349-93a5-4521-a618-d074c75f2420</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>85785f18-a3a1-43c6-8753-621acf971d48</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ12</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV12</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG13</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ13</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>c5f3730e-5c0a-4806-bfd5-4687773fdf3a</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG13</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>548530e2-8e97-407a-b070-09e376bc9b78</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ13</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV13</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG14</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Partial</MatchTypeCode>
                    <OfferID>0471c8e3-1593-40a9-8d54-d3a9059de19e</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG14</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>8c567488-3697-4293-b36a-a028f1a6b688</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV14</ServiceID>
                        </Service>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDateTime>2025-05-17T23:59:00.000</PaymentTimeLimitDateTime>
                        </PaymentTimeLimit>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
            </CarrierOffers>
        </OffersGroup>
        <ShoppingResponse>
            <ShoppingResponseRefID>18848792-3c79-4e58-882e-f53523817470</ShoppingResponseRefID>
        </ShoppingResponse>
    </Response>
    <PayloadAttributes>
        <CorrelationID>8cdc4aef-6d24-4672-b6f0-8c454d135270</CorrelationID>
        <Timestamp>2020-09-30T17:47:18.947</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_AirShoppingRS>
{% endhighlight %}

</details>

<details>
  <summary><b>AirShopping RS - Spanish resident discounted fares</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRS">
    <Response>
        <DataLists>
            <BaggageAllowanceList>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA1</BaggageAllowanceID>
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
                    <DestCode>MAD</DestCode>
                    <OriginCode>TFN</OriginCode>
                    <OriginDestID>OD1</OriginDestID>
                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ11</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ13</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>TFS</DestCode>
                    <OriginCode>BCN</OriginCode>
                    <OriginDestID>OD2</OriginDestID>
                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ17</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>TFN</DestCode>
                    <OriginCode>BCN</OriginCode>
                    <OriginDestID>OD3</OriginDestID>
                    <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ12</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ15</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ16</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ18</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ19</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ21</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>MAD</DestCode>
                    <OriginCode>TFS</OriginCode>
                    <OriginDestID>OD4</OriginDestID>
                    <PaxJourneyRefID>PJ20</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H50M0S</Duration>
                    <PaxJourneyID>PJ1</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT7H45M0S</Duration>
                    <PaxJourneyID>PJ2</PaxJourneyID>
                    <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT5H50M0S</Duration>
                    <PaxJourneyID>PJ3</PaxJourneyID>
                    <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H45M0S</Duration>
                    <PaxJourneyID>PJ4</PaxJourneyID>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H45M0S</Duration>
                    <PaxJourneyID>PJ5</PaxJourneyID>
                    <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H45M0S</Duration>
                    <PaxJourneyID>PJ6</PaxJourneyID>
                    <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H45M0S</Duration>
                    <PaxJourneyID>PJ7</PaxJourneyID>
                    <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT5H10M0S</Duration>
                    <PaxJourneyID>PJ8</PaxJourneyID>
                    <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT6H30M0S</Duration>
                    <PaxJourneyID>PJ9</PaxJourneyID>
                    <PaxSegmentRefID>SEG11</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT5H45M0S</Duration>
                    <PaxJourneyID>PJ10</PaxJourneyID>
                    <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG13</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H50M0S</Duration>
                    <PaxJourneyID>PJ11</PaxJourneyID>
                    <PaxSegmentRefID>SEG14</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT8H5M0S</Duration>
                    <PaxJourneyID>PJ12</PaxJourneyID>
                    <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H45M0S</Duration>
                    <PaxJourneyID>PJ13</PaxJourneyID>
                    <PaxSegmentRefID>SEG15</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H50M0S</Duration>
                    <PaxJourneyID>PJ14</PaxJourneyID>
                    <PaxSegmentRefID>SEG16</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT7H25M0S</Duration>
                    <PaxJourneyID>PJ15</PaxJourneyID>
                    <PaxSegmentRefID>SEG17</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG18</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT7H15M0S</Duration>
                    <PaxJourneyID>PJ16</PaxJourneyID>
                    <PaxSegmentRefID>SEG19</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG20</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT6H0M0S</Duration>
                    <PaxJourneyID>PJ17</PaxJourneyID>
                    <PaxSegmentRefID>SEG19</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG21</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT5H10M0S</Duration>
                    <PaxJourneyID>PJ18</PaxJourneyID>
                    <PaxSegmentRefID>SEG19</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG18</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT5H55M0S</Duration>
                    <PaxJourneyID>PJ19</PaxJourneyID>
                    <PaxSegmentRefID>SEG22</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG20</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H55M0S</Duration>
                    <PaxJourneyID>PJ20</PaxJourneyID>
                    <PaxSegmentRefID>SEG23</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT8H5M0S</Duration>
                    <PaxJourneyID>PJ21</PaxJourneyID>
                    <PaxSegmentRefID>SEG22</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                </PaxJourney>
            </PaxJourneyList>
            <PaxList>
                <Pax>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <AgeMeasure>8</AgeMeasure>
                    <PaxID>PAX2</PaxID>
                    <PTC>CHD</PTC>
                </Pax>
            </PaxList>
            <PaxSegmentList>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-19T01:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL1</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-18T21:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT2H50M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1586</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG1</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T19:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL2</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T18:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>418</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG2</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-22T00:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFS</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL3</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T22:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT3H0M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1553</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG3</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T21:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL4</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T19:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>422</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG4</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-18T20:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL5</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-18T16:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT2H45M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1574</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG5</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-18T22:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL6</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-18T18:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT2H45M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1578</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG6</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-18T18:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL7</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-18T14:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT2H45M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1572</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG7</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-18T17:50:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL8</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-18T14:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT2H45M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1570</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG8</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T14:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>321</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL9</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T13:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>412</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG9</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T17:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL10</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T15:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT2H55M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1577</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG10</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T16:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL11</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T15:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>414</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG11</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T20:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL12</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T18:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT2H55M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1585</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG12</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T22:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL13</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T20:50:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT2H55M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1589</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG13</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-18T10:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL14</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-18T06:50:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT2H50M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1590</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG14</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-18T15:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL15</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-18T11:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT2H45M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1566</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG15</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-18T12:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL16</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-18T08:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT2H50M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1562</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG16</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T08:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL17</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T07:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>426</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG17</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T13:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL18</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T11:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT2H55M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1569</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG18</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T10:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>321</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL19</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T09:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>404</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG19</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T15:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFN</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL20</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T13:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT2H55M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1573</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG20</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T14:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFS</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL21</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T12:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT3H5M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1545</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG21</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-21T12:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL22</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-21T10:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>408</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG22</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2025-12-18T18:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                        <TerminalName>4</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32Q</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL23</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2025-12-18T15:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>TFS</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT2H55M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1546</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>IB</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG23</PaxSegmentID>
                </PaxSegment>
            </PaxSegmentList>
            <PriceClassList>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>Cabin: Economy</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Hand baggage: 1 piece (56x40x25cm)</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Baggage in hold: Not included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Meal: Not included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Seat  selection: For a fee</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Front seat: Payable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Preferred seat: Not avalilable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Extra legroom seat: Payable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Priority Boarding.: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Change: Allowed with a fee; not allowed for no-shows.</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Refund (in each direction): No refunds</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Exclusive check-in counter: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Access to VIP lounges: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Fast Track: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Priority baggage delivery: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>AVIOS: Yes</DescText>
                    </Desc>
                    <Desc>
                        <DescText>WiFi: For a fee</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Bring your flight forward: Not Included.</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Fast Lane: Not included.</DescText>
                    </Desc>
                    <Name>Basic</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>Cabin: Economy</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Hand baggage: 1 piece (56x40x25cm)</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Baggage in hold: 1 piece included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Meal: Not included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Seat  selection: Free from 24 hours before the flight</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Front seat: Payable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Preferred seat: Not avalilable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Extra legroom seat: Payable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Priority Boarding.: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Change: Allowed with a fee; not allowed for no-shows.</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Refund (in each direction): No refunds</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Exclusive check-in counter: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Access to VIP lounges: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Fast Track: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Priority baggage delivery: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>AVIOS: Yes</DescText>
                    </Desc>
                    <Desc>
                        <DescText>WiFi: For a fee</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Bring your flight forward: Not Included.</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Fast Lane: Not included.</DescText>
                    </Desc>
                    <Name>Optimal</Name>
                    <PriceClassID>PC2</PriceClassID>
                </PriceClass>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>BUSINESS</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>Cabin: Business</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Hand baggage: 1 piece (56x40x25cm)</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Baggage in hold: 1 piece included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Meal: Included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Seat  selection: Free from 24 hours before the flight</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Front seat: Not avalilable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Preferred seat: Payable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Extra legroom seat: Not avalilable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Priority Boarding.: Yes, group 1</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Change: Allowed with a fee; not allowed for no-shows.</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Refund (in each direction): No refunds</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Exclusive check-in counter: Yes</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Access to VIP lounges: Yes</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Fast Track: Yes</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Priority baggage delivery: Yes</DescText>
                    </Desc>
                    <Desc>
                        <DescText>AVIOS: Yes</DescText>
                    </Desc>
                    <Desc>
                        <DescText>WiFi: For a fee</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Bring your flight forward: Not Included.</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Fast Lane: Not included.</DescText>
                    </Desc>
                    <Name>Business Optima</Name>
                    <PriceClassID>PC3</PriceClassID>
                </PriceClass>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>Cabin: Economy</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Hand baggage: 1 piece (56x40x25cm)</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Baggage in hold: 1 piece included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Meal: Not included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Seat  selection: Included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Front seat: Included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Preferred seat: Not avalilable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Extra legroom seat: Payable</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Priority Boarding.: Yes, group 2</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Change: Allowed at any time.</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Refund (in each direction): Allowed without penalty, except in the case no-shows, which do not qualify for a refund</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Exclusive check-in counter: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Access to VIP lounges: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Fast Track: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Priority baggage delivery: No</DescText>
                    </Desc>
                    <Desc>
                        <DescText>AVIOS: Yes</DescText>
                    </Desc>
                    <Desc>
                        <DescText>WiFi: For a fee</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Bring your flight forward: Go to the airport check-in desk in good time and, if there are seats available, you will be able to catch an earlier flight at no extra cost. </DescText>
                    </Desc>
                    <Desc>
                        <DescText>Fast Lane: Not included.</DescText>
                    </Desc>
                    <Name>Flexible Canary</Name>
                    <PriceClassID>PC4</PriceClassID>
                </PriceClass>
            </PriceClassList>
        </DataLists>
        <OffersGroup>
            <CarrierOffers>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <Desc>
                        <DescText>DISCOUNT/SD/RC/215.25EUR</DescText>
                    </Desc>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferID>76e7a354-262b-4380-8d31-846874698e14</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>A</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>K</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">86.10</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">47.29</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">133.39</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                        </FareDetail>
                        <OfferItemID>b7e66c0f-bb7b-4b70-b0e4-5810712241e0</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDateTime>2025-10-31T23:59:59.000</PaymentTimeLimitDateTime>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">86.10</BaseAmount>
                            <TotalAmount CurCode="EUR">133.39</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV1</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>A</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>K</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">59.57</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">47.29</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">106.86</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>8c6fde9e-c38f-46c1-8dfb-d1db4cc3b54c</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDateTime>2025-10-31T23:59:59.000</PaymentTimeLimitDateTime>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">59.57</BaseAmount>
                            <TotalAmount CurCode="EUR">106.86</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV2</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>IB</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">145.67</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">94.58</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">240.25</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <Desc>
                        <DescText>DISCOUNT/SD/RC/215.25EUR</DescText>
                    </Desc>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <MatchTypeCode>Full</MatchTypeCode>
                    <OfferID>2824ec9a-d07f-4dba-ad65-9d4d0c7abf71</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>A</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>K</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">86.10</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">47.29</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">133.39</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                        </FareDetail>
                        <OfferItemID>0e99a79b-901b-4fab-8537-343286af364b</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDateTime>2025-10-31T23:59:59.000</PaymentTimeLimitDateTime>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">86.10</BaseAmount>
                            <TotalAmount CurCode="EUR">133.39</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV3</ServiceID>
                        </Service>
                    </OfferItem>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>A</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>K</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <FarePriceTypeCode>70J</FarePriceTypeCode>
                                <Price>
                                    <BaseAmount CurCode="EUR">59.57</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">47.29</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">106.86</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>014ad5b0-9e53-4877-ac56-6b6144608520</OfferItemID>
                        <PaymentTimeLimit>
                            <PaymentTimeLimitDateTime>2025-10-31T23:59:59.000</PaymentTimeLimitDateTime>
                        </PaymentTimeLimit>
                        <Price>
                            <BaseAmount CurCode="EUR">59.57</BaseAmount>
                            <TotalAmount CurCode="EUR">106.86</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV4</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>IB</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">145.67</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">94.58</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">240.25</TotalAmount>
                    </TotalPrice>
                </Offer>
            </CarrierOffers>
        </OffersGroup>
        <ShoppingResponse>
            <ShoppingResponseRefID>6293fd37-80ba-495d-a4a6-9501381d55d5</ShoppingResponseRefID>
        </ShoppingResponse>
    </Response>
    <PayloadAttributes>
        <CorrelationID>3b84928e-d1a6-3192-ae3c-67851ad4392a</CorrelationID>
        <Timestamp>2025-10-30T14:28:40.446+01:00</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_AirShoppingRS>
{% endhighlight %}

</details>
