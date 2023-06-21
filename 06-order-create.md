---
layout: page
title:  "Order Create"
nav_order: 6
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
| Party | Must contain agency ID as sender | Mandatory |
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
| ContactInfoList | Must contains the client contact | Mandatory |
| PaxList | List of passengers (same as AirShoppingRQ) with more information (name, document, etc) + client contact reference. Possible values for the title ('MR', 'MRS' and 'MISS') | Mandatory |

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
| Services | List of flight/serviceDefinition associations with PAX and StatusCode: {::nomarkdown}<ul><li>RQ: on request (availability is waiting to be confirmed, OrderRetrieveRQ has to be called periodically until status is updated)</li><li>K: pending (OrderChangeRQ has to be executed to issue tickets)</li><li>SB: issuance in progress (waiting to be confirmed, OrderRetrieveRQ has to be called periodically until status is updated)</li><li>T: tickets issued</li><li>X: cancelled</li></ul> {:/} | Mandatory |

# Samples

<details>
  <summary><b>OrderCreateRQ (Hold booking)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderCreateRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderCreateRQ">
    <PayloadAttributes>
        <CorrelationID>a222c960-0d2c-4507-bd2c-59362825cc76</CorrelationID>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <Request>
        <CreateOrder>
            <SelectedOffer>
                <OfferRefID>601a1ef9-4ac3-471f-98a4-b635e2991d1f</OfferRefID>
                <OwnerCode>AF</OwnerCode>
                <SelectedOfferItem>
                    <OfferItemRefID>8e092c73-ce79-40b5-ba63-9046b0f001f5</OfferItemRefID>
                </SelectedOfferItem>
                <SelectedOfferItem>
                    <OfferItemRefID>3f4f71eb-c800-4dae-adca-4f61f51a7de2</OfferItemRefID>
                </SelectedOfferItem>
                <SelectedOfferItem>
                    <OfferItemRefID>d59bb99f-d764-457e-9946-08ead0428113</OfferItemRefID>
                </SelectedOfferItem>
                <ShoppingResponseRefID>2d62d243-8837-4e4d-a91c-45550a2fd6fa</ShoppingResponseRefID>
            </SelectedOffer>
        </CreateOrder>
        <DataLists>
            <ContactInfoList>
                <ContactInfo>
                    <ContactInfoID>CONT1</ContactInfoID>
                    <EmailAddress>
                        <EmailAddressText>florian.garnier@orchestra.eu</EmailAddressText>
                    </EmailAddress>
                    <Phone>
                        <PhoneNumber>330677203692</PhoneNumber>
                    </Phone>
                    <PostalAddress>
                        <CityName>Paris</CityName>
                        <CountryCode>FR</CountryCode>
                        <PostalCode>75002</PostalCode>
                        <StreetText>38 av opera</StreetText>
                    </PostalAddress>
                </ContactInfo>
            </ContactInfoList>
            <PaxList>
                <Pax>
                    <Birthdate>1986-05-02</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <Individual>
                        <GenderCode>M</GenderCode>
                        <GivenName>Florian</GivenName>
                        <IndividualID>IND1</IndividualID>
                        <Surname>Garnier</Surname>
                        <TitleName>MR</TitleName>
                    </Individual>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <Birthdate>1983-02-10</Birthdate>
                    <Individual>
                        <GenderCode>F</GenderCode>
                        <GivenName>Floria</GivenName>
                        <IndividualID>IND2</IndividualID>
                        <Surname>Garnier</Surname>
                        <TitleName>MRS</TitleName>
                    </Individual>
                    <PaxID>PAX2</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
            </PaxList>
        </DataLists>
    </Request>
