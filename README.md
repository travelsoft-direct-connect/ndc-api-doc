---
description: Example with XML samples
xml:
- AirShoppingRQ
- AirShoppingRS
---

# With XML samples

<details>
  <summary><b>Air Shopping RQ</b></summary>

```xml
  <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:get="http://www.iata.org/IATA/EDIST/2017.2">
    <soapenv:Header/>
    <soapenv:Body>
        <AirShoppingRQ Version="17.2" PrimaryLangID="EN" AltLangID="EN" xmlns="http://www.iata.org/IATA/EDIST/2017.2">
            <Document>
                <Name>BA</Name>
            </Document>
            <Party>
                <Sender>
                    <TravelAgencySender>
                        <IATA_Number>000000</IATA_Number>
                        <AgencyID>Test_Agent</AgencyID>
                    </TravelAgencySender>
                </Sender>
            </Party>
            <CoreQuery>
                <OriginDestinations>
                    <OriginDestination>
                        <Departure>
                            <AirportCode>LHR</AirportCode>
                            <Date>2018-10-15</Date>
                        </Departure>
                        <Arrival>
                            <AirportCode>JFK</AirportCode>
                        </Arrival>
                        <CalendarDates DaysBefore="3" DaysAfter="3"/>
                    </OriginDestination>
                </OriginDestinations>
            </CoreQuery>
            <Preference>
                <FarePreferences>
                    <Types>
                        <Type>759</Type>
                    </Types>
                </FarePreferences>
                <FlightPreferences>
                    <Characteristic>
                        <DirectPreferences/>
                    </Characteristic>
                </FlightPreferences>
                <CabinPreferences>
                    <CabinType>
                        <Code>5</Code>
                    </CabinType>
                </CabinPreferences>
            </Preference>
            <DataLists>
                <PassengerList>
                    <Passenger PassengerID="T1">
                        <PTC>ADT</PTC>
                        <Individual>
                            <Birthdate>1970-01-01</Birthdate>
                            <Gender>Male</Gender>
                            <NameTitle>Mr</NameTitle>
                            <GivenName>Frequent</GivenName>
                            <Surname>Flyer</Surname>
                        </Individual>
                        <LoyaltyProgramAccount>
                            <Airline>
                                <AirlineDesignator>BA</AirlineDesignator>
                            </Airline>
                            <AccountNumber>12345678</AccountNumber>
                        </LoyaltyProgramAccount>
                    </Passenger>
                    <Passenger PassengerID="T2">
                        <PTC>ADT</PTC>
                        <Birthdate>2006-02-18</Birthdate>
                    </Passenger>
                    <Passenger PassengerID="T3">
                        <PTC>CHD</PTC>
                    </Passenger>
                    <Passenger PassengerID="T4">
                        <PTC>INF</PTC>
                    </Passenger>
                </PassengerList>
            </DataLists>
        </AirShoppingRQ>
    </soapenv:Body>
  </soapenv:Envelope>
```

</details>

