Release notes
=============

This document describes all the changes made to the *Echo API* document,
starting from its first released version.


2.0.1
-----

* Fix an outdated link reference.


2.0.0
-----

 * This API now requires implementers to upgrade their implementations to
   [Version 2](https://github.com/erasmus-without-paper/ewp-specs-sec-intro/tree/stable-v2)
   of the *Authentication and Security* document.

   In particular, this means that the clients MUST be aware of the fact, that
   the server is no longer required to support methods of authentication and
   encryption which it *was* required to support in the previous versions of
   this API. Clients (such as the [Echo API
   Validator](https://developers.erasmuswithoutpaper.eu/#validator)) SHOULD
   consult the newly introduced `<http-security>` element in the server's
   manifest entry before making their requests.

 * Because we are releasing a new major release (which is no longer
   backward-compatible with the previously released stable `1.x.x` releases),
   XML namespaces were changed to reflect that.

   In particular, API-entry namespace was changed from:

   ```
   https://github.com/erasmus-without-paper/ewp-specs-api-echo/blob/stable-v1/manifest-entry.xsd
   ```

   to:

   ```
   https://github.com/erasmus-without-paper/ewp-specs-api-echo/blob/stable-v2/manifest-entry.xsd
   ```

   And the Echo-response namespace was changed from:

   ```
   https://github.com/erasmus-without-paper/ewp-specs-api-echo/tree/stable-v1
   ```

   to:

   ```
   https://github.com/erasmus-without-paper/ewp-specs-api-echo/tree/stable-v2
   ```


1.1.1
-----

* Explicitly declare that this version still requires the use of
  [Version 1](https://github.com/erasmus-without-paper/ewp-specs-sec-intro/tree/stable-v1)
  of the *Authentication and Security* document. You can find more information
  on the planned process of updating security requirements
  [here](https://github.com/erasmus-without-paper/ewp-specs-sec-intro/issues/1).


1.1.0
-----

* Majority of the document has been moved to a separate document [describing
  TLS Client Certificate
  Authentication](https://github.com/erasmus-without-paper/ewp-specs-sec-cliauth-tlscert)
  ([details](https://github.com/erasmus-without-paper/ewp-specs-architecture/issues/21)).

* The document has been re-formatted, so that it resembles all the other API
  specifications. The "step by step" instructions were removed (because most
  of them are now part of the TLS Client Authentication document).


1.0.4
-----

* `minOccurs` and `maxOccurs` are now provided explicitly
  ([why?](https://github.com/erasmus-without-paper/general-issues/issues/22)).


1.0.3
-----

* "You SHOULD allow both GET and POST request methods" was changed to "you
  MUST".

* Added more details in regard of how client certificates are to be verified.
  In particular, explained that *not* supplying any certificate must result
  in an error.


1.0.2
-----

* Added a new *Debugging* section to the document. It briefly explains how
  developers can use their browsers to debug their Echo API implementations.

* Fixed a minor mistake (`SHA-1` was used instead of `SHA-256`).


1.0.1
-----

Fix the wording/vocabulary for more consistent usage across all documents.


1.0.0
-----

Initial release.
