Echo API
========

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

**EWP Echo API** allows beginner EWP developers to test the security of their
EWP Network connections. It seems obvious, but it requires the developer to
implement the core security framework (which will be needed by *all the other*
APIs implemented later).


Requirements
------------

In order to properly implement the Echo API, you will need to:

 * Listen for requests at an URL of your choosing. You SHOULD publish this URL
   in your Manifest file (under `apis-implemented/echo/url`), so that other
   developers may discover and test it.

 * When request is received and SSL negotiation is started, you MUST **request
   and verify the client's certificate**. The request MUST come from within the
   EWP Network.
   
   * Remember, that self-signed client certificates are **allowed** within the
     EWP Network. If the client's certificate is self-signed, then its SHA-1
     fingerprint MUST match one of the `fingerprint`s published by the
     [Registry][registry-spec]. You MUST NOT use the common name (CA) for
     matching in this case.
     
   * If the client's certificate is signed by a trusted CA, then - apart from
     fingerprint matching - you MUST attempt to match its common name (CA)
     against one of the `common-name`s published by the [Registry]
     [registry-spec].
     
   * Consult XSD annotations for `client-certificates-in-use`, as described in
     the [Discovery Manifest API specification][discovery-api].
   
 * If the verification has **failed**, respond with a **HTTP 403** status. You
   MAY return some descriptive error message too, but currently it is not
   required.

 * If the verification has **succeeded**, respond with **HTTP 200** status, and
   a return a plaintext `Echo` string (case sensitive). 

And that's pretty much it.


[registry-spec]: https://github.com/erasmus-without-paper/ewp-specs-api-registry/blob/master/README.md
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery/blob/master/README.md
[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management/blob/master/README.md#statuses
