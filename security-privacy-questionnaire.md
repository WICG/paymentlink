# [Self-Review Questionnaire: Security and Privacy](https://w3ctag.github.io/security-questionnaire/)

> 01.  What information does this feature expose,
>      and for what purposes?

Payment Link URIs are exposed by websites and received by payment clients, which are either integrated wallets within the user agent or opted in browser extensions installed by the user. These URIs include a scheme (e.g., upi, bitcoin, paypal) and parameters like payee address, amount, and currency. This information is necessary for the user agent to identify the payment method, offer users payment options and initiate payment. The user agent mediates access to this information, giving users control over which payment clients are active and can access payment links through browser settings. The user agent will clearly indicate when a payment link is detected and which payment client can handle it, ensuring user awareness and consent. Payment clients are linked and authorized to be used through the user agent.

Purposes of Exposing the Information:
- Payment Initiation: The primary purpose is to allow payment clients (the wallet being used by the user agent") to handle supported payment methods, streamlining the payment process for users.
- Validation: The user agent uses the information in the payment link URI to validate the payment method and ensure it's supported before prompting the user. For example, certain wallet supports limited set of schemes and hosts.
- User Choice: By recognizing the payment link information, the user agent gains the capability to present the user with a choice of available payment options, allowing them to select their preferred method.

The privacy implications of this are discussed [in the explainer](./docs/explainer.md#privacy-considerations).

> 02.  Do features in your specification expose the minimum amount of information
>      necessary to implement the intended functionality?

Yes it is. The only information exposed is the payment link URI itself.

The URI should contain the essential details required for the user agent and payment clients to:
- Identify the payment method: The URI scheme indicates the specific payment system being used (e.g., UPI, Bitcoin).
- Initiate the payment: The URI may include parameters like payee address, amount, and currency, which are necessary to start the transaction.   
- Validate the payment method: The user agent uses the URI to check if the payment method is supported before prompting the user.

> 03.  Do the features in your specification expose personal information,
>      personally-identifiable information (PII), or information derived from
>      either?

No. Although the payment link URI may include parameters like payee address, amount, and currency, none of them belongs to above categories.

> 04.  How do the features in your specification deal with sensitive information?

The feature does not deal with sensitive information.

> 05.  Do the features in your specification introduce state
>      that persists across browsing sessions?

No. The feature operates in a stateless manner.

> 06.  Do the features in your specification expose information about the
>      underlying platform to origins?

No.

> 07.  Does this specification allow an origin to send data to the underlying
>      platform?

Yes. The specification allows origins to send data to the platform only within the well-defined context of payment initiation, with user consent and security measures in place.

> 08.  Do features in this specification enable access to device sensors?

No.

> 09.  Do features in this specification enable new script execution/loading
>      mechanisms?

No.

> 10.  Do features in this specification allow an origin to access other devices?

No.

> 11.  Do features in this specification allow an origin some measure of control over
>      a user agent's native UI?

Possibly. When a valid payment link is detected and the user has enabled this type of payment link by enrolling with their wallet, the user agent may present an UI to the user, allowing them to choose their preferred payment method and confirm the transaction. In this sense, the origin indirectly triggers the display of this UI by providing the payment link.

> 12.  What temporary identifiers do the features in this specification create or
>      expose to the web?

None.

> 13.  How does this specification distinguish between behavior in first-party and
>      third-party contexts?

The payment link needs to be on on a page only on top-level context. Otherwise, the user agent shouldn't trigger the payment flow.

> 14.  How do the features in this specification work in the context of a browser’s
>      Private Browsing or Incognito mode?

The functionality remains. We didn't anticipate different behaviors.

> 15.  Does this specification have both "Security Considerations" and "Privacy
>      Considerations" sections?

There is no specification yet, but there is a [security considerations](./docs/explainer.md#security-considerations) and a [privacy considerations](./docs/explainer.md#privacy-considerations) section in the explainer.

> 16.  Do features in your specification enable origins to downgrade default
>      security protections?

No.

> 17.  What happens when a document that uses your feature is kept alive in BFCache
>      (instead of getting destroyed) after navigation, and potentially gets reused
>      on future navigations back to the document?

The payment link detection happens during the DOM construction. Depends on the implementation by browsers, the push payment flow can be triggered again with users' consent.

> 18.  What happens when a document that uses your feature gets disconnected?

The push payment flow should fail. User agents should handle the disconnection error gracefully.

> 19.  What should this questionnaire have asked?

Seems fine.
