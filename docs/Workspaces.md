# Summary #

The Workspaces API is a representation of a given Workspace within Huddle.

## Beta warning ##

The Workspaces API is in Beta

|  |
|:-|

# Operations #

## Retrieve a given Workspace ##

You can GET a given Workspace and it will display the information for that Workspace.

### XML Example ###

In this example we are asking for a Workspace with id 123

#### Request ####
```
GET /people/workspaces/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<workspace>
  <links>
    <link rel="self" href="..." />
    <link rel="company" href="..." />
    <link rel="teams" href="..." />
    <link rel="bulkInvitation" href=".." />
  </links>
  <name>My Important Workspace</name>
  <archived>false</archived>
  <locked>false</locked>
  <managers>
    <manager>
      <links>
        <link rel="self" href="..." />
        <link rel="avatar" href="..." />
      </links>
      <displayName>Andy McLoughlin</displayName>
    </manager>
  </managers>
  <createdDate>2014-07-04T14:13:57.447Z</createdDate>
  <memberCount>42</memberCount>
</workspace>
```


### JSON Example ###

In this example we are asking for a company specifying json in the accept header.

#### Request ####
```
GET /people/workspaces/123 HTTP/1.1
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
  "links" : [
    { "rel" : "self", "href" : "..." },
    { "rel" : "company", "href" : "..." },
    { "rel" : "teams", "href" : "..."},
    { "rel" : "bulkInvitation", "href" : "..."}
  ],
  "name" : "My Important Workspace",
  "archived" : false,
  "locked" : false,
  "managers": [{
    "links": [
      { "rel": "self", "href": "..."},
      { "rel": "avatar", "href": "..."}
    ],
    "displayName": "Andy McLoughlin"
  }],
  "createdDate" : "2014-07-04T14:13:57.447Z",
  "memberCount" : 42
}

```

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this Workspace. | GET         |
| company  | The URI of the Company the Workspace belongs to. | GET         |
| teams    | The URI of the Teams list for this Workspace. | GET         |
| bulkInvitation | The URI used to invite members into the Teams of a Workspace. | POST        |

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Actor not in Workspace or not a Company Manager|403 Forbidden|
|Actor not in Company|404 Not Found|
|Workspace does not exist|404 Not Found|