</IATA_OrderCreateRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderCreateRQ (Instant payment)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderCreateRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderCreateRQ">
    <Party>
        <Sender>
            <TravelAgency>
                <AgencyID>1234</AgencyID>
            </TravelAgency>
        </Sender>
    </Party>
    <PayloadAttributes>
        <CorrelationID>87cd9bb7-85c7-3ccf-a42c-c6a71cb48c04</CorrelationID>
        <PrimaryLangID>FR</PrimaryLangID>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <Request>
        <CreateOrder>
            <SelectedOffer>
                <OfferRefID>c6b1031d-6390-46e5-81c8-5eab72080b0b</OfferRefID>
                <OwnerCode>AF</OwnerCode>
                <SelectedOfferItem>
                    <OfferItemRefID>fd03303b-014c-4e16-9f50-a7955925ec7a</OfferItemRefID>
                    <PaxRefID>PAX1</PaxRefID>
                    <PaxRefID>PAX2</PaxRefID>
                </SelectedOfferItem>
                <SelectedOfferItem>
                    <OfferItemRefID>134feb1a-e026-4190-ba52-fdfaae2bdb98</OfferItemRefID>
                    <PaxRefID>PAX1</PaxRefID>
                    <SelectedALaCarteOfferItem>
                        <Qty>1</Qty>
                    </SelectedALaCarteOfferItem>
                </SelectedOfferItem>
                <SelectedOfferItem>
                    <OfferItemRefID>5c80c5e6-bc13-4cd8-ba00-64c24486ad1c</OfferItemRefID>
                    <PaxRefID>PAX2</PaxRefID>
                    <SelectedALaCarteOfferItem>
                        <Qty>1</Qty>
                    </SelectedALaCarteOfferItem>
                </SelectedOfferItem>
                <ShoppingResponseRefID>87c93851-804f-4ff2-b2b8-2699ad9ac8eb</ShoppingResponseRefID>
            </SelectedOffer>
        </CreateOrder>
        <DataLists>
            <ContactInfoList>
                <ContactInfo>
                    <ContactInfoID>CONT1</ContactInfoID>
                    <EmailAddress>
                        <EmailAddressText>florian.garnier@orchestra.eu</EmailAddressText>
                    </EmailAddress>
                    <Phone>
                        <CountryDialingCode>33</CountryDialingCode>
                        <PhoneNumber>622761972</PhoneNumber>
                    </Phone>
                    <PostalAddress>
                        <CityName>Paris</CityName>
                        <ContactTypeText>AddressAtOrigin</ContactTypeText>
                        <CountryCode>FR</CountryCode>
                        <PostalCode>75002</PostalCode>
                        <StreetText>38 avenue de l'opera</StreetText>
                    </PostalAddress>
                </ContactInfo>
            </ContactInfoList>
            <PaxList>
                <Pax>
                    <Birthdate>1986-01-01</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <Individual>
                        <Birthdate>1986-01-01</Birthdate>
                        <GenderCode>M</GenderCode>
                        <GivenName>Florian</GivenName>
                        <IndividualID>IND1</IndividualID>
                        <Surname>Garnier</Surname>
                        <TitleName>MR</TitleName>
                    </Individual>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <Birthdate>1988-01-01</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <Individual>
                        <Birthdate>1988-01-01</Birthdate>
                        <GenderCode>F</GenderCode>
                        <GivenName>Flora</GivenName>
                        <IndividualID>IND2</IndividualID>
                        <Surname>Garnier</Surname>
                        <TitleName>MRS</TitleName>
                    </Individual>
                    <PaxID>PAX2</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
            </PaxList>
        </DataLists>
        <PaymentFunctions>
            <PaymentProcessingDetails>
                <Amount CurCode="EUR">246.12</Amount>
                <PaymentMethod>
                    <Cash/>
                </PaymentMethod>
                <TypeCode>CA</TypeCode>
            </PaymentProcessingDetails>
        </PaymentFunctions>
    </Request>
