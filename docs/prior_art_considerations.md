# Facilitated Payment link type in HTML Prior Art Considerations
Last Update: Jan 21, 2025

## Authors:
- Junhui He

## Background
Several prior efforts have aimed to improve online payments and website monetization. This section examines some notable examples, particularly the microformats rel-payment proposal, which shares the most similarity with our proposal. Thus, we've decided to change our proposal from rel="payment" to rel="facilitated-payment" due to the prior art investigations below. And rel="facilitated-payment" was not used or proposed anywhere based on HTTP archive investigation.

## [Microformats rel-payment](https://microformats.org/wiki/rel-payment)
### Purpose
To create a general-purpose mechanism for indicating links related to various forms of support, including financial donations, tips, and affiliate links. [This proposal](https://microformats.org/wiki/rel-payment) is the most directly related prior art due to its use of the *rel="payment"* attribute. 
### Mechanism
Uses *rel="payment"* on `<a>` or `<link>` elements. It encourages visible `<a>` elements for better user experience and fraud prevention and suggests using the `title` attribute to describe the payment method.
### Usage
Content aggregators (like RSS readers) and other user agents could detect these links and present a standard UI for users to make payments or offer support.
### Reported Implementations
The microformats wiki lists several websites that are using *rel="payment"* links, including FireAnt, blip.tv and others. It also mentions some podcast players (Overcast, Transistor.fm, Player.fm, Castro) that support parsing *rel="payment"* from podcast episode show notes. Based on our investigations using HTTP archive, *rel="payment"* has already been used by some websites.
### Current Status and Limitations
#### Deprecation Considered
The microformats community is [considering deprecating](https://microformats.org/wiki/rel-payment-issues) *rel-payment* in favor of a microformats2 property. 
#### Limited Adoption
Despite the listed examples, the actual widespread adoption of *rel="payment"* is limited.
#### Lack of Specificity
The proposal is very general and does not define specific payment methods or URI schemes, making it less suitable for direct integration with payment processors or for the specific use case of push payments in e-commerce.
### Relation to Current Proposal
Both proposals share the core idea of using a specific link relationship to indicate a payment link. However, this proposal differs significantly in scope, implementation, and intended use case. Our proposal focuses on streamlining push payments in e-commerce, relies on browser detection of hidden `<link>` elements, and aims for integration with payment clients. Our *rel="facilitated-payment"* proposal is intended to be a machine-readable presentation of information that is already present elsewhere on the webpage(e.g. a QR image). It contrasts with the microformats proposal's broader scope, emphasis on visible links for general support, and lack of specificity regarding payment methods.

## [Web Monetization](https://webmonetization.org/specification/)
### Purpose
To enable a continuous stream of payment from users to websites they visit, providing an alternative revenue model for websites.
### Mechanism
Uses *rel="monetization"* on `<link>` elements, which indicates a payment pointer that specifies the recipient's wallet address. Users need a Web Monetization provider to send payments.
### Status
A proposed standard currently incubating in the WICG. Efforts are underway to prototype in Chromium.
### Relation to Current Proposal
Web Monetization addresses a different aspect of online payments. It focuses on passive, ongoing micropayments, whereas our proposal targets active, user-initiated push payments. The two could be complementary, with websites using both to offer users different payment options.

## [Payment Request API](https://www.w3.org/TR/payment-request/)
### Purpose
To allow browsers to act as intermediaries between merchants, users, and payment handlers, streamlining the checkout process.
### Mechanism
Merchants use JavaScript to create a *PaymentRequest* object, specifying supported payment methods, transaction details, and optional data requests (like shipping address). The browser handles the payment flow and UI.
### Status
Shipped / W3C recommendation
### Relation to Current Proposal 
The Payment Request API is designed for active, browser-mediated payment flows, often used for pull payments. *rel="facilitated-payment"* is for passive detection of push payment methods, leaving the user interaction and payment initiation to a separate payment client. PaymentRequest integration is considered a heavy lift for merchant websites, whereas *rel="facilitated-payment"* is aiming to be easier for merchants, since the link should contain the information that is already present on the page in another form, for example: a QR code image.

## Conclusion
Our proposal builds upon the foundational idea of using *rel="facilitated-payment"*, as *rel="payment"* is pioneered by the microformats community. However, it addresses the limitations of previous approaches, particularly the microformats proposal, by:
1. Focusing on a specific use case: Facilitating push payments in e-commerce.
2. Defining a clear mechanism for browser detection and integration with payment clients.
3. Working towards standardization of payment method URI schemes (in future iterations).
4. Defining *rel=”facilitated-payment”* as a page level attribute.

By addressing these points, our proposal aims to provide a more robust, practical, and standards-compliant solution for improving the online payment experience, distinct from and complementary to other efforts like Microformats, Web Monetization, and the Payment Request API.
