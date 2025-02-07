## Frequently Asked Questions (FAQ)

This FAQ is designed to answer common questions about the API. If you need further assistance, please contact contact@orchestra.eu.

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

A: In the test environment, occasional maintenance may cause the servers to be temporarily inaccessible. If the error persists, please contact Orchestra for further assistance.  

---

**Q5: I am using the test environment. Why do I get a "SOAP Failure"?**  

A: In the test environment, some airlines may experience issues. Please try selecting a different airline. If the error persists across multiple airlines, contact Orchestra for support.  

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

ChatBot
{% raw %}
{% include iframe.html %}
{% endraw %}
