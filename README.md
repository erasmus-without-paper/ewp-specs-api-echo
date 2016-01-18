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

 * You MUST ask the client for its certificate and verify if the request is
   coming from within the EWP Network. At least one of the following conditions
   must be met:

   * The request is signed with a **trusted** client certificate, and the
     certificate's common name (CA) matches at least one of the common names
     published in the [Registry][registry-spec].

   * The request is signed with **any** client certificate (might be
     self-signed), and the certificate's SHA-1 fingerprint matches at least one
     of the fingerprints published in the [Registry][registry-spec].

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
