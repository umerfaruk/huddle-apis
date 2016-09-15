# Summary #
Exports all Workspaces of a Company to a CSV file.
## Beta warning ##
The API is in Beta
# Operations #
## Create Workspace Export ##
This resource supports the creation of a CSV file containing all Workspaces of a Company. As this can take some time, it is treated as an asynchronous process.

A successful POST will initiate this process and return a 202 Accepted response containing a link header with a URI to GET the progress of the action.

The link to this API can be found on the [Company](Company) resource with the rel "workspaces-export". (Still to be implemented)
#### Request ####
**JSON**
```
POST /people/companies/123/workspaces/export HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
**XML**
```
POST /people/companies/123/workspaces/export HTTP/1.1
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

<workspaceExport>
  <link rel="self" href="..." />
  <status>InProgress</status>
<workspaceExport>
```
#### Response - Workspace Limit Exceeded ####
Due to the processing cost, this API is limited to Companies that have under 20,000 Workspaces

**JSON**
```
HTTP/1.1 403 Forbidden
Content-Type: application/vnd.huddle.data+json

{ "errors": [ "WorkspaceLimitExceeded" ] }
```
**XML**
```
HTTP/1.1 403 Forbidden
Content-Type: application/vnd.huddle.data+xml

<errorResult>
  <error>WorkspaceLimitExceeded</error>
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
GET /people/companies/123/workspaces/export/d8a3d254-9b2a-40c0-b4f5-dbd94c13430a HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
**XML**
```
GET /people/companies/123/workspaces/export/d8a3d254-9b2a-40c0-b4f5-dbd94c13430a HTTP/1.1
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

<workspacesExport>
  <link rel="self" href="..." />
  <status>InProgress</status>
<workspacesExport>
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

<workspacesExport>
  <link rel="self" href="..." />
  <link rel="content" href="..." />
  <status>Finished</status>
<workspacesExport>
```
#### Response - Error####
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

<workspacesExport>
  <link rel="self" href="..." />
  <status>Error</status>
<workspacesExport>
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
GET /people/companies/123/workspaces/export/d8a3d254-9b2a-40c0-b4f5-dbd94c13430a/content HTTP/1.1
Accept: text/csv
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### Response ####
**CSV**

The first row of the CSV contains the field names
```
HTTP/1.1 200 OK
Content-Type: text/csv

Workspace Name, No. of Members, Created Date, No. Workspace Managers, Workspace Managers (Comma separated emails)
"My first workspace", 5, "2015-12-31T23:59:59", 2, "isidore.mchohenheim@example.com,john.doe@example.com" 
```
#### Response - Other (no body) ####
|Status Code|Reason|
|:----------|:-----|
|403|Not a Manager of Company|
|404|Company or Progress Not Found|

