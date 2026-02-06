## Frequently Asked Questions (FAQ)

This FAQ is designed to answer common questions about the API. If you need further assistance, please contact contacts@travelsoft.fr.

---

**Q1: Why did I get a "Session Expired" message?**

A: There are several possible reasons why this message may appear:

- **Missing Token in Header:** Ensure that the session token is correctly included in the request header.
- **Token Expired:** The token has a default expiration time of one hour. If more time has passed, a new token must be generated.
- **Session Cookies Expired:** The session cookies may have expired. Clear your cookies and retry the request.

---

**Q2: Why do I receive an error "Form of payment missing or invalid for ticket/document," even after entering a form of payment?**

A: This issue may occur if your payment limit or balance has not been initialized or is set to zero. In such cases, you should contact an agency to resolve the issue.  

---


**Q3: Can I add ancillaries, select a seat, or change a flight after booking?**

A: No, post-booking modifications are strictly limited to cancellations. Adding ancillaries, selecting seats, or changing flights is not allowed once the booking is complete.  

---

**Q4: I am using the test environment. Why do I get "error 405"?**  

A: In the test environment, occasional maintenance may cause the servers to be temporarily inaccessible. If the error persists, please contact Travelsoft for further assistance.  

---

**Q5: I am using the test environment. Why do I get a "SOAP Failure"?**  

A: In the test environment, some airlines may experience issues. Please try selecting a different airline. If the error persists across multiple airlines, contact Travelsoft for support.  

---

**Q6: How can I view flight information?**  

A: All flight details, such as origin, destination, segments, and services, can be found in the Datalist of the AirShoppingRS, OfferPriceRS, and OrderViews responses.  

---

**Q7: Why am I not receiving information on meals, cabin baggage, or seats for some airlines?**  

A: Some airlines do not provide complete flight details. For further information, please contact the respective airline directly.  

---

**Q8: Can I use this API as a cache?**  

A: No, this API is not designed to be used as a cache but as a direct shopping solution.  

---

**Q9: Can I get alternatives offers after selecting one from AirShopping (Upselling)?**  

A: No, this API does not currently support retrieving alternative offers after an initial selection.  

---

**Q10: Why do I receive an "Invalid offer item selection" error?**  

A: This error occurs when the offer item(s) referenced in your request do not match any of the valid offer items from the shopping response. In other words, the **OfferID** and/or **OfferItemID** you are using might be invalid for this operation or fail to meet eligibility criteria (e.g., incorrect passenger or flight associations).  

To troubleshoot this issue, follow these steps:  

- **Verify the identifiers:** Ensure that the **OfferID** and each **OfferItemID** in your request **exactly** match those from the shopping response. Any discrepancy can trigger this error.  
- **Check eligibility conditions:** Confirm that all eligibility criteria (such as passenger associations or flight segment references) are met for each offer item.  
- **Review required items:** Make sure no required offer item has been omitted and that no inapplicable offer item has been included.  

---

**Q11: How many service restrictions can airlines impose?**  

A: Some airlines do not allow booking multiple services. There are three types of restrictions:  

- **One per passenger `OnePerPax`:** You can book only one of this service per passenger.  
- **One per order `OnePerOrder`:** You can book only one of this service for the entire order.  
- **No restriction `None`:** There are no limitations for this service.  

---

**Q12: Why do I get the error "Form of payment missing or invalid for ticket/document" during booking issuance, even if my FOP is valid?**  

A: This error typically occurs due to insufficient funds in your account balance. Even if your form of payment (FOP) is valid, the transaction cannot be processed if there are not enough funds available. Please contact Travelsoft or your agency to ensure that your account has sufficient funds to complete the payment for the flight.  

---

## Still have questions?   
💬 [**Ask "Elya" - The Virtual Assistant**](chatbot.html)