</IATA_OrderCreateRQ>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderViewRS (Hold booking)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderViewRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderViewRS">
    <Response>
        <DataLists>
            <ContactInfoList>
                <ContactInfo>
                    <ContactInfoID>CONT1</ContactInfoID>
                    <EmailAddress>
                        <EmailAddressText>florian.garnier@orchestra.eu</EmailAddressText>
                    </EmailAddress>
                    <Phone>
                        <PhoneNumber>330677203692</PhoneNumber>
                    </Phone>
                    <PostalAddress>
                        <CityName>Paris</CityName>
                        <CountryCode>FR</CountryCode>
                        <PostalCode>75002</PostalCode>
                        <StreetText>38 av opera</StreetText>
                    </PostalAddress>
                </ContactInfo>
            </ContactInfoList>
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
                    <Birthdate>1986-05-02</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <Birthdate>1983-02-10</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
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
            <PriceClassList>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>1 hand baggage item and 1 accessory only (12 kg total*)</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Cancellation is not possible</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Change fee + fare difference</DescText>
                    </Desc>
                    <Desc>
                        <DescText>No refund if you missed your flight</DescText>
                    </Desc>
                    <Desc>
                        <DescText>* This applies to flights operated by KLM or Air France. For other airlines, please check the airline's website for baggage rules</DescText>
                    </Desc>
                    <Name>Light</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
            </PriceClassList>
            <ServiceDefinitionList>
                <ServiceDefinition>
                    <Desc>
                        <DescText>1 luggage item</DescText>
                    </Desc>
                    <Name>1 luggage item</Name>
                    <ServiceDefinitionID>SD1</ServiceDefinitionID>
                </ServiceDefinition>
            </ServiceDefinitionList>
        </DataLists>
        <Order>
            <BookingRef>
                <BookingEntity>
                    <Carrier>
                        <AirlineDesigCode>AF</AirlineDesigCode>
                    </Carrier>
                </BookingEntity>
                <BookingID>UGSUQY</BookingID>
            </BookingRef>
            <OrderID>e3d15950-21ab-446a-a03b-22370308da64</OrderID>
            <OrderItem>
                <FareDetail>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>X</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <FareBasisCode>XL5VUIL1</FareBasisCode>
                        <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FarePriceType>
                        <Price>
                            <BaseAmount CurCode="EUR">151.00000000000000000000</BaseAmount>
                            <TaxSummary>
                                <Tax>
                                    <Amount CurCode="EUR">200.00000000000000000000</Amount>
                                    <TaxCode>TAX_GROUP_1_PAX_1_2</TaxCode>
                                    <TaxName>Fuel surcharge (YQ/YR) - PAX1,2</TaxName>
                                </Tax>
                                <Tax>
                                    <Amount CurCode="EUR">77.66000000000000000000</Amount>
                                    <TaxCode>GENERAL_TAXES_PAX_1_2</TaxCode>
                                    <TaxName>Taxes - PAX1,2</TaxName>
                                </Tax>
                                <TotalTaxAmount CurCode="EUR">277.66000000000000000000</TotalTaxAmount>
                            </TaxSummary>
                            <TotalAmount CurCode="EUR">428.66000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                    <PaxRefID>PAX2</PaxRefID>
                </FareDetail>
                <OrderItemID>57287dcc-4327-47c3-9b3e-37e34271d2b2</OrderItemID>
                <Price>
                    <BaseAmount CurCode="EUR">302.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">857.32000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV391</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV392</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV393</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV394</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV395</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV396</ServiceID>
                    <StatusCode>K</StatusCode>
                </Service>
            </OrderItem>
            <OrderItem>
                <FareDetail>
                    <FarePriceType>
                        <Price>
                            <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                            <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                </FareDetail>
                <OrderItemID>5dff8cd2-6876-4b41-845a-a84bf2f6166a</OrderItemID>
                <Price>
                    <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <ServiceDefinitionRef>
                            <ServiceDefinitionRefID>SD1</ServiceDefinitionRefID>
                        </ServiceDefinitionRef>
                    </ServiceAssociations>
                    <ServiceID>SV397</ServiceID>
                </Service>
            </OrderItem>
            <OrderItem>
                <FareDetail>
                    <FarePriceType>
                        <Price>
                            <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                            <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                </FareDetail>
                <OrderItemID>d765d398-1bf0-40d7-9e1b-2239d26d35f8</OrderItemID>
                <Price>
                    <BaseAmount CurCode="EUR">50.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">50.00000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <ServiceDefinitionRef>
                            <ServiceDefinitionRefID>SD1</ServiceDefinitionRefID>
                        </ServiceDefinitionRef>
                    </ServiceAssociations>
                    <ServiceID>SV398</ServiceID>
                </Service>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <ServiceDefinitionRef>
                            <ServiceDefinitionRefID>SD1</ServiceDefinitionRefID>
                        </ServiceDefinitionRef>
                    </ServiceAssociations>
                    <ServiceID>SV399</ServiceID>
                </Service>
            </OrderItem>
            <OwnerCode>AF</OwnerCode>
            <StatusCode>OPENED</StatusCode>
            <TotalPrice>
                <BaseAmount CurCode="EUR">402.00000000000000000000</BaseAmount>
                <TaxSummary>
                    <Tax>
                        <Amount CurCode="EUR">400.00000000000000000000</Amount>
                        <TaxCode>TAX_GROUP_1_PAX_1_2</TaxCode>
                        <TaxName>Fuel surcharge (YQ/YR) - PAX1,2</TaxName>
                    </Tax>
                    <Tax>
                        <Amount CurCode="EUR">155.32000000000000000000</Amount>
                        <TaxCode>GENERAL_TAXES_PAX_1_2</TaxCode>
                        <TaxName>Taxes - PAX1,2</TaxName>
                    </Tax>
                    <TotalTaxAmount CurCode="EUR">555.32000000000000000000</TotalTaxAmount>
                </TaxSummary>
                <TotalAmount CurCode="EUR">957.32000000000000000000</TotalAmount>
            </TotalPrice>
        </Order>
        <Warning>
            <Code>SUPPLIER_WARN</Code>
            <DescText>Tax is missing from Ticket, therefore tax is read from Quote instead of Ticket</DescText>
            <OwnerName>AIRFRANCE</OwnerName>
        </Warning>
    </Response>
    <PayloadAttributes>
        <CorrelationID>a222c960-0d2c-4507-bd2c-59362825cc76</CorrelationID>
        <Timestamp>2020-09-30T17:44:16.725</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_OrderViewRS>
{% endhighlight %}

