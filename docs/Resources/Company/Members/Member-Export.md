# Summary #
Exports all Members of a Company to a CSV file.
## Beta warning ##
The API is in Beta
# Operations #
## Create Member Export ##
This resource supports the creation of a CSV file containing all Members of a Company. As this can take some time, it is treated as an asynchronous process.

A successful POST will initiate this process and return a 202 Accepted response containing a link header with a URI to GET the progress of the action.

The link to this API can be found on the [Company](Company) resource with the rel "members-export".
#### Request ####
**JSON**
```
POST /people/companies/123/members/export HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
**XML**
```
POST /people/companies/123/members/export HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### Response - Accepted ####
**JSON**
```
HTTP/1.1 202 Accepted
Content-Type: application/vnd.huddle.data+json
Link: <...>;rel="progress"

{
  "links": [
    { "rel": "self", "href": "..." }
  ],
  "status": "InProgress"
}
```
**XML**
```
HTTP/1.1 202 Accepted
Content-Type: application/vnd.huddle.data+xml
Link: <...>;rel="progress"

<memberExport>
  <link rel="self" href="..." />
  <status>InProgress</status>
<memberExport>
```
#### Response - Member Limit Exceeded ####
Due to the processing cost, this API is limited to Companies that have under 10,000 Members

**JSON**
```
HTTP/1.1 403 Forbidden
Content-Type: application/vnd.huddle.data+json

{ "errors": [ "MemberLimitExceeded" ] }
```
**XML**
```
HTTP/1.1 403 Forbidden
Content-Type: application/vnd.huddle.data+xml

<errorResult>
  <error>MemberLimitExceeded</error>
<errorResult>
```
#### Response - Other (no body) ####
|Status Code|Reason|
|:----------|:-----|
|403|Not a Manager of Company|
|404|Company Not Found|
## Get Progress ##
This resource returns the progress of the action. Clients are expected to poll the progress endpoint to retrieve the current status of the operation. On completion, it will return a link header with a URI to download the CSV file.
#### Request ####
**JSON**
```
GET /people/companies/123/members/export/d8a3d254-9b2a-40c0-b4f5-dbd94c13430a HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
**XML**
```
GET /people/companies/123/members/export/d8a3d254-9b2a-40c0-b4f5-dbd94c13430a HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### Response - In Progress ####
**JSON**
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json

{
  "links": [
    { "rel": "self", "href": "..." }
  ],
  "status": "InProgress"
}
```
**XML**
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml

<membersExport>
  <link rel="self" href="..." />
  <status>InProgress</status>
<membersExport>
```
#### Response - Finished ####
**JSON**
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
Link: <...>;rel="content"

{
  "links": [
    { "rel": "self", "href": "..." },
    { "rel": "content", "href": "..." }
  ],
  "status": "Finished"
}
```
**XML**
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Link: <...>;rel="content"

<membersExport>
  <link rel="self" href="..." />
  <link rel="content" href="..." />
  <status>Finished</status>
<membersExport>
```
#### Response - Error ####
**JSON**
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json

{
  "links": [
    { "rel": "self", "href": "..." }
  ],
  "status": "Error"
}
```
**XML**
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml

<membersExport>
  <link rel="self" href="..." />
  <status>Error</status>
<membersExport>
```
#### Response - Other (no body) ####
|Status Code|Reason|
|:----------|:-----|
|403|Not a Manager of Company|
|404|Progress Not Found|
## Download CSV ##
The endpoint to download the CSV file once it has been created.
#### Request ####
**CSV**
```
GET /people/companies/123/members/export/d8a3d254-9b2a-40c0-b4f5-dbd94c13430a/content HTTP/1.1
Accept: text/csv
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### Response ####
**CSV**

The first row of the CSV contains the field names
```
HTTP/1.1 200 OK
Content-Type: text/csv

FirstName,LastName,Email,Role,LastLoginDate,IsCompanyManager,Status
Isidore,McHohenheim,isidore.mchohenheim@example.com,Manager,2015-12-31T23:59:59,Yes,Active
John,Doe,john.doe@example.com,CEO,2015-12-31T23:59:59,No,Inactive
```
#### Response - Other (no body) ####
|Status Code|Reason|
|:----------|:-----|
|403|Not a Manager of Company|
|404|Company or Progress Not Found|