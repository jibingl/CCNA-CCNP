# REST (Representational State Transfer)
REST or RESTful is a framework for APIs.

## CRUD
Table 1 - REST **CRUD** Operations
CRUD |Purpose |HTTP Verb |
-----|--------|----------|
Create | Create new variable| POST|
Read | Retrive values of variable| GET|
Update | Change the value of variable| PUT, PATCH| 
Delete | Delete variable|DELETE|

## HTTP
URI Unified Resource Identifier: indicates the resource it is trying to access.  
Example of URI:
```
https://sandboxdnac.cisco.com/dna/intent/api/v1/network-device
+----+  +--------------------+-------------------------------+
Scheme      Authority              Path
```
Table 2 - HTTP Responses:
Code| Facility | Description| Example|
----|----------|------------|--------|
1xx |informational |The request was received, continuing process |**102 processing** |
2xx |successful | THe request has received, understood, and accepted |**200 OK**; **201 Created** (response to POST) |
3xx |redirection | Further action need to be taken in order to complete the request |**301 Moved Permanently** |
4xx | client error| The request contains bad syntax or can't be fulfilled |**403 Unauthorized**; **404 Not Found** the resource |
5xx | server error| The server failed to fulfill an aparently valid request |**500 Internal Server Error** can't hadle the request |

## REST APIs
For REST APIs to instruct applications to communicate over network, networking protocol must be used to facilitate the communications. HTTP(s) is the most common choice.
Note: REST APIs and HTTP are both stateless.
