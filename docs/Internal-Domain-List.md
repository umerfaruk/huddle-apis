# Summary #

The Internal Domain List of a Company is used to classify which Members of the Company are considered Internal and External. External Members of a Company have reduced visibility of the Members of the Company when using Member Autocomplete.

## Beta warning ##

The Internal Domain List API is in Beta

# Operations #

## Retrieving the Internal Domain List ##
If an Internal Domain List has not been created for a Company then it will have the default Internal Domain List, which is turned off and has no entries in it.

### XML Example ###
#### Request ####
```
GET /people/companies/123/internalDomains
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<internalDomains>
  <links>
    <link href="..." rel="self"/>
    <link href="..." rel="parent"/>
    <link href="..." rel="edit"/>
  </links>
  <enabled>true<enabled>
  <entries>
    <entry>huddle.net<entry>
    <entry>yahoo.com</entry>
  </entries>
</internalDomains>
```

### JSON Example ###
#### Request ####
```
GET /people/companies/123/internalDomains
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```
{
  "links": [{
    "rel": "self",
    "href": "..."
  },{
    "rel": "parent",
    "href": "..."
  },{
    "rel": "edit",
    "href": "..."
  }],
  "enabled": true,
  "entries": [
    "huddle.net",
    "yahoo.com"
  ]
}
```

### Link relations ###
|Name|Description|Methods|
|:---|:-------|:------|
|self|The current URI of this company internal domain list|GET|
|parent|The URI of the company|GET|
|edit|The URI to update the internal domain list|PUT|

### Other Responses ###
|Response|Case|
|:---|:-------|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Not a Company Manager or an Admin|
