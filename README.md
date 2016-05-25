Echo API
========

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

**EWP Echo API** makes it simpler for EWP developers to design and test the
core of their EWP implementations. It is RECOMMENDED for all developers to
implement it (and keep it updated) **at least in their development EWP Hosts**,
to avoid potential problems in the future.

The requirements listed below might seem obvious, but they require the
developer to implement the core security framework which will be needed
throughout the development of **all the other** EWP features.


Requirements
------------

In order to properly implement the Echo API, you will need to:


### Step 1. Verify the SSL Certificate

This step is designed to make sure that you **follow EWP security policies**.

 * You MUST ask the client for its certificate and verify if the request is
   coming from within the EWP Network. The certificate's SHA-1 fingerprint MUST
   match at least one of the fingerprints published in the [Registry Service]
   [registry-spec]. Note, that [clients certificates MAY be self-signed]
   (https://github.com/erasmus-without-paper/ewp-specs-architecture/issues/3)
   (you MUST allow such certificates).

 * If the verification has **failed**, respond with a **HTTP 403** status. You
   SHOULD return some descriptive error message too (wrapped in XML), as
   described in the [general error handling rules][error-handling].

 * If the verification has **succeeded**, proceed to the next step.


### Step 2. Verify request method and content type

 * You SHOULD allow both GET and POST request methods. If other request
   types are received (e.g. PUT), respond with **HTTP 405** error status, as
   described in the [general error handling rules][error-handling]

 * If POST is received, then you MUST be able to decode at least the
   `application/x-www-form-urlencoded` content type. It is not required to
   support any other encodings (such as `multipart/form-data`), and you MAY
   respond with a **HTTP 415** error status upon receiving such unsupported
   encodings.

 * Note, that in some other EWP APIs it will often be recommended to support
   only the POST HTTP method. However most read-only APIs will be accessibly
   via both GET and POST (because GET has a limited query length).


### Step 3. Retrieve the `echo` variable

This step is designed to make sure you **access XML elements using their
proper namespace URIs**.

 * Take the GET or POST parameter named `vars`, and parse its contents as XML.
   You MAY expect the contents to match the XML Schema for `vars` element
   attached in the [request-vars.xsd](request-vars.xsd) file. It is NOT
   REQUIRED to validate the contents against the schema.

 * Extract the `echo` parameter (as described in the schema). You will need it
   in the next step.

 * Please note, that in other EWP APIs it will usually not be required to
   parse XML files in requests. Variables (such as the `echo` variable) will
   be delivered to you via the regular GET parameter. We make it more a bit
   more complicated here just for the purpose of exercise (we found that some
   developers don't have much experience with XML namespaces). 


### Step 4. Identify the EWP Host and respond

This step is designed to make sure you can **identify the requesting EWP Host**
and that you **encode your XML output properly**.

 * Respond with a **HTTP 200** status, and a document described by the
   [response.xsd](response.xsd) schema.

 * Among other things, the response MUST contain the list of requester's HEI
   IDs (which can be retrieved from the Registry's response), and the `echo`
   variable retrieved in the previous step. See the schema file for details.


Deployment
----------

The last requirement is to publish the URL of your Echo API implementation in
your Manifest file, so that other developers (and, possibly, continuous
integration scripts) may discover and test it.

The format of the manifest file entry is described in the [manifest-entry.xsd]
(manifest-entry.xsd) file. You will need to use a proper `xmlns` when you are
including it in your manifest file.


[registry-spec]: https://github.com/erasmus-without-paper/ewp-specs-api-registry/blob/master/README.md
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery/blob/stable-v1/README.md
[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management/blob/stable-v1/README.md#statuses
[error-handling]: https://github.com/erasmus-without-paper/ewp-specs-architecture#error-handling
