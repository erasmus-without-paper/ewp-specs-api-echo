Echo API
========

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

**This version (v1.x.x) of Echo API is now DEPRECATED. Please upgrade to
version 2.x.x.**

**EWP Echo API** might seem trivial in itself, but it requires EWP developers
to design and test the authentication and security framework which they will
use throughout the development of **all the other** EWP features. It is
RECOMMENDED for all developers to implement it (and keep it updated) **at least
in their development EWP Hosts**, to avoid potential problems in the future.

It also familiarizes developers with the way EWP APIs are documented (many
important parts are documented in XSD files!).


Authentication and Encryption
-----------------------------

This version (v1.x.x) of Echo API follows the rules described in [EWP
Authentication and Security, Version 1][sec-v1] document. It requires
implementers to support a very specific set of security solutions:

 * For client authentication, [TLS Client Certificate
   Authentication][cliauth-tlscert] method MUST be used, and self-signed
   client certificates MUST be accepted by the server.

 * For server authentication, [TLS Server Certificate
   Authentication][srvauth-tlscert] method MUST be used.

 * Regular TLS MUST be used for both [request][reqencr-tls] and
   [response][resencr-tls] encryption.

 * Other methods MAY be supported, but it is NOT REQUIRED to support them (and
   the server doesn't declare support for them in his manifest file).

Please note, that soon there will be a new (v2.x.x) version of this API, which
will have different authentication and encryption requirements.


Request method
--------------

Requests MUST be made with either HTTP GET or HTTP POST method. Servers MUST
support both these methods.

*Hint:* You MAY support other encodings (such as `multipart/form-data`), but
you are not required to. As described in the [general error handling
rules][error-handling], you SHOULD respond with a **HTTP 415** error status if
you receive a POST in an encoding which you don't support.


Request parameters
------------------

Parameters MUST be provided either in a query string (for GET requests), or in
the `application/x-www-form-urlencoded` format (for POST requests). Servers
MAY reject all other formats.


### `echo` (optional, repeatable)

Servers MUST retrieve `echo` values (plural) from request. The `echo` parameter
is *repeatable*. It means that it can appear **more than once** (e.g.
`url?echo=...&echo=...`). Such repeatable parameters will be used throughout
other EWP APIs, so you should be able to retrieve them properly.

*Optional* means, that the list of `echo` values is allowed to be of
zero-length. It is valid to call the echo URL with no `echo` parameters.


Handling of invalid parameters
------------------------------

 * General [error handling rules][error-handling] apply. (This means that you
   MUST follow these rules. These apply to most of the other EWP APIs too.)

 * Usually APIs will describe much more rules here. E.g. which combinations
   of parameters should result in which kind of error, etc. However, in case
   of Echo API, you will need to support only the "general cases".


Response
--------

Servers MUST respond with a valid XML document described by the
[get-response.xsd](get-response.xsd) schema. See the schema annotations for
further information.


Deployment
----------

The last requirement is to publish the URL of your Echo API implementation in
your Manifest file, so that other developers (and, possibly, continuous
integration scripts, like the [Echo API Validator][echo-validator]) may
discover and test it.

The format of the Echo API manifest entry is described in the
[manifest-entry.xsd](manifest-entry.xsd) file. You will need to use a proper
`xmlns` when you are including it in your manifest file.

*Hint:* The deployment step looks exactly the same for all APIs, so in most
cases it is not described, again and again, in all API specifications.


[registry-spec]: https://github.com/erasmus-without-paper/ewp-specs-api-registry
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery
[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[error-handling]: https://github.com/erasmus-without-paper/ewp-specs-architecture#error-handling
[standard-sec-methods]: https://github.com/erasmus-without-paper/ewp-specs-sec-intro#standard-methods
[echo-validator]: https://developers.erasmuswithoutpaper.eu/#validator
[cliauth-none]: https://github.com/erasmus-without-paper/ewp-specs-sec-cliauth-none
[cliauth-tlscert]: https://github.com/erasmus-without-paper/ewp-specs-sec-cliauth-tlscert
[cliauth-none]: https://github.com/erasmus-without-paper/ewp-specs-sec-cliauth-none
[cliauth-tlscert]: https://github.com/erasmus-without-paper/ewp-specs-sec-cliauth-tlscert
[cliauth-httpsig]: https://github.com/erasmus-without-paper/ewp-specs-sec-cliauth-httpsig
[srvauth-tlscert]: https://github.com/erasmus-without-paper/ewp-specs-sec-srvauth-tlscert
[srvauth-httpsig]: https://github.com/erasmus-without-paper/ewp-specs-sec-srvauth-httpsig
[reqencr-tls]: https://github.com/erasmus-without-paper/ewp-specs-sec-reqencr-tls
[resencr-tls]: https://github.com/erasmus-without-paper/ewp-specs-sec-resencr-tls
[sec-v1]: https://github.com/erasmus-without-paper/ewp-specs-sec-intro/tree/stable-v1
