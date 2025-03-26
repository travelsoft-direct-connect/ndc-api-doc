---
layout: page
title:  "Seat Availability"
nav_order: 5
---

# SeatAvailability operation
{: .no_toc }
The seat availability method returns seat maps and seat prices for the selected flight offer in request.

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

# SeatAvailabilityRQ

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

# SeatAvailabilityRS

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
| DataLists | The response data lists (seat profiles, etc) | Mandatory |
| ALaCarteOffer | The offer element detailed [below](#alacarteoffer) | Mandatory |
| SeatMaps | List of seat map elements detailed [below](#seatmap) | Mandatory |

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
| Eligibility | Eligibility of this offer item (flight segment + PAX association) | Mandatory |
| Service | The service ID | Mandatory |
| UnitPrice | Price of this offer item | Mandatory |

### SeatMap
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PaxSegmentRefID | The flight segment reference | Mandatory |
| CabinCompartments | List of cabin compartments detailed [below](#cabincompartment) | Mandatory |

#### CabinCompartment
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| CabinType | The cabin type of this compartment (ECONOMY, BUSINESS, etc) | Optional |
| DeckCode | The aircraft deck code (Main, Upper, Lower) | Optional |
| CabinComponents | List of non-seat related cabin feature or facility (e.g. lavatory, galley, closet, storage, etc.) | Optional |
| ColumnIDs | List of column IDs of this compartment (A, B, C, D, E, etc) | Optional |
| FirstRowNumber | First row number of this compartment | Optional |
| LastRowNumber | Last row number of this compartment | Optional |
| SeatRows | List of seat rows detailed [below](#seatrow) | Mandatory |

##### SeatRow
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| RowNumber | The row number | Mandatory |
| RowCharacteristicCodes | The seat row characteristic codes (see IATA Code set Directory, 9825-Seat characteristic) | Optional |
| Seats | List of seat rows detailed [below](#seat) | Mandatory |

###### Seat
{: .no_toc }

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| ColumnID | The column ID of the seat | Mandatory |
| OccupationStatusCode | The seat occupation status (see IATA Code set Directory, 9865-Seat occupation) | Optional |
| SeatCharacteristicCodes | The seat characteristic codes (see IATA Code set Directory, 9825-Seat characteristic) | Optional |
| SeatProfileRefID | The seat profile references if seat characteristics are factorized as profile | Optional |
| OfferItemRefIDs | References of related offer items | Mandatory if seat is available |

# Samples

<details>
  <summary><b>SeatAvailabilityRQ</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_SeatAvailabilityRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_SeatAvailabilityRQ">
    <Party>
        <Sender>
            <TravelAgency>
                <AgencyID>agency1234</AgencyID>
            </TravelAgency>
        </Sender>
    </Party>
    <PayloadAttributes>
        <CorrelationID>102e3cac-aa0f-3a92-a506-1a82173cb0da</CorrelationID>
        <PrimaryLangID>FR</PrimaryLangID>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <Request>
        <CoreRequest>
            <Offer>
                <OfferID>1dd4aa6e-835a-4f37-bb38-7f1d21951424</OfferID>
                <OwnerCode>TA</OwnerCode>
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
        <ResponseParameters>
            <CurParameter>
                <RequestedCurCode>EUR</RequestedCurCode>
            </CurParameter>
            <Device/>
            <LangUsage>
                <LangCode>fr-FR</LangCode>
            </LangUsage>
        </ResponseParameters>
        <ShoppingResponse>
            <OwnerCode>TA</OwnerCode>
            <ShoppingResponseRefID>96aa55f2-d2da-431e-a4b1-2c85452ca415</ShoppingResponseRefID>
        </ShoppingResponse>
    </Request>
</IATA_SeatAvailabilityRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>SeatAvailabilityRS</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_SeatAvailabilityRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_SeatAvailabilityRS">
    <Response>
        <ALaCarteOffer>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>cbc864e8-3aac-4c87-be2b-e9404ac92de4</OfferItemID>
                <Service>
                    <ServiceID>SV121</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">22.77</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>3f8fb3bd-5408-4de7-ac72-ac2bf675c8a0</OfferItemID>
                <Service>
                    <ServiceID>SV122</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">22.77</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>ff217a73-037b-4652-8d4b-8198c182814e</OfferItemID>
                <Service>
                    <ServiceID>SV123</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">22.77</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>852ce4d5-6c53-4eeb-8b83-a4088a82b3e0</OfferItemID>
                <Service>
                    <ServiceID>SV124</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">22.77</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>94b1d4a1-43bb-4cf0-a142-1994312d68f9</OfferItemID>
                <Service>
                    <ServiceID>SV125</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>9f7ed797-0303-41d0-a165-fdcc518fe06d</OfferItemID>
                <Service>
                    <ServiceID>SV126</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>8f3928af-37f6-4312-bc7d-4cc3402484f8</OfferItemID>
                <Service>
                    <ServiceID>SV127</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>1b146a6e-40aa-4e07-8854-5d9d2ed504c6</OfferItemID>
                <Service>
                    <ServiceID>SV128</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>9cdb98d9-814c-4a42-b949-f0a419460222</OfferItemID>
                <Service>
                    <ServiceID>SV129</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>eb45438a-e142-4a29-840a-37ecb7410132</OfferItemID>
                <Service>
                    <ServiceID>SV130</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>7cd2d8ec-7e8a-4094-9db7-27ffb3dc09ae</OfferItemID>
                <Service>
                    <ServiceID>SV131</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>860aee4d-4071-4f4b-9324-ba2039f523aa</OfferItemID>
                <Service>
                    <ServiceID>SV132</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>8eaf1331-80b6-49e5-b134-2b741fc2326e</OfferItemID>
                <Service>
                    <ServiceID>SV133</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>0c969d69-77cc-4dd9-8564-4106c20752ac</OfferItemID>
                <Service>
                    <ServiceID>SV134</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>c0c2ef02-752a-48ba-8c9f-b88ef2fa268e</OfferItemID>
                <Service>
                    <ServiceID>SV135</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>23bc450b-7a18-4aff-ae53-b1775666b571</OfferItemID>
                <Service>
                    <ServiceID>SV136</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>f34f6099-a921-4e5b-8fe0-a3e03e717517</OfferItemID>
                <Service>
                    <ServiceID>SV137</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>887890bc-81fe-4373-8087-fc56ababe3e0</OfferItemID>
                <Service>
                    <ServiceID>SV138</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>51d9f869-e968-46ca-8efc-80fbc1107a6b</OfferItemID>
                <Service>
                    <ServiceID>SV139</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>21845fb8-71c4-4b57-a5a4-2028e41d0c79</OfferItemID>
                <Service>
                    <ServiceID>SV140</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>08b84c9c-7847-4c0d-81bd-16a91ce33082</OfferItemID>
                <Service>
                    <ServiceID>SV141</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>793cd24f-f560-48f9-aa97-f644f0bacf14</OfferItemID>
                <Service>
                    <ServiceID>SV142</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>376dc577-076f-49d4-a45f-c304fe053f2a</OfferItemID>
                <Service>
                    <ServiceID>SV143</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>65640d8c-ac04-4836-9761-36814e14ce6a</OfferItemID>
                <Service>
                    <ServiceID>SV144</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>284bbfef-82bb-4ecd-95a0-1af36cd5641e</OfferItemID>
                <Service>
                    <ServiceID>SV145</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>f4b54c19-ab06-444d-b30d-2c3988a0f109</OfferItemID>
                <Service>
                    <ServiceID>SV146</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>cccf7c26-aace-45d1-8c3d-7d0d5942fb24</OfferItemID>
                <Service>
                    <ServiceID>SV147</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>c367e019-686e-4075-a5ac-782410694424</OfferItemID>
                <Service>
                    <ServiceID>SV148</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>f04e4bd4-a875-4d27-8d60-58443207cc03</OfferItemID>
                <Service>
                    <ServiceID>SV149</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>abdeaf7a-6774-467b-ada8-ad9d29e4c5a0</OfferItemID>
                <Service>
                    <ServiceID>SV150</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>c49251b2-4997-489c-ba1b-50a8a158c654</OfferItemID>
                <Service>
                    <ServiceID>SV151</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>1cc03c12-b4b5-48b2-bfa2-f2efa6925619</OfferItemID>
                <Service>
                    <ServiceID>SV152</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>7f5f4d07-a40a-4352-a618-aea858f81b0a</OfferItemID>
                <Service>
                    <ServiceID>SV153</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>ff9e948a-0ef0-4a14-90d2-d068f945ad0c</OfferItemID>
                <Service>
                    <ServiceID>SV154</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>93b7f291-c5ee-4a53-ac46-230c6cd79d05</OfferItemID>
                <Service>
                    <ServiceID>SV155</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>55d654f5-b60d-403f-b237-1ff3b989a226</OfferItemID>
                <Service>
                    <ServiceID>SV156</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>1a9957e5-68c3-4bb5-bc29-899097fde238</OfferItemID>
                <Service>
                    <ServiceID>SV157</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>b0fb49db-71e5-43a3-b533-f4bffa9e2b24</OfferItemID>
                <Service>
                    <ServiceID>SV158</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>12ede0fd-5596-467e-a9f1-e7230526c53d</OfferItemID>
                <Service>
                    <ServiceID>SV159</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>c6f645dd-a9c9-4ab8-849e-6a8ac09a5683</OfferItemID>
                <Service>
                    <ServiceID>SV160</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>e0f53865-5658-4ccd-aedb-39e863dfd2f8</OfferItemID>
                <Service>
                    <ServiceID>SV161</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX1</PaxRefID>
                </Eligibility>
                <OfferItemID>26821ab2-8869-4a3c-84fe-e429282e665e</OfferItemID>
                <Service>
                    <ServiceID>SV162</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>406b122a-96c6-450e-83a6-b554890b2087</OfferItemID>
                <Service>
                    <ServiceID>SV301</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">22.77</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>a5e71983-9f2b-4877-ab45-40178cc0de30</OfferItemID>
                <Service>
                    <ServiceID>SV302</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">22.77</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>29759ed7-f3c4-4db9-b7bb-04ec72387690</OfferItemID>
                <Service>
                    <ServiceID>SV303</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">22.77</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>793df448-af48-4615-9ece-295b673cf8b0</OfferItemID>
                <Service>
                    <ServiceID>SV304</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">22.77</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>c241acd8-6d3f-4178-9426-a31a2ea79d06</OfferItemID>
                <Service>
                    <ServiceID>SV305</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>08980b24-fa0c-48b4-bba5-ddb505d837f1</OfferItemID>
                <Service>
                    <ServiceID>SV306</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>67acc34b-c48c-4337-bc9e-d5790951c79f</OfferItemID>
                <Service>
                    <ServiceID>SV307</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>0a4ac415-66bb-42a4-ae94-1ec5e2a69a0d</OfferItemID>
                <Service>
                    <ServiceID>SV308</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>f80d228d-9dfa-4e77-9a2f-091373309450</OfferItemID>
                <Service>
                    <ServiceID>SV309</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>29520e7c-93a5-4aaa-93ec-37219902c70c</OfferItemID>
                <Service>
                    <ServiceID>SV310</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>260f9706-dd9d-45b6-b9e6-fa1b1cbb89d6</OfferItemID>
                <Service>
                    <ServiceID>SV311</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>43daf237-d72d-458f-b375-719e15ef326f</OfferItemID>
                <Service>
                    <ServiceID>SV312</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">20.38</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>9034357d-9e60-4327-91c4-e58a19244e9a</OfferItemID>
                <Service>
                    <ServiceID>SV313</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>c6ec90a8-1279-492f-8d04-d000451c4ad8</OfferItemID>
                <Service>
                    <ServiceID>SV314</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>f74d85af-6243-4056-b461-e2bf8c09d82a</OfferItemID>
                <Service>
                    <ServiceID>SV315</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>504472f2-4b0c-4488-98ee-cf1118e944e7</OfferItemID>
                <Service>
                    <ServiceID>SV316</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>e4ea044d-0caa-4fd8-a4b5-1038b16b0644</OfferItemID>
                <Service>
                    <ServiceID>SV317</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>ec79199a-437c-43eb-b969-73d1993ddbee</OfferItemID>
                <Service>
                    <ServiceID>SV318</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>d3b92c73-c422-44db-a99f-15ed15ded66c</OfferItemID>
                <Service>
                    <ServiceID>SV319</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>024860c2-3669-4b87-a7f3-7577537386f0</OfferItemID>
                <Service>
                    <ServiceID>SV320</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>54f0e53f-b9a9-4ac8-8fcf-2311196e7c08</OfferItemID>
                <Service>
                    <ServiceID>SV321</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>82b3e7bd-25a3-4975-8078-f5aaae87e6b8</OfferItemID>
                <Service>
                    <ServiceID>SV322</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>362d4cc2-0d78-4e23-a1c7-6083f58f2527</OfferItemID>
                <Service>
                    <ServiceID>SV323</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>eee1f333-abdc-4091-be56-cb4ec8d7580b</OfferItemID>
                <Service>
                    <ServiceID>SV324</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">7.19</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>b27ea1cf-96bb-48c9-b741-9d7e849d2924</OfferItemID>
                <Service>
                    <ServiceID>SV325</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>6e5939a5-9540-43a4-af3f-afe70bf927ac</OfferItemID>
                <Service>
                    <ServiceID>SV326</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>48efebdd-9718-49fb-878d-60b9dda76e89</OfferItemID>
                <Service>
                    <ServiceID>SV327</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>a0830c0c-a264-4d43-aee3-9815b6fae034</OfferItemID>
                <Service>
                    <ServiceID>SV328</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>ead630f2-036d-49c4-a714-8f54cd88039a</OfferItemID>
                <Service>
                    <ServiceID>SV329</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>3bd3a754-c55b-454f-8545-20a7212fc636</OfferItemID>
                <Service>
                    <ServiceID>SV330</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>904e7bd0-81c3-4fc0-b80e-45a8feb717aa</OfferItemID>
                <Service>
                    <ServiceID>SV331</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>7e50fa08-9197-44b5-8be6-4cfd094a7fcf</OfferItemID>
                <Service>
                    <ServiceID>SV332</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>159028fb-fc6c-4194-98c6-76161423c4d5</OfferItemID>
                <Service>
                    <ServiceID>SV333</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>0fb81afe-4445-4480-8b99-5c12ed4eb49e</OfferItemID>
                <Service>
                    <ServiceID>SV334</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>741754d7-cee7-47fc-a3a3-32d67e117ad5</OfferItemID>
                <Service>
                    <ServiceID>SV335</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>c35fce85-1c10-4565-a936-7e5f42e521c2</OfferItemID>
                <Service>
                    <ServiceID>SV336</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>886b3701-38a3-496c-9c44-d1050826fd72</OfferItemID>
                <Service>
                    <ServiceID>SV337</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>bc813337-5562-4f05-93e9-17f5ae43748d</OfferItemID>
                <Service>
                    <ServiceID>SV338</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>4acb9e64-d737-4cdc-9cbc-5697bfaa2e98</OfferItemID>
                <Service>
                    <ServiceID>SV339</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>315dc5e0-ceca-435d-b1d1-313bba802afa</OfferItemID>
                <Service>
                    <ServiceID>SV340</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>e711aae9-1042-4ada-b934-9201da3aaa90</OfferItemID>
                <Service>
                    <ServiceID>SV341</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <ALaCarteOfferItem>
                <Eligibility>
                    <FlightAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </FlightAssociations>
                    <PaxRefID>PAX2</PaxRefID>
                </Eligibility>
                <OfferItemID>759b20b1-d834-4521-99a2-3441adc5df3d</OfferItemID>
                <Service>
                    <ServiceID>SV342</ServiceID>
                </Service>
                <UnitPrice>
                    <TotalAmount CurCode="EUR">9.59</TotalAmount>
                </UnitPrice>
            </ALaCarteOfferItem>
            <OfferID>dada8b1c-c730-41d1-9c74-e3097a395036</OfferID>
        </ALaCarteOffer>
        <DataLists>
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
                        <AircraftScheduledDateTime>2024-05-17T07:33:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MDE</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <DatedOperatingLegID>DOL22</DatedOperatingLegID>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2024-05-17T06:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BOG</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT1H3M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AV</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>9358</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AV</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG4</PaxSegmentID>
                </PaxSegment>
            </PaxSegmentList>
        </DataLists>
        <SeatMap>
            <CabinCompartment>
                <CabinComponent>
                    <CabinComponentTypeCode>LA</CabinComponentTypeCode>
                    <ColumnID>A</ColumnID>
                    <FirstRowNumber>1</FirstRowNumber>
                </CabinComponent>
                <CabinComponent>
                    <CabinComponentTypeCode>G</CabinComponentTypeCode>
                    <ColumnID>K</ColumnID>
                    <FirstRowNumber>1</FirstRowNumber>
                </CabinComponent>
                <CabinType>
                    <CabinTypeName>ECONOMY</CabinTypeName>
                </CabinType>
                <ColumnID>A</ColumnID>
                <ColumnID>C</ColumnID>
                <ColumnID>D</ColumnID>
                <ColumnID>K</ColumnID>
                <DeckCode>Main</DeckCode>
                <FirstRowNumber>1</FirstRowNumber>
                <LastRowNumber>3</LastRowNumber>
                <SeatRow>
                    <RowCharacteristicCode>E</RowCharacteristicCode>
                    <RowNumber>1</RowNumber>
                    <Seat>
                        <ColumnID>A</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>cbc864e8-3aac-4c87-be2b-e9404ac92de4</OfferItemRefID>
                        <OfferItemRefID>406b122a-96c6-450e-83a6-b554890b2087</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>C</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>3f8fb3bd-5408-4de7-ac72-ac2bf675c8a0</OfferItemRefID>
                        <OfferItemRefID>a5e71983-9f2b-4877-ab45-40178cc0de30</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>D</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>ff217a73-037b-4652-8d4b-8198c182814e</OfferItemRefID>
                        <OfferItemRefID>29759ed7-f3c4-4db9-b7bb-04ec72387690</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>K</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>852ce4d5-6c53-4eeb-8b83-a4088a82b3e0</OfferItemRefID>
                        <OfferItemRefID>793df448-af48-4615-9ece-295b673cf8b0</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                </SeatRow>
                <SeatRow>
                    <RowNumber>2</RowNumber>
                    <Seat>
                        <ColumnID>A</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>94b1d4a1-43bb-4cf0-a142-1994312d68f9</OfferItemRefID>
                        <OfferItemRefID>c241acd8-6d3f-4178-9426-a31a2ea79d06</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>C</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>9f7ed797-0303-41d0-a165-fdcc518fe06d</OfferItemRefID>
                        <OfferItemRefID>08980b24-fa0c-48b4-bba5-ddb505d837f1</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>D</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>8f3928af-37f6-4312-bc7d-4cc3402484f8</OfferItemRefID>
                        <OfferItemRefID>67acc34b-c48c-4337-bc9e-d5790951c79f</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>K</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>1b146a6e-40aa-4e07-8854-5d9d2ed504c6</OfferItemRefID>
                        <OfferItemRefID>0a4ac415-66bb-42a4-ae94-1ec5e2a69a0d</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                </SeatRow>
                <SeatRow>
                    <RowNumber>3</RowNumber>
                    <Seat>
                        <ColumnID>A</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>9cdb98d9-814c-4a42-b949-f0a419460222</OfferItemRefID>
                        <OfferItemRefID>f80d228d-9dfa-4e77-9a2f-091373309450</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>C</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>eb45438a-e142-4a29-840a-37ecb7410132</OfferItemRefID>
                        <OfferItemRefID>29520e7c-93a5-4aaa-93ec-37219902c70c</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>D</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>7cd2d8ec-7e8a-4094-9db7-27ffb3dc09ae</OfferItemRefID>
                        <OfferItemRefID>260f9706-dd9d-45b6-b9e6-fa1b1cbb89d6</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>K</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>860aee4d-4071-4f4b-9324-ba2039f523aa</OfferItemRefID>
                        <OfferItemRefID>43daf237-d72d-458f-b375-719e15ef326f</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                </SeatRow>
            </CabinCompartment>
            <CabinCompartment>
                <CabinType>
                    <CabinTypeName>ECONOMY</CabinTypeName>
                </CabinType>
                <ColumnID>A</ColumnID>
                <ColumnID>B</ColumnID>
                <ColumnID>C</ColumnID>
                <ColumnID>D</ColumnID>
                <ColumnID>E</ColumnID>
                <ColumnID>K</ColumnID>
                <DeckCode>Main</DeckCode>
                <FirstRowNumber>4</FirstRowNumber>
                <LastRowNumber>9</LastRowNumber>
                <SeatRow>
                    <RowNumber>4</RowNumber>
                    <Seat>
                        <ColumnID>A</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>f34f6099-a921-4e5b-8fe0-a3e03e717517</OfferItemRefID>
                        <OfferItemRefID>e4ea044d-0caa-4fd8-a4b5-1038b16b0644</OfferItemRefID>
                        <SeatCharacteristicCode>Q</SeatCharacteristicCode>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>B</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>21845fb8-71c4-4b57-a5a4-2028e41d0c79</OfferItemRefID>
                        <OfferItemRefID>024860c2-3669-4b87-a7f3-7577537386f0</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                        <SeatCharacteristicCode>Q</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>C</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>8eaf1331-80b6-49e5-b134-2b741fc2326e</OfferItemRefID>
                        <OfferItemRefID>9034357d-9e60-4327-91c4-e58a19244e9a</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                        <SeatCharacteristicCode>Q</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>D</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>0c969d69-77cc-4dd9-8564-4106c20752ac</OfferItemRefID>
                        <OfferItemRefID>c6ec90a8-1279-492f-8d04-d000451c4ad8</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                        <SeatCharacteristicCode>Q</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>E</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>08b84c9c-7847-4c0d-81bd-16a91ce33082</OfferItemRefID>
                        <OfferItemRefID>54f0e53f-b9a9-4ac8-8fcf-2311196e7c08</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                        <SeatCharacteristicCode>Q</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>K</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>793cd24f-f560-48f9-aa97-f644f0bacf14</OfferItemRefID>
                        <OfferItemRefID>82b3e7bd-25a3-4975-8078-f5aaae87e6b8</OfferItemRefID>
                        <SeatCharacteristicCode>M</SeatCharacteristicCode>
                        <SeatCharacteristicCode>Q</SeatCharacteristicCode>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                </SeatRow>
                <SeatRow>
                    <RowNumber>5</RowNumber>
                    <Seat>
                        <ColumnID>A</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>e637c2d9-89d3-47fa-9eba-5e65774a7315</OfferItemRefID>
                        <OfferItemRefID>a45ed784-229e-4df3-a7cf-41206271d563</OfferItemRefID>
                        <SeatCharacteristicCode>M</SeatCharacteristicCode>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>B</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>37db5b98-de59-4126-822b-e433c8c1f78b</OfferItemRefID>
                        <OfferItemRefID>89693c23-f06d-43e7-bdf9-11665cc51dcc</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                        <SeatCharacteristicCode>M</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>C</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>1f8bf7e0-31c7-4cd1-9f32-4004fe8c9579</OfferItemRefID>
                        <OfferItemRefID>336a1aaa-3274-4724-832d-172406c41536</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                        <SeatCharacteristicCode>M</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>D</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>c73621b5-d42a-4ff5-b1ab-68725834101e</OfferItemRefID>
                        <OfferItemRefID>f12cbba6-2d13-4c04-ad5c-7a9a0249afac</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                        <SeatCharacteristicCode>M</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>E</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>12a410cf-e324-43a4-afee-6e2a2cbb5153</OfferItemRefID>
                        <OfferItemRefID>fd3ea165-93b5-483b-9f28-7c0e550a62e8</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                        <SeatCharacteristicCode>M</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>K</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>36f88679-0701-4181-adf9-2319ca90382d</OfferItemRefID>
                        <OfferItemRefID>cc067e0e-0501-45c5-a57d-a49398708242</OfferItemRefID>
                        <SeatCharacteristicCode>M</SeatCharacteristicCode>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                </SeatRow>
                <SeatRow>
                    <RowNumber>6</RowNumber>
                    <Seat>
                        <ColumnID>A</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>93b7f291-c5ee-4a53-ac46-230c6cd79d05</OfferItemRefID>
                        <OfferItemRefID>741754d7-cee7-47fc-a3a3-32d67e117ad5</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>B</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>1f7bc8b5-24b6-4a95-b950-0d32f7fb7f16</OfferItemRefID>
                        <OfferItemRefID>0c42061c-cf7c-4080-a19f-1c3d8df240c4</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>C</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>284bbfef-82bb-4ecd-95a0-1af36cd5641e</OfferItemRefID>
                        <OfferItemRefID>b27ea1cf-96bb-48c9-b741-9d7e849d2924</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>D</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>f4b54c19-ab06-444d-b30d-2c3988a0f109</OfferItemRefID>
                        <OfferItemRefID>6e5939a5-9540-43a4-af3f-afe70bf927ac</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>E</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>4349cc3f-9857-42e7-b491-d8d2cd9bb5a5</OfferItemRefID>
                        <OfferItemRefID>0cba048d-6119-4ce8-84be-60feb28d4c28</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>K</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>55d654f5-b60d-403f-b237-1ff3b989a226</OfferItemRefID>
                        <OfferItemRefID>c35fce85-1c10-4565-a936-7e5f42e521c2</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                </SeatRow>
                <SeatRow>
                    <RowNumber>7</RowNumber>
                    <Seat>
                        <ColumnID>A</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>1a9957e5-68c3-4bb5-bc29-899097fde238</OfferItemRefID>
                        <OfferItemRefID>886b3701-38a3-496c-9c44-d1050826fd72</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>B</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>6e8cfdfb-5b0f-470e-b315-b5bf2ad3b807</OfferItemRefID>
                        <OfferItemRefID>2bc753e8-3bed-46da-9db3-a204cbfa8223</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>C</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>cccf7c26-aace-45d1-8c3d-7d0d5942fb24</OfferItemRefID>
                        <OfferItemRefID>48efebdd-9718-49fb-878d-60b9dda76e89</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>D</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>c367e019-686e-4075-a5ac-782410694424</OfferItemRefID>
                        <OfferItemRefID>a0830c0c-a264-4d43-aee3-9815b6fae034</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>E</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>de504854-ae88-4c73-88c8-0a934abf3662</OfferItemRefID>
                        <OfferItemRefID>0a3be7d1-0599-44a9-88f7-7aa4666d0b72</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>K</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>b0fb49db-71e5-43a3-b533-f4bffa9e2b24</OfferItemRefID>
                        <OfferItemRefID>bc813337-5562-4f05-93e9-17f5ae43748d</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                </SeatRow>
                <SeatRow>
                    <RowNumber>8</RowNumber>
                    <Seat>
                        <ColumnID>A</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>12ede0fd-5596-467e-a9f1-e7230526c53d</OfferItemRefID>
                        <OfferItemRefID>4acb9e64-d737-4cdc-9cbc-5697bfaa2e98</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>B</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>68636cd2-eca5-40f0-907d-d49cbf68779b</OfferItemRefID>
                        <OfferItemRefID>ce12c917-5def-47e4-8ebc-d6967cf56fcd</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>C</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>f04e4bd4-a875-4d27-8d60-58443207cc03</OfferItemRefID>
                        <OfferItemRefID>ead630f2-036d-49c4-a714-8f54cd88039a</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>D</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>abdeaf7a-6774-467b-ada8-ad9d29e4c5a0</OfferItemRefID>
                        <OfferItemRefID>3bd3a754-c55b-454f-8545-20a7212fc636</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>E</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>e2819100-47c1-4706-9fbb-45dbc0e22c04</OfferItemRefID>
                        <OfferItemRefID>443ccc50-2738-4cd6-9ca2-04fd92613c75</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>K</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>c6f645dd-a9c9-4ab8-849e-6a8ac09a5683</OfferItemRefID>
                        <OfferItemRefID>315dc5e0-ceca-435d-b1d1-313bba802afa</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                </SeatRow>
                <SeatRow>
                    <RowNumber>9</RowNumber>
                    <Seat>
                        <ColumnID>A</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>e0f53865-5658-4ccd-aedb-39e863dfd2f8</OfferItemRefID>
                        <OfferItemRefID>e711aae9-1042-4ada-b934-9201da3aaa90</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>B</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>20e1a8b8-0fa8-4ab0-82b0-9cda5d7e7772</OfferItemRefID>
                        <OfferItemRefID>5d4f8470-e2c9-4245-bb99-51243d582c4e</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>C</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>c49251b2-4997-489c-ba1b-50a8a158c654</OfferItemRefID>
                        <OfferItemRefID>904e7bd0-81c3-4fc0-b80e-45a8feb717aa</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>D</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>1cc03c12-b4b5-48b2-bfa2-f2efa6925619</OfferItemRefID>
                        <OfferItemRefID>7e50fa08-9197-44b5-8be6-4cfd094a7fcf</OfferItemRefID>
                        <SeatCharacteristicCode>A</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>E</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>23a2b3fe-1dd1-4ff1-8638-699fcf0adda0</OfferItemRefID>
                        <OfferItemRefID>eeab281b-8517-4ce4-b4cb-79264b5cff75</OfferItemRefID>
                        <SeatCharacteristicCode>MS</SeatCharacteristicCode>
                    </Seat>
                    <Seat>
                        <ColumnID>K</ColumnID>
                        <OccupationStatusCode>F</OccupationStatusCode>
                        <OfferItemRefID>26821ab2-8869-4a3c-84fe-e429282e665e</OfferItemRefID>
                        <OfferItemRefID>759b20b1-d834-4521-99a2-3441adc5df3d</OfferItemRefID>
                        <SeatCharacteristicCode>W</SeatCharacteristicCode>
                    </Seat>
                </SeatRow>
            </CabinCompartment>
            <PaxSegmentRefID>SEG4</PaxSegmentRefID>
        </SeatMap>
        <ShoppingResponse>
            <ShoppingResponseRefID>96aa55f2-d2da-431e-a4b1-2c85452ca415</ShoppingResponseRefID>
        </ShoppingResponse>
    </Response>
    <PayloadAttributes>
        <CorrelationID>102e3cac-aa0f-3a92-a506-1a82173cb0da</CorrelationID>
        <Timestamp>2024-04-05T14:42:04.067+02:00</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_SeatAvailabilityRS>
{% endhighlight %}

</details>
