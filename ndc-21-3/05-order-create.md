---
layout: page
title:  "Order Create"
parent: NDC 21.3
nav_order: 5
---

# OrderCreate operation
{: .no_toc }
The order create method allows to create an order from an offer selection. The passenger information must be sent in request to be able to create order in the airline system. If the order creation succeeds, the response contains an order ID that can be used for servicing flows.

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

# OrderCreateRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| DistributionChain | Must contain agency ID as Seller | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| CreateOrder | The order to create, detailed [below](#createorder) | Mandatory |
| DataLists | The request data lists detailed [below](#datalists) | Mandatory |
| PaymentFunctions | Only if instant payment is required (PaymentTimeLimitDuration is PT0S in OfferPriceRS) or if direct ticket issue has to be forced at order creation. If so, an element 'PaymentProcessingDetails' has to be set with Cash method. | Optional |

### CreateOrder
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| SelectedOffer | The selected offer previously priced with offerPrice operation | Mandatory |

### DataLists
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| ContactInfoList | Must contains the client contact, detailed [below](#contactinfo) | Mandatory |
| PaxList | List of passengers (same as AirShoppingRQ) with more information (name, document, etc), detailed [below](#pax) | Mandatory |

#### ContactInfo
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| ContactInfoID | The contact info ID (to use as reference at PAX level) | Mandatory |
| EmailAddress | The email address | Mandatory |
| Phone | The phone number | Mandatory |
| PostalAddress | The postal address | Mandatory |

#### Pax
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PTC              | PTC                                           | Mandatory          |
| PaxID            | Pax ID                                        | Mandatory          |
| Remark           | Remarks, see detailed [below](#remarks)       | Optional           |
| Birthdate        | date of birth                                 | Mandatory          |
| ContactInfoRefID | client contact reference, see [ContactInfoList](#datalists) | Optional           |
| Individual       | Pax information, detailed [below](#individual)    | Mandatory          |
| IdentityDoc      | Pax Document, detailed [below](#identitydoc)      | Optional           |

##### Remarks
{: .no_toc }

| Element      | Description           | Optional/Mandatory |
| --- | --- | --- |
| RemarkText   | Additional data for requested special fares<br />Spanish residents: {::nomarkdown}<ul><li>MC&#124;380012 - where 380012 is an example of a Spanish municipality code</li></ul>{:/} | Mandatory |


##### Individual
{: .no_toc }

| Element      | Description           | Optional/Mandatory |
| ------------ | --------------------- | ------------------ |
| IndividualID | ID                    | Mandatory          |
| Birthdate    | date of birth         | Mandatory          |
| GivenName    | First name            | Mandatory          |
| Surname      | Last name             | Mandatory          |
| TitleName    | 'MR', 'MRS' or 'MISS' | Mandatory          |

##### IdentityDoc
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| IdentityDocID | Document ID<br />By default it is the document number<br />For fiscal code use case, the expected format is:<br />(Code/ID/CompanyName)<br />For example: <br />- CUIL/123456789<br />- CUIL/123456789/MyAgencyName | Mandatory |
| IdentityDocTypeCode | Document Code, possible values (PT, VS, ID, DL or FC)<br />PT = Passport<br />VS = Visa<br />ID = Identity card (mandatory for Spanish resident discounted fares)<br />DL = Driving License<br />FC = Fiscal code  | Mandatory |
| Birthdate |  | Optional |
| CitizenshipCountryCode |  | Optional |
| ExpiryDate |  | Optional |
| GivenName | First name | Optional |
| IssueDate |  | Optional |
| IssuingCountryCode |  | Optional |
| ResidenceCountryCode |  | Optional |
| Surname | Last name  | Optional |
| TitleName | 'MR', 'MRS' or 'MISS' | Optional |
| Visa |  | Optional |

# OrderCreate - OrderViewRS

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
| OrderID | The order ID (to use for servicing) | Mandatory |
| BookingRefs | List of booking references | Mandatory |
| StatusCode | The order status {::nomarkdown}<ul><li>OPENED: order confirmed</li><li>FROZEN: on request (order to be confirmed)</li><li>CLOSED: order cancelled</li></ul> {:/} | Mandatory |
| TotalPrice | The total price of the whole order | Mandatory |
| OrderItems | List of order items detailed [below](#orderitem) | Mandatory |

#### OrderItem
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderItemID | ID of the order item | Mandatory |
| PaymentTimeLimitDateTime | The ticketing time limit (if not instant payment) | Optional |
| PriceGuaranteeTimeLimitDateTime | The price guarantee time limit (if not instant payment) | Optional |
| FareDetail | Contains the PAX associations, the unit price in FarePriceType, and more information for each segment in FareComponent | Mandatory |
| Price | The total price of this offer item | Mandatory |
| Services | List of flight/serviceDefinition associations with PAX and StatusCode: {::nomarkdown}<ul><li>RQ: on request (availability is waiting to be confirmed, OrderRetrieveRQ has to be called periodically until status is updated)</li><li>K: pending (OrderChangeRQ has to be executed to issue tickets)</li><li>SB: issuance in progress (waiting to be confirmed, OrderRetrieveRQ has to be called periodically until status is updated)</li><li>T: tickets issued</li><li>X: cancelled</li></ul> {:/}  Note: If the StatusCode is absent, the service is included, so no further action is necessary | Mandatory |

# Samples

<details>
  <summary><b>OrderCreateRQ (Hold booking)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderCreateRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
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
		<ns2:CreateOrder>
			<ns2:AcceptSelectedQuotedOfferList>
				<ns2:SelectedPricedOffer>
					<ns2:OfferRefID>009bd994-285d-4c00-8140-6afeb185fe38</ns2:OfferRefID>
					<ns2:OwnerCode>BA</ns2:OwnerCode>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>1f6a66e2-eb8f-4eb3-b05f-dfe58a8f9191</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:PaxRefID>PAX2</ns2:PaxRefID>
					</ns2:SelectedOfferItem>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>1da39e1d-3689-4953-b3ec-bbc3123fa7c2</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:SelectedALaCarteOfferItem>
							<ns2:Qty>1</ns2:Qty>
						</ns2:SelectedALaCarteOfferItem>
					</ns2:SelectedOfferItem>
				</ns2:SelectedPricedOffer>
			</ns2:AcceptSelectedQuotedOfferList>
		</ns2:CreateOrder>
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
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_OrderCreateRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderCreateRQ (Instant payment)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderCreateRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
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
		<ns2:CreateOrder>
			<ns2:AcceptSelectedQuotedOfferList>
				<ns2:SelectedPricedOffer>
					<ns2:OfferRefID>009bd994-285d-4c00-8140-6afeb185fe38</ns2:OfferRefID>
					<ns2:OwnerCode>BA</ns2:OwnerCode>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>1f6a66e2-eb8f-4eb3-b05f-dfe58a8f9191</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:PaxRefID>PAX2</ns2:PaxRefID>
					</ns2:SelectedOfferItem>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>1da39e1d-3689-4953-b3ec-bbc3123fa7c2</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:SelectedALaCarteOfferItem>
							<ns2:Qty>1</ns2:Qty>
						</ns2:SelectedALaCarteOfferItem>
					</ns2:SelectedOfferItem>
				</ns2:SelectedPricedOffer>
			</ns2:AcceptSelectedQuotedOfferList>
		</ns2:CreateOrder>
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
		<ns2:PaymentFunctions>
			<ns2:PaymentProcessingDetails>
				<ns2:Amount CurCode="EUR">371.73</ns2:Amount>
				<ns2:PaymentMethod>
					<ns2:SettlementPlan>
						<ns2:PaymentTypeCode>Cash</ns2:PaymentTypeCode>
					</ns2:SettlementPlan>
				</ns2:PaymentMethod>
				<ns2:PaymentRefID>PAY1</ns2:PaymentRefID>
			</ns2:PaymentProcessingDetails>
		</ns2:PaymentFunctions>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_OrderCreateRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderCreateRQ (Passport)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderCreateRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
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
		<ns2:CreateOrder>
			<ns2:AcceptSelectedQuotedOfferList>
				<ns2:SelectedPricedOffer>
					<ns2:OfferRefID>009bd994-285d-4c00-8140-6afeb185fe38</ns2:OfferRefID>
					<ns2:OwnerCode>BA</ns2:OwnerCode>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>1f6a66e2-eb8f-4eb3-b05f-dfe58a8f9191</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:PaxRefID>PAX2</ns2:PaxRefID>
					</ns2:SelectedOfferItem>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>1da39e1d-3689-4953-b3ec-bbc3123fa7c2</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:SelectedALaCarteOfferItem>
							<ns2:Qty>1</ns2:Qty>
						</ns2:SelectedALaCarteOfferItem>
					</ns2:SelectedOfferItem>
				</ns2:SelectedPricedOffer>
			</ns2:AcceptSelectedQuotedOfferList>
		</ns2:CreateOrder>
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
					<ns2:IdentityDoc>
                        <ns2:Birthdate>1980-02-03</ns2:Birthdate>
                        <ns2:CitizenshipCountryCode>BD</ns2:CitizenshipCountryCode>
                        <ns2:ExpiryDate>2025-05-03</ns2:ExpiryDate>
                        <ns2:GivenName>John</ns2:GivenName>
                        <ns2:IdentityDocID>78965412306</ns2:IdentityDocID>
                        <ns2:IdentityDocTypeCode>PT</ns2:IdentityDocTypeCode>
                        <ns2:IssueDate>2020-07-14</ns2:IssueDate>
                        <ns2:IssuingCountryCode>BD</ns2:IssuingCountryCode>
                        <ns2:ResidenceCountryCode>BD</ns2:ResidenceCountryCode>
                        <ns2:Surname>Doe</ns2:Surname>
                        <ns2:TitleName>MR</ns2:TitleName>
                        <ns2:Visa>
                            <ns2:VisaID>4451512345</ns2:VisaID>
                            <ns2:VisaTypeCode>VS</ns2:VisaTypeCode>
                        </ns2:Visa>
                    </ns2:IdentityDoc>
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
			</ns2:PaxList>
		</ns2:DataLists>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_OrderCreateRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderCreateRQ (Fiscal code)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderCreateRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
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
		<ns2:CreateOrder>
			<ns2:AcceptSelectedQuotedOfferList>
				<ns2:SelectedPricedOffer>
					<ns2:OfferRefID>009bd994-285d-4c00-8140-6afeb185fe38</ns2:OfferRefID>
					<ns2:OwnerCode>BA</ns2:OwnerCode>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>1f6a66e2-eb8f-4eb3-b05f-dfe58a8f9191</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:PaxRefID>PAX2</ns2:PaxRefID>
					</ns2:SelectedOfferItem>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>1da39e1d-3689-4953-b3ec-bbc3123fa7c2</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:SelectedALaCarteOfferItem>
							<ns2:Qty>1</ns2:Qty>
						</ns2:SelectedALaCarteOfferItem>
					</ns2:SelectedOfferItem>
				</ns2:SelectedPricedOffer>
			</ns2:AcceptSelectedQuotedOfferList>
		</ns2:CreateOrder>
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
					<ns2:IdentityDoc>
                        <ns2:IdentityDocID>CUIL/123456789/MyAgencyName</ns2:IdentityDocID>
                        <ns2:IdentityDocTypeCode>FC</ns2:IdentityDocTypeCode>
                    </ns2:IdentityDoc>
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
			</ns2:PaxList>
		</ns2:DataLists>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_OrderCreateRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderCreateRQ (Spanish residents)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderCreateRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
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
		<ns2:CreateOrder>
			<ns2:AcceptSelectedQuotedOfferList>
				<ns2:SelectedPricedOffer>
					<ns2:OfferRefID>009bd994-285d-4c00-8140-6afeb185fe38</ns2:OfferRefID>
					<ns2:OwnerCode>BA</ns2:OwnerCode>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>1f6a66e2-eb8f-4eb3-b05f-dfe58a8f9191</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:PaxRefID>PAX2</ns2:PaxRefID>
					</ns2:SelectedOfferItem>
					<ns2:SelectedOfferItem>
						<ns2:OfferItemRefID>1da39e1d-3689-4953-b3ec-bbc3123fa7c2</ns2:OfferItemRefID>
						<ns2:PaxRefID>PAX1</ns2:PaxRefID>
						<ns2:SelectedALaCarteOfferItem>
							<ns2:Qty>1</ns2:Qty>
						</ns2:SelectedALaCarteOfferItem>
					</ns2:SelectedOfferItem>
				</ns2:SelectedPricedOffer>
			</ns2:AcceptSelectedQuotedOfferList>
		</ns2:CreateOrder>
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
					<ns2:Remark>
                        <ns2:RemarkText>MC|380012</ns2:RemarkText>
                    </ns2:Remark>
                    <ns2:Birthdate>2001-04-13</ns2:Birthdate>
					<ns2:ContactInfoRefID>CONT1</ns2:ContactInfoRefID>
					<ns2:IdentityDoc>
                        <ns2:IdentityDocID>30000529L</ns2:IdentityDocID>
                        <ns2:IdentityDocTypeCode>ID</ns2:IdentityDocTypeCode>
                    </ns2:IdentityDoc>
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
			</ns2:PaxList>
		</ns2:DataLists>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_OrderCreateRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderViewRS (Hold booking)</b></summary>

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
					<ns3:DatedMarketingSegmentId>DMS25</ns3:DatedMarketingSegmentId>
					<ns3:DatedOperatingSegmentRefId>DOS25</ns3:DatedOperatingSegmentRefId>
					<ns3:Dep>
						<ns3:AircraftScheduledDateTime>2026-03-12T18:30:00.000+01:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>CDG</ns3:IATA_LocationCode>
						<ns3:TerminalName>2C</ns3:TerminalName>
					</ns3:Dep>
					<ns3:MarketingCarrierFlightNumberText>315</ns3:MarketingCarrierFlightNumberText>
				</ns3:DatedMarketingSegment>
				<ns3:DatedMarketingSegment>
					<ns3:Arrival>
						<ns3:AircraftScheduledDateTime>2026-03-19T10:35:00.000+01:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>CDG</ns3:IATA_LocationCode>
						<ns3:TerminalName>2C</ns3:TerminalName>
					</ns3:Arrival>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedMarketingSegmentId>DMS26</ns3:DatedMarketingSegmentId>
					<ns3:DatedOperatingSegmentRefId>DOS26</ns3:DatedOperatingSegmentRefId>
					<ns3:Dep>
						<ns3:AircraftScheduledDateTime>2026-03-19T08:15:00.000+01:00</ns3:AircraftScheduledDateTime>
						<ns3:IATA_LocationCode>LHR</ns3:IATA_LocationCode>
						<ns3:TerminalName>5</ns3:TerminalName>
					</ns3:Dep>
					<ns3:MarketingCarrierFlightNumberText>304</ns3:MarketingCarrierFlightNumberText>
				</ns3:DatedMarketingSegment>
			</ns3:DatedMarketingSegmentList>
			<ns3:DatedOperatingSegmentList>
				<ns3:DatedOperatingSegment>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedOperatingLegRefID>DOL25</ns3:DatedOperatingLegRefID>
					<ns3:DatedOperatingSegmentId>DOS25</ns3:DatedOperatingSegmentId>
					<ns3:Duration>P0Y0M0DT1H15M0S</ns3:Duration>
				</ns3:DatedOperatingSegment>
				<ns3:DatedOperatingSegment>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedOperatingLegRefID>DOL26</ns3:DatedOperatingLegRefID>
					<ns3:DatedOperatingSegmentId>DOS26</ns3:DatedOperatingSegmentId>
					<ns3:Duration>P0Y0M0DT1H20M0S</ns3:Duration>
				</ns3:DatedOperatingSegment>
			</ns3:DatedOperatingSegmentList>
			<ns3:OriginDestList>
				<ns3:OriginDest>
					<ns3:DestCode>LHR</ns3:DestCode>
					<ns3:OriginCode>CDG</ns3:OriginCode>
					<ns3:OriginDestID>OD5</ns3:OriginDestID>
					<ns3:PaxJourneyRefID>PJ17</ns3:PaxJourneyRefID>
				</ns3:OriginDest>
				<ns3:OriginDest>
					<ns3:DestCode>CDG</ns3:DestCode>
					<ns3:OriginCode>LHR</ns3:OriginCode>
					<ns3:OriginDestID>OD6</ns3:OriginDestID>
					<ns3:PaxJourneyRefID>PJ18</ns3:PaxJourneyRefID>
				</ns3:OriginDest>
			</ns3:OriginDestList>
			<ns3:PaxJourneyList>
				<ns3:PaxJourney>
					<ns3:Duration>P0Y0M0DT1H15M0S</ns3:Duration>
					<ns3:PaxJourneyID>PJ17</ns3:PaxJourneyID>
					<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
				</ns3:PaxJourney>
				<ns3:PaxJourney>
					<ns3:Duration>P0Y0M0DT1H20M0S</ns3:Duration>
					<ns3:PaxJourneyID>PJ18</ns3:PaxJourneyID>
					<ns3:PaxSegmentRefID>SEG18</ns3:PaxSegmentRefID>
				</ns3:PaxJourney>
			</ns3:PaxJourneyList>
			<ns3:PaxList>
				<ns3:Pax>
					<ns3:Birthdate>2001-04-13</ns3:Birthdate>
					<ns3:ContactInfoRefID>CONT1</ns3:ContactInfoRefID>
					<ns3:Individual>
						<ns3:GenderCode>M</ns3:GenderCode>
						<ns3:GivenName>John</ns3:GivenName>
						<ns3:Surname>Doe</ns3:Surname>
						<ns3:TitleName>MR</ns3:TitleName>
					</ns3:Individual>
					<ns3:PaxID>PAX1</ns3:PaxID>
					<ns3:PTC>ADT</ns3:PTC>
				</ns3:Pax>
				<ns3:Pax>
					<ns3:Birthdate>2000-11-14</ns3:Birthdate>
					<ns3:ContactInfoRefID>CONT1</ns3:ContactInfoRefID>
					<ns3:Individual>
						<ns3:GenderCode>F</ns3:GenderCode>
						<ns3:GivenName>Jane</ns3:GivenName>
						<ns3:Surname>Doe</ns3:Surname>
						<ns3:TitleName>MRS</ns3:TitleName>
					</ns3:Individual>
					<ns3:PaxID>PAX2</ns3:PaxID>
					<ns3:PTC>ADT</ns3:PTC>
				</ns3:Pax>
			</ns3:PaxList>
			<ns3:PaxSegmentList>
				<ns3:PaxSegment>
					<ns3:DatedMarketingSegmentRefId>DMS25</ns3:DatedMarketingSegmentRefId>
					<ns3:PaxSegmentID>SEG17</ns3:PaxSegmentID>
				</ns3:PaxSegment>
				<ns3:PaxSegment>
					<ns3:DatedMarketingSegmentRefId>DMS26</ns3:DatedMarketingSegmentRefId>
					<ns3:PaxSegmentID>SEG18</ns3:PaxSegmentID>
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
					<ns3:PriceClassID>PC4</ns3:PriceClassID>
				</ns3:PriceClass>
			</ns3:PriceClassList>
		</ns3:DataLists>
		<ns3:Order>
			<ns3:OrderID>599189</ns3:OrderID>
			<ns3:OrderItem>
				<ns3:FareDetail>
					<ns3:FareComponent>
						<ns3:CabinType>
							<ns3:CabinTypeCode>Q</ns3:CabinTypeCode>
							<ns3:CabinTypeName>ECONOMY</ns3:CabinTypeName>
						</ns3:CabinType>
						<ns3:FareTypeCode>70J</ns3:FareTypeCode>
						<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
						<ns3:PaxSegmentRefID>SEG18</ns3:PaxSegmentRefID>
						<ns3:PriceClassRefID>PC4</ns3:PriceClassRefID>
					</ns3:FareComponent>
					<ns3:FarePriceType>
						<ns3:Price>
							<ns3:BaseAmount CurCode="EUR">0.00</ns3:BaseAmount>
							<ns3:TaxSummary>
								<ns3:Tax>
									<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
									<ns3:TaxCode>GENERAL_TAXES_PAX_1</ns3:TaxCode>
									<ns3:TaxName>Taxes - PAX1</ns3:TaxName>
								</ns3:Tax>
								<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
							</ns3:TaxSummary>
							<ns3:TotalAmount CurCode="EUR">113.36</ns3:TotalAmount>
						</ns3:Price>
					</ns3:FarePriceType>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				</ns3:FareDetail>
				<ns3:OrderItemID>a02ffce0-8a6c-55f4-a4eb-319267693964</ns3:OrderItemID>
				<ns3:PaymentTimeLimitDateTime>2026-02-01T04:59:00.000+01:00</ns3:PaymentTimeLimitDateTime>
				<ns3:Price>
					<ns3:BaseAmount CurCode="EUR">0.00</ns3:BaseAmount>
					<ns3:TaxSummary>
						<ns3:Tax>
							<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
							<ns3:TaxCode>GENERAL_TAXES_PAX_1</ns3:TaxCode>
							<ns3:TaxName>Taxes - PAX1</ns3:TaxName>
						</ns3:Tax>
						<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
					</ns3:TaxSummary>
					<ns3:TotalAmount CurCode="EUR">113.36</ns3:TotalAmount>
				</ns3:Price>
				<ns3:PriceGuaranteeTimeLimitDateTime>2026-01-28T22:34:40.000+01:00</ns3:PriceGuaranteeTimeLimitDateTime>
				<ns3:Service>
					<ns3:BookingRef>
						<ns3:BookingEntity>
							<ns3:Carrier>
								<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
							</ns3:Carrier>
						</ns3:BookingEntity>
						<ns3:BookingID>X2YUOK</ns3:BookingID>
					</ns3:BookingRef>
					<ns3:OrderServiceAssociation>
						<ns3:PaxSegmentRef>
							<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:ServiceID>SV741</ns3:ServiceID>
					<ns3:StatusCode>K</ns3:StatusCode>
				</ns3:Service>
				<ns3:Service>
					<ns3:BookingRef>
						<ns3:BookingEntity>
							<ns3:Carrier>
								<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
							</ns3:Carrier>
						</ns3:BookingEntity>
						<ns3:BookingID>X2YUOK</ns3:BookingID>
					</ns3:BookingRef>
					<ns3:OrderServiceAssociation>
						<ns3:PaxSegmentRef>
							<ns3:PaxSegmentRefID>SEG18</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:ServiceID>SV742</ns3:ServiceID>
					<ns3:StatusCode>K</ns3:StatusCode>
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
						<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
						<ns3:PaxSegmentRefID>SEG18</ns3:PaxSegmentRefID>
						<ns3:PriceClassRefID>PC4</ns3:PriceClassRefID>
					</ns3:FareComponent>
					<ns3:FarePriceType>
						<ns3:Price>
							<ns3:BaseAmount CurCode="EUR">0.00</ns3:BaseAmount>
							<ns3:TaxSummary>
								<ns3:Tax>
									<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
									<ns3:TaxCode>GENERAL_TAXES_PAX_2</ns3:TaxCode>
									<ns3:TaxName>Taxes - PAX2</ns3:TaxName>
								</ns3:Tax>
								<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
							</ns3:TaxSummary>
							<ns3:TotalAmount CurCode="EUR">113.36</ns3:TotalAmount>
						</ns3:Price>
					</ns3:FarePriceType>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
				</ns3:FareDetail>
				<ns3:OrderItemID>9b219351-633f-56e2-8654-6b8aa711007a</ns3:OrderItemID>
				<ns3:PaymentTimeLimitDateTime>2026-02-01T04:59:00.000+01:00</ns3:PaymentTimeLimitDateTime>
				<ns3:Price>
					<ns3:BaseAmount CurCode="EUR">0.00</ns3:BaseAmount>
					<ns3:TaxSummary>
						<ns3:Tax>
							<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
							<ns3:TaxCode>GENERAL_TAXES_PAX_2</ns3:TaxCode>
							<ns3:TaxName>Taxes - PAX2</ns3:TaxName>
						</ns3:Tax>
						<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
					</ns3:TaxSummary>
					<ns3:TotalAmount CurCode="EUR">113.36</ns3:TotalAmount>
				</ns3:Price>
				<ns3:PriceGuaranteeTimeLimitDateTime>2026-01-28T22:34:40.000+01:00</ns3:PriceGuaranteeTimeLimitDateTime>
				<ns3:Service>
					<ns3:BookingRef>
						<ns3:BookingEntity>
							<ns3:Carrier>
								<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
							</ns3:Carrier>
						</ns3:BookingEntity>
						<ns3:BookingID>X2YUOK</ns3:BookingID>
					</ns3:BookingRef>
					<ns3:OrderServiceAssociation>
						<ns3:PaxSegmentRef>
							<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
					<ns3:ServiceID>SV743</ns3:ServiceID>
					<ns3:StatusCode>K</ns3:StatusCode>
				</ns3:Service>
				<ns3:Service>
					<ns3:BookingRef>
						<ns3:BookingEntity>
							<ns3:Carrier>
								<ns3:AirlineDesigCode>BA</ns3:AirlineDesigCode>
							</ns3:Carrier>
						</ns3:BookingEntity>
						<ns3:BookingID>X2YUOK</ns3:BookingID>
					</ns3:BookingRef>
					<ns3:OrderServiceAssociation>
						<ns3:PaxSegmentRef>
							<ns3:PaxSegmentRefID>SEG18</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
					<ns3:ServiceID>SV744</ns3:ServiceID>
					<ns3:StatusCode>K</ns3:StatusCode>
				</ns3:Service>
			</ns3:OrderItem>
			<ns3:OwnerCode>BA</ns3:OwnerCode>
			<ns3:StatusCode>OPENED</ns3:StatusCode>
			<ns3:TotalPrice>
				<ns3:BaseAmount CurCode="EUR">0.00</ns3:BaseAmount>
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
					<ns3:TotalTaxAmount CurCode="EUR">226.72</ns3:TotalTaxAmount>
				</ns3:TaxSummary>
				<ns3:TotalAmount CurCode="EUR">226.72</ns3:TotalAmount>
			</ns3:TotalPrice>
		</ns3:Order>
		<ns3:Warning>
			<ns3:Code>SUPPLIER_WARN</ns3:Code>
			<ns3:DescText>All services may not be delivered as the requested fare component may include a codeshare flight or an interline itinerary.</ns3:DescText>
			<ns3:OwnerName>BRITISH_AIRWAYS</ns3:OwnerName>
		</ns3:Warning>
	</ns4:Response>
	<ns4:PayloadAttributes>
		<ns3:CorrelationID>9f0b62c8-a376-3181-8f2e-bff97798861c</ns3:CorrelationID>
		<ns3:Timestamp>2026-01-28T17:34:43.791+01:00</ns3:Timestamp>
		<ns3:VersionNumber>21.3</ns3:VersionNumber>
	</ns4:PayloadAttributes>
</ns4:IATA_OrderViewRS>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderViewRS (Instant payment)</b></summary>

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
					<ns3:DatedMarketingSegmentId>DMS25</ns3:DatedMarketingSegmentId>
					<ns3:DatedOperatingSegmentRefId>DOS25</ns3:DatedOperatingSegmentRefId>
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
					<ns3:DatedMarketingSegmentId>DMS26</ns3:DatedMarketingSegmentId>
					<ns3:DatedOperatingSegmentRefId>DOS26</ns3:DatedOperatingSegmentRefId>
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
					<ns3:DatedOperatingLegRefID>DOL25</ns3:DatedOperatingLegRefID>
					<ns3:DatedOperatingSegmentId>DOS25</ns3:DatedOperatingSegmentId>
					<ns3:Duration>P0Y0M0DT1H15M0S</ns3:Duration>
				</ns3:DatedOperatingSegment>
				<ns3:DatedOperatingSegment>
					<ns3:CarrierDesigCode>BA</ns3:CarrierDesigCode>
					<ns3:DatedOperatingLegRefID>DOL26</ns3:DatedOperatingLegRefID>
					<ns3:DatedOperatingSegmentId>DOS26</ns3:DatedOperatingSegmentId>
					<ns3:Duration>P0Y0M0DT1H20M0S</ns3:Duration>
				</ns3:DatedOperatingSegment>
			</ns3:DatedOperatingSegmentList>
			<ns3:OriginDestList>
				<ns3:OriginDest>
					<ns3:DestCode>LHR</ns3:DestCode>
					<ns3:OriginCode>CDG</ns3:OriginCode>
					<ns3:OriginDestID>OD5</ns3:OriginDestID>
					<ns3:PaxJourneyRefID>PJ17</ns3:PaxJourneyRefID>
				</ns3:OriginDest>
				<ns3:OriginDest>
					<ns3:DestCode>CDG</ns3:DestCode>
					<ns3:OriginCode>LHR</ns3:OriginCode>
					<ns3:OriginDestID>OD6</ns3:OriginDestID>
					<ns3:PaxJourneyRefID>PJ18</ns3:PaxJourneyRefID>
				</ns3:OriginDest>
			</ns3:OriginDestList>
			<ns3:PaxJourneyList>
				<ns3:PaxJourney>
					<ns3:Duration>P0Y0M0DT1H15M0S</ns3:Duration>
					<ns3:PaxJourneyID>PJ17</ns3:PaxJourneyID>
					<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
				</ns3:PaxJourney>
				<ns3:PaxJourney>
					<ns3:Duration>P0Y0M0DT1H20M0S</ns3:Duration>
					<ns3:PaxJourneyID>PJ18</ns3:PaxJourneyID>
					<ns3:PaxSegmentRefID>SEG18</ns3:PaxSegmentRefID>
				</ns3:PaxJourney>
			</ns3:PaxJourneyList>
			<ns3:PaxList>
				<ns3:Pax>
					<ns3:Birthdate>2001-04-13</ns3:Birthdate>
					<ns3:ContactInfoRefID>CONT1</ns3:ContactInfoRefID>
					<ns3:Individual>
						<ns3:GenderCode>M</ns3:GenderCode>
						<ns3:GivenName>John</ns3:GivenName>
						<ns3:Surname>Doe</ns3:Surname>
						<ns3:TitleName>MR</ns3:TitleName>
					</ns3:Individual>
					<ns3:PaxID>PAX1</ns3:PaxID>
					<ns3:PTC>ADT</ns3:PTC>
				</ns3:Pax>
				<ns3:Pax>
					<ns3:Birthdate>2000-11-14</ns3:Birthdate>
					<ns3:ContactInfoRefID>CONT1</ns3:ContactInfoRefID>
					<ns3:Individual>
						<ns3:GenderCode>F</ns3:GenderCode>
						<ns3:GivenName>Jane</ns3:GivenName>
						<ns3:Surname>Doe</ns3:Surname>
						<ns3:TitleName>MRS</ns3:TitleName>
					</ns3:Individual>
					<ns3:PaxID>PAX2</ns3:PaxID>
					<ns3:PTC>ADT</ns3:PTC>
				</ns3:Pax>
			</ns3:PaxList>
			<ns3:PaxSegmentList>
				<ns3:PaxSegment>
					<ns3:DatedMarketingSegmentRefId>DMS25</ns3:DatedMarketingSegmentRefId>
					<ns3:PaxSegmentID>SEG17</ns3:PaxSegmentID>
				</ns3:PaxSegment>
				<ns3:PaxSegment>
					<ns3:DatedMarketingSegmentRefId>DMS26</ns3:DatedMarketingSegmentRefId>
					<ns3:PaxSegmentID>SEG18</ns3:PaxSegmentID>
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
					<ns3:Desc>
						<ns3:DescText>1 Bagage supplémentaire</ns3:DescText>
					</ns3:Desc>
					<ns3:Desc>
						<ns3:DescText>Aller (adulte 1)</ns3:DescText>
					</ns3:Desc>
					<ns3:Name>1 Bagage supplémentaire</ns3:Name>
					<ns3:ServiceDefinitionID>SD11</ns3:ServiceDefinitionID>
				</ns3:ServiceDefinition>
			</ns3:ServiceDefinitionList>
		</ns3:DataLists>
		<ns3:Order>
			<ns3:OrderID>599187</ns3:OrderID>
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
				<ns3:OrderItemID>a8a08340-069b-5dbd-8912-176262062c34</ns3:OrderItemID>
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
									<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
								</ns3:PaxSegmentRef>
							</ns3:OrderFlightAssociations>
							<ns3:ServiceDefinitionRefID>SD11</ns3:ServiceDefinitionRefID>
						</ns3:ServiceDefinitionRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:ServiceID>SV727</ns3:ServiceID>
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
						<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
						<ns3:PaxSegmentRefID>SEG18</ns3:PaxSegmentRefID>
						<ns3:PriceClassRefID>PC5</ns3:PriceClassRefID>
					</ns3:FareComponent>
					<ns3:FarePriceType>
						<ns3:Price>
							<ns3:BaseAmount CurCode="EUR">0.00</ns3:BaseAmount>
							<ns3:TaxSummary>
								<ns3:Tax>
									<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
									<ns3:TaxCode>GENERAL_TAXES_PAX_1</ns3:TaxCode>
									<ns3:TaxName>Taxes - PAX1</ns3:TaxName>
								</ns3:Tax>
								<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
							</ns3:TaxSummary>
							<ns3:TotalAmount CurCode="EUR">113.36</ns3:TotalAmount>
						</ns3:Price>
					</ns3:FarePriceType>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
				</ns3:FareDetail>
				<ns3:OrderItemID>f515684e-1618-566b-b14e-8d479bde327d</ns3:OrderItemID>
				<ns3:Price>
					<ns3:BaseAmount CurCode="EUR">0.00</ns3:BaseAmount>
					<ns3:TaxSummary>
						<ns3:Tax>
							<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
							<ns3:TaxCode>GENERAL_TAXES_PAX_1</ns3:TaxCode>
							<ns3:TaxName>Taxes - PAX1</ns3:TaxName>
						</ns3:Tax>
						<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
					</ns3:TaxSummary>
					<ns3:TotalAmount CurCode="EUR">113.36</ns3:TotalAmount>
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
							<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:ServiceID>SV728</ns3:ServiceID>
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
							<ns3:PaxSegmentRefID>SEG18</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX1</ns3:PaxRefID>
					<ns3:ServiceID>SV729</ns3:ServiceID>
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
						<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
						<ns3:PaxSegmentRefID>SEG18</ns3:PaxSegmentRefID>
						<ns3:PriceClassRefID>PC5</ns3:PriceClassRefID>
					</ns3:FareComponent>
					<ns3:FarePriceType>
						<ns3:Price>
							<ns3:BaseAmount CurCode="EUR">0.00</ns3:BaseAmount>
							<ns3:TaxSummary>
								<ns3:Tax>
									<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
									<ns3:TaxCode>GENERAL_TAXES_PAX_2</ns3:TaxCode>
									<ns3:TaxName>Taxes - PAX2</ns3:TaxName>
								</ns3:Tax>
								<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
							</ns3:TaxSummary>
							<ns3:TotalAmount CurCode="EUR">113.36</ns3:TotalAmount>
						</ns3:Price>
					</ns3:FarePriceType>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
				</ns3:FareDetail>
				<ns3:OrderItemID>e21e7267-410b-5a4d-bf8e-94a555eacd10</ns3:OrderItemID>
				<ns3:Price>
					<ns3:BaseAmount CurCode="EUR">0.00</ns3:BaseAmount>
					<ns3:TaxSummary>
						<ns3:Tax>
							<ns3:Amount CurCode="EUR">113.36</ns3:Amount>
							<ns3:TaxCode>GENERAL_TAXES_PAX_2</ns3:TaxCode>
							<ns3:TaxName>Taxes - PAX2</ns3:TaxName>
						</ns3:Tax>
						<ns3:TotalTaxAmount CurCode="EUR">113.36</ns3:TotalTaxAmount>
					</ns3:TaxSummary>
					<ns3:TotalAmount CurCode="EUR">113.36</ns3:TotalAmount>
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
							<ns3:PaxSegmentRefID>SEG17</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
					<ns3:ServiceID>SV730</ns3:ServiceID>
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
							<ns3:PaxSegmentRefID>SEG18</ns3:PaxSegmentRefID>
						</ns3:PaxSegmentRef>
					</ns3:OrderServiceAssociation>
					<ns3:PaxRefID>PAX2</ns3:PaxRefID>
					<ns3:ServiceID>SV731</ns3:ServiceID>
					<ns3:StatusCode>T</ns3:StatusCode>
				</ns3:Service>
			</ns3:OrderItem>
			<ns3:OwnerCode>BA</ns3:OwnerCode>
			<ns3:StatusCode>OPENED</ns3:StatusCode>
			<ns3:TotalPrice>
				<ns3:BaseAmount CurCode="EUR">62.50</ns3:BaseAmount>
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
					<ns3:TotalTaxAmount CurCode="EUR">226.72</ns3:TotalTaxAmount>
				</ns3:TaxSummary>
				<ns3:TotalAmount CurCode="EUR">289.22</ns3:TotalAmount>
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
			<ns3:PaymentRefID>PAY1</ns3:PaymentRefID>
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
			<ns3:PaymentRefID>PAY1</ns3:PaymentRefID>
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
			<ns3:OwnerName>BRITISH_AIRWAYS</ns3:OwnerName>
		</ns3:Warning>
		<ns3:Warning>
			<ns3:Code>362</ns3:Code>
			<ns3:DescText>NDC_ORDERCREATE_9071 - We are unable to display your booking, but it has been created successfully. Please retrieve it using the reference [X2UY9C].</ns3:DescText>
			<ns3:OwnerName>BRITISH_AIRWAYS</ns3:OwnerName>
		</ns3:Warning>
		<ns3:Warning>
			<ns3:Code>362</ns3:Code>
			<ns3:DescText>Le PNR et/ou la liste de noms ne peuvent pas être affichés</ns3:DescText>
			<ns3:OwnerName>BRITISH_AIRWAYS</ns3:OwnerName>
		</ns3:Warning>
	</ns4:Response>
	<ns4:PayloadAttributes>
		<ns3:CorrelationID>97a37f8d-96bd-33ac-a15e-dfc529bdfc32</ns3:CorrelationID>
		<ns3:Timestamp>2026-01-28T17:04:48.223+01:00</ns3:Timestamp>
		<ns3:VersionNumber>21.3</ns3:VersionNumber>
	</ns4:PayloadAttributes>
	<ns4:PaymentFunctions>
		<ns3:PaymentProcessingSummary>
			<ns3:Amount CurCode="EUR">371.72000000000000000000</ns3:Amount>
			<ns3:PaymentID>PAY1</ns3:PaymentID>
			<ns3:PaymentProcessingSummaryPaymentMethod>
				<ns3:SettlementPlan>
					<ns3:PaymentTypeCode>Cash</ns3:PaymentTypeCode>
				</ns3:SettlementPlan>
			</ns3:PaymentProcessingSummaryPaymentMethod>
		</ns3:PaymentProcessingSummary>
	</ns4:PaymentFunctions>
</ns4:IATA_OrderViewRS>
{% endhighlight %}

</details>
