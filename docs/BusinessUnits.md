# Summary #

The Business Unit API is a representation of Business Units that belong to a Company within Huddle.

## Beta warning ##

The Business Unit API is in Beta

|  |
|:-|

# Operations #

## Retrieve Business Units of a given Company ##

You can GET the Business Units for a given Company and it will display all Business Units for that Company.

### XML Example ###

In this example we are asking for the Business Units of a Company with id 123

#### Request ####
```
GET /people/companies/123/businessunits HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<businessUnits>
  <link rel="self" href="people/companies/123/businessunits" />

  <businessUnit>
    <link rel="self" href="..." />
    <link rel="company" href="..." />
    <link rel="editableAccountSettings" href="..." />
    <link rel="edit" href="..." />

    <name>Business Unit</name>
  </businessUnit>
  <businessUnit>
    <link rel="self" href="..." />
    <link rel="company" href="..." />
    <link rel="editableAccountSettings" href="..." />
    <link rel="edit" href="..." />

    <name>Second Business Unit</name>
  </businessUnit>
</businessUnits>
```


### JSON Example ###

In this example we are asking for a company specifying json in the accept header.

#### Request ####
```
GET /people/companies/123/businessunits HTTP/1.1
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
    "links" : [{ "rel" : "self", "href" : "..." }],
    "businessUnits" : [{
      "links" : [
        { "rel" : "self", "href" : "..." },
        { "rel" : "company", "href" : "..." },
        { "rel" : "editableAccountSettings", "href" : "..." }
        { "rel" : "edit", "href" : "..." }
      ],
      "name" : "Business Unit"
    },{
      "links" : [
        { "rel" : "self", "href" : "..." },
        { "rel" : "company", "href" : "..." }
        { "rel" : "editableAccountSettings", "href" : "..." }
        { "rel" : "edit", "href" : "..." }
      ],
      "name" : "Second Business Unit"
    }]
  }

```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Actor does not have company manager permissions|403 Forbidden|
|Company does not exist|404 Not Found|

## Edit a Business Unit ##
In order to edit a Business Unit, you first need to obtain the edit URI (@rel=edit) from a representation of that Business Unit. By issuing a GET request against the edit URI you can get the subset of the resource data that can be modified locally. You can then issue a PUT request against that URI including the new representation of the Business Unit in the body.
### XML Example ###
In this example we are updating the name of the Business Unit, specifying XML in the accept header.

Issuing a GET request will return a representation of the resource data that we can edit.
#### Request ####
```
GET /people/companies/123/businessunits/456/edit
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### Response ####
```
<businessunit>
   <name>Marketing</name>
</businessunit>
```

To update the resource, we can issue a PUT request using the representation we received in the previous GET request.
#### Request ####
```
PUT /people/companies/123/businessunits/456/edit
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<businessunit>
   <name>Sales</name>
</businessunit>
```
#### Response ####
```
HTTP/1.1 204 No Content
Location: /people/companies/123/businessunits/456
```
### JSON Example ###
In this example we are updating the name of the Business Unit, specifying JSON in the accept header.

Issuing a GET request will return a representation of the resource data that we can edit.
#### Request ####
```
GET /people/companies/123/businessunits/456/edit
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### Response ####
```
{
   "name":"Marketing"
}
```

To update the resource, we can issue a PUT request using the representation we received in the previous GET request.
#### Request ####
```
PUT /people/companies/123/businessunits/456/edit
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
{
   "name": "Sales"
}
```
#### Response ####
```
HTTP/1.1 204 No Content
Location: /people/companies/123/businessunits/456
```
#### Other responses ####
|Case|Response|
|:---|:-------|
|Name exceeds maximum length|400 Bad Request|
|Invalid authorization token|401 Unauthorized|
|Actor does not have company manager permissions|403 Forbidden|
|Business unit does not exist|404 Not found|
|Empty request body|415 Unsupported media type|
