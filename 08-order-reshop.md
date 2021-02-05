---
layout: page
title:  "Order Reshop"
nav_order: 8
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
| Party | Must contain agency ID as sender | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderRefID | ID of the order to reshop | Mandatory |
| UpdateOrder | The update information to quote, detailed [below](#updateorder) | Optional |

### UpdateOrder
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| ReshopOrder/ServiceOrder/DeleteOrderItem | Indicates to retrieve cancellation fee (OrderItemRefID can contain the order ID) | Mandatory |

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
<IATA_OrderReshopRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderReshopRQ">
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
        <OrderRefID>544759</OrderRefID>
        <UpdateOrder>
            <ReshopOrder>
                <ServiceOrder>
                    <DeleteOrderItem>
                        <OrderItemRefID>544759</OrderItemRefID>
                    </DeleteOrderItem>
                </ServiceOrder>
            </ReshopOrder>
        </UpdateOrder>
    </Request>
</IATA_OrderReshopRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderReshopRS - Cancellation Fee</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderReshopRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderReshopRS">
    <Response>
        <DataLists>
            <PenaltyList>
                <Penalty>
                    <CancelFeeInd>true</CancelFeeInd>
                    <PenaltyID>PNL1</PenaltyID>
                    <Price>
                        <TotalAmount>0</TotalAmount>
                    </Price>
                    <TypeCode>Cancellation</TypeCode>
                </Penalty>
            </PenaltyList>
        </DataLists>
        <ReshopResults>
            <ReshopOffers>
                <Offer>
                    <OfferID>5185fe97-0eaa-42fc-96f8-00dfcf7331f0</OfferID>
                    <PenaltyRefID>PNL1</PenaltyRefID>
                </Offer>
            </ReshopOffers>
        </ReshopResults>
    </Response>
    <PayloadAttributes>
        <CorrelationID>c3421ac5-96cd-3aed-b40b-aca63b056173</CorrelationID>
        <Timestamp>2021-02-04T10:27:23.022+01:00</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_OrderReshopRS>
{% endhighlight %}

</details>
