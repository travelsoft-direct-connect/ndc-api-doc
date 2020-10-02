# AirShopping operation

The air shopping operation allows to initiate a shopping session and returns a list of flight offers depending on criteria given in request.

---------------------------------------

## Release notes

| Version | Notes |
| --- | --- |
| 1.0 | Initial version. |

## Mandatory HTTP header

- *AuthToken*: token value retrieved from login response

## Control header

The provider to request must be sent in the control header. For example:

```xml
<Control Provider="VUELING" />
```

# AirShoppingRQ

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Party | Must contain agency ID as sender | Mandatory |
| PayloadAttributes | Version + CorrelationID (to group log messages) | Optional |
| Request | The request element detailed [below](#request) | Mandatory |

## Request

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| FlightCriteria | List of origin/destination criteria (one OD for one-way, two ODs for round-trip or open-jaw) | Mandatory |
| Paxs | List of passengers with type (PTC): <ul><li>ADT for adults</li><li>CHD for children + birth date or age </li><li>INF for infants + birth date or age</li></ul> | Mandatory |
| ResponseParameters | <ul><li>Currency requested (EUR by default)</li><li>Language requested (ignored if not supported by the provider)</li></ul> | Optional |
| ShoppingCriteria | The shopping criteria containing at least the preferred transport class (ECONOMY, BUSINESS, etc) | Mandatory |


# AirShoppingRS

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| PayloadAttributes | Same as requested + timestamp | Mandatory |
| Response | The response element detailed [below](#response) | Mandatory |

## Response

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| Warnings | List of warnings returned by provider | Optional |
| ShoppingResponse | The Shopping session ID to use for next requests | Mandatory |
| DataLists | The response data lists (journeys, segments, etc) | Mandatory |
| Offers | List of flight offers detailed [below](#offer). For a round-trip search, offers can be returned as <ul><li>combination mode (round-trip offers => only one offer to select)</li><li>flat mode (outbound offers + inbound offers => two offers to select)</li>| Mandatory |

### Offer

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferID | The offer ID | Mandatory |
| OwnerCode | The airline owner code | Mandatory |
| JourneyOverview | Overview of contained journeys with price class links | Mandatory |
| BaggageAllowance | The baggage allowance for each pax/segment | Mandatory |
| TotalPrice | The total price of this offer | Mandatory |
| OfferItems | List of offer items detailed [below](#offeritem) | Mandatory |

#### OfferItem

| Element | Description | Optional/Mandatory |
| --- | --- | --- |
| OfferItemID | The offer item ID | Mandatory |
| FareDetail | Contains the PAX associations, the unit price in FarePriceType, and more information for each segment in FareComponent | Mandatory |
| Price | The total price of this offer item | Mandatory |
| Services | List of flight associations with PAX | Mandatory |


# Samples

<details>
  <summary><b>AirShoppingRQ - OneWay</b></summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRQ">
    <Party>
        <Sender>
            <TravelAgency>
                <AgencyID>1234</AgencyID>
            </TravelAgency>
        </Sender>
    </Party>
    <PayloadAttributes>
        <CorrelationID>48e036b6-9b15-4d79-b550-bfa9bc53ef1c</CorrelationID>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <Request>
        <FlightCriteria>
            <OriginDestCriteria>
                <DestArrivalCriteria>
                    <IATA_LocationCode>MAD</IATA_LocationCode>
                </DestArrivalCriteria>
                <OriginDepCriteria>
                    <Date>2020-10-06</Date>
                    <IATA_LocationCode>BCN</IATA_LocationCode>
                </OriginDepCriteria>
            </OriginDestCriteria>
        </FlightCriteria>
        <Paxs>
            <Pax>
                <PaxID>PAX1</PaxID>
                <PTC>ADT</PTC>
            </Pax>
            <Pax>
                <PaxID>PAX2</PaxID>
                <PTC>ADT</PTC>
            </Pax>
        </Paxs>
        <ResponseParameters>
            <CurParameter>
                <RequestedCurCode>EUR</RequestedCurCode>
            </CurParameter>
            <LangUsage>
                <LangCode>en-GB</LangCode>
            </LangUsage>
        </ResponseParameters>
        <ShoppingCriteria>
            <FlightCriteria>
                <CabinType>
                    <CabinTypeName>ECONOMY</CabinTypeName>
                </CabinType>
            </FlightCriteria>
        </ShoppingCriteria>
    </Request>
</IATA_AirShoppingRQ>
```

</details>

<details>
  <summary><b>AirShoppingRQ - RoundTrip</b></summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRQ xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRQ">
    <Party>
        <Sender>
            <TravelAgency>
                <AgencyID>1234</AgencyID>
            </TravelAgency>
        </Sender>
    </Party>
    <PayloadAttributes>
        <CorrelationID>a222c960-0d2c-4507-bd2c-59362825cc76</CorrelationID>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
    <Request>
        <FlightCriteria>
            <OriginDestCriteria>
                <DestArrivalCriteria>
                    <IATA_LocationCode>JNB</IATA_LocationCode>
                </DestArrivalCriteria>
                <OriginDepCriteria>
                    <Date>2020-10-12</Date>
                    <IATA_LocationCode>CDG</IATA_LocationCode>
                </OriginDepCriteria>
            </OriginDestCriteria>
            <OriginDestCriteria>
                <DestArrivalCriteria>
                    <IATA_LocationCode>CDG</IATA_LocationCode>
                </DestArrivalCriteria>
                <OriginDepCriteria>
                    <Date>2020-10-23</Date>
                    <IATA_LocationCode>JNB</IATA_LocationCode>
                </OriginDepCriteria>
            </OriginDestCriteria>
        </FlightCriteria>
        <Paxs>
            <Pax>
                <PaxID>PAX1</PaxID>
                <PTC>ADT</PTC>
            </Pax>
            <Pax>
                <PaxID>PAX2</PaxID>
                <PTC>ADT</PTC>
            </Pax>
        </Paxs>
        <ResponseParameters>
            <CurParameter>
                <RequestedCurCode>EUR</RequestedCurCode>
            </CurParameter>
            <LangUsage>
                <LangCode>en-GB</LangCode>
            </LangUsage>
        </ResponseParameters>
        <ShoppingCriteria>
            <FlightCriteria>
                <CabinType>
                    <CabinTypeName>ECONOMY</CabinTypeName>
                </CabinType>
            </FlightCriteria>
        </ShoppingCriteria>
    </Request>
</IATA_AirShoppingRQ>
```

</details>

<details>
  <summary><b>AirShopping RS - Error</b></summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRS">
    <Error>
        <Code>ERR400</Code>
        <DescText>[{"errorCode":"ERR_C07","errorField":"[depPort]","explanation":"Missing Request Parameter"}]</DescText>
        <OwnerName>PEGASUS</OwnerName>
    </Error>
    <PayloadAttributes>
        <CorrelationID>011b118d-99d6-4101-af8b-3ea1874912f7</CorrelationID>
        <Timestamp>2020-09-30T17:13:13.121</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_AirShoppingRS>
```

</details>

<details>
  <summary><b>AirShopping RS - OneWay</b></summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRS">
    <Response>
        <DataLists>
            <BaggageAllowanceList>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA1</BaggageAllowanceID>
                    <PieceAllowance>
                        <ApplicablePartyText>Traveler</ApplicablePartyText>
                        <TotalQty>0</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA2</BaggageAllowanceID>
                    <PieceAllowance>
                        <ApplicablePartyText>Traveler</ApplicablePartyText>
                        <TotalQty>1</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
            </BaggageAllowanceList>
            <OriginDestList>
                <OriginDest>
                    <DestCode>MAD</DestCode>
                    <OriginCode>BCN</OriginCode>
                    <OriginDestID>OD1</OriginDestID>
                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ1</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ2</PaxJourneyID>
                    <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ3</PaxJourneyID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ4</PaxJourneyID>
                    <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ5</PaxJourneyID>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ6</PaxJourneyID>
                    <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ7</PaxJourneyID>
                    <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ8</PaxJourneyID>
                    <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ9</PaxJourneyID>
                    <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ10</PaxJourneyID>
                    <PaxSegmentRefID>SEG10</PaxSegmentRefID>
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
                        <AircraftScheduledDateTime>2020-10-06T11:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T09:50:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1004</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG1</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T14:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T12:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1006</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG2</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T17:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T16:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1014</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG3</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T23:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T21:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1028</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG4</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T10:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T08:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1002</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG5</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T15:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T14:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1022</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG6</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T19:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T17:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1008</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG7</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T20:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T19:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1010</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG8</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T08:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T07:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1018</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG9</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T08:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>MAD</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T07:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>BCN</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1016</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>VY</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG10</PaxSegmentID>
                </PaxSegment>
            </PaxSegmentList>
            <PriceClassList>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>10kg carry-on bag (55x40x20cm max)</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Personal item which must fit under the seat in front of the passenger (35x20x20cm max)</DescText>
                    </Desc>
                    <Name>BASIC PRODUCT CLASS</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>25kg checked-in bag</DescText>
                    </Desc>
                    <Desc>
                        <DescText>10kg carry-on bag (55x40x20cm max)</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Personal item which must fit under the seat in front of the passenger (35x20x20cm max)</DescText>
                    </Desc>
                    <Name>OPTIMA PRODUCT CLASS</Name>
                    <PriceClassID>PC2</PriceClassID>
                </PriceClass>
            </PriceClassList>
        </DataLists>
        <OffersGroup>
            <CarrierOffers>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>96296fe2-542d-430e-950e-4ddcf18e97cb</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>PB01</FareBasisCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">32.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">32.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>5de21c9c-fe1c-4bf6-9e59-d94fb552a339</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">64.98</BaseAmount>
                            <TotalAmount CurCode="EUR">64.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV1</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">64.98</BaseAmount>
                        <TotalAmount CurCode="EUR">64.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>f3b25cb6-6b55-4e2e-9d5b-c51ad071ef39</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>PB01</FareBasisCode>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">32.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">32.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>54451b3d-dd63-48a2-bb19-d25a5d66cfa5</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">64.98</BaseAmount>
                            <TotalAmount CurCode="EUR">64.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV2</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">64.98</BaseAmount>
                        <TotalAmount CurCode="EUR">64.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>551cd0bb-bde1-4cd3-9b8c-44665ba67631</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>PB01</FareBasisCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">32.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">32.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>dcc770ca-603c-49a5-a269-1b472f8c1b1d</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">64.98</BaseAmount>
                            <TotalAmount CurCode="EUR">64.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV3</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">64.98</BaseAmount>
                        <TotalAmount CurCode="EUR">64.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>bc6bf9cb-7c7b-4ce1-8461-0f7441b7b047</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>PB01</FareBasisCode>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">32.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">32.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>ff8f107c-7528-482b-82f3-81013daefc35</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">64.98</BaseAmount>
                            <TotalAmount CurCode="EUR">64.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV4</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">64.98</BaseAmount>
                        <TotalAmount CurCode="EUR">64.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>ddcda207-fec4-4560-9dbe-0f3da725d847</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>PB01</FareBasisCode>
                                <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">36.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">36.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>a4611c63-4899-4144-9378-4e0d75df4abd</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">72.98</BaseAmount>
                            <TotalAmount CurCode="EUR">72.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV5</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">72.98</BaseAmount>
                        <TotalAmount CurCode="EUR">72.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>1f49fb80-84db-423d-ba78-875420f5e6e7</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>PB01</FareBasisCode>
                                <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">36.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">36.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>436b408a-14a6-4e5e-867b-624d5389a18c</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">72.98</BaseAmount>
                            <TotalAmount CurCode="EUR">72.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV6</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">72.98</BaseAmount>
                        <TotalAmount CurCode="EUR">72.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>caf1b3aa-96ce-4cf6-8354-8e973af31037</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>PB01</FareBasisCode>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">36.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">36.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>089c6da4-7632-43ed-b440-17aad5c778cd</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">72.98</BaseAmount>
                            <TotalAmount CurCode="EUR">72.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV7</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">72.98</BaseAmount>
                        <TotalAmount CurCode="EUR">72.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>cf061175-82e7-4a61-8596-fd316d1cb85d</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>PB01</FareBasisCode>
                                <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">36.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">36.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>4387355e-c747-4fc0-b144-73511fcedc2d</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">72.98</BaseAmount>
                            <TotalAmount CurCode="EUR">72.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV8</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">72.98</BaseAmount>
                        <TotalAmount CurCode="EUR">72.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>5c29f12c-af6b-417b-8066-a23c8e1553fa</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>PB01</FareBasisCode>
                                <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">40.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">40.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>7d33a480-08f7-4bfd-9a66-8460f7932ba4</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">80.98</BaseAmount>
                            <TotalAmount CurCode="EUR">80.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV9</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">80.98</BaseAmount>
                        <TotalAmount CurCode="EUR">80.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>a3e2d66a-7228-433b-aec0-e5d35ff164b0</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>OPTI</FareBasisCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">47.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">47.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>da55dc32-b52c-452b-8d44-11ade872a7bf</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">94.98</BaseAmount>
                            <TotalAmount CurCode="EUR">94.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV10</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">94.98</BaseAmount>
                        <TotalAmount CurCode="EUR">94.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>e9a86ad4-32e7-4154-b7a3-8c08635caf6d</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>OPTI</FareBasisCode>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">47.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">47.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>358194d6-a823-42f5-9010-c2fcb7df4f06</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">94.98</BaseAmount>
                            <TotalAmount CurCode="EUR">94.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV11</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">94.98</BaseAmount>
                        <TotalAmount CurCode="EUR">94.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>7a8f3cf5-f237-4442-9502-cfb80b043430</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>OPTI</FareBasisCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">47.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">47.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>5ffb65be-37d6-427f-89e3-a86d88cf5cc4</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">94.98</BaseAmount>
                            <TotalAmount CurCode="EUR">94.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV12</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">94.98</BaseAmount>
                        <TotalAmount CurCode="EUR">94.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>6d683d27-391f-445f-96ab-46cba4cdf1f2</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>OPTI</FareBasisCode>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">47.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">47.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>2e8942d5-b87a-4f7a-bc51-84e995a6bd9c</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">94.98</BaseAmount>
                            <TotalAmount CurCode="EUR">94.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV13</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">94.98</BaseAmount>
                        <TotalAmount CurCode="EUR">94.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>460105fe-8434-4d56-b095-12c6548367f5</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>OPTI</FareBasisCode>
                                <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">51.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">51.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>73ce7525-6b4d-460e-9f4c-6e64c230c80b</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">102.98</BaseAmount>
                            <TotalAmount CurCode="EUR">102.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV14</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">102.98</BaseAmount>
                        <TotalAmount CurCode="EUR">102.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>398aa06b-1233-460a-9071-f4797c7f8d46</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>OPTI</FareBasisCode>
                                <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">51.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">51.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>c77cb2e2-26d2-4118-8a53-c57d99b4b09b</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">102.98</BaseAmount>
                            <TotalAmount CurCode="EUR">102.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV15</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">102.98</BaseAmount>
                        <TotalAmount CurCode="EUR">102.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>08caa10d-25bc-4c3f-94c3-3332abe5d506</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>OPTI</FareBasisCode>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">51.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">51.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>7cdbda05-20e6-40a1-81d2-cef8f4faf692</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">102.98</BaseAmount>
                            <TotalAmount CurCode="EUR">102.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV16</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">102.98</BaseAmount>
                        <TotalAmount CurCode="EUR">102.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>c2ae01ca-95b8-4d41-8bab-c1ce330cb6b6</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>OPTI</FareBasisCode>
                                <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">51.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">51.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>806aff68-31c5-4f82-bb11-fb962e63c39c</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">102.98</BaseAmount>
                            <TotalAmount CurCode="EUR">102.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV17</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">102.98</BaseAmount>
                        <TotalAmount CurCode="EUR">102.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>01808d0e-2ff2-48c3-ab88-d3e9c3579a14</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>OPTI</FareBasisCode>
                                <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">55.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">55.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>fd4f9d5f-2c6a-4e9e-bd13-992e09d530b5</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">110.98</BaseAmount>
                            <TotalAmount CurCode="EUR">110.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV18</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">110.98</BaseAmount>
                        <TotalAmount CurCode="EUR">110.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>766b1b68-ce3d-4724-87d7-86319879f2e5</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>PB01</FareBasisCode>
                                <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">279.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">279.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>04d3e5c9-8c1b-4c53-9687-98ce8ce1a545</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">558.98</BaseAmount>
                            <TotalAmount CurCode="EUR">558.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV19</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">558.98</BaseAmount>
                        <TotalAmount CurCode="EUR">558.98</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>48e036b6-9b15-4d79-b550-bfa9bc53ef1c</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>OPTI</FareBasisCode>
                                <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">305.49</BaseAmount>
                                    <TotalAmount CurCode="EUR">305.49</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>39a2094e-85ec-42d0-b3d8-bc2df0ce939d</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">610.98</BaseAmount>
                            <TotalAmount CurCode="EUR">610.98</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV20</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>VY</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">610.98</BaseAmount>
                        <TotalAmount CurCode="EUR">610.98</TotalAmount>
                    </TotalPrice>
                </Offer>
            </CarrierOffers>
        </OffersGroup>
        <ShoppingResponse>
            <ShoppingResponseRefID>bcb3f747-b0b1-4a29-9de9-cfebfba5034d</ShoppingResponseRefID>
        </ShoppingResponse>
    </Response>
    <PayloadAttributes>
        <CorrelationID>48e036b6-9b15-4d79-b550-bfa9bc53ef1c</CorrelationID>
        <Timestamp>2020-09-30T17:47:59.780</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_AirShoppingRS>
```

</details>

<details>
  <summary><b>AirShopping RS - RoundTrip (mode combination)</b></summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRS">
    <Response>
        <DataLists>
            <BaggageAllowanceList>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA1</BaggageAllowanceID>
                    <PieceAllowance>
                        <ApplicablePartyText>Traveler</ApplicablePartyText>
                        <TotalQty>0</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA2</BaggageAllowanceID>
                    <PieceAllowance>
                        <ApplicablePartyText>Traveler</ApplicablePartyText>
                        <TotalQty>1</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
            </BaggageAllowanceList>
            <OriginDestList>
                <OriginDest>
                    <DestCode>JNB</DestCode>
                    <OriginCode>CDG</OriginCode>
                    <OriginDestID>OD1</OriginDestID>
                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ15</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>CDG</DestCode>
                    <OriginCode>JNB</OriginCode>
                    <OriginDestID>OD2</OriginDestID>
                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ11</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ12</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ13</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT13H0M0S</Duration>
                    <PaxJourneyID>PJ1</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT10H45M0S</Duration>
                    <PaxJourneyID>PJ2</PaxJourneyID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT13H0M0S</Duration>
                    <PaxJourneyID>PJ3</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT14H10M0S</Duration>
                    <PaxJourneyID>PJ4</PaxJourneyID>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT14H10M0S</Duration>
                    <PaxJourneyID>PJ5</PaxJourneyID>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT13H0M0S</Duration>
                    <PaxJourneyID>PJ6</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT14H10M0S</Duration>
                    <PaxJourneyID>PJ7</PaxJourneyID>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT10H40M0S</Duration>
                    <PaxJourneyID>PJ8</PaxJourneyID>
                    <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT14H30M0S</Duration>
                    <PaxJourneyID>PJ9</PaxJourneyID>
                    <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT16H10M0S</Duration>
                    <PaxJourneyID>PJ10</PaxJourneyID>
                    <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT16H40M0S</Duration>
                    <PaxJourneyID>PJ11</PaxJourneyID>
                    <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG11</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT14H30M0S</Duration>
                    <PaxJourneyID>PJ12</PaxJourneyID>
                    <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG13</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT16H10M0S</Duration>
                    <PaxJourneyID>PJ13</PaxJourneyID>
                    <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG14</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT16H40M0S</Duration>
                    <PaxJourneyID>PJ14</PaxJourneyID>
                    <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG15</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT25H5M0S</Duration>
                    <PaxJourneyID>PJ15</PaxJourneyID>
                    <PaxSegmentRefID>SEG16</PaxSegmentRefID>
                    <PaxSegmentRefID>SEG17</PaxSegmentRefID>
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
                        <AircraftScheduledDateTime>2020-10-12T09:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>320</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T08:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2006</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG1</PaxSegmentID>
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
                            <CarrierAircraftTypeCode>77W</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T10:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>0138</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG2</PaxSegmentID>
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
                            <CarrierAircraftTypeCode>77W</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T10:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>0591</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG6</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-13T05:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>JNB</IATA_LocationCode>
                        <TerminalName>A</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>77W</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T18:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2E</TerminalName>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>0990</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG7</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-24T10:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>77W</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-23T23:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>JNB</IATA_LocationCode>
                        <TerminalName>B</TerminalName>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>8282</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG8</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-24T13:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>73H</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-24T12:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>8233</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG9</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-24T15:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-24T13:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1641</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG10</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-24T15:50:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>318</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-24T14:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1741</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG11</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-24T10:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>77W</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-23T23:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>JNB</IATA_LocationCode>
                        <TerminalName>B</TerminalName>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>0592</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG12</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-24T13:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>73H</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-24T12:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>1233</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG13</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-24T15:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>32A</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-24T13:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2009</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG14</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-24T15:50:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>318</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-24T14:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2013</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG15</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T21:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>73H</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T20:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>CDG</IATA_LocationCode>
                        <TerminalName>2F</TerminalName>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>8242</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG16</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-13T21:20:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>JNB</IATA_LocationCode>
                        <TerminalName>B</TerminalName>
                    </Arrival>
                    <DatedOperatingLeg>
                        <Arrival/>
                        <CarrierAircraftType>
                            <CarrierAircraftTypeCode>77W</CarrierAircraftTypeCode>
                        </CarrierAircraftType>
                        <Dep/>
                    </DatedOperatingLeg>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-13T10:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>AMS</IATA_LocationCode>
                    </Dep>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>AF</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>8283</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>KL</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG17</PaxSegmentID>
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
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>1 hand baggage item and 1 accessory (12 kg total*) and 1 check-in baggage item(s) (23 kg each) included</DescText>
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
                    <Name>Standard</Name>
                    <PriceClassID>PC2</PriceClassID>
                </PriceClass>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>1 hand baggage item and 1 accessory (12 kg total*) and 1 check-in baggage item(s) (23 kg each) included</DescText>
                    </Desc>
                    <Desc>
                        <DescText>SkyPriority benefits on flights operated by KLM or AIR FRANCE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Take an earlier or later flight</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Choice of standard seat</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Refund if you decide to cancel</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Free change + possible fare difference</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Refund if you missed your flight</DescText>
                    </Desc>
                    <Desc>
                        <DescText>* This applies to flights operated by KLM or Air France. For other airlines, please check the airline's website for baggage rules</DescText>
                    </Desc>
                    <Name>Flex</Name>
                    <PriceClassID>PC3</PriceClassID>
                </PriceClass>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>PREMIUM_ECONOMY</CabinTypeName>
                    </CabinType>
                    <Desc>
                        <DescText>2 hand baggage items and 1 accessory (18 kg total*) and 2 check-in baggage item(s) (23 kg each) included per passenger</DescText>
                    </Desc>
                    <Desc>
                        <DescText>SkyPriority benefits on flights operated by KLM or AIR FRANCE</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Choice of standard seat</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Cancellation is not possible</DescText>
                    </Desc>
                    <Desc>
                        <DescText>Free change + possible fare difference</DescText>
                    </Desc>
                    <Desc>
                        <DescText>No refund if you missed your flight</DescText>
                    </Desc>
                    <Desc>
                        <DescText>* This applies to flights operated by KLM or Air France. For other airlines, please check the airline's website for baggage rules</DescText>
                    </Desc>
                    <Name>Premium Economy</Name>
                    <PriceClassID>PC4</PriceClassID>
                </PriceClass>
            </PriceClassList>
        </DataLists>
        <OffersGroup>
            <CarrierOffers>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>3845f6dd-6c84-442e-bc06-4432f92f89b6</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>X</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>XL5VUIL1</FareBasisCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">151.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">277.66</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">428.66</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>0efd2b1d-3733-446e-9d6a-badb94b8fa00</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">302.00</BaseAmount>
                            <TotalAmount CurCode="EUR">857.32</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV1</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>AF</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">302.00</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">555.32</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">857.32</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>92bed895-a4c7-48c7-b8b4-fad2d5372ea5</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>X</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>XL5VUIL1</FareBasisCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">151.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">277.66</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">428.66</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>cb425372-bcd3-4289-8727-be737a9210ec</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">302.00</BaseAmount>
                            <TotalAmount CurCode="EUR">857.32</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV2</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>AF</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">302.00</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">555.32</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">857.32</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>afff8ccc-85c6-4e24-aedd-2d0a33d3ab71</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>X</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>XL5VUIL1</FareBasisCode>
                                <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">151.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">277.66</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">428.66</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>d3c577b9-fd57-42d2-b9ac-97eb58e2e0e3</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">302.00</BaseAmount>
                            <TotalAmount CurCode="EUR">857.32</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV3</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>AF</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">302.00</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">555.32</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">857.32</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>23bedc85-dd6a-482b-ac29-2f0c608ed478</OfferID>
                    <OfferItem>
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
                                    <BaseAmount CurCode="EUR">151.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">277.66</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">428.66</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>e99b73dc-16a1-4b9b-a8bc-f26b6299f5bb</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">302.00</BaseAmount>
                            <TotalAmount CurCode="EUR">857.32</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV4</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>AF</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">302.00</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">555.32</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">857.32</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA2</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC2</PriceClassRefID>
                        </JourneyPriceClass>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>b697e85f-93d3-4953-ba1d-5a54d4525e38</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>X</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>XL5VUMS1</FareBasisCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC2</PriceClassRefID>
                            </FareComponent>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>X</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>XL5VUIL1</FareBasisCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">191.00</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">277.66</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">468.66</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>c306bdd2-35b5-4b14-9af5-317a045c5267</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">382.00</BaseAmount>
                            <TotalAmount CurCode="EUR">937.32</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV5</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>AF</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">382.00</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">555.32</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">937.32</TotalAmount>
                    </TotalPrice>
                </Offer>
            </CarrierOffers>
        </OffersGroup>
        <ShoppingResponse>
            <ShoppingResponseRefID>2d62d243-8837-4e4d-a91c-45550a2fd6fa</ShoppingResponseRefID>
        </ShoppingResponse>
    </Response>
    <PayloadAttributes>
        <CorrelationID>a222c960-0d2c-4507-bd2c-59362825cc76</CorrelationID>
        <Timestamp>2020-09-30T17:37:13.616</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_AirShoppingRS>
```

</details>

<details>
  <summary><b>AirShopping RS - RoundTrip (mode flat)</b></summary>

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<IATA_AirShoppingRS xmlns="http://www.iata.org/IATA/2015/00/2019.2/IATA_AirShoppingRS">
    <Response>
        <DataLists>
            <BaggageAllowanceList>
                <BaggageAllowance>
                    <BaggageAllowanceID>BA1</BaggageAllowanceID>
                    <PieceAllowance>
                        <ApplicablePartyText>Traveler</ApplicablePartyText>
                        <TotalQty>1</TotalQty>
                    </PieceAllowance>
                    <TypeCode>Checked</TypeCode>
                </BaggageAllowance>
            </BaggageAllowanceList>
            <OriginDestList>
                <OriginDest>
                    <DestCode>ADA</DestCode>
                    <OriginCode>SAW</OriginCode>
                    <OriginDestID>OD1</OriginDestID>
                    <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                </OriginDest>
                <OriginDest>
                    <DestCode>SAW</DestCode>
                    <OriginCode>ADA</OriginCode>
                    <OriginDestID>OD2</OriginDestID>
                    <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ11</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ12</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ13</PaxJourneyRefID>
                    <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                </OriginDest>
            </OriginDestList>
            <PaxJourneyList>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ1</PaxJourneyID>
                    <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ2</PaxJourneyID>
                    <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ3</PaxJourneyID>
                    <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ4</PaxJourneyID>
                    <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <PaxJourneyID>PJ5</PaxJourneyID>
                    <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ6</PaxJourneyID>
                    <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ7</PaxJourneyID>
                    <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ8</PaxJourneyID>
                    <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ9</PaxJourneyID>
                    <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ10</PaxJourneyID>
                    <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ11</PaxJourneyID>
                    <PaxSegmentRefID>SEG11</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ12</PaxJourneyID>
                    <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ13</PaxJourneyID>
                    <PaxSegmentRefID>SEG13</PaxSegmentRefID>
                </PaxJourney>
                <PaxJourney>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <PaxJourneyID>PJ14</PaxJourneyID>
                    <PaxSegmentRefID>SEG14</PaxSegmentRefID>
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
                        <AircraftScheduledDateTime>2020-10-06T01:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T00:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2090</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG1</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T07:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T06:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2080</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG2</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T10:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T08:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2082</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG3</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T12:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T11:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2086</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG4</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T18:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T16:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H25M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2092</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG5</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-06T21:45:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T20:15:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2096</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG6</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-07T00:55:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-06T23:25:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2098</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG7</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T07:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T05:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2099</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG8</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T09:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T08:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2081</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG9</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T12:30:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T11:00:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2083</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG10</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T14:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T13:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2087</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG11</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T18:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T16:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2089</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG12</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-12T20:05:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T18:35:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2093</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG13</PaxSegmentID>
                </PaxSegment>
                <PaxSegment>
                    <Arrival>
                        <AircraftScheduledDateTime>2020-10-13T00:10:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>SAW</IATA_LocationCode>
                    </Arrival>
                    <Dep>
                        <AircraftScheduledDateTime>2020-10-12T22:40:00</AircraftScheduledDateTime>
                        <IATA_LocationCode>ADA</IATA_LocationCode>
                    </Dep>
                    <Duration>P0Y0M0DT1H30M0S</Duration>
                    <MarketingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                        <MarketingCarrierFlightNumberText>2097</MarketingCarrierFlightNumberText>
                    </MarketingCarrierInfo>
                    <OperatingCarrierInfo>
                        <CarrierDesigCode>PC</CarrierDesigCode>
                    </OperatingCarrierInfo>
                    <PaxSegmentID>SEG14</PaxSegmentID>
                </PaxSegment>
            </PaxSegmentList>
            <PriceClassList>
                <PriceClass>
                    <CabinType>
                        <CabinTypeName>ECONOMY</CabinTypeName>
                    </CabinType>
                    <Name>ECONOMY</Name>
                    <PriceClassID>PC1</PriceClassID>
                </PriceClass>
            </PriceClassList>
        </DataLists>
        <OffersGroup>
            <CarrierOffers>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>1d6f03a6-3568-487d-a603-8a26fc69ddf2</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG1</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>2bff1296-3e3b-4e1a-bf07-25d3bf755c58</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ1</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV1</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>eedef893-2755-444d-8306-64508ebc0cf6</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG2</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>34e18eb0-4c17-491d-ac67-4d10e1583ab6</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ2</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV2</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>edd88984-9beb-4912-a91c-096efc8ce241</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG3</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>7709256e-c894-49d0-818b-73b7ac3c3d1a</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ3</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV3</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>a0a443bb-16ea-4e03-8e83-a0e687e503a1</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG4</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>b2743ed4-9e87-4ceb-ac80-aed1ae88fd56</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ4</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV4</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>ab0a3725-2e9e-4ae4-9fcf-5ff4dd786eb1</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG5</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>0754ec29-14dd-421f-a141-b3f45d0d6365</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ5</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV5</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>75c6f2fb-69f1-4b37-a22c-e71de9b22a00</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG6</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>8fb5a37c-5912-4353-b60d-63c35cf601c1</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ6</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV6</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>f6c6df39-126d-441a-b7ea-0f581e6a8acd</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>R</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>ROWKLTR</FareBasisCode>
                                <PaxSegmentRefID>SEG7</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">3.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">24.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>2a28dbfc-1d5b-4d24-9ea2-af0a5ca0535d</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">48.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ7</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV7</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">6.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">48.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>8203e42a-fbab-4a42-9b69-78adb1d67e86</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG8</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>e232a1c6-529e-43f8-863b-b8c7a018995f</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ8</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV8</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>8feb63a6-4072-4c77-9023-d84bdfeda6e6</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG9</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>d7c9e00e-aa95-4dfb-bee5-fb75087a0cbe</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ9</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV9</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>de99749d-bd46-4d4f-918e-21f76c6f98e2</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG10</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>99fca37d-4fd6-4672-81fd-8421d54f0395</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ10</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV10</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG11</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ11</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>5dfac5fb-a4e9-4710-a21c-b0e72e02ea14</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG11</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>af0db36f-4bf8-4322-9912-251e82f0046e</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ11</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV11</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ12</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>0c301349-93a5-4521-a618-d074c75f2420</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG12</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>85785f18-a3a1-43c6-8753-621acf971d48</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ12</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV12</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG13</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ13</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>c5f3730e-5c0a-4806-bfd5-4687773fdf3a</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG13</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>548530e2-8e97-407a-b070-09e376bc9b78</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ13</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV13</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
                <Offer>
                    <BaggageAllowance>
                        <BaggageAllowanceRefID>BA1</BaggageAllowanceRefID>
                        <BaggageFlightAssociations>
                            <PaxSegmentRefID>SEG14</PaxSegmentRefID>
                        </BaggageFlightAssociations>
                        <PaxRefID>PAX1</PaxRefID>
                        <PaxRefID>PAX2</PaxRefID>
                    </BaggageAllowance>
                    <JourneyOverview>
                        <JourneyPriceClass>
                            <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                            <PriceClassRefID>PC1</PriceClassRefID>
                        </JourneyPriceClass>
                    </JourneyOverview>
                    <OfferID>0471c8e3-1593-40a9-8d54-d3a9059de19e</OfferID>
                    <OfferItem>
                        <FareDetail>
                            <FareComponent>
                                <CabinType>
                                    <CabinTypeCode>D</CabinTypeCode>
                                    <CabinTypeName>ECONOMY</CabinTypeName>
                                </CabinType>
                                <FareBasisCode>DOW/NTRA</FareBasisCode>
                                <PaxSegmentRefID>SEG14</PaxSegmentRefID>
                                <PriceClassRefID>PC1</PriceClassRefID>
                            </FareComponent>
                            <FarePriceType>
                                <Price>
                                    <BaseAmount CurCode="EUR">21.0</BaseAmount>
                                    <TaxSummary>
                                        <TotalTaxAmount CurCode="EUR">1.0</TotalTaxAmount>
                                    </TaxSummary>
                                    <TotalAmount CurCode="EUR">22.0</TotalAmount>
                                </Price>
                            </FarePriceType>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                        </FareDetail>
                        <OfferItemID>8c567488-3697-4293-b36a-a028f1a6b688</OfferItemID>
                        <Price>
                            <BaseAmount CurCode="EUR">42.0</BaseAmount>
                            <TotalAmount CurCode="EUR">44.0</TotalAmount>
                        </Price>
                        <Service>
                            <PaxRefID>PAX1</PaxRefID>
                            <PaxRefID>PAX2</PaxRefID>
                            <ServiceAssociations>
                                <PaxJourneyRefID>PJ14</PaxJourneyRefID>
                            </ServiceAssociations>
                            <ServiceID>SV14</ServiceID>
                        </Service>
                    </OfferItem>
                    <OwnerCode>PC</OwnerCode>
                    <TotalPrice>
                        <BaseAmount CurCode="EUR">42.0</BaseAmount>
                        <TaxSummary>
                            <TotalTaxAmount CurCode="EUR">2.0</TotalTaxAmount>
                        </TaxSummary>
                        <TotalAmount CurCode="EUR">44.0</TotalAmount>
                    </TotalPrice>
                </Offer>
            </CarrierOffers>
        </OffersGroup>
        <ShoppingResponse>
            <ShoppingResponseRefID>18848792-3c79-4e58-882e-f53523817470</ShoppingResponseRefID>
        </ShoppingResponse>
    </Response>
    <PayloadAttributes>
        <CorrelationID>8cdc4aef-6d24-4672-b6f0-8c454d135270</CorrelationID>
        <Timestamp>2020-09-30T17:47:18.947</Timestamp>
        <VersionNumber>19.2</VersionNumber>
    </PayloadAttributes>
</IATA_AirShoppingRS>
```

</details>
