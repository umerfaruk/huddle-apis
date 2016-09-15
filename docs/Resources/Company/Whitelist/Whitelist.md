# Summary #

A Whitelist represents the allowed emails and email domains that members of a Company can have. A Company can only have one Whitelist and it can either apply to all Workspace in the Company or a sub-set of them. When [retrieving a Company](Company#Retrieve_a_given_company), the Company will advertise a "whitelist" link, which can be used by a Company Manager to retrive  the Whitelist.

## Beta warning ##

The Whitelist API is in Beta

# Operations #

## Retrieving the Whitelist ##
If a Whitelist has not been created for a Company then a default Whitelist, which is disabled and has no workspaces or entries, will be returned.

### XML Example ###
#### Request ####
```
GET /people/companies/123/whitelist
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
  <links>
    <link href="..." rel="self"/>
    <link href="..." rel="parent"/>
    <link href="..." rel="edit"/>
  </links>
  <enabled>true<enabled>
  <entries>
    <entry>huddle.net<entry>
    <entry>dom@yahoo.com</entry>
  </entries>
  <workspaces>
    <workspace>
      <link rel="self" href="..." />
      <name>My Important Workspace</name>
    </workspace>
    <workspace>
      <link rel="self" href="..." />
      <name>My Secret Workspace</name>
    </workspace>
  </workspaces>
</whitelist>
```

### JSON Example ###
#### Request ####
```
GET /people/companies/123/whitelist
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
    "dom@yahoo.com"
  ],
  "workspaces" : [{
      "links" : [
        { "rel" : "self", "href" : "..." }
      ],
      "name" : "My Important Workspace"
    },{
      "links" : [
        { "rel" : "self", "href" : "..." }
      ],
      "name" : "My Secret Workspace"
    }]
}
```

### Link relations ###
|Name|Description|Methods|
|:---|:-------|:------|
|self|The current URI of this company whitelist|GET|
|parent|The URI of the company|GET|
|edit|The URI to update the whitelist|PUT|

### Other Responses ###
|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Not a Company Manager|403 Forbidden|
