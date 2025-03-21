# Facilitated payment links and Payment Request 

The [current draft spec](https://wicg.github.io/paymentlink/) describes very
general steps for hooking up a given facilitated payment link to one or more
'payment wallets', which are themselves not defined.

In discussions with the WPWG and TAG, it seems likely that we can instead
describe facilitated payment links as a form of 'declarative' [Payment
Request](https://w3c.github.io/payment-request/), and thus integrate into the
existing specification for and ecosystem of Payment Handlers (potentially both
[web](https://w3c.github.io/payment-handler/) and
[native](https://web.dev/articles/android-payment-apps-developers-guide)). This
document attempts to describe a high level vision of what that might look like,
along with some open questions that would need to be addressed.

## High Level Algorithm

1. Given a `<link rel="facilitated-payment" href="<scheme>://<host>/<path>?<query>">`
2. If the `<scheme>` is "https", then:
    1. Construct a PaymentRequest object with parameters:
        - `supportedMethods: "<scheme>://<host>/<path>" (e.g., "https://wallet.example/pay")`
        - `data: ConvertQueryToDictionary(<query>)` (to be defined)
    2. The user agent checks [PaymentRequest.canMakePayment()](https://w3c.github.io/payment-request/#canmakepayment-method) for the constructed Payment Request
    3. If true, the user agent displays some UX that lets user trigger the payment handler.
    4. If the user selects to trigger this payment handler, trigger [PaymentRequest.show()](https://w3c.github.io/payment-request/#show-method).
    5. The user agent would then drop the response from PaymentRequest.show(). (Payment links are designed for merchants that have back-end integration with payment apps, where the back-end (server-to-server) calls are responsible for communication of a successful transaction.)
3. Else if the `<scheme>` is not "https", then see [How to handle non-HTTPS schemes?](#how-to-handle-non-https-schemes) below!

## Open Questions

### When would the UX be shown to the user to offer them the payment handler?

Firstly, it is likely that we would specify (normatively or non-normatively)
that the browser wait until document load has settled before triggering any UX
for payment links on the page. We would likely also specify a short pause
*after* a payment link is newly added to the page, to allow for collecting
multiple payment links together (see [Handling multiple paylinks on the
page](#handling-multiple-paylinks-on-the-page)).

In addition, it is important for user agents to make sure that payment links do
not become a vector for spamming users with un-wanted browser UX. This is also
a problem for Payment Request, and is why the spec [allows browsers to require
a transient user activation](https://w3c.github.io/payment-request/#show-method)
to call `show()`, and generally allows the user agent to reject `show()` calls
if it believes they are not legitimate.

### Handling multiple paylinks on the page

A site may want to include multiple payment links on the same page (perhaps
coming from integrations with different PSPs or payment method libraries). This
can likely be treated similarly to a single PaymentRequest call with multiple
[PaymentMethodData](https://w3c.github.io/payment-request/#dom-paymentmethoddata)),
if we specify some logic around 'collecting together' multiple payment links on
the page.

### Is it useful for pages to be able to have an event listener for the outcome, or a way to hook into the Payment Request that is triggered?

This is definitely feasible (e.g. via defining `onpaymentrequestshow` or
`onpaymentrequestcomplete`, etc), and simply needs us to determine if its useful.

### How to handle non-HTTPS schemes?

With the current facilitated payment links proposal, [Chromium
intends](https://github.com/WICG/paymentlink/pull/15) to support the following
non-HTTPS schemes: `duitnow`, `shopeepay`, and `tngd`. Some of these are
"single entity" schemes which could instead be represented by a `https` URL
(e.g., https://shopeepay.com/), while others are more like protocols that could
be served by multiple independent payment apps.

Handling payment method 'protocols', rather than specific organizations, is a
partially-unsolved problem in Payment Request. The Payment Request API
specification does support [standardized payment method
identifiers](https://www.w3.org/TR/payment-method-id/#dfn-standardized-payment-method-identifier),
which could address the need. However, due to lackof demand, there are no
standardized payment method identifiers currently defined for protocols. It may
be that for certain regulated payment ecosystems, standardized payment methods
could be successful.

Altenratively, in the [Payment Method
Manifest](https://www.w3.org/TR/payment-method-manifest/) spec, the
`supportedOrigins` concept allows a central entity to list supported apps from
different domains. This could be used for a protocol case, e.g.
https://upi.example could indicate that Bank1, Bank2, and Bank3 are all allowed
to handle https://upi.example payments. However this does still require a
centralized authority (https://upi.example, in this example).

Both of these approaches could be used for paylinks, and might also move us
away from non-HTTPS schemes.
