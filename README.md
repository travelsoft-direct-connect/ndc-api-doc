# AirShopping operation

The air shopping operation allows to initiate a shopping session and returns a list of flight offers depending on criteria given in request.

---------------------------------------

## Release notes

<table>
 <tbody>
  <tr>
    <th>1.0</th>
    <td>Initial version.</td>
  <tr>
 </tbody>
</table>

## Mandatory HTTP header

- AuthToken

## Control header

<Control Provider="VUELING" ApiVersion="1.0" />

# AirShoppingRQ

<table>
 <tbody>
  <tr>
    <th>Element</th>
    <th>Description</th>
    <th>Optional/Mandatory</th>
  <tr>
  <tr>
    <td>Party</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>PayloadAttributes</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>Request</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
 </tbody>
</table>

## Request

<table>
 <tbody>
  <tr>
    <th>Element</th>
    <th>Description</th>
    <th>Optional/Mandatory</th>
  <tr>
  <tr>
    <td>FlightCriteria</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>Paxs</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>ResponseParameters</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>ShoppingCriteria</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
 </tbody>
</table>


# AirShoppingRS

<table>
 <tbody>
  <tr>
    <th>Element</th>
    <th>Description</th>
    <th>Optional/Mandatory</th>
  <tr>
  <tr>
    <td>PayloadAttributes</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>Response</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
 </tbody>
</table>

## Response

<table>
 <tbody>
  <tr>
    <th>Element</th>
    <th>Description</th>
    <th>Optional/Mandatory</th>
  <tr>
  <tr>
    <td>Warnings</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>ShoppingResponse</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>DataLists</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>Offers</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
 </tbody>
</table>

### Offer

<table>
 <tbody>
  <tr>
    <th>Element</th>
    <th>Description</th>
    <th>Optional/Mandatory</th>
  <tr>
  <tr>
    <td>OfferID, OwnerCode</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>JourneyOverview</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>BaggageAllowance</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>TotalPrice</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>OfferItems</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
 </tbody>
</table>

#### OfferItem

<table>
 <tbody>
  <tr>
    <th>Element</th>
    <th>Description</th>
    <th>Optional/Mandatory</th>
  <tr>
  <tr>
    <td>OfferItemID</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>FareDetail</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>Price</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
  <tr>
    <td>Services</td>
    <td>Description</td>
    <td>Mandatory</td>
  <tr>
 </tbody>
</table>


# Samples

- roundtrip_combined
- roundtrip_flatten
- oneway
- error

<details>
  <summary><b>Air Shopping RQ</b></summary>

  ```xml
  <sample>
    <sample></sample>
  </sample>
  ```

</details>

<details>
  <summary><b>Air Shopping RS</b></summary>

  ```xml
  <sample></sample>
  ```

</details>
