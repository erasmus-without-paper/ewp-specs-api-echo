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

 * When request is received, you MUST **verify the client's certificate**. The
   request MUST come from within the EWP Network (you will need to use [EWP
   Registry][registry] to help you determine that). 

 * If the verification has **failed**, respond with a **HTTP 403** status (you
   MAY return some message too, but currently it is not required).

 * If the verification has **succeeded**, respond with a **HTTP 200** status.

And that's pretty much it.


[registry]: https://github.com/erasmus-without-paper/ewp-specs-architecture/blob/master/README.md#registry)
[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management/blob/master/README.md#statuses