</details>

<details>
  <summary><b>OrderViewRS (Instant payment)</b></summary>

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_OrderViewRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_OrderViewRS">
    <Response>
        <DataLists>
            <ContactInfoList>
                <ContactInfo>
                    <ContactInfoID>CONT1</ContactInfoID>
                    <EmailAddress>
                        <EmailAddressText>florian.garnier@orchestra.eu</EmailAddressText>
                    </EmailAddress>
                    <Phone>
                        <CountryDialingCode>33</CountryDialingCode>
                        <PhoneNumber>622761972</PhoneNumber>
                    </Phone>
                    <PostalAddress>
                        <CityName>Paris</CityName>
                        <CountryCode>FR</CountryCode>
                        <PostalCode>75002</PostalCode>
                        <StreetText>38 avenue de l'opera</StreetText>
                    </PostalAddress>
                </ContactInfo>
            </ContactInfoList>
            <OriginDestList>
                <OriginDest>
                    <DestCode>BCN</DestCode>
                    <OriginCode>PAR</OriginCode>
                    <OriginDestID>OD3</OriginDestID>
                    <PaxJourneyRefID>PJ21</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>PAR</DestCode>
                    <OriginCode>BCN</OriginCode>
                    <OriginDestID>OD4</OriginDestID>
                    <PaxJourneyRefID>PJ22</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H50M0S</Duration>
                    <PaxJourneyID>PJ21</PaxJourneyID>
                    <PaxSegmentRefID>SEG28</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT2H0M0S</Duration>
                    <PaxJourneyID>PJ22</PaxJourneyID>
                    <PaxSegmentRefID>SEG29</PaxSegmentRefID>
                </PaxJourney>
            </PaxJourneyList>
            <PaxList>
                <Pax>
                    <Birthdate>1986-01-01</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <Individual>
                        <GenderCode>M</GenderCode>
                        <GivenName>Florian</GivenName>
                        <Surname>Garnier</Surname>
                        <TitleName>MR</TitleName>
                    </Individual>
                    <PaxID>PAX1</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
                <Pax>
                    <Birthdate>1988-01-01</Birthdate>
                    <ContactInfoRefID>CONT1</ContactInfoRefID>
                    <Individual>
                        <GenderCode>F</GenderCode>
                        <GivenName>Flora</GivenName>
                        <Surname>Garnier</Surname>
                        <TitleName>MRS</TitleName>
                    </Individual>
                    <PaxID>PAX2</PaxID>
                    <PTC>ADT</PTC>
                </Pax>
            </PaxList>
            <PaxSegmentList>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2023-01-09T22:55:00.000+01:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>321</CarrierAircraftTypeCode>
                            <CarrierAircraftTypeName>321</CarrierAircraftTypeName>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2023-01-09T21:05:00.000+01:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT1H50M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1448</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG28</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2023-01-14T16:55:00.000+01:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>321</CarrierAircraftTypeCode>
                            <CarrierAircraftTypeName>321</CarrierAircraftTypeName>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2023-01-14T14:55:00.000+01:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                        <TerminalName>1</TerminalName>
                    </Dep>
                    <Duration>P0Y0M0DT2H0M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1649</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG29</PaxSegmentID>
                </PaxSegment>
            </PaxSegmentList>
            <PriceClassList>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Name>Light</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
            </PriceClassList>
            <ServiceDefinitionList>
                <ServiceDefinition>
                    <Desc>
                        <DescText>1 luggage item 23kg</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Outbound (adult 1)</DescText>
                    </Desc>
                    <Name>1 luggage item 23kg</Name>
                    <ServiceDefinitionID>SD9</ServiceDefinitionID>
                </ServiceDefinition>
                <ServiceDefinition>
                    <Desc>
                        <DescText>1 luggage item 23kg</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Outbound (adulte 2)</DescText>
                    </Desc>
                    <Name>1 luggage item 23kg</Name>
                    <ServiceDefinitionID>SD10</ServiceDefinitionID>
                </ServiceDefinition>
            </ServiceDefinitionList>
        </DataLists>
        <Order>
            <BookingRef>
                <BookingEntity>
                    <Carrier>
                        <AirlineDesigCode>AF</AirlineDesigCode>
                    </Carrier>
                </BookingEntity>
                <BookingID>WP3JLH</BookingID>
            </BookingRef>
            <BookingRef>
                <BookingEntity>
                    <Org>
                        <Name>ORCHESTRA</Name>
                        <OrgID>ORCHESTRA</OrgID>
                    </Org>
                </BookingEntity>
                <BookingID>WP3JLH</BookingID>
            </BookingRef>
            <OrderID>3449</OrderID>
            <OrderItem>
                <FareDetail>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>M</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <FareBasisCode>XS50OALG</FareBasisCode>
                        <PaxSegmentRefID>SEG28</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FareComponent>
                        <CabinType>
                            <CabinTypeCode>M</CabinTypeCode>
                            <CabinTypeName>ECONOMY</CabinTypeName>
                        </CabinType>
                        <FareBasisCode>GS50OALG</FareBasisCode>
                        <PaxSegmentRefID>SEG29</PaxSegmentRefID>
                        <PriceClassRefID>PC1</PriceClassRefID>
                    </FareComponent>
                    <FarePriceType>
                        <FarePriceTypeCode>70J</FarePriceTypeCode>
                        <Price>
                            <BaseAmount CurCode="EUR">55.00000000000000000000</BaseAmount>
                            <TaxSummary>
                                <Tax>
                                    <Amount CurCode="EUR">49.06000000000000000000</Amount>
                                    <TaxCode>GENERAL_TAXES_PAX_1_2</TaxCode>
                                    <TaxName>Taxes - PAX1,2</TaxName>
                                </Tax>
                                <Tax>
                                    <Amount CurCode="EUR">1.00000000000000000000</Amount>
                                    <TaxCode>TAX_GROUP_1_PAX_1_2</TaxCode>
                                    <TaxName>Fuel surcharge (YQ/YR) - PAX1,2</TaxName>
                                </Tax>
                                <TotalTaxAmount CurCode="EUR">50.06000000000000000000</TotalTaxAmount>
                            </TaxSummary>
                            <TotalAmount CurCode="EUR">105.06000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                    <PaxRefID>PAX2</PaxRefID>
                </FareDetail>
                <OrderItemID>3dffa83a-f4f2-5aa7-8a01-23e2ef763c14</OrderItemID>
                <Price>
                    <BaseAmount CurCode="EUR">110.00000000000000000000</BaseAmount>
                    <TaxSummary>
                        <Tax>
                            <Amount CurCode="EUR">98.12000000000000000000</Amount>
                            <TaxCode>GENERAL_TAXES_PAX_1_2</TaxCode>
                            <TaxName>Taxes - PAX1,2</TaxName>
                        </Tax>
                        <Tax>
                            <Amount CurCode="EUR">2.00000000000000000000</Amount>
                            <TaxCode>TAX_GROUP_1_PAX_1_2</TaxCode>
                            <TaxName>Fuel surcharge (YQ/YR) - PAX1,2</TaxName>
                        </Tax>
                        <TotalTaxAmount CurCode="EUR">100.12000000000000000000</TotalTaxAmount>
                    </TaxSummary>
                    <TotalAmount CurCode="EUR">210.12000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG28</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV636</ServiceID>
                    <StatusCode>T</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG28</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV637</ServiceID>
                    <StatusCode>T</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG29</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV638</ServiceID>
                    <StatusCode>T</StatusCode>
                </Service>
                <Service>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <PaxSegmentRefID>SEG29</PaxSegmentRefID>
                    </ServiceAssociations>
                    <ServiceID>SV639</ServiceID>
                    <StatusCode>T</StatusCode>
                </Service>
            </OrderItem>
            <OrderItem>
                <FareDetail>
                    <FarePriceType>
                        <FarePriceTypeCode>70J</FarePriceTypeCode>
                        <Price>
                            <BaseAmount CurCode="EUR">25.00000000000000000000</BaseAmount>
                            <TotalAmount CurCode="EUR">25.00000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX1</PaxRefID>
                </FareDetail>
                <OrderItemID>b9dad960-aa3e-5ca7-9334-08cbef0b2b03</OrderItemID>
                <Price>
                    <BaseAmount CurCode="EUR">25.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">25.00000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX1</PaxRefID>
                    <ServiceAssociations>
                        <ServiceDefinitionRef>
                            <FlightAssociations>
                                <PaxJourneyRefID>PJ21</PaxJourneyRefID>
                            </FlightAssociations>
                            <ServiceDefinitionRefID>SD9</ServiceDefinitionRefID>
                        </ServiceDefinitionRef>
                    </ServiceAssociations>
                    <ServiceID>SV640</ServiceID>
                    <StatusCode>T</StatusCode>
                </Service>
            </OrderItem>
            <OrderItem>
                <FareDetail>
                    <FarePriceType>
                        <FarePriceTypeCode>70J</FarePriceTypeCode>
                        <Price>
                            <BaseAmount CurCode="EUR">25.00000000000000000000</BaseAmount>
                            <TotalAmount CurCode="EUR">25.00000000000000000000</TotalAmount>
                        </Price>
                    </FarePriceType>
                    <PaxRefID>PAX2</PaxRefID>
                </FareDetail>
                <OrderItemID>910bbd3a-245d-5459-9e1d-91ac380d36cc</OrderItemID>
                <Price>
                    <BaseAmount CurCode="EUR">25.00000000000000000000</BaseAmount>
                    <TotalAmount CurCode="EUR">25.00000000000000000000</TotalAmount>
                </Price>
                <Service>
                    <PaxRefID>PAX2</PaxRefID>
                    <ServiceAssociations>
                        <ServiceDefinitionRef>
                            <FlightAssociations>
                                <PaxJourneyRefID>PJ21</PaxJourneyRefID>
                            </FlightAssociations>
                            <ServiceDefinitionRefID>SD10</ServiceDefinitionRefID>
                        </ServiceDefinitionRef>
                    </ServiceAssociations>
                    <ServiceID>SV641</ServiceID>
                    <StatusCode>T</StatusCode>
                </Service>
            </OrderItem>
            <OwnerCode>AF</OwnerCode>
            <StatusCode>OPENED</StatusCode>
            <TotalPrice>
                <BaseAmount CurCode="EUR">160.00000000000000000000</BaseAmount>
                <TaxSummary>
                    <Tax>
                        <Amount CurCode="EUR">98.12000000000000000000</Amount>
                        <TaxCode>GENERAL_TAXES_PAX_1_2</TaxCode>
                        <TaxName>Taxes - PAX1,2</TaxName>
                    </Tax>
                    <Tax>
                        <Amount CurCode="EUR">2.00000000000000000000</Amount>
                        <TaxCode>TAX_GROUP_1_PAX_1_2</TaxCode>
                        <TaxName>Fuel surcharge (YQ/YR) - PAX1,2</TaxName>
                    </Tax>
                    <TotalTaxAmount CurCode="EUR">100.12000000000000000000</TotalTaxAmount>
                </TaxSummary>
                <TotalAmount CurCode="EUR">260.12000000000000000000</TotalAmount>
            </TotalPrice>
        </Order>
        <TicketDocInfo>
            <BookingRef>
                <BookingEntity>
                    <Carrier>
                        <AirlineDesigCode>AF</AirlineDesigCode>
                    </Carrier>
                </BookingEntity>
                <BookingID>WP3JLH</BookingID>
            </BookingRef>
            <PaxRefID>PAX1</PaxRefID>
            <PaymentInfoRefID>PAY1</PaymentInfoRefID>
            <Ticket>
                <Coupon>
                    <CouponNumber>1</CouponNumber>
                    <CouponStatusCode>I</CouponStatusCode>
                </Coupon>
                <ReportingTypeCode>BSP</ReportingTypeCode>
                <TicketDocTypeCode>T</TicketDocTypeCode>
                <TicketNumber>0571478293245</TicketNumber>
            </Ticket>
            <Ticket>
                <Coupon>
                    <CouponNumber>1</CouponNumber>
                    <CouponStatusCode>I</CouponStatusCode>
                </Coupon>
                <ReportingTypeCode>BSP</ReportingTypeCode>
                <TicketDocTypeCode>J</TicketDocTypeCode>
                <TicketNumber>0571506321294</TicketNumber>
            </Ticket>
        </TicketDocInfo>
        <TicketDocInfo>
            <BookingRef>
                <BookingEntity>
                    <Carrier>
                        <AirlineDesigCode>AF</AirlineDesigCode>
                    </Carrier>
                </BookingEntity>
                <BookingID>WP3JLH</BookingID>
            </BookingRef>
            <PaxRefID>PAX2</PaxRefID>
            <PaymentInfoRefID>PAY1</PaymentInfoRefID>
            <Ticket>
                <Coupon>
                    <CouponNumber>1</CouponNumber>
                    <CouponStatusCode>I</CouponStatusCode>
                </Coupon>
                <ReportingTypeCode>BSP</ReportingTypeCode>
                <TicketDocTypeCode>T</TicketDocTypeCode>
                <TicketNumber>0571478293244</TicketNumber>
            </Ticket>
            <Ticket>
                <Coupon>
                    <CouponNumber>1</CouponNumber>
                    <CouponStatusCode>I</CouponStatusCode>
                </Coupon>
                <ReportingTypeCode>BSP</ReportingTypeCode>
                <TicketDocTypeCode>J</TicketDocTypeCode>
                <TicketNumber>0571506321295</TicketNumber>
            </Ticket>
        </TicketDocInfo>
    </Response>
    <PayloadAttributes>
        <CorrelationID>87cd9bb7-85c7-3ccf-a42c-c6a71cb48c04</CorrelationID>
        <Timestamp>2022-10-12T11:44:49.624+02:00</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <PaymentFunctions>
        <PaymentProcessingSummary>
            <Amount CurCode="EUR">260.12000000000000000000</Amount>
            <PaymentID>PAY1</PaymentID>
            <PaymentMethod>
                <Cash/>
            </PaymentMethod>
            <TypeCode>Cash</TypeCode>
        </PaymentProcessingSummary>
    </PaymentFunctions>
</IATA_OrderViewRS>
{% endhighlight %}

</details>
