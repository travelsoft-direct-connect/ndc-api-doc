---
layout: page
title:  "Order Cancel"
nav_order: 10
---

# OrderCancel operation
{: .no_toc }
The order cancel method allows to cancel/refund/void an order for all passengers in the airline system. The order reshop method must be called before to retrieve potential cancellation fee.

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

# OrderCancelRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Party | Must contain agency ID as sender | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Order | The order detailed [below](#order) | Mandatory |

### Order
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderID | ID of the order to cancel | Mandatory |

# OrderCancelRS

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Response | The response element detailed [below](#response) | Mandatory |
| PayloadAttributes | Same as requested + timestamp | Mandatory |

## Response
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| ChangeFees | The cancel fee detailed [below](#changefees) | Mandatory |
| OrderRefID | The order ID (same as requested) | Mandatory |
| Warnings | List of warnings returned by provider | Optional |

### ChangeFees
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Price | The cancellation fee amount | Mandatory |
| TypeCode | Type "Cancellation" | Mandatory |

# Samples

<details>
  <summary><b>OrderCancelRQ</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderCancelRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderCancelRQ">
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
        <Order>
            <OrderID>544759</OrderID>
        </Order>
    </Request>
</IATA_OrderCancelRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderCancelRS</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderCancelRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderCancelRS">
    <Response>
        <ChangeFees>
            <CancelFeeInd>true</CancelFeeInd>
            <PenaltyID>PNL1</PenaltyID>
            <Price>
                <TotalAmount>0</TotalAmount>
            </Price>
            <TypeCode>Cancellation</TypeCode>
        </ChangeFees>
        <OrderRefID>544759</OrderRefID>
    </Response>
    <PayloadAttributes>
        <CorrelationID>c3421ac5-96cd-3aed-b40b-aca63b056173</CorrelationID>
        <Timestamp>2021-02-04T10:28:03.006+01:00</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_OrderCancelRS>
{% endhighlight %}

</details>
