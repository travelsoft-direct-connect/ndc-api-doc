---
layout: page
title:  "Order Change"
parent: NDC 21.3
nav_order: 8
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
<IATA_OrderChangeRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
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
		<ns2:CorrelationID>1ff5189c-486a-4b7a-a245-f664015696ad</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:DataLists>
			<ns2:ContactInfoList>
				<ns2:ContactInfo>
					<ns2:ContactInfoID>CONT1</ns2:ContactInfoID>
					<ns2:EmailAddress>
						<ns2:EmailAddressText>john.doe@orchestra.eu</ns2:EmailAddressText>
					</ns2:EmailAddress>
					<ns2:IndividualRefID>IND1</ns2:IndividualRefID>
					<ns2:Phone>
						<ns2:AreaCodeNumber/>
						<ns2:CountryDialingCode>33</ns2:CountryDialingCode>
						<ns2:PhoneNumber>102030405</ns2:PhoneNumber>
					</ns2:Phone>
					<ns2:PostalAddress>
						<ns2:CityName>Paris</ns2:CityName>
						<ns2:ContactTypeText>AddressAtOrigin</ns2:ContactTypeText>
						<ns2:CountryCode>FR</ns2:CountryCode>
						<ns2:PostalCode>75001</ns2:PostalCode>
						<ns2:StreetText>5 rue de la Rue</ns2:StreetText>
					</ns2:PostalAddress>
				</ns2:ContactInfo>
			</ns2:ContactInfoList>
			<ns2:PaxList>
				<ns2:Pax>
					<ns2:Birthdate>2001-04-13</ns2:Birthdate>
					<ns2:ContactInfoRefID>CONT1</ns2:ContactInfoRefID>
					<ns2:Individual>
						<ns2:Birthdate>2001-04-13</ns2:Birthdate>
						<ns2:GenderCode>M</ns2:GenderCode>
						<ns2:GivenName>John</ns2:GivenName>
						<ns2:IndividualID>IND1</ns2:IndividualID>
						<ns2:Surname>Doe</ns2:Surname>
						<ns2:TitleName>MR</ns2:TitleName>
					</ns2:Individual>
					<ns2:PaxID>PAX1</ns2:PaxID>
					<ns2:PTC>ADT</ns2:PTC>
				</ns2:Pax>
				<ns2:Pax>
					<ns2:Birthdate>2002-03-02</ns2:Birthdate>
					<ns2:ContactInfoRefID>CONT1</ns2:ContactInfoRefID>
					<ns2:Individual>
						<ns2:Birthdate>2002-03-02</ns2:Birthdate>
						<ns2:GenderCode>F</ns2:GenderCode>
						<ns2:GivenName>Jane</ns2:GivenName>
						<ns2:IndividualID>IND2</ns2:IndividualID>
						<ns2:Surname>Doe</ns2:Surname>
						<ns2:TitleName>MRS</ns2:TitleName>
					</ns2:Individual>
					<ns2:PaxID>PAX2</ns2:PaxID>
					<ns2:PTC>ADT</ns2:PTC>
				</ns2:Pax>
			</ns2:PaxList>
		</ns2:DataLists>
		<ns2:Order>
			<ns2:OrderID>599253</ns2:OrderID>
			<ns2:OwnerCode>EY</ns2:OwnerCode>
		</ns2:Order>
		<ns2:OrderChangeParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
		</ns2:OrderChangeParameters>
		<ns2:PaymentFunctions>
			<ns2:OrderAssociation>
				<ns2:OrderItemRefID>3e80e4a6-6db7-54c8-afb4-2cf90dfcdd91</ns2:OrderItemRefID>
				<ns2:OrderRefID>599253</ns2:OrderRefID>
			</ns2:OrderAssociation>
			<ns2:PaymentProcessingDetails>
				<ns2:Amount CurCode="EUR">1762.20</ns2:Amount>
				<ns2:PaymentMethod>
					<ns2:SettlementPlan>
						<ns2:PaymentTypeCode>Cash</ns2:PaymentTypeCode>
					</ns2:SettlementPlan>
				</ns2:PaymentMethod>
				<ns2:PaymentRefID>PAY1</ns2:PaymentRefID>
			</ns2:PaymentProcessingDetails>
		</ns2:PaymentFunctions>
	</Request>
