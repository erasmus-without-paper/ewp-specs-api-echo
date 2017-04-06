Release notes
=============

This document describes all the changes made to the *Echo API* document,
starting from its first released version.


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
