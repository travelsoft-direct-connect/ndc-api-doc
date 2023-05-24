---
layout: page
title:  "Service List"
nav_order: 4
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
| Party | Must contain agency ID as sender | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| CoreRequest | Must contain the flight offer selected | Mandatory |
| Paxs | List of passengers (same as AirShoppingRQ) | Mandatory |
| ShoppingResponse | The Shopping session ID from previous request | Mandatory |

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
| ShoppingResponse | The Shopping session ID to use for next requests | Mandatory |
| DataLists | The response data lists (service definitions, etc) | Mandatory |
| ALaCarteOffer | The offer element detailed [below](#alacarteoffer) | Mandatory |

### ALaCarteOffer
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferID | The offer ID | Mandatory |
| ALaCarteOfferItems | List of offer items detailed [below](#alacarteofferitem) | Mandatory |

#### ALaCarteOfferItem
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
<IATA_ServiceListRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_ServiceListRQ">
    <PayloadAttributes>
        <CorrelationID>a222c960-0d2c-4507-bd2c-59362825cc76</CorrelationID>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <Request>
        <CoreRequest>
            <Offer>
                <OfferID>23bedc85-dd6a-482b-ac29-2f0c608ed478</OfferID>
                <OfferItem>
                    <OfferItemID>e99b73dc-16a1-4b9b-a8bc-f26b6299f5bb</OfferItemID>
                </OfferItem>
                <OwnerCode>AF</OwnerCode>
            </Offer>
        </CoreRequest>
        <Pax>
            <PaxID>PAX1</PaxID>
            <PTC>ADT</PTC>
        </Pax>
        <Pax>
            <PaxID>PAX2</PaxID>
            <PTC>ADT</PTC>
        </Pax>
        <ShoppingResponse>
            <ShoppingResponseRefID>2d62d243-8837-4e4d-a91c-45550a2fd6fa</ShoppingResponseRefID>
        </ShoppingResponse>
    </Request>
</IATA_ServiceListRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>ServiceListRS</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_ServiceListRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_ServiceListRS">
    <Response>
        <ALaCarteOffer>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>353e5fce-965f-4fc5-ad68-c4b430b87ad4</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD1</ServiceDefinitionRefID>
                    <ServiceID>SV326</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>06c6e0f8-1f4a-41f7-9634-ed971bce8069</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD2</ServiceDefinitionRefID>
                    <ServiceID>SV327</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">130.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">130.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>97504622-8c4e-449f-9049-27928bdd02aa</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD3</ServiceDefinitionRefID>
                    <ServiceID>SV328</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">290.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">290.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>1bdba8b3-bd36-4551-b983-37258f22beb6</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD4</ServiceDefinitionRefID>
                    <ServiceID>SV329</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">450.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">450.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>52ddadb9-1d9e-45ad-bc25-0cda3aeab76d</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD5</ServiceDefinitionRefID>
                    <ServiceID>SV330</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">610.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">610.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>fbf6d504-cecf-4a00-9113-86027648c56d</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD6</ServiceDefinitionRefID>
                    <ServiceID>SV331</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">770.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">770.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>0718422b-9628-49db-a93a-eade9f5612fc</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD7</ServiceDefinitionRefID>
                    <ServiceID>SV332</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">930.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">930.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>1af04a04-f9f7-43ce-9f3c-ef88e3a81b9f</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD8</ServiceDefinitionRefID>
                    <ServiceID>SV333</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">1090.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">1090.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>d282deac-e2f6-4143-8b28-b4714ed2b0d3</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD1</ServiceDefinitionRefID>
                    <ServiceID>SV334</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>5b83e206-2047-4f22-a81d-a5f8aeb025ae</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD2</ServiceDefinitionRefID>
                    <ServiceID>SV335</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">130.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">130.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>983effdc-5d00-479e-a76d-de676cdb7592</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD3</ServiceDefinitionRefID>
                    <ServiceID>SV336</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">290.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">290.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>ec0cdfea-a5bb-4d63-9650-239644c35e8a</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD4</ServiceDefinitionRefID>
                    <ServiceID>SV337</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">450.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">450.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>10e3fea8-a90a-4dc5-86fd-9b1a9a587339</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD5</ServiceDefinitionRefID>
                    <ServiceID>SV338</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">610.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">610.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>9f3e46c5-8662-42b5-991b-a724b57a8fd2</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD6</ServiceDefinitionRefID>
                    <ServiceID>SV339</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">770.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">770.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>9a9a6122-37b6-4a89-b5a8-211f31873a29</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD7</ServiceDefinitionRefID>
                    <ServiceID>SV340</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">930.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">930.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>43f68400-e806-4a03-a2fa-3231e25751db</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD8</ServiceDefinitionRefID>
                    <ServiceID>SV341</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">1090.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">1090.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>463d2c99-fe21-40b4-9cf4-feb29da73f4b</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD1</ServiceDefinitionRefID>
                    <ServiceID>SV342</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>fee8dbfb-706e-4017-928f-8e863ebe8d4e</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD2</ServiceDefinitionRefID>
                    <ServiceID>SV343</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">130.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">130.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>41bd113c-a002-4ec6-8f29-67fe29ff63e5</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD3</ServiceDefinitionRefID>
                    <ServiceID>SV344</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">290.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">290.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>ca08673a-eee0-4eb2-aa70-ebf2bcdeb9b5</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD4</ServiceDefinitionRefID>
                    <ServiceID>SV345</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">450.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">450.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>86464f9d-4a1c-4b19-bed4-b070e4f0cecb</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD5</ServiceDefinitionRefID>
                    <ServiceID>SV346</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">610.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">610.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>24b276e9-0632-48b0-94f9-a547b9772b3f</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD6</ServiceDefinitionRefID>
                    <ServiceID>SV347</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">770.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">770.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>2b391c69-3bfe-4a71-b063-d8ef9203fcbe</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD7</ServiceDefinitionRefID>
                    <ServiceID>SV348</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">930.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">930.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>0b987f63-b7d4-46f4-8c19-e9b4e5066a8e</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD8</ServiceDefinitionRefID>
                    <ServiceID>SV349</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">1090.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">1090.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>0adbae72-c377-495a-aef8-101bb692c4e7</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD1</ServiceDefinitionRefID>
                    <ServiceID>SV350</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>afed23a0-50f7-461c-ac7d-74dcbee31def</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD2</ServiceDefinitionRefID>
                    <ServiceID>SV351</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">130.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">130.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>7de2b832-1b4f-4200-87df-980549536913</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD3</ServiceDefinitionRefID>
                    <ServiceID>SV352</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">290.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">290.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>1b9a853a-cc07-448a-b1ae-8cbe84b22360</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD4</ServiceDefinitionRefID>
                    <ServiceID>SV353</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">450.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">450.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>194e73f6-4b50-4914-9c9f-6de35bcfb639</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD5</ServiceDefinitionRefID>
                    <ServiceID>SV354</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">610.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">610.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>faba21f8-6e3b-4847-9ce2-26e69d3e65e8</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD6</ServiceDefinitionRefID>
                    <ServiceID>SV355</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">770.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">770.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>7fbe6e2b-3704-4f4a-bec8-81bbbf4725f5</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD7</ServiceDefinitionRefID>
                    <ServiceID>SV356</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">930.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">930.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>86222a05-427e-47de-a6e3-83a31155200b</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD8</ServiceDefinitionRefID>
                    <ServiceID>SV357</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">1090.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">1090.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>ab72d59c-bdc1-46bc-ad53-b9902fe769b1</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD9</ServiceDefinitionRefID>
                    <ServiceID>SV358</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>213b7108-e69f-4bb1-8ea8-d1ded05b57d7</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD10</ServiceDefinitionRefID>
                    <ServiceID>SV359</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>e725f0e1-acdd-4b1a-92f6-70f557b5db09</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD11</ServiceDefinitionRefID>
                    <ServiceID>SV360</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>82d4430d-f632-44d0-bcf5-99bee5ff1218</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD12</ServiceDefinitionRefID>
                    <ServiceID>SV361</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>f867913e-d5bc-4961-820e-0d4866d221ef</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD13</ServiceDefinitionRefID>
                    <ServiceID>SV362</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>09f22af5-1908-4171-8194-ee87f50ce352</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD14</ServiceDefinitionRefID>
                    <ServiceID>SV363</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>78cc2e3c-7b24-491e-b35c-28da2e2ebf8c</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD15</ServiceDefinitionRefID>
                    <ServiceID>SV364</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>a279c063-8f7f-4cab-b5a7-8286d0516cda</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD16</ServiceDefinitionRefID>
                    <ServiceID>SV365</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>dd294806-bf65-42aa-983a-680ae5a5146e</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD17</ServiceDefinitionRefID>
                    <ServiceID>SV366</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>84881d41-0f59-4019-95c5-e26b47aa22e0</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD18</ServiceDefinitionRefID>
                    <ServiceID>SV367</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>060c5df8-da94-4780-81e2-181304a4fa44</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD19</ServiceDefinitionRefID>
                    <ServiceID>SV368</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>b16dc605-fbb7-4a54-b4e6-7b9d5b1d9c9d</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD9</ServiceDefinitionRefID>
                    <ServiceID>SV369</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>25d4d7ad-69ed-46a4-8a33-f44244837583</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD10</ServiceDefinitionRefID>
                    <ServiceID>SV370</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>328c13f3-bee1-4166-a52c-55ba742b7801</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD11</ServiceDefinitionRefID>
                    <ServiceID>SV371</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>02353dc0-894b-4d2b-bcac-ccd724539f16</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD12</ServiceDefinitionRefID>
                    <ServiceID>SV372</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>30407ecc-9460-49d5-819c-0353fc752785</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD13</ServiceDefinitionRefID>
                    <ServiceID>SV373</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>faff7c61-a913-4cfc-b41f-28e5489dae3a</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD14</ServiceDefinitionRefID>
                    <ServiceID>SV374</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>22b32be7-05a2-4c79-855c-9131cac34a0b</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD15</ServiceDefinitionRefID>
                    <ServiceID>SV375</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>df4acf3c-1c07-4bae-97d7-c6aced70f712</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD16</ServiceDefinitionRefID>
                    <ServiceID>SV376</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>0dc597d0-2ed7-41d7-b436-013308669f56</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD17</ServiceDefinitionRefID>
                    <ServiceID>SV377</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>f4b15992-4f18-4037-a87c-987ab67b68bd</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD18</ServiceDefinitionRefID>
                    <ServiceID>SV378</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>b1c456b2-bb54-4ecd-a718-89066de6b600</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD19</ServiceDefinitionRefID>
                    <ServiceID>SV379</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>7268f26e-5f83-4516-b507-68b97f7dfd55</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD9</ServiceDefinitionRefID>
                    <ServiceID>SV380</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>49c89b90-2202-4fcf-a42a-4ee6ec1deb43</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD13</ServiceDefinitionRefID>
                    <ServiceID>SV381</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>a8aaf1b7-bd49-43ab-a0a5-878631e36fde</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD15</ServiceDefinitionRefID>
                    <ServiceID>SV382</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>f2380eb4-805e-4c78-816c-69d56e27e9d0</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD18</ServiceDefinitionRefID>
                    <ServiceID>SV383</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>1ebefb63-e4a7-4e31-a72d-043f4d236443</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD9</ServiceDefinitionRefID>
                    <ServiceID>SV384</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>2574b6fb-ab8d-4b49-9c50-b53a28077695</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD13</ServiceDefinitionRefID>
                    <ServiceID>SV385</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>d95afa73-2266-499f-adc9-113bb7bd0c29</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD15</ServiceDefinitionRefID>
                    <ServiceID>SV386</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>f4ec09c3-eee4-4c4e-8048-8a5603fc8259</OfferItemID>
                <Service>
                    <ServiceDefinitionRefID>SD18</ServiceDefinitionRefID>
                    <ServiceID>SV387</ServiceID>
                </Service>
                <UnitPrice>
                    <BaseAmount CurCode="EUR">0.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">0.00000000000000000000</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <OfferID>2076a058-e502-44ab-94b1-80b3e1ef8bcd</OfferID>
        </ALaCarteOffer>
        <DataLists>
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
                <ServiceDefinition>
                    <ServiceTaxonomy>
                        <DescText>Checked Baggage</DescText>
                        <TaxonomyCode>13EC</TaxonomyCode>
                    </ServiceTaxonomy>
                    <Desc>
                        <DescText>2 luggage items</DescText>
                    </Desc>
                    <Name>2 luggage items</Name>
                    <ServiceDefinitionID>SD2</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <ServiceTaxonomy>
                        <DescText>Checked Baggage</DescText>
                        <TaxonomyCode>13EC</TaxonomyCode>
                    </ServiceTaxonomy>
                    <Desc>
                        <DescText>3 luggage items</DescText>
                    </Desc>
                    <Name>3 luggage items</Name>
                    <ServiceDefinitionID>SD3</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <ServiceTaxonomy>
                        <DescText>Checked Baggage</DescText>
                        <TaxonomyCode>13EC</TaxonomyCode>
                    </ServiceTaxonomy>
                    <Desc>
                        <DescText>4 luggage items</DescText>
                    </Desc>
                    <Name>4 luggage items</Name>
                    <ServiceDefinitionID>SD4</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <ServiceTaxonomy>
                        <DescText>Checked Baggage</DescText>
                        <TaxonomyCode>13EC</TaxonomyCode>
                    </ServiceTaxonomy>
                    <Desc>
                        <DescText>5 luggage items</DescText>
                    </Desc>
                    <Name>5 luggage items</Name>
                    <ServiceDefinitionID>SD5</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <ServiceTaxonomy>
                        <DescText>Checked Baggage</DescText>
                        <TaxonomyCode>13EC</TaxonomyCode>
                    </ServiceTaxonomy>
                    <Desc>
                        <DescText>6 luggage items</DescText>
                    </Desc>
                    <Name>6 luggage items</Name>
                    <ServiceDefinitionID>SD6</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <ServiceTaxonomy>
                        <DescText>Checked Baggage</DescText>
                        <TaxonomyCode>13EC</TaxonomyCode>
                    </ServiceTaxonomy>
                    <Desc>
                        <DescText>7 luggage items</DescText>
                    </Desc>
                    <Name>7 luggage items</Name>
                    <ServiceDefinitionID>SD7</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <ServiceTaxonomy>
                        <DescText>Checked Baggage</DescText>
                        <TaxonomyCode>13EC</TaxonomyCode>
                    </ServiceTaxonomy>
                    <Desc>
                        <DescText>8 luggage items</DescText>
                    </Desc>
                    <Name>8 luggage items</Name>
                    <ServiceDefinitionID>SD8</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Asian vegetarian meal</DescText>
                    </Desc>
                    <Name>Asian vegetarian meal</Name>
                    <ServiceDefinitionID>SD9</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Infant/baby food</DescText>
                    </Desc>
                    <Name>Infant/baby food</Name>
                    <ServiceDefinitionID>SD10</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Bland meal</DescText>
                    </Desc>
                    <Name>Bland meal</Name>
                    <ServiceDefinitionID>SD11</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Diabetic meal</DescText>
                    </Desc>
                    <Name>Diabetic meal</Name>
                    <ServiceDefinitionID>SD12</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Gluten-free meal</DescText>
                    </Desc>
                    <Name>Gluten-free meal</Name>
                    <ServiceDefinitionID>SD13</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Hindu (non vegetarian) meal</DescText>
                    </Desc>
                    <Name>Hindu (non vegetarian) meal</Name>
                    <ServiceDefinitionID>SD14</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Kosher meal</DescText>
                    </Desc>
                    <Name>Kosher meal</Name>
                    <ServiceDefinitionID>SD15</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Low sodium, no salt added</DescText>
                    </Desc>
                    <Name>Low sodium, no salt added</Name>
                    <ServiceDefinitionID>SD16</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Moslem meal</DescText>
                    </Desc>
                    <Name>Moslem meal</Name>
                    <ServiceDefinitionID>SD17</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Vegetarian meal (non-dairy)</DescText>
                    </Desc>
                    <Name>Vegetarian meal (non-dairy)</Name>
                    <ServiceDefinitionID>SD18</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>Vegetarian meal (lacto-ovo)</DescText>
                    </Desc>
                    <Name>Vegetarian meal (lacto-ovo)</Name>
                    <ServiceDefinitionID>SD19</ServiceDefinitionID>
                </ServiceDefinition>
            </ServiceDefinitionList>
        </DataLists>
        <ShoppingResponse>
            <ShoppingResponseRefID>2d62d243-8837-4e4d-a91c-45550a2fd6fa</ShoppingResponseRefID>
        </ShoppingResponse>
    </Response>
    <PayloadAttributes>
        <CorrelationID>a222c960-0d2c-4507-bd2c-59362825cc76</CorrelationID>
        <Timestamp>2020-09-30T17:38:33.683</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_ServiceListRS>
{% endhighlight %}

</details>
