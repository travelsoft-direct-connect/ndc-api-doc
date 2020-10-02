---
layout: page
title:  "Order Create"
nav_order: 6
---

# OrderCreate operation

The order create method allows to create an order from an offer selection. The passenger information must be sent in request to be able to create order in the airline system. If the order creation succeeds, the response contains an order ID that can be used for servicing flows.

---------------------------------------

## Release notes

| Version | Notes |
| --- | --- |
| 1.0 | Initial version. |

## Mandatory HTTP header

- *AuthToken*: token value retrieved from login response

## Control header

The provider to request must be sent in the control header. For example:

{% highlight xml %}
<Control Provider="VUELING" />
{% endhighlight %}

# OrderCreateRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Party | Must contain agency ID as sender | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| CreateOrder | The order to create detailed [below](#createorder) | Mandatory |
| DataLists | The request data lists detailed [below](#datalists) | Mandatory |
| PaymentFunctions | Only if direct ticket issue has to be forced at order creation. If so, an element 'PaymentProcessingDetails' has to be set with Cash method. | Optional |

### CreateOrder

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| SelectedOffer | The selected offer previously priced with offerPrice operation | Mandatory |

### DataLists

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| ContactInfoList | Must contains the client contact | Mandatory |
| PaxList | List of passengers (same as AirShoppingRQ) with more information (name, document, etc) + client contact reference | Mandatory |

# OrderCreate - OrderViewRS

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PayloadAttributes | Same as requested + timestamp | Mandatory |
| Response | The response element detailed [below](#response) | Mandatory |

## Response

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Warnings | List of warnings returned by provider | Optional |
| DataLists | The response data lists (journeys, segments, service definitions, etc) | Mandatory |
| Order | The order element detailed [below](#order) | Mandatory |

### Order

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderID | The order ID (to use for servicing) | Mandatory |
| BookingRefs | List of booking references | Mandatory |
| TotalPrice | The total price of the whole order | Mandatory |
| OrderItems | List of order items detailed [below](#orderitem) | Mandatory |

#### OrderItem

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OrderItemID | ID of the order item | Mandatory |
| FareDetail | Contains the PAX associations, the unit price in FarePriceType, and more information for each segment in FareComponent | Mandatory |
| Price | The total price of this offer item | Mandatory |
| Services | List of flight/serviceDefinition associations with PAX | Mandatory |

# Samples

<details>
  <summary><b>OrderCreateRQ</b></summary>

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
  <summary><b>OrderViewRS</b></summary>

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
