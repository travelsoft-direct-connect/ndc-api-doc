---
layout: page
title:  "Order Reshop"
parent: NDC 21.3
nav_order: 7
---

# OrderReshop operation
{: .no_toc }
The order reshop method allows to quote order updates. For example, to cancel an order, the order reshop method must be called before to get cancellation fee (returns 0 if void allowed).

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

# OrderReshopRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| DistributionChain | Must contain agency ID as Seller | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderRefID | ID of the order to reshop | Mandatory |

# OrderReshopRS

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Response | The response element detailed [below](#response) | Mandatory |
| PayloadAttributes | Same as requested + timestamp | Mandatory |

## Response
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| DataLists | The response data lists (penalty list, etc) | Mandatory |
| ReshopResults | The reshop results detailed [below](#reshopresults) | Mandatory |
| Warnings | List of warnings returned by provider | Optional |

### ReshopResults
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| ReshopOffers | List of reshop offers detailed [below](#offer) | Mandatory |

#### Offer
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferID | ID of the reshop offer | Mandatory |
| PenaltyRefID | Reference to the penalty data to get in DataLists (contains penalty amount, type code, etc). | Mandatory |

# Samples

<details>
  <summary><b>OrderReshopRQ - Cancellation Fee</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderReshopRQ xmlns="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns2="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns3="http://www.w3.org/2000/09/xmldsig#" xmlns:ns4="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns5="http://www.travelsoft.fr/orchestra/ndc/login">
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
		<ns2:OrderRefID>599187</ns2:OrderRefID>
		<ns2:ResponseParameters>
			<ns2:CurParameter>
				<ns2:CurCode>EUR</ns2:CurCode>
			</ns2:CurParameter>
			<ns2:LangUsage>
				<ns2:LangCode>fr-FR</ns2:LangCode>
			</ns2:LangUsage>
		</ns2:ResponseParameters>
	</Request>
</IATA_OrderReshopRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderReshopRS - Cancellation Fee</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ns4:IATA_OrderReshopRS xmlns="http://www.travelsoft.fr/orchestra/ndc/headers" xmlns:ns2="http://www.travelsoft.fr/orchestra/ndc/login" xmlns:ns3="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersCommonTypes" xmlns:ns4="http://www.iata.org/IATA/2015/EASD/00/IATA_OffersAndOrdersMessage" xmlns:ns5="http://www.w3.org/2000/09/xmldsig#">
	<ns4:Response>
		<ns3:DataLists>
			<ns3:PenaltyList>
				<ns3:Penalty>
					<ns3:PenaltyID>PNL1</ns3:PenaltyID>
					<ns3:Price>
						<ns3:TotalAmount CurCode="EUR">0.00</ns3:TotalAmount>
					</ns3:Price>
					<ns3:TypeCode>Cancellation</ns3:TypeCode>
				</ns3:Penalty>
			</ns3:PenaltyList>
		</ns3:DataLists>
		<ns3:ReshopResults>
			<ns3:ReshopOffers>
				<ns3:Offer>
					<ns3:OfferID>60f4ffa6-5889-418e-9db9-a59ab7a4e80b</ns3:OfferID>
					<ns3:OwnerCode>BA</ns3:OwnerCode>
					<ns3:PenaltyRefID>PNL1</ns3:PenaltyRefID>
				</ns3:Offer>
			</ns3:ReshopOffers>
		</ns3:ReshopResults>
	</ns4:Response>
	<ns4:PayloadAttributes>
		<ns3:CorrelationID>09401279-fb44-3766-bc60-62c3b45702db</ns3:CorrelationID>
		<ns3:Timestamp>2026-01-28T17:17:21.790+01:00</ns3:Timestamp>
		<ns3:VersionNumber>21.3</ns3:VersionNumber>
	</ns4:PayloadAttributes>
</ns4:IATA_OrderReshopRS>
{% endhighlight %}

</details>
