# Summary #
The Editable Company is a representation of Companies within Huddle that shows what properties can be edited and allows them to be updated.
## Beta warning ##
The Company API is in Beta
# Operations #
## Retrieving the EditableCompany ##

A GET to this endpoint shows which properties are editable and provides a template for the PUT. The link for this can be found by [retrieving the Company](Company#retrieve-a-given-company).
### XML Example ###
#### Request ####
```
GET /people/companies/123/edit HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/xml
```
```
<company>
  <name>Huddle</name>
</company>
```
### JSON Example ###
#### Request ####
```
GET /people/companies/123/edit HTTP/1.1
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
  "name" : "Huddle"
}
```

### Other Responses ###
|Case|Response|
|:---|:-------|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Actor is not Company Manager or Admin|
|404 Not Found|Company does not exist|

---

## Updating the Company ##

A PUT to this endpoint will update the Company to the specified properties.

### XML Example ###

In this example we are updating the name of Company 123 to Huddle.
#### Request ####
```
PUT /people/companies/123/edit HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<company>
  <name>Huddle</name>
</company>
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/xml
```
```
<company>
  <name>Huddle</name>
</company>
```

### JSON Example ###
#### Request ####
```
PUT /people/companies/123/edit HTTP/1.1
Accept: application/vnd.huddle.data+json
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
{
  "name": "Huddle"
}
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```
{
  "name" : "Huddle"
}
```

### Other Responses ###
|Case|Response|
|:---|:-------|
|400 Bad Request|Name is not there or over 255 characters|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Actor is not Company Manager or Admin|
|404 Not Found|Company does not exist|
|409 Conflict|Name is already in use|