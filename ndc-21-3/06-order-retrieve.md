---
layout: page
title:  "Order Retrieve"
parent: NDC 21.3
nav_order: 6
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
| DistributionChain | Must contain agency ID as Seller | Mandatory |
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
| Warning | List of warnings returned by provider, see [warnings](#warnings) | Optional |
| DataLists | The response data lists (journeys, segments, service definitions, etc) | Mandatory |
| Order | The order element detailed [below](#order) | Mandatory |

### Order
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderID | The order ID (same as requested) | Mandatory |
| BookingRefs | List of booking references | Mandatory |
| StatusCode | The order status {::nomarkdown}<ul><li>OPENED: order confirmed</li><li>FROZEN: on request (order to be confirmed)</li><li>CLOSED: order cancelled</li></ul> {:/} | Mandatory |
| TotalPrice | The total price of the whole order | Mandatory |
| OrderItems | List of order items detailed [below](#orderitem) | Mandatory |

#### OrderItem
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderItemID | ID of the order item | Mandatory |
| PaymentTimeLimitDateTime | The ticketing time limit (only if payment must be done) | Optional |
| PriceGuaranteeTimeLimitDateTime | The price guarantee time limit (only if payment must be done) | Optional |
| FareDetail | Contains the PAX associations, the unit price in FarePriceType, and more information for each segment in FareComponent | Mandatory |
| Price | The total price of this offer item | Mandatory |
| SellerFollowUpAction | List of post-booking actions required, to accept schedule change for example. Action codes: {::nomarkdown}<ul><li>Contact Airline</li><li>Accept</li></ul> {:/} | Optional |
| Services | List of flight/serviceDefinition associations with PAX and StatusCode: {::nomarkdown}<ul><li>RQ: on request (availability is waiting to be confirmed, OrderRetrieveRQ has to be called again until status is updated)</li><li>K: pending (OrderChangeRQ has to be executed to issue tickets)</li><li>SB: issuance in progress (waiting to be confirmed, OrderRetrieveRQ has to be called again until status is updated)</li><li>TK: schedule change (see possible actions in SellerFollowUpAction node)</li><li>T: tickets issued</li><li>X: cancelled</li></ul> {:/}  Note: If the StatusCode is absent, the service is included, so no further action is necessary | Mandatory |

### Warning
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Code | The status code of the warning<br />Spanish residents: {::nomarkdown} <ul><li>RESIDENCY_VERIFICATION_STATUS</li></ul> {:/} | Mandatory |
| DescText | The description of the warning<br />Spanish residents: {::nomarkdown} <ul><li>&lt;passenger_name&gt; - NOT_VERIFIED</li><li>&lt;passenger_name&gt; - PENDING</li></ul> {:/} | Mandatory |

# Samples

<details>
  <summary><b>OrderRetrieveRQ</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderRetrieveRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
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
		<ns2:CorrelationID>09401279-fb44-3766-bc60-62c3b45702db</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:OrderValidationFilterCriteria>
			<ns2:OrderFilterCriteria>
				<ns2:OrderID>599187</ns2:OrderID>
				<ns2:OwnerCode>BA</ns2:OwnerCode>
			</ns2:OrderFilterCriteria>
		</ns2:OrderValidationFilterCriteria>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_OrderRetrieveRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderViewRS</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns4:IATA_OrderViewRS xmlns="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns2="http://www.travelsoft.fr/orchestra/ndc/login" xmlns:ns3="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns4="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns5="http://www.w3.org/2000/09/xmldsig#">
	<ns4:Response>
		<ns3:DataLists>
			<ns3:ContactInfoList>
				<ns3:ContactInfo>
					<ns3:ContactInfoID>CONT1</ns3:ContactInfoID>
					<ns3:EmailAddress>
						<ns3:EmailAddressText>john.doe@orchestra.eu</ns3:EmailAddressText>
					</ns3:EmailAddress>
					<ns3:Phone>
						<ns3:CountryDialingCode>33</ns3:CountryDialingCode>
						<ns3:PhoneNumber>102030405</ns3:PhoneNumber>
					</ns3:Phone>
					<ns3:PostalAddress>
						<ns3:CityName>Paris</ns3:CityName>
						<ns3:CountryCode>FR</ns3:CountryCode>
						<ns3:PostalCode>75001</ns3:PostalCode>
						<ns3:StreetText>5 rue de la Rue</ns3:StreetText>
					</ns3:PostalAddress>
				</ns3:ContactInfo>
			</ns3:ContactInfoList>
			<ns3:DatedMarketingSegmentList>
				<ns3:DatedMarketingSegment>
					<ns3:Arrival>
						<ns3:AircraftScheduledDateTime>2026-03-12T18:45:00.000+01:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>LHR</ns3:IATA_LocationCode>
						<ns3:TerminalName>5</ns3:TerminalName>
					</ns3:Arrival>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedMarketingSegmentId>DMS1</ns3:DatedMarketingSegmentId>
					<ns3:DatedOperatingSegmentRefId>DOS1</ns3:DatedOperatingSegmentRefId>
					<ns3:Dep>
						<ns3:AircraftScheduledDateTime>2026-03-12T18:30:00.000+01:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>CDG</ns3:IATA_LocationCode>
						<ns3:TerminalName>2C</ns3:TerminalName>
					</ns3:Dep>
					<ns3:MarketingCarrierFlightNumberText>315</ns3:MarketingCarrierFlightNumberText>
				</ns3:DatedMarketingSegment>
				<ns3:DatedMarketingSegment>
					<ns3:Arrival>
						<ns3:AircraftScheduledDateTime>2026-03-19T22:25:00.000+01:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>CDG</ns3:IATA_LocationCode>
						<ns3:TerminalName>2C</ns3:TerminalName>
					</ns3:Arrival>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedMarketingSegmentId>DMS2</ns3:DatedMarketingSegmentId>
					<ns3:DatedOperatingSegmentRefId>DOS2</ns3:DatedOperatingSegmentRefId>
					<ns3:Dep>
						<ns3:AircraftScheduledDateTime>2026-03-19T20:05:00.000+01:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>LHR</ns3:IATA_LocationCode>
						<ns3:TerminalName>5</ns3:TerminalName>
					</ns3:Dep>
					<ns3:MarketingCarrierFlightNumberText>322</ns3:MarketingCarrierFlightNumberText>
				</ns3:DatedMarketingSegment>
			</ns3:DatedMarketingSegmentList>
			<ns3:DatedOperatingSegmentList>
				<ns3:DatedOperatingSegment>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedOperatingLegRefID>DOL1</ns3:DatedOperatingLegRefID>
					<ns3:DatedOperatingSegmentId>DOS1</ns3:DatedOperatingSegmentId>
					<ns3:Duration>P0Y0M0DT1H15M0S</ns3:Duration>
				</ns3:DatedOperatingSegment>
				<ns3:DatedOperatingSegment>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedOperatingLegRefID>DOL2</ns3:DatedOperatingLegRefID>
					<ns3:DatedOperatingSegmentId>DOS2</ns3:DatedOperatingSegmentId>
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
					<ns3:PaxJourneyRefID>PJ2</ns3:PaxJourneyRefID>
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
					<ns3:PaxJourneyID>PJ2</ns3:PaxJourneyID>
					<ns3:PaxSegmentRefID>SEG2</ns3:PaxSegmentRefID>
				</ns3:PaxJourney>
			</ns3:PaxJourneyList>
			<ns3:PaxList>
				<ns3:Pax>
					<ns3:Birthdate>2001-04-13+02:00</ns3:Birthdate>
					<ns3:ContactInfoRefID>CONT1</ns3:ContactInfoRefID>
					<ns3:Individual>
						<ns3:GivenName>JOHN</ns3:GivenName>
						<ns3:Surname>DOE</ns3:Surname>
						<ns3:TitleName>MR</ns3:TitleName>
					</ns3:Individual>
					<ns3:PaxID>PAX1</ns3:PaxID>
					<ns3:PTC>ADT</ns3:PTC>
				</ns3:Pax>
				<ns3:Pax>
					<ns3:Birthdate>2000-11-14+01:00</ns3:Birthdate>
					<ns3:ContactInfoRefID>CONT1</ns3:ContactInfoRefID>
					<ns3:Individual>
						<ns3:GivenName>JANE</ns3:GivenName>
						<ns3:Surname>DOE</ns3:Surname>
						<ns3:TitleName>MRS</ns3:TitleName>
					</ns3:Individual>
					<ns3:PaxID>PAX2</ns3:PaxID>
					<ns3:PTC>ADT</ns3:PTC>
				</ns3:Pax>
			</ns3:PaxList>
			<ns3:PaxSegmentList>
				<ns3:PaxSegment>
					<ns3:DatedMarketingSegmentRefId>DMS1</ns3:DatedMarketingSegmentRefId>
					<ns3:PaxSegmentID>SEG1</ns3:PaxSegmentID>
				</ns3:PaxSegment>
				<ns3:PaxSegment>
					<ns3:DatedMarketingSegmentRefId>DMS2</ns3:DatedMarketingSegmentRefId>
					<ns3:PaxSegmentID>SEG2</ns3:PaxSegmentID>
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
					<ns3:PriceClassID>PC1</ns3:PriceClassID>
				</ns3:PriceClass>
			</ns3:PriceClassList>
			<ns3:ServiceDefinitionList>
				<ns3:ServiceDefinition>
					<ns3:Desc>
						<ns3:DescText>1 bagage</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>Aller (adulte 1)</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>1 bagage</ns3:Name>
					<ns3:ServiceDefinitionID>SD1</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
			</ns3:ServiceDefinitionList>
		</ns3:DataLists>
		<ns3:Order>
			<ns3:OrderID>599187</ns3:OrderID>
			<ns3:OrderItem>
				<ns3:FareDetail>
					<ns3:FareComponent>
						<ns3:CabinType>
							<ns3:CabinTypeCode>Q</ns3:CabinTypeCode>
							<ns3:CabinTypeName>ECONOMY</ns3:CabinTypeName>
						</ns3:CabinType>
						<ns3:FareTypeCode>70J</ns3:FareTypeCode>
						<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						<ns3:PaxSegmentRefID>SEG2</ns3:PaxSegmentRefID>
						<ns3:PriceClassRefID>PC1</ns3:PriceClassRefID>
					</ns3:FareComponent>
					<ns3:FarePriceType>
						<ns3:Price>
							<ns3:BaseAmount CurCode="EUR">41.25</ns3:BaseAmount>
							<ns3:TaxSummary>
								<ns3:Tax>
									<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
									<ns3:TaxCode>GENERAL_TAXES_PAX_1</ns3:TaxCode>
									<ns3:TaxName>Taxes - PAX1</ns3:TaxName>
								</ns3:Tax>
								<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
							</ns3:TaxSummary>
							<ns3:TotalAmount CurCode="EUR">154.61</ns3:TotalAmount>
						</ns3:Price>
					</ns3:FarePriceType>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				</ns3:FareDetail>
				<ns3:OrderItemID>f515684e-1618-566b-b14e-8d479bde327d</ns3:OrderItemID>
				<ns3:Price>
					<ns3:BaseAmount CurCode="EUR">41.25</ns3:BaseAmount>
					<ns3:TaxSummary>
						<ns3:Tax>
							<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
							<ns3:TaxCode>GENERAL_TAXES_PAX_1</ns3:TaxCode>
							<ns3:TaxName>Taxes - PAX1</ns3:TaxName>
						</ns3:Tax>
						<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
					</ns3:TaxSummary>
					<ns3:TotalAmount CurCode="EUR">154.61</ns3:TotalAmount>
				</ns3:Price>
				<ns3:Service>
					<ns3:BookingRef>
						<ns3:BookingEntity>
							<ns3:Carrier>
								<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
							</ns3:Carrier>
						</ns3:BookingEntity>
						<ns3:BookingID>X2UY9C</ns3:BookingID>
					</ns3:BookingRef>
					<ns3:OrderServiceAssociation>
						<ns3:PaxSegmentRef>
							<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:ServiceID>SV1</ns3:ServiceID>
					<ns3:StatusCode>T</ns3:StatusCode>
				</ns3:Service>
				<ns3:Service>
					<ns3:BookingRef>
						<ns3:BookingEntity>
							<ns3:Carrier>
								<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
							</ns3:Carrier>
						</ns3:BookingEntity>
						<ns3:BookingID>X2UY9C</ns3:BookingID>
					</ns3:BookingRef>
					<ns3:OrderServiceAssociation>
						<ns3:PaxSegmentRef>
							<ns3:PaxSegmentRefID>SEG2</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:ServiceID>SV2</ns3:ServiceID>
					<ns3:StatusCode>T</ns3:StatusCode>
				</ns3:Service>
			</ns3:OrderItem>
			<ns3:OrderItem>
				<ns3:FareDetail>
					<ns3:FareComponent>
						<ns3:CabinType>
							<ns3:CabinTypeCode>Q</ns3:CabinTypeCode>
							<ns3:CabinTypeName>ECONOMY</ns3:CabinTypeName>
						</ns3:CabinType>
						<ns3:FareTypeCode>70J</ns3:FareTypeCode>
						<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						<ns3:PaxSegmentRefID>SEG2</ns3:PaxSegmentRefID>
						<ns3:PriceClassRefID>PC1</ns3:PriceClassRefID>
					</ns3:FareComponent>
					<ns3:FarePriceType>
						<ns3:Price>
							<ns3:BaseAmount CurCode="EUR">41.25</ns3:BaseAmount>
							<ns3:TaxSummary>
								<ns3:Tax>
									<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
									<ns3:TaxCode>GENERAL_TAXES_PAX_2</ns3:TaxCode>
									<ns3:TaxName>Taxes - PAX2</ns3:TaxName>
								</ns3:Tax>
								<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
							</ns3:TaxSummary>
							<ns3:TotalAmount CurCode="EUR">154.61</ns3:TotalAmount>
						</ns3:Price>
					</ns3:FarePriceType>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
				</ns3:FareDetail>
				<ns3:OrderItemID>e21e7267-410b-5a4d-bf8e-94a555eacd10</ns3:OrderItemID>
				<ns3:Price>
					<ns3:BaseAmount CurCode="EUR">41.25</ns3:BaseAmount>
					<ns3:TaxSummary>
						<ns3:Tax>
							<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
							<ns3:TaxCode>GENERAL_TAXES_PAX_2</ns3:TaxCode>
							<ns3:TaxName>Taxes - PAX2</ns3:TaxName>
						</ns3:Tax>
						<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
					</ns3:TaxSummary>
					<ns3:TotalAmount CurCode="EUR">154.61</ns3:TotalAmount>
				</ns3:Price>
				<ns3:Service>
					<ns3:BookingRef>
						<ns3:BookingEntity>
							<ns3:Carrier>
								<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
							</ns3:Carrier>
						</ns3:BookingEntity>
						<ns3:BookingID>X2UY9C</ns3:BookingID>
					</ns3:BookingRef>
					<ns3:OrderServiceAssociation>
						<ns3:PaxSegmentRef>
							<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
					<ns3:ServiceID>SV3</ns3:ServiceID>
					<ns3:StatusCode>T</ns3:StatusCode>
				</ns3:Service>
				<ns3:Service>
					<ns3:BookingRef>
						<ns3:BookingEntity>
							<ns3:Carrier>
								<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
							</ns3:Carrier>
						</ns3:BookingEntity>
						<ns3:BookingID>X2UY9C</ns3:BookingID>
					</ns3:BookingRef>
					<ns3:OrderServiceAssociation>
						<ns3:PaxSegmentRef>
							<ns3:PaxSegmentRefID>SEG2</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
					<ns3:ServiceID>SV4</ns3:ServiceID>
					<ns3:StatusCode>T</ns3:StatusCode>
				</ns3:Service>
			</ns3:OrderItem>
			<ns3:OrderItem>
				<ns3:FareDetail>
					<ns3:FarePriceType>
						<ns3:Price>
							<ns3:BaseAmount CurCode="EUR">62.50</ns3:BaseAmount>
							<ns3:TotalAmount CurCode="EUR">62.50</ns3:TotalAmount>
						</ns3:Price>
					</ns3:FarePriceType>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				</ns3:FareDetail>
				<ns3:OrderItemID>4f31038d-fcfa-5957-b46b-3dc203170d62</ns3:OrderItemID>
				<ns3:Price>
					<ns3:BaseAmount CurCode="EUR">62.50</ns3:BaseAmount>
					<ns3:TotalAmount CurCode="EUR">62.50</ns3:TotalAmount>
				</ns3:Price>
				<ns3:Service>
					<ns3:BookingRef>
						<ns3:BookingEntity>
							<ns3:Carrier>
								<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
							</ns3:Carrier>
						</ns3:BookingEntity>
						<ns3:BookingID>X2UY9C</ns3:BookingID>
					</ns3:BookingRef>
					<ns3:OrderServiceAssociation>
						<ns3:ServiceDefinitionRef>
							<ns3:OrderFlightAssociations>
								<ns3:PaxSegmentRef>
									<ns3:PaxSegmentRefID>SEG1</ns3:PaxSegmentRefID>
								</ns3:PaxSegmentRef>
							</ns3:OrderFlightAssociations>
							<ns3:ServiceDefinitionRefID>SD1</ns3:ServiceDefinitionRefID>
						</ns3:ServiceDefinitionRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:ServiceID>SV5</ns3:ServiceID>
				</ns3:Service>
			</ns3:OrderItem>
			<ns3:OwnerCode>BA</ns3:OwnerCode>
			<ns3:StatusCode>OPENED</ns3:StatusCode>
			<ns3:TotalPrice>
				<ns3:BaseAmount CurCode="EUR">145.00</ns3:BaseAmount>
				<ns3:TaxSummary>
					<ns3:Tax>
						<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
						<ns3:TaxCode>GENERAL_TAXES_PAX_1</ns3:TaxCode>
						<ns3:TaxName>Taxes - PAX1</ns3:TaxName>
					</ns3:Tax>
					<ns3:Tax>
						<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
						<ns3:TaxCode>GENERAL_TAXES_PAX_2</ns3:TaxCode>
						<ns3:TaxName>Taxes - PAX2</ns3:TaxName>
					</ns3:Tax>
					<ns3:TotalTaxAmount CurCode="EUR">226.73</ns3:TotalTaxAmount>
				</ns3:TaxSummary>
				<ns3:TotalAmount CurCode="EUR">371.73</ns3:TotalAmount>
			</ns3:TotalPrice>
		</ns3:Order>
		<ns3:TicketDocInfo>
			<ns3:BookingRef>
				<ns3:BookingEntity>
					<ns3:Carrier>
						<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
					</ns3:Carrier>
				</ns3:BookingEntity>
				<ns3:BookingID>X2UY9C</ns3:BookingID>
			</ns3:BookingRef>
			<ns3:PaxRefID>PAX1</ns3:PaxRefID>
			<ns3:Ticket>
				<ns3:Coupon>
					<ns3:CouponNumber>1</ns3:CouponNumber>
					<ns3:CouponStatusCode>I</ns3:CouponStatusCode>
				</ns3:Coupon>
				<ns3:ReportingTypeCode>BSP</ns3:ReportingTypeCode>
				<ns3:TicketDocTypeCode>T</ns3:TicketDocTypeCode>
				<ns3:TicketNumber>125-2222194227</ns3:TicketNumber>
			</ns3:Ticket>
			<ns3:Ticket>
				<ns3:Coupon>
					<ns3:CouponNumber>1</ns3:CouponNumber>
					<ns3:CouponStatusCode>I</ns3:CouponStatusCode>
				</ns3:Coupon>
				<ns3:ReportingTypeCode>BSP</ns3:ReportingTypeCode>
				<ns3:TicketDocTypeCode>J</ns3:TicketDocTypeCode>
				<ns3:TicketNumber>125-4251507209</ns3:TicketNumber>
			</ns3:Ticket>
		</ns3:TicketDocInfo>
		<ns3:TicketDocInfo>
			<ns3:BookingRef>
				<ns3:BookingEntity>
					<ns3:Carrier>
						<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
					</ns3:Carrier>
				</ns3:BookingEntity>
				<ns3:BookingID>X2UY9C</ns3:BookingID>
			</ns3:BookingRef>
			<ns3:PaxRefID>PAX2</ns3:PaxRefID>
			<ns3:Ticket>
				<ns3:Coupon>
					<ns3:CouponNumber>1</ns3:CouponNumber>
					<ns3:CouponStatusCode>I</ns3:CouponStatusCode>
				</ns3:Coupon>
				<ns3:ReportingTypeCode>BSP</ns3:ReportingTypeCode>
				<ns3:TicketDocTypeCode>T</ns3:TicketDocTypeCode>
				<ns3:TicketNumber>125-2222194226</ns3:TicketNumber>
			</ns3:Ticket>
		</ns3:TicketDocInfo>
		<ns3:Warning>
			<ns3:Code>SUPPLIER_WARN</ns3:Code>
			<ns3:DescText>All services may not be delivered as the requested fare component may include a codeshare flight or an interline itinerary.</ns3:DescText>
		</ns3:Warning>
	</ns4:Response>
	<ns4:PayloadAttributes>
		<ns3:CorrelationID>09401279-fb44-3766-bc60-62c3b45702db</ns3:CorrelationID>
		<ns3:Timestamp>2026-01-28T17:17:11.326+01:00</ns3:Timestamp>
		<ns3:VersionNumber>21.3</ns3:VersionNumber>
	</ns4:PayloadAttributes>
</ns4:IATA_OrderViewRS>
{% endhighlight %}

</details>
