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
   coming from within the EWP Network. At least one of the following conditions
   must be met:

   * The client is using a **trusted** client certificate (signed by a CA), and
     the certificate's common name (CN) matches *at least one* of the common
     names published in the [Registry][registry-spec].

   * The client is using **any** certificate (might be self-signed), and the
     certificate's SHA-1 fingerprint matches at least one of the fingerprints
     published in the [Registry][registry-spec].

 * If the verification has **failed**, respond with a **HTTP 403** status. You
   MAY return some descriptive error message too, but currently it is not
   required.

 * If the verification has **succeeded**, proceed to the next step.


### Step 2. Verify request method and content type

 * You SHOULD allow both GET and POST request methods. If other request
   types are received (e.g. PUT), respond with **HTTP 405** error status.

 * If POST is received, then you MUST be able to decode at least the
   `application/x-www-form-urlencoded` content type. It is not required to
   support any other encodings (such as `multipart/form-data`), and you SHOULD
   respond with a **HTTP 415** error status upon receiving such unsupported
   encodings.

 * Please note, that in other EWP APIs it will usually be recommended to
   support only a single HTTP method - either GET or POST. We require you to
   support both of them in the Echo API though, just for the purpose of
   exercise.


### Step 3. Retrieve the `echo` variable

This step is designed to make sure you **access XML elements using their
proper qualified names (QNames)**.

 * Take the GET or POST parameter named `vars`, and parse its contents as XML.
   You MAY expect the contents to match the XML Schema attached in the
   [request-vars.xsd](request-vars.xsd) file. It is NOT REQUIRED to validate
   the contents against the schema.

 * Extract the `echo` parameter (as described in the schema). You will need it
   in the next step.

 * Please note, that in other EWP APIs it will usually not be required to
   parse XML files in requests. Variables (such as the `echo` variable) will
   normally be delivered to you via the GET parameter directly. We make it
   more complicated here just for the purpose of exercise.


### Step 4. Identify the EWP Host and respond

This step is designed to make sure you can **identify the requesting EWP Host**
and that you **encode your XML output properly**.

 * Respond with a **HTTP 200** status, and a document described by the
   [response.xsd](response.xsd) schema.

 * The response MUST contain the list of requester's HEI IDs (which can be
   retrieved from the Registry's response), and the `echo` variable retrieved
   in the previous step. See the schema file for details.


Deployment
----------

The last requirement is to publish the URL of your Echo API implementation in
your Manifest file (as described in the `apis-implemented/echo/url` element),
so that other developers (and, possibly, continuous integration scripts) may
discover and test it.


[registry-spec]: https://github.com/erasmus-without-paper/ewp-specs-api-registry/blob/master/README.md
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery/blob/stable-v1/README.md
[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management/blob/stable-v1/README.md#statuses
