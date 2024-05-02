Outgoing Mobilities Index endpoint
==================================

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This endpoint allows the receiving institution to access a list of all
mobilities it can read on the sending institution's server.


Request method
--------------

 * Requests MUST be made with either HTTP GET or HTTP POST method. Servers MUST
   support both these methods. Servers SHOULD reject all other request methods.

 * Clients are advised to use POST when passing large number of parameters
   (servers MAY set a limit on their GET query string length).


Request parameters
------------------

Parameters MUST be provided in the regular `application/x-www-form-urlencoded`
format.


### `receiving_academic_year_id` (optional)

An academic year identifier, in the `YYYY/YYYY` format, as defined in the
[EWP Academic Term Data Types](https://github.com/erasmus-without-paper/ewp-specs-types-academic-term)
document. If given, then the server MUST return only such mobilities which
have taken place (or are planned to take place) during this single academic
year.

This parameter was added to allow the client to synchronize only a smaller
subset of all mobilities more easily (because many clients don't want to store
mobilities which ended long ago).


### `modified_since` (optional)

A datetime string in the [`xs:dateTime` format][xs-datetime], e.g.
`2004-02-12T15:19:21+01:00`.

If given, then the server SHOULD filter the returned mobility IDs to the ones
that have been either created or modified after the given point in time.

 * Servers MAY include mobilities which were *not* modified. For example, if
   the server only *suspects* that the mobility has been modified, then it is
   okay to include such mobility's ID in the response.

 * As we previously explained [here][index-pulling], clients MAY use the
   `index` and `get` endpoints as a pull-based method of synchronization,
   complementary to CNRs. It is RECOMMENDED for the
   servers to support this parameter, to avoid unnecessary network traffic.


Permissions
-----------

Only selected Outgoing Mobility objects should be accessible to the caller.
Visibility of objects SHOULD be kept in sync with the visibility described in
the `get` endpoint. That is:

 * Servers MUST publish IDs of every mobility that is accessible (to the same
   caller) via the `get` endpoint.

 * Server MUST NOT publish IDs of objects which would NOT be accessible (to the
   same caller) via its `get` endpoint. Clients which receive mobility IDs from
   the `index` endpoint should be able to fetch all objects referenced by those
   IDs.


Response
--------

Servers MUST respond with a valid XML document described by the
[index-response.xsd](index-response.xsd) schema. See the schema annotations
for further information.


[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[registry-spec]: https://github.com/erasmus-without-paper/ewp-specs-api-registry
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery
[echo]: https://github.com/erasmus-without-paper/ewp-specs-api-echo
[institutions-api]: https://github.com/erasmus-without-paper/ewp-specs-api-institutions
[index-pulling]: https://github.com/erasmus-without-paper/ewp-specs-architecture#index-pulling
[xs-datetime]: https://www.w3.org/TR/xmlschema11-2/#dateTime