</IATA_OrderChangeRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderViewRS - Ticket Issue</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns2:IATA_OrderViewRS xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#">
    <ns2:Response>
        <DataLists>
            <ContactInfoList>
                <ContactInfo>
                    <ContactInfoID>CONT1</ContactInfoID>
                    <EmailAddress>
                        <EmailAddressText>john.doe@orchestra.eu</EmailAddressText>
                    </EmailAddress>
                    <Phone>
                        <CountryDialingCode>33</CountryDialingCode>
                        <PhoneNumber>102030405</PhoneNumber>
                    </Phone>
                    <PostalAddress>
                        <CityName>Paris</CityName>
                        <CountryCode>FR</CountryCode>
                        <PostalCode>75001</PostalCode>
                        <StreetText>5 rue de la Rue</StreetText>
                    </PostalAddress>
                </ContactInfo>
            </ContactInfoList>
            <DatedMarketingSegmentList>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-12T07:10:00.000+01:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>EY</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS1</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS1</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-12T02:30:00.000+01:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AUH</IATA_LocationCode>
                        <TerminalName>A</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>31</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
                <DatedMarketingSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2026-03-19T19:35:00.000+01:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AUH</IATA_LocationCode>
                        <TerminalName>A</TerminalName>
                    </Arrival>
                    <CarrierDesigCode>EY</CarrierDesigCode>
                    <DatedMarketingSegmentId>DMS2</DatedMarketingSegmentId>
                    <DatedOperatingSegmentRefId>DOS2</DatedOperatingSegmentRefId>
                    <Dep>
                        <AircraftScheduledDateTime>2026-03-19T09:55:00.000+01:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <MarketingCarrierFlightNumberText>32</MarketingCarrierFlightNumberText>
                </DatedMarketingSegment>
            </DatedMarketingSegmentList>
            <DatedOperatingSegmentList>
                <DatedOperatingSegment>
                    <CarrierDesigCode>EY</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL1</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS1</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT7H40M0S</Duration>
                </DatedOperatingSegment>
                <DatedOperatingSegment>
                    <CarrierDesigCode>EY</CarrierDesigCode>
                    <DatedOperatingLegRefID>DOL2</DatedOperatingLegRefID>
                    <DatedOperatingSegmentId>DOS2</DatedOperatingSegmentId>
                    <Duration>P0Y0M0DT6H40M0S</Duration>
                </DatedOperatingSegment>
            </DatedOperatingSegmentList>
            <OriginDestList>
                <OriginDest>
                    <DestCode>CDG</DestCode>
                    <OriginCode>AUH</OriginCode>
                    <OriginDestID>OD1</OriginDestID>
                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>AUH</DestCode>
                    <OriginCode>CDG</OriginCode>
                    <OriginDestID>OD2</OriginDestID>
                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT7H40M0S</Duration>
                    <PaxJourneyID>PJ1</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT6H40M0S</Duration>
                    <PaxJourneyID>PJ2</PaxJourneyID>
                    <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                </PaxJourney>
            </PaxJourneyList>
            <PaxList>
                <Pax>
                    <Birthdate>2001-04-13+02:00</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <Individual>
                        <GivenName>John</GivenName>
                        <Surname>Doe</Surname>
                        <TitleName>MR</TitleName>
                    </Individual>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <Birthdate>2002-03-02+01:00</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <Individual>
                        <GivenName>Jane</GivenName>
                        <Surname>Doe</Surname>
                        <TitleName>MRS</TitleName>
                    </Individual>
                    <PaxID>PAX2</PaxID>
                    <PTC>ADT</PTC>
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
            </PaxSegmentList>
            <PriceClassList>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>Cancellation / Refund - Cancellation isn't allowed</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Change / Reissue - Changes not allowed</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Miles earned per guest - 15%</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Cabin baggage allowance - Find more information on baggage terms at https://www.etihad.com/en-ae/fly-etihad/baggage</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Checked baggage allowance per guest - Checked baggage allowance isn't included. Find more information on baggage terms at https://www.etihad.com/en-ae/fly-etihad/baggage</DescText>
                    </Desc>
                    <Name>Economy Basic</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
            </PriceClassList>
        </DataLists>
        <Order>
            <OrderID>599253</OrderID>
            <OrderItem>
                <FareDetail>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>5</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <FareBasisCode>VLE14H89</FareBasisCode>
                        <FareTypeCode>70J</FareTypeCode>
                        <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>5</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <FareBasisCode>QLE05H8R</FareBasisCode>
                        <FareTypeCode>70J</FareTypeCode>
                        <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FarePriceType>
                        <Price>
                            <BaseAmount CurCode="EUR">624.00</BaseAmount>
                            <TaxSummary>
                                <Tax>
                                    <Amount CurCode="EUR">150.92</Amount>
                                    <TaxCode>TAX_GROUP_1_PAX_1_2</TaxCode>
                                    <TaxName>Surcharge carburant (YQ/YR) - PAX1,2</TaxName>
                                </Tax>
                                <Tax>
                                    <Amount CurCode="EUR">106.18</Amount>
                                    <TaxCode>GENERAL_TAXES_PAX_1_2</TaxCode>
                                    <TaxName>Taxes - PAX1,2</TaxName>
                                </Tax>
                                <TotalTaxAmount CurCode="EUR">257.10</TotalTaxAmount>
                            </TaxSummary>
                            <TotalAmount CurCode="EUR">881.10</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                    <PaxRefID>PAX2</PaxRefID>
                </FareDetail>
                <OrderItemID>b6445e44-f30a-576a-9b02-c375a78e2e4e</OrderItemID>
                <Price>
                    <BaseAmount CurCode="EUR">1248.00</BaseAmount>
                    <TaxSummary>
                        <Tax>
                            <Amount CurCode="EUR">301.84</Amount>
                            <TaxCode>TAX_GROUP_1_PAX_1_2</TaxCode>
                            <TaxName>Surcharge carburant (YQ/YR) - PAX1,2</TaxName>
                        </Tax>
                        <Tax>
                            <Amount CurCode="EUR">212.36</Amount>
                            <TaxCode>GENERAL_TAXES_PAX_1_2</TaxCode>
                            <TaxName>Taxes - PAX1,2</TaxName>
                        </Tax>
                        <TotalTaxAmount CurCode="EUR">514.20</TotalTaxAmount>
                    </TaxSummary>
                    <TotalAmount CurCode="EUR">1762.20</TotalAmount>
                </Price>
                <Service>
                    <BookingRef>
                        <BookingEntity>
                            <Carrier>
                                <AirlineDesigCode>EY</AirlineDesigCode>
                            </Carrier>
                        </BookingEntity>
                        <BookingID>7J8FAI</BookingID>
                    </BookingRef>
                    <OrderServiceAssociation>
                        <PaxSegmentRef>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        </PaxSegmentRef>
                    </OrderServiceAssociation>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceID>SV1</ServiceID>
                    <StatusCode>T</StatusCode>
                </Service>
                <Service>
                    <BookingRef>
                        <BookingEntity>
                            <Carrier>
                                <AirlineDesigCode>EY</AirlineDesigCode>
                            </Carrier>
                        </BookingEntity>
                        <BookingID>7J8FAI</BookingID>
                    </BookingRef>
                    <OrderServiceAssociation>
                        <PaxSegmentRef>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        </PaxSegmentRef>
                    </OrderServiceAssociation>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceID>SV2</ServiceID>
                    <StatusCode>T</StatusCode>
                </Service>
                <Service>
                    <BookingRef>
                        <BookingEntity>
                            <Carrier>
                                <AirlineDesigCode>EY</AirlineDesigCode>
                            </Carrier>
                        </BookingEntity>
                        <BookingID>7J8FAI</BookingID>
                    </BookingRef>
                    <OrderServiceAssociation>
                        <PaxSegmentRef>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                        </PaxSegmentRef>
                    </OrderServiceAssociation>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceID>SV3</ServiceID>
                    <StatusCode>T</StatusCode>
                </Service>
                <Service>
                    <BookingRef>
                        <BookingEntity>
                            <Carrier>
                                <AirlineDesigCode>EY</AirlineDesigCode>
                            </Carrier>
                        </BookingEntity>
                        <BookingID>7J8FAI</BookingID>
                    </BookingRef>
                    <OrderServiceAssociation>
                        <PaxSegmentRef>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                        </PaxSegmentRef>
                    </OrderServiceAssociation>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceID>SV4</ServiceID>
                    <StatusCode>T</StatusCode>
                </Service>
            </OrderItem>
            <OwnerCode>EY</OwnerCode>
            <StatusCode>OPENED</StatusCode>
            <TotalPrice>
                <BaseAmount CurCode="EUR">1248.00</BaseAmount>
                <TaxSummary>
                    <Tax>
                        <Amount CurCode="EUR">301.84</Amount>
                        <TaxCode>TAX_GROUP_1_PAX_1_2</TaxCode>
                        <TaxName>Surcharge carburant (YQ/YR) - PAX1,2</TaxName>
                    </Tax>
                    <Tax>
                        <Amount CurCode="EUR">212.36</Amount>
                        <TaxCode>GENERAL_TAXES_PAX_1_2</TaxCode>
                        <TaxName>Taxes - PAX1,2</TaxName>
                    </Tax>
                    <TotalTaxAmount CurCode="EUR">514.20</TotalTaxAmount>
                </TaxSummary>
                <TotalAmount CurCode="EUR">1762.20</TotalAmount>
            </TotalPrice>
        </Order>
        <TicketDocInfo>
            <BookingRef>
                <BookingEntity>
                    <Carrier>
                        <AirlineDesigCode>EY</AirlineDesigCode>
                    </Carrier>
                </BookingEntity>
                <BookingID>7J8FAI</BookingID>
            </BookingRef>
            <PaxRefID>PAX1</PaxRefID>
            <Ticket>
                <Coupon>
                    <CouponNumber>1</CouponNumber>
                    <CouponStatusCode>I</CouponStatusCode>
                </Coupon>
                <ReportingTypeCode>BSP</ReportingTypeCode>
                <TicketDocTypeCode>T</TicketDocTypeCode>
                <TicketNumber>6072407134937</TicketNumber>
            </Ticket>
        </TicketDocInfo>
        <TicketDocInfo>
            <BookingRef>
                <BookingEntity>
                    <Carrier>
                        <AirlineDesigCode>EY</AirlineDesigCode>
                    </Carrier>
                </BookingEntity>
                <BookingID>7J8FAI</BookingID>
            </BookingRef>
            <PaxRefID>PAX2</PaxRefID>
            <Ticket>
                <Coupon>
                    <CouponNumber>1</CouponNumber>
                    <CouponStatusCode>I</CouponStatusCode>
                </Coupon>
                <ReportingTypeCode>BSP</ReportingTypeCode>
                <TicketDocTypeCode>T</TicketDocTypeCode>
                <TicketNumber>6072407134938</TicketNumber>
            </Ticket>
        </TicketDocInfo>
    </ns2:Response>
    <ns2:PayloadAttributes>
        <CorrelationID>8d3d0be8-2e11-4131-bd76-551be172ce1b</CorrelationID>
        <Timestamp>2026-01-30T11:28:49.602+01:00</Timestamp>
        <VersionNumber>21.3</VersionNumber>
    </ns2:PayloadAttributes>
