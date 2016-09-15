Taken from [OAuth 2.0 Framework](https://tools.ietf.org/html/rfc6749#section-1.1)

   _**client**_

>  An application making protected resource requests on behalf of the
>       resource owner and with its authorization.  The term "client" does
>       not imply any particular implementation characteristics (e.g.,
>       whether the application executes on a server, a desktop, or other
>       devices).

***

## Getting Clients

Returns a list of clients; `isPermitted` indicates that the company allows the client to access data belonging to that company in Huddle.  For users authorized as Huddle Administrator the response includes `add`, `permit` and `delete` links.

### JSON ###
#### Request ####
```
GET /people/companies/:id/clients HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```json
{
	"links": [{
		"rel": "self",
		"href": "..."
	},{
		"rel": "add",
		"href": "..."
	},{
		"rel": "permit",
		"href": "..."
	}],
	"clients": [{
		"links": [{
			"rel": "self",
			"href": "..."
		},{
			"rel": "delete",
			"href": "..."
		}],
		"name": "Huddle for Mac",
		"isPermitted": true,
		"isPublic": true
	}, {
		"links": [{
			"rel": "self",
			"href": "..."
		},{
			"rel": "delete",
			"href": "..."
		}],
		"name": "Huddle for Office",
		"isPermitted": true,
		"isPublic": true
	}]
}
```
### XML ###
#### Request ####
```
GET /people/companies/:id/clients HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<clients>
    <links>
        <link rel="self" href="..." />
        <link rel="add" href="..." />
        <link rel="permit" href="..." />
    </links>
    <client>
        <links>
            <link rel="self" href="..." />
            <link rel="delete" href="..." />
        </links>
        <name>Huddle for Mac</name>
        <isPermitted>true</isPermitted>
        <isPublic>true</isPublic>
    </client>
    <client>
        <links>
            <link rel="self" href="..." />
            <link rel="delete" href="..." />
        </links>
        <name>Huddle for Office</name>
        <isPermitted>true</isPermitted>
        <isPublic>true</isPublic>
    </client>
</clients>
```

### Response - Other (no body) ###
|Status Code|Reason|
|:----------|:-----|
|401|Invalid authorization token|
|403|Not a Manager of Company or Huddle Administrator|
|404|Company or Client Not Found|


## Getting Permitted Clients
Get the clients permitted by a company to access data belonging to that company in Huddle.

### JSON ###
#### Request ####
```
GET /people/companies/:id/clients/permitted HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```json
{
	"permittedClients": [{
		"rel": "client",
		"href": "..."
	}, {
		"rel": "client",
		"href": "..."
	}]
}
```

### XML ###
#### Request ####
```
GET /people/companies/:id/clients/permitted HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<permittedClients>
	<link rel="client" href="..." />
	<link rel="client" href="..." />
</permittedClients>
```

### Response - Other (no body) ###
|Status Code|Reason|
|:----------|:-----|
|401|Invalid authorization token|
|403|Not a Manager of Company or Huddle Administrator|
|404|Company or Client Not Found|

## Permitting Clients
Replaces the current permitted client list with the clients specified in the request.  These will become the only clients permitted by a company to access data belonging to that company in Huddle.

### JSON ###
#### Request ####
```
PUT /people/companies/:id/clients/permitted HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```json
{
	"permittedClients": [{
		"rel": "client",
		"href": "..."
	}, {
		"rel": "client",
		"href": "..."
	}]
}
```
#### Response ####
```
HTTP/1.1 204 OK
```

### XML ###
#### Request ####
```
PUT /people/companies/:id/clients/permitted HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```xml
<permittedClients>
	<link rel="client" href="..." />
	<link rel="client" href="..." />
</permittedClients>
```

#### Response ####
```
HTTP/1.1 204 OK
```

### Response - Other (no body) ###
|Status Code|Reason|
|:----------|:-----|
|400|Clients were not able to be permitted|
|401|Invalid authorization token|
|403|Not a Manager of Company or Huddle Administrator|
|404|Company or Client Not Found|

# Administration only APIs

To use these APIs the client must be authorized as a Huddle Administrator.

## Make a client available on a company list

Makes a client available to a company with `IsPermitted` set to the default value of `false`.  Once a client is made available to a company, it can be permitted using the non-administration APIs.  Only one client at a time can be made available with this request.  To make multiple clients available to a company, this API must be called once for each client.

### JSON ###
#### Request ####
```
POST /people/companies/:id/clients HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```json
{
	"rel": "client",
	"href": "..."
}
```

#### Response ####
```
HTTP/1.1 200 OK
```

### XML ###
#### Request ####
```
POST /people/companies/:id/clients HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```xml
<link rel="client" href="..." />
```

#### Response ####
```
HTTP/1.1 200 OK
```

### Response - Other (no body) ###
|Status Code|Reason|
|:----------|:-----|
|401|Invalid authorization token|
|403|Not a Huddle Administrator|
|404|Company or Client Not Found|

## Remove a client from a company list

Removes a client from the list of clients available to a company, deleting any existing permitted setting for that client.

### JSON ###
#### Request ####
```
DELETE /people/companies/:id/clients/:clientid HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
```

### XML ###
#### Request ####
```
DELETE /people/companies/:id/clients/:clientid HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
```

### Response - Other (no body) ###
|Status Code|Reason|
|:----------|:-----|
|401|Invalid authorization token|
|403|Not a Huddle Administrator|
|404|Company or Client Not Found|

## Make a client available for all companies

Makes a client available to all company with `IsPermitted` set to the default value of `false`.  Once a client is made available to a company, it can be permitted using the non-administration APIs.  Only one client at a time can be made available with this request.  To make multiple clients available to a company, this API must be called once for each client.

A progress link is returned as both a Link header and within the body of the response. Details on monitoring progress can be found in the [query progress section](https://github.com/Huddle/huddle-apis/wiki/Managing%20Permitted%20Clients#query-progress). 

### JSON ###
#### Request ####
```
POST /people/bulk-process/clients HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
{
	"add": {
		"rel": "client",
		"href": "..."
	}
}
```

#### Response ####
```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+json
Link: <...>;rel="progress"
```

```json
{
  "link": {
    "rel": "progress",
    "href": "..."
  },
}
```

### XML ###
#### Request ####
```
POST /people/bulk-process/clients HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
<clients>
    <add rel="client" href="..." />
</clients>
```

#### Response ####
```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
Link: <...>;rel="progress"
```
```
<clients>
  <link rel="progress" href="..." />
</clients>
```

### Response - Other (no body) ###
|Status Code|Reason|
|:----------|:-----|
|401|Invalid authorization token|
|403|Not a Huddle Administrator|
|404|Client Not Found|

## Permit a client for all companies

Permits a client for all companies, overwriting any existing permitted setting for a given company/client.

A progress link is returned as both a Link header and within the body of the response. Details on monitoring progress can be found in the [query progress section](https://github.com/Huddle/huddle-apis/wiki/Managing%20Permitted%20Clients#query-progress).

### JSON ###
#### Request ####
```
POST /people/bulk-process/clients HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
{
	"permit": {
		"rel": "client",
		"href": "..."
	}
}
```

#### Response ####
```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+json
Link: <...>;rel="progress"
```

```json
{
  "link": {
    "rel": "progress",
    "href": "..."
  },
}
```

### XML ###
#### Request ####
```
POST /people/bulk-process/clients HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
<clients>
    <permit rel="client" href="..." />
</clients>
```

#### Response ####
```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
Link: <...>;rel="progress"
```
```
<clients>
  <link rel="progress" href="..." />
</clients>
```

### Response - Other (no body) ###
|Status Code|Reason|
|:----------|:-----|
|401|Invalid authorization token|
|403|Not a Huddle Administrator|
|404|Client Not Found|

## Remove a client from all companies

Removes a client from the list of clients available to all companies, deleting any existing permitted setting for a given company/client.

A progress link is returned as both a Link header and within the body of the response. Details on monitoring progress can be found in the [query progress section](https://github.com/Huddle/huddle-apis/wiki/Managing%20Permitted%20Clients#query-progress).

### JSON ###
#### Request ####
```
POST /people/bulk-process/clients HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
{
	"remove": {
		"rel": "client",
		"href": "..."
	}
}
```

#### Response ####
```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+json
Link: <...>;rel="progress"
```

```json
{
  "link": {
    "rel": "progress",
    "href": "..."
  },
}
```

### XML ###
#### Request ####
```
POST /people/bulk-process/clients HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
<clients>
    <remove rel="client" href="..." />
</clients>
```

#### Response ####
```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
Link: <...>;rel="progress"
```
```
<clients>
  <link rel="progress" href="..." />
</clients>
```

### Response - Other (no body) ###
|Status Code|Reason|
|:----------|:-----|
|401|Invalid authorization token|
|403|Not a Huddle Administrator|
|404|Client Not Found|

## Query Progress

### JSON ###
#### Request ####
```
GET /people/bulk-process/clients/progress/:requestId  HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
Link: <...>;rel="progress"
```

```json
{
  "link": {
    "rel": "self",
    "href": "..."
  },
  "status": "InProgress"
}
```


### XML ###
#### Request ####
```
GET /people/bulk-process/clients/progress/:requestId  HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Link: <...>;rel="progress"
```
```
<clients>
  <link rel="self" href="..." />
  <status>InProgress</status>
</clients>
```

### Status values
|Status| Reason |
|:-----|:-------|
|InProgress|Request is currently in progress|
|Finished|Request has completed successfully|
|Error|Requested has finished in error|

### Response - Other (no body) ###
|Status Code|Reason|
|:----------|:-----|
|401|Invalid authorization token|
|403|Not a Huddle Administrator|
|404|Request Not Found|