<details>
  <summary><b>Air Shopping RS</b></summary>

  ```xml
    <soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
        <soap:Body>
            <AirShoppingRS Version="IATA_VERSION_2017_2" xmlns="http://www.iata.org/IATA/EDIST/2017.2">
                <Document>
                    <Name>BA</Name>
                </Document>
                <Success/>
                <ShoppingResponseID>
                    <ResponseID>tx-08-201-33692e77-dcdb-4d41-8795-6be2029d637c</ResponseID>
                </ShoppingResponseID>
                <OffersGroup>
                    <AirlineOffers>
                        <AirlineOfferSnapshot>
                            <PassengerQuantity>4</PassengerQuantity>
                        </AirlineOfferSnapshot>
                        <Offer OfferID="OF-adfc1e7b-4f82-4969-ac77-6b36a4cc647a" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">4424.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-adfc1e7b-4f82-4969-ac77-6b36a4cc647a-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1566.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-adfc1e7b-4f82-4969-ac77-6b36a4cc647a-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1566.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1315.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">251.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_1</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-adfc1e7b-4f82-4969-ac77-6b36a4cc647a-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1488.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-adfc1e7b-4f82-4969-ac77-6b36a4cc647a-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1488.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1315.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">173.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_1</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-adfc1e7b-4f82-4969-ac77-6b36a4cc647a-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1159.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-adfc1e7b-4f82-4969-ac77-6b36a4cc647a-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1159.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">986.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">173.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_1</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-adfc1e7b-4f82-4969-ac77-6b36a4cc647a-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">209.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-adfc1e7b-4f82-4969-ac77-6b36a4cc647a-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">209.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">131.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_1</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <Offer OfferID="OF-17374afe-6f85-4007-899e-c79bdba0fd7d" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">4424.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-17374afe-6f85-4007-899e-c79bdba0fd7d-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1566.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-17374afe-6f85-4007-899e-c79bdba0fd7d-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1566.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1315.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">251.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_1</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-17374afe-6f85-4007-899e-c79bdba0fd7d-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1488.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-17374afe-6f85-4007-899e-c79bdba0fd7d-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1488.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1315.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">173.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_1</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-17374afe-6f85-4007-899e-c79bdba0fd7d-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1159.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-17374afe-6f85-4007-899e-c79bdba0fd7d-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1159.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">986.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">173.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_1</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-17374afe-6f85-4007-899e-c79bdba0fd7d-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">209.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-17374afe-6f85-4007-899e-c79bdba0fd7d-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">209.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">131.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_1</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <Offer OfferID="OF-1b4bfaf0-c0ca-4f3b-8b9a-64d291394c73" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">4424.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-1b4bfaf0-c0ca-4f3b-8b9a-64d291394c73-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1566.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-1b4bfaf0-c0ca-4f3b-8b9a-64d291394c73-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1566.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1315.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">251.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_2</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-1b4bfaf0-c0ca-4f3b-8b9a-64d291394c73-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1488.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-1b4bfaf0-c0ca-4f3b-8b9a-64d291394c73-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1488.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1315.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">173.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_2</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-1b4bfaf0-c0ca-4f3b-8b9a-64d291394c73-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1159.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-1b4bfaf0-c0ca-4f3b-8b9a-64d291394c73-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1159.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">986.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">173.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_2</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-1b4bfaf0-c0ca-4f3b-8b9a-64d291394c73-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">209.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-1b4bfaf0-c0ca-4f3b-8b9a-64d291394c73-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">209.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">131.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_2</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <Offer OfferID="OF-67373742-343c-4551-a2e7-a4d538f7a6c3" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">4424.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-67373742-343c-4551-a2e7-a4d538f7a6c3-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1566.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-67373742-343c-4551-a2e7-a4d538f7a6c3-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight4</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1566.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1315.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">251.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_2</PriceClassRef>
                                        <SegmentRefs>BA1593</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-67373742-343c-4551-a2e7-a4d538f7a6c3-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1488.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-67373742-343c-4551-a2e7-a4d538f7a6c3-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight4</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1488.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1315.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">173.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_2</PriceClassRef>
                                        <SegmentRefs>BA1593</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-67373742-343c-4551-a2e7-a4d538f7a6c3-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1159.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-67373742-343c-4551-a2e7-a4d538f7a6c3-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight4</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1159.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">986.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">173.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_2</PriceClassRef>
                                        <SegmentRefs>BA1593</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-67373742-343c-4551-a2e7-a4d538f7a6c3-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">209.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-67373742-343c-4551-a2e7-a4d538f7a6c3-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight4</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">209.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">131.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_2</PriceClassRef>
                                        <SegmentRefs>BA1593</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <Offer OfferID="OF-10d8857e-d147-4dd3-829d-e08387b41e2d" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">5524.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-10d8857e-d147-4dd3-829d-e08387b41e2d-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-10d8857e-d147-4dd3-829d-e08387b41e2d-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1564.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">329.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_3</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-10d8857e-d147-4dd3-829d-e08387b41e2d-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-10d8857e-d147-4dd3-829d-e08387b41e2d-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1564.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">329.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_3</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-10d8857e-d147-4dd3-829d-e08387b41e2d-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1502.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-10d8857e-d147-4dd3-829d-e08387b41e2d-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1502.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1173.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">329.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_3</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-10d8857e-d147-4dd3-829d-e08387b41e2d-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">234.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-10d8857e-d147-4dd3-829d-e08387b41e2d-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">234.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">156.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_3</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <Offer OfferID="OF-833eed72-046c-4dd5-86ff-9bf0c3dcc702" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">5524.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-833eed72-046c-4dd5-86ff-9bf0c3dcc702-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-833eed72-046c-4dd5-86ff-9bf0c3dcc702-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1564.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">329.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_3</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-833eed72-046c-4dd5-86ff-9bf0c3dcc702-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-833eed72-046c-4dd5-86ff-9bf0c3dcc702-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1564.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">329.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_3</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-833eed72-046c-4dd5-86ff-9bf0c3dcc702-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1502.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-833eed72-046c-4dd5-86ff-9bf0c3dcc702-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1502.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1173.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">329.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_3</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-833eed72-046c-4dd5-86ff-9bf0c3dcc702-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">234.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-833eed72-046c-4dd5-86ff-9bf0c3dcc702-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">234.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">156.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_3</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <Offer OfferID="OF-62a6555c-728a-43bf-b454-b37c647a79b1" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">5524.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-62a6555c-728a-43bf-b454-b37c647a79b1-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-62a6555c-728a-43bf-b454-b37c647a79b1-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1564.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">329.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_4</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-62a6555c-728a-43bf-b454-b37c647a79b1-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-62a6555c-728a-43bf-b454-b37c647a79b1-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1893.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1564.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">329.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_4</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-62a6555c-728a-43bf-b454-b37c647a79b1-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">1502.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-62a6555c-728a-43bf-b454-b37c647a79b1-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">1502.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">1173.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">329.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_4</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-62a6555c-728a-43bf-b454-b37c647a79b1-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">234.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-62a6555c-728a-43bf-b454-b37c647a79b1-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">234.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">156.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_4</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <Offer OfferID="OF-f77db34f-041b-4f8a-82c8-419cc7b5542f" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">17811.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-f77db34f-041b-4f8a-82c8-419cc7b5542f-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-f77db34f-041b-4f8a-82c8-419cc7b5542f-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">5830.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_5</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-f77db34f-041b-4f8a-82c8-419cc7b5542f-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-f77db34f-041b-4f8a-82c8-419cc7b5542f-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">5830.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_5</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-f77db34f-041b-4f8a-82c8-419cc7b5542f-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">4744.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-f77db34f-041b-4f8a-82c8-419cc7b5542f-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">4744.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">4372.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_5</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-f77db34f-041b-4f8a-82c8-419cc7b5542f-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">661.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-f77db34f-041b-4f8a-82c8-419cc7b5542f-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight1</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">661.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">583.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_5</PriceClassRef>
                                        <SegmentRefs>BA0183</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <Offer OfferID="OF-f6dad278-4ce5-41d9-858a-5ebef44c746a" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">17811.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-f6dad278-4ce5-41d9-858a-5ebef44c746a-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-f6dad278-4ce5-41d9-858a-5ebef44c746a-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">5830.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_5</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-f6dad278-4ce5-41d9-858a-5ebef44c746a-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-f6dad278-4ce5-41d9-858a-5ebef44c746a-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">5830.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_5</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-f6dad278-4ce5-41d9-858a-5ebef44c746a-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">4744.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-f6dad278-4ce5-41d9-858a-5ebef44c746a-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">4744.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">4372.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_5</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-f6dad278-4ce5-41d9-858a-5ebef44c746a-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">661.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-f6dad278-4ce5-41d9-858a-5ebef44c746a-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight2</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">661.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">583.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_5</PriceClassRef>
                                        <SegmentRefs>BA0179</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <Offer OfferID="OF-4c40e0d0-1e87-41c4-aeac-d9a4e764a37a" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">17811.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-4c40e0d0-1e87-41c4-aeac-d9a4e764a37a-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-4c40e0d0-1e87-41c4-aeac-d9a4e764a37a-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">5830.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_6</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-4c40e0d0-1e87-41c4-aeac-d9a4e764a37a-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-4c40e0d0-1e87-41c4-aeac-d9a4e764a37a-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">5830.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_6</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-4c40e0d0-1e87-41c4-aeac-d9a4e764a37a-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">4744.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-4c40e0d0-1e87-41c4-aeac-d9a4e764a37a-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">4744.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">4372.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_6</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-4c40e0d0-1e87-41c4-aeac-d9a4e764a37a-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">661.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-4c40e0d0-1e87-41c4-aeac-d9a4e764a37a-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight3</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">661.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">583.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_6</PriceClassRef>
                                        <SegmentRefs>BA1510</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <Offer OfferID="OF-71a2d0f3-3701-4366-b614-7c34f6b7b00d" Owner="BA" RequestedDateInd="true">
                            <OfferExpirationDateTime>2018-10-15T16:29:13.526Z</OfferExpirationDateTime>
                            <PaymentTimeLimitDateTime>2018-10-18T23:59:59.000Z</PaymentTimeLimitDateTime>
                            <TotalPrice>
                                <SimpleCurrencyPrice Code="GBP">17811.14</SimpleCurrencyPrice>
                            </TotalPrice>
                            <OfferItem OfferItemID="OF-71a2d0f3-3701-4366-b614-7c34f6b7b00d-OI-1">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-71a2d0f3-3701-4366-b614-7c34f6b7b00d-OI-1-SVC-1">
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <FlightRefs>Flight4</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH1</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">5830.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_6</PriceClassRef>
                                        <SegmentRefs>BA1593</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-71a2d0f3-3701-4366-b614-7c34f6b7b00d-OI-2">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-71a2d0f3-3701-4366-b614-7c34f6b7b00d-OI-2-SVC-1">
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <FlightRefs>Flight4</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH2</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">6202.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">5830.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_6</PriceClassRef>
                                        <SegmentRefs>BA1593</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-71a2d0f3-3701-4366-b614-7c34f6b7b00d-OI-3">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">4744.41</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-71a2d0f3-3701-4366-b614-7c34f6b7b00d-OI-3-SVC-1">
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <FlightRefs>Flight4</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH3</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">4744.41</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">4372.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">372.41</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_6</PriceClassRef>
                                        <SegmentRefs>BA1593</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                            <OfferItem OfferItemID="OF-71a2d0f3-3701-4366-b614-7c34f6b7b00d-OI-4">
                                <TotalPriceDetail>
                                    <TotalAmount>
                                        <SimpleCurrencyPrice Code="GBP">661.91</SimpleCurrencyPrice>
                                    </TotalAmount>
                                </TotalPriceDetail>
                                <Service ServiceID="OF-71a2d0f3-3701-4366-b614-7c34f6b7b00d-OI-4-SVC-1">
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <FlightRefs>Flight4</FlightRefs>
                                </Service>
                                <FareDetail>
                                    <PassengerRefs>SH4</PassengerRefs>
                                    <Price>
                                        <TotalAmount>
                                            <SimpleCurrencyPrice Code="GBP">661.91</SimpleCurrencyPrice>
                                        </TotalAmount>
                                        <BaseAmount Code="GBP">583.00</BaseAmount>
                                        <Taxes>
                                            <Total Code="GBP">78.91</Total>
                                        </Taxes>
                                    </Price>
                                    <FareComponent>
                                        <PriceClassRef>PCR_6</PriceClassRef>
                                        <SegmentRefs>BA1593</SegmentRefs>
                                    </FareComponent>
                                </FareDetail>
                            </OfferItem>
                        </Offer>
                        <PriceCalendar>
                            <PriceCalendarDate OriginDestinationReference="OD1">2018-10-16</PriceCalendarDate>
                            <TotalPrice Code="GBP">1564.46</TotalPrice>
                        </PriceCalendar>
                        <PriceCalendar>
                            <PriceCalendarDate OriginDestinationReference="OD1">2018-10-17</PriceCalendarDate>
                            <TotalPrice Code="GBP">1564.46</TotalPrice>
                        </PriceCalendar>
                        <PriceCalendar>
                            <PriceCalendarDate OriginDestinationReference="OD1">2018-10-18</PriceCalendarDate>
                            <TotalPrice Code="GBP">1564.46</TotalPrice>
                        </PriceCalendar>
                    </AirlineOffers>
                </OffersGroup>
                <DataLists>
                    <PassengerList>
                        <Passenger PassengerID="SH1">
                            <PTC>ADT</PTC>
                        </Passenger>
                        <Passenger PassengerID="SH2">
                            <PTC>ADT</PTC>
                            <Birthdate>2006-02-18</Birthdate>
                        </Passenger>
                        <Passenger PassengerID="SH3">
                            <PTC>CHD</PTC>
                        </Passenger>
                        <Passenger PassengerID="SH4">
                            <PTC>INF</PTC>
                        </Passenger>
                    </PassengerList>
                    <FareList>
                        <FareGroup ListKey="FBCODE1ADT">
                            <Fare>
                                <FareCode>70J</FareCode>
                            </Fare>
                            <FareBasisCode>
                                <Code>Y1N0C9M0/Y</Code>
                            </FareBasisCode>
                        </FareGroup>
                        <FareGroup ListKey="FBCODE2ADT">
                            <Fare>
                                <FareCode>70J</FareCode>
                            </Fare>
                            <FareBasisCode>
                                <Code>W1N0C9S0/Y</Code>
                            </FareBasisCode>
                        </FareGroup>
                        <FareGroup ListKey="FBCODE3ADT">
                            <Fare>
                                <FareCode>70J</FareCode>
                            </Fare>
                            <FareBasisCode>
                                <Code>J1N0C9S0/Y</Code>
                            </FareBasisCode>
                        </FareGroup>
                    </FareList>
                    <FlightSegmentList>
                        <FlightSegment SegmentKey="BA0183">
                            <Departure>
                                <AirportCode>LHR</AirportCode>
                                <Date>2018-10-15</Date>
                                <Time>19:50</Time>
                                <AirportName>HEATHROW</AirportName>
                                <Terminal>
                                    <Name>5</Name>
                                </Terminal>
                            </Departure>
                            <Arrival>
                                <AirportCode>JFK</AirportCode>
                                <Date>2018-10-15</Date>
                                <Time>22:30</Time>
                                <AirportName>JOHN F KENNEDY</AirportName>
                                <Terminal>
                                    <Name>7</Name>
                                </Terminal>
                            </Arrival>
                            <MarketingCarrier>
                                <AirlineID>BA</AirlineID>
                                <Name>British Airways</Name>
                                <FlightNumber>0183</FlightNumber>
                            </MarketingCarrier>
                            <OperatingCarrier>
                                <AirlineID>BA</AirlineID>
                                <Name>British Airways</Name>
                            </OperatingCarrier>
                            <Equipment>
                                <AircraftCode>744</AircraftCode>
                                <Name>Boeing 747 jet</Name>
                            </Equipment>
                            <FlightDetail>
                                <FlightDuration>
                                    <Value>PT7H40M</Value>
                                </FlightDuration>
                                <Stops>
                                    <StopQuantity>0</StopQuantity>
                                </Stops>
                            </FlightDetail>
                        </FlightSegment>
                        <FlightSegment SegmentKey="BA0179">
                            <Departure>
                                <AirportCode>LHR</AirportCode>
                                <Date>2018-10-15</Date>
                                <Time>18:05</Time>
                                <AirportName>HEATHROW</AirportName>
                                <Terminal>
                                    <Name>5</Name>
                                </Terminal>
                            </Departure>
                            <Arrival>
                                <AirportCode>JFK</AirportCode>
                                <Date>2018-10-15</Date>
                                <Time>21:00</Time>
                                <AirportName>JOHN F KENNEDY</AirportName>
                                <Terminal>
                                    <Name>7</Name>
                                </Terminal>
                            </Arrival>
                            <MarketingCarrier>
                                <AirlineID>BA</AirlineID>
                                <Name>British Airways</Name>
                                <FlightNumber>0179</FlightNumber>
                            </MarketingCarrier>
                            <OperatingCarrier>
                                <AirlineID>BA</AirlineID>
                                <Name>British Airways</Name>
                            </OperatingCarrier>
                            <Equipment>
                                <AircraftCode>777</AircraftCode>
                                <Name>Boeing 777 jet</Name>
                            </Equipment>
                            <FlightDetail>
                                <FlightDuration>
                                    <Value>PT7H55M</Value>
                                </FlightDuration>
                                <Stops>
                                    <StopQuantity>0</StopQuantity>
                                </Stops>
                            </FlightDetail>
                        </FlightSegment>
                        <FlightSegment SegmentKey="BA1510">
                            <Departure>
                                <AirportCode>LHR</AirportCode>
                                <Date>2018-10-15</Date>
                                <Time>17:00</Time>
                                <AirportName>HEATHROW</AirportName>
                                <Terminal>
                                    <Name>3</Name>
                                </Terminal>
                            </Departure>
                            <Arrival>
                                <AirportCode>JFK</AirportCode>
                                <Date>2018-10-15</Date>
                                <Time>20:00</Time>
                                <AirportName>JOHN F KENNEDY</AirportName>
                                <Terminal>
                                    <Name>8</Name>
                                </Terminal>
                            </Arrival>
                            <MarketingCarrier>
                                <AirlineID>BA</AirlineID>
                                <Name>British Airways</Name>
                                <FlightNumber>1510</FlightNumber>
                            </MarketingCarrier>
                            <OperatingCarrier>
                                <AirlineID>AA</AirlineID>
                                <Name>American Airlines</Name>
                            </OperatingCarrier>
                            <Equipment>
                                <AircraftCode>77W</AircraftCode>
                                <Name>Boeing 777 jet</Name>
                            </Equipment>
                            <FlightDetail>
                                <FlightDuration>
                                    <Value>PT8H</Value>
                                </FlightDuration>
                                <Stops>
                                    <StopQuantity>0</StopQuantity>
                                </Stops>
                            </FlightDetail>
                        </FlightSegment>
                        <FlightSegment SegmentKey="BA1593">
                            <Departure>
                                <AirportCode>LHR</AirportCode>
                                <Date>2018-10-15</Date>
                                <Time>19:30</Time>
                                <AirportName>HEATHROW</AirportName>
                                <Terminal>
                                    <Name>3</Name>
                                </Terminal>
                            </Departure>
                            <Arrival>
                                <AirportCode>JFK</AirportCode>
                                <Date>2018-10-15</Date>
                                <Time>22:30</Time>
                                <AirportName>JOHN F KENNEDY</AirportName>
                                <Terminal>
                                    <Name>8</Name>
                                </Terminal>
                            </Arrival>
                            <MarketingCarrier>
                                <AirlineID>BA</AirlineID>
                                <Name>British Airways</Name>
                                <FlightNumber>1593</FlightNumber>
                            </MarketingCarrier>
                            <OperatingCarrier>
                                <AirlineID>AA</AirlineID>
                                <Name>American Airlines</Name>
                            </OperatingCarrier>
                            <Equipment>
                                <AircraftCode>772</AircraftCode>
                                <Name>Boeing 777 jet</Name>
                            </Equipment>
                            <FlightDetail>
                                <FlightDuration>
                                    <Value>PT8H</Value>
                                </FlightDuration>
                                <Stops>
                                    <StopQuantity>0</StopQuantity>
                                </Stops>
                            </FlightDetail>
                        </FlightSegment>
                    </FlightSegmentList>
                    <FlightList>
                        <Flight FlightKey="Flight1">
                            <Journey>
                                <Time>PT7H40M</Time>
                            </Journey>
                            <SegmentReferences>BA0183</SegmentReferences>
                        </Flight>
                        <Flight FlightKey="Flight2">
                            <Journey>
                                <Time>PT7H55M</Time>
                            </Journey>
                            <SegmentReferences>BA0179</SegmentReferences>
                        </Flight>
                        <Flight FlightKey="Flight3">
                            <Journey>
                                <Time>PT8H</Time>
                            </Journey>
                            <SegmentReferences>BA1510</SegmentReferences>
                        </Flight>
                        <Flight FlightKey="Flight4">
                            <Journey>
                                <Time>PT8H</Time>
                            </Journey>
                            <SegmentReferences>BA1593</SegmentReferences>
                        </Flight>
                    </FlightList>
                    <OriginDestinationList>
                        <OriginDestination OriginDestinationKey="OD1">
                            <DepartureCode>LON</DepartureCode>
                            <ArrivalCode>NYC</ArrivalCode>
                            <FlightReferences>Flight1 Flight2 Flight3 Flight4</FlightReferences>
                        </OriginDestination>
                    </OriginDestinationList>
                    <PriceClassList>
                        <PriceClass PriceClassID="PCR_1">
                            <Name>World Traveller</Name>
                            <ClassOfService refs="BA0183 FBCODE1ADT">
                                <Code SeatsLeft="9">Y</Code>
                                <MarketingName>World Traveller</MarketingName>
                            </ClassOfService>
                            <ClassOfService refs="BA0179 FBCODE1ADT">
                                <Code SeatsLeft="9">Y</Code>
                                <MarketingName>World Traveller</MarketingName>
                            </ClassOfService>
                        </PriceClass>
                        <PriceClass PriceClassID="PCR_2">
                            <Name>ECONOMY</Name>
                            <ClassOfService refs="BA1510 FBCODE1ADT">
                                <Code SeatsLeft="7">Y</Code>
                                <MarketingName>ECONOMY</MarketingName>
                            </ClassOfService>
                            <ClassOfService refs="BA1593 FBCODE1ADT">
                                <Code SeatsLeft="7">Y</Code>
                                <MarketingName>ECONOMY</MarketingName>
                            </ClassOfService>
                        </PriceClass>
                        <PriceClass PriceClassID="PCR_3">
                            <Name>World Traveller Plus</Name>
                            <ClassOfService refs="BA0183 FBCODE2ADT">
                                <Code SeatsLeft="9">W</Code>
                                <MarketingName>World Traveller Plus</MarketingName>
                            </ClassOfService>
                            <ClassOfService refs="BA0179 FBCODE2ADT">
                                <Code SeatsLeft="9">W</Code>
                                <MarketingName>World Traveller Plus</MarketingName>
                            </ClassOfService>
                        </PriceClass>
                        <PriceClass PriceClassID="PCR_4">
                            <Name>PREMIUMECONOMY</Name>
                            <ClassOfService refs="BA1510 FBCODE2ADT">
                                <Code SeatsLeft="7">W</Code>
                                <MarketingName>PREMIUMECONOMY</MarketingName>
                            </ClassOfService>
                        </PriceClass>
                        <PriceClass PriceClassID="PCR_5">
                            <Name>Club World</Name>
                            <ClassOfService refs="BA0183 FBCODE3ADT">
                                <Code SeatsLeft="9">J</Code>
                                <MarketingName>Club World</MarketingName>
                            </ClassOfService>
                            <ClassOfService refs="BA0179 FBCODE3ADT">
                                <Code SeatsLeft="9">J</Code>
                                <MarketingName>Club World</MarketingName>
                            </ClassOfService>
                        </PriceClass>
                        <PriceClass PriceClassID="PCR_6">
                            <Name>BUSINESS</Name>
                            <ClassOfService refs="BA1510 FBCODE3ADT">
                                <Code SeatsLeft="7">J</Code>
                                <MarketingName>BUSINESS</MarketingName>
                            </ClassOfService>
                            <ClassOfService refs="BA1593 FBCODE3ADT">
                                <Code SeatsLeft="7">J</Code>
                                <MarketingName>BUSINESS</MarketingName>
                            </ClassOfService>
                        </PriceClass>
                    </PriceClassList>
                </DataLists>
                <Metadata>
                    <Other>
                        <OtherMetadata>
                            <CurrencyMetadatas>
                                <CurrencyMetadata MetadataKey="GBP">
                                    <Decimals>2</Decimals>
                                </CurrencyMetadata>
                            </CurrencyMetadatas>
                        </OtherMetadata>
                    </Other>
                </Metadata>
            </AirShoppingRS>
        </soap:Body>
    </soap:Envelope>
  ```

</details>