</ns2:IATA_OrderViewRS>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderChangeRQ - Accept disruption</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderChangeRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
    <Request>
        <ns2:ChangeOrderChoice>
            <ns2:AcceptChange>
                <ns2:OrderItemRefID>32764ce2-5548-4498-b36f-58a10c3906f9</ns2:OrderItemRefID>
                <ns2:OrderItemRefID>1dbb3411-1bc3-4e6c-896e-b90cc7a585b6</ns2:OrderItemRefID>
            </ns2:AcceptChange>
        </ns2:ChangeOrderChoice>
        <ns2:Order>
            <ns2:OrderID>544755</ns2:OrderID>
            <ns2:OwnerCode>BA</ns2:OwnerCode>
        </ns2:Order>
    </Request>
</IATA_OrderChangeRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderChangeRQ - Cancel</b></summary>

{% highlight xml %}
<IATA_OrderChangeRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
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
		<ns2:CorrelationID>1d9eb36f-dc7e-32a2-a29f-59a8b42e23a9</ns2:CorrelationID>
		<ns2:PrimaryLangID>FR</ns2:PrimaryLangID>
		<ns2:VersionNumber>21.3</ns2:VersionNumber>
	</PayloadAttributes>
	<Request>
		<ns2:ChangeOrderChoice>
			<ns2:AcceptCancelledOffer>
				<ns2:OfferID>8f70a1e9-049c-4243-86d0-801a8fcd1082</ns2:OfferID>
				<ns2:OwnerCode>BA</ns2:OwnerCode>
			</ns2:AcceptCancelledOffer>
		</ns2:ChangeOrderChoice>
		<ns2:DataLists>
			<ns2:ContactInfoList>
				<ns2:ContactInfo>
					<ns2:ContactInfoID>CONT1</ns2:ContactInfoID>
					<ns2:EmailAddress>
						<ns2:EmailAddressText>john.doe@orchestra.eu</ns2:EmailAddressText>
					</ns2:EmailAddress>
					<ns2:IndividualRefID>IND1</ns2:IndividualRefID>
					<ns2:Phone>
						<ns2:AreaCodeNumber/>
						<ns2:CountryDialingCode>33</ns2:CountryDialingCode>
						<ns2:PhoneNumber>102030405</ns2:PhoneNumber>
					</ns2:Phone>
					<ns2:PostalAddress>
						<ns2:CityName>Paris</ns2:CityName>
						<ns2:ContactTypeText>AddressAtOrigin</ns2:ContactTypeText>
						<ns2:CountryCode>FR</ns2:CountryCode>
						<ns2:PostalCode>75001</ns2:PostalCode>
						<ns2:StreetText>5 rue de la Rue</ns2:StreetText>
					</ns2:PostalAddress>
				</ns2:ContactInfo>
			</ns2:ContactInfoList>
			<ns2:PaxList>
				<ns2:Pax>
					<ns2:Birthdate>2001-04-13</ns2:Birthdate>
					<ns2:ContactInfoRefID>CONT1</ns2:ContactInfoRefID>
					<ns2:Individual>
						<ns2:Birthdate>2001-04-13</ns2:Birthdate>
						<ns2:GenderCode>M</ns2:GenderCode>
						<ns2:GivenName>John</ns2:GivenName>
						<ns2:IndividualID>IND1</ns2:IndividualID>
						<ns2:Surname>Doe</ns2:Surname>
						<ns2:TitleName>MR</ns2:TitleName>
					</ns2:Individual>
					<ns2:PaxID>PAX1</ns2:PaxID>
					<ns2:PTC>ADT</ns2:PTC>
				</ns2:Pax>
				<ns2:Pax>
					<ns2:Birthdate>2000-11-14</ns2:Birthdate>
					<ns2:ContactInfoRefID>CONT1</ns2:ContactInfoRefID>
					<ns2:Individual>
						<ns2:Birthdate>2000-11-14</ns2:Birthdate>
						<ns2:GenderCode>F</ns2:GenderCode>
						<ns2:GivenName>Jane</ns2:GivenName>
						<ns2:IndividualID>IND2</ns2:IndividualID>
						<ns2:Surname>Doe</ns2:Surname>
						<ns2:TitleName>MRS</ns2:TitleName>
					</ns2:Individual>
					<ns2:PaxID>PAX2</ns2:PaxID>
					<ns2:PTC>ADT</ns2:PTC>
				</ns2:Pax>
			</ns2:PaxList>
		</ns2:DataLists>
		<ns2:Order>
			<ns2:OrderID>599187</ns2:OrderID>
			<ns2:OwnerCode>BA</ns2:OwnerCode>
		</ns2:Order>
		<ns2:OrderChangeParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
		</ns2:OrderChangeParameters>
	</Request>
</IATA_OrderChangeRQ>
