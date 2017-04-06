Echo API
========

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

**EWP Echo API** might seem trivial in itself, but it requires EWP developers
to design and test the authentication and security framework which they will
use throughout the development of **all the other** EWP features. It is
RECOMMENDED for all developers to implement it (and keep it updated) **at least
in their development EWP Hosts**, to avoid potential problems in the future.

It also familiarizes developers with the way EWP APIs are documented (many
important parts are documented in XSD files!).


Authentication and Encryption
-----------------------------

This version of this API uses [standard EWP Authentication and Security,
Version 2][sec-v2]. Server implementers choose which security methods they
support by declaring them in their Manifest API entry.

Since Echo API is implemented primarily for testing everyone's security
framework, servers are RECOMMENDED to support *all* currently specified
[standard authentication and encryption methods][standard-sec-methods], with
some minor exceptions:

 * It is FORBIDDEN to support [Anonymous Clients][cliauth-none] in Echo API.

 * If you (the server implementer) are certain that you *won't* be supporting
   some particular security method in *any* of your *other* APIs, then it's
   okay to "skip" supporting this method in Echo API too. (This is especially
   true in case of [TLS Client Certificate Authentication][cliauth-tlscert],
   which - based on the input from EWP developers - turned out to be difficult
   to implement in some architectures).

 * When new security methods are introduced in the future, it's usually okay to
   "lag behind" a little. That is, you usually won't be required to support
   new security methods immediately after they are introduced. However, in
   time, **some older security methods MAY get deprecated, or even banned** -
   which might result in you getting cut off from the rest of the EWP Network
   (first, you may get banned by some more restrictive partners, and later on,
   by the Registry Service administrators). So you should keep an eye on that!

It is also RECOMMENDED that you support all these security methods at a *single
endpoint/URL* (as opposed to having separate API-entries in your manifest, per
each possible combination of security methods). At the time we are writing
this, all standard methods are designed in a way that enables them to be used
interchangeably on single URL (and we hope it will stay this way).


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

Note, that this deployment step looks exactly the same for all APIs, so in most
cases API designers skip it in their API specifications.


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
[sec-v2]: https://github.com/erasmus-without-paper/ewp-specs-sec-intro/tree/stable-v2
