# Summary #

The GET operation shows which fields are editable and provides a template for the PUT operation, which updates the Whitelist. The link for editing the Whitelist can be found by [retrieving the Whitelist](Whitelist#retrieve-a-given-company).

## Beta warning ##

The Whitelist API is in Beta

|  |
|:-|

# Operations #

## Retrieving the EditableWhitelist ##
If a Whitelist has not been created for a Company then a default EditableWhitelist, which is disabled and has no workspaces or entries, will be returned.

### XML Example ###
#### Request ####
```
GET /people/companies/123/whitelist/edit
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<whitelist>
  <enabled>true</enabled>
  <entries>
    <entry>huddle.net</entry>
    <entry>dom@yahoo.com</entry>
  </entries>
  <workspaces>
    <link href="..." rel="workspace"/>
  </workpaces>
</whitelist>
```

### JSON Example ###
#### Request ####
```
GET /people/companies/123/whitelist/edit
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
    "dom@yahoo.com"
  ],
  "workspaces": [{ 
    "rel": "workspace",  
    "href": ".."
  }]
}
```

#### Other Responses ####
|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Not a Company Manager|403 Forbidden|


---

## Creating/Updating the Whitelist ##
A PUT to the Whitelist API will create or update it (overwriting the previous Whitelist setup for the company) to the contents of the request.

All emails and email domains should be well formed and a maximum of 500 entries can be added to the Whitelist.

Adding no workspaces will make the Whitelist apply to all workspaces.

The Actor must be a Company Manager to update the Whitelist.

### XML Example ###
#### Request ####
```
PUT /people/companies/123/whitelist/edit
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<whitelist>
  <enabled>true</enabled>
  <entries>
    <entry>@huddle.net</entry>
    <entry>dom@yahoo.com</entry>
  </entries>
  <workspaces>
    <link href="..." rel="workspace"/>
  </workpaces>
</whitelist>
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<whitelist>
  <enabled>true<enabled>
  <entries>
    <entry>huddle.net<entry>
    <entry>dom@yahoo.com</entry>
  </entries>
  <workspaces>
    <link href="..." rel="workspace"/>
  </workpaces>
</whitelist>
```

### JSON Example ###
#### Request ####
```
PUT /people/companies/123/whitelist/edit
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
{
  "enabled": true,
  "entries": [
    "@huddle.net",
    "dom@yahoo.com"
  ],
  "workspaces": [{ 
    "rel": "workspace",  
    "href": ".."
  }]
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
    "dom@yahoo.com"
  ],
  "workspaces": [{ 
    "rel": "workspace",  
    "href": ".."
  }]
}
```

#### Other Responses ####
|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Not a Company Manager|403 Forbidden|
|Greater than 500 entries in the Whitelist|400 Bad Request|
|Any of the entries are invalid email addresses or domains|400 Bad Request|
|Whitelist does not exist|404 Not Found|
