# Summary #

The Editable Internal Domain List of a Company is used to edit the [Internal Domain List] of a Company.

## Beta warning ##

The Internal Domain List API is in Beta

# Operations #

## Retrieving the Editable Internal Domain List ##
This represents the fields of the Internal Domain List that can be updated.

### XML Example ###
#### Request ####
```
GET /people/companies/123/internalDomains/edit
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
  <enabled>true</enabled>
  <entries>
    <entry>huddle.net</entry>
    <entry>yahoo.com</entry>
  </entries>
</internalDomains>
```

### JSON Example ###
#### Request ####
```
GET /people/companies/123/internalDomains/edit
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
  "enabled": true,
  "entries": [
    "huddle.net",
    "yahoo.com"
  ]
}
```

### Other Responses ###
|Response|Case|
|:---|:-------|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Not a Company Manager or Admin|

## Creating/Updating the Editable Internal Domain List ##
A PUT will create or update the Internal Domain List replacing what was previously there. All the domains must be well formed.

### XML Example ###
#### Request ####
```
PUT /people/companies/123/internalDomains/edit
Accept: application/vnd.huddle.data+xml
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<internalDomains>
  <enabled>true</enabled>
  <entries>
    <entry>huddle.net</entry>
    <entry>yahoo.com</entry>
  </entries>
</internalDomains>
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<internalDomains>
  <enabled>true</enabled>
  <entries>
    <entry>huddle.net</entry>
    <entry>yahoo.com</entry>
  </entries>
</internalDomains>
```

### JSON Example ###
#### Request ####
```
PUT /people/companies/123/internalDomains/edit
Accept: application/vnd.huddle.data+json
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
{
  "enabled": true,
  "entries": [
    "huddle.net",
    "yahoo.com"
  ]
}
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```
{
  "enabled": true,
  "entries": [
    "huddle.net",
    "yahoo.com"
  ]
}
```

### Other Responses ###
|Response|Case|
|:---|:-------|
|400 Bad Request|Supplied domains are not all valid|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Not a Company Manager|
