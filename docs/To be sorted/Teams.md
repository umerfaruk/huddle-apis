# Summary #

The Teams API is a representation of Teams in a Workspace within Huddle.

## Beta warning ##

The Teams API is in Beta

|  |
|:-|

# Operations #

## Retrieve the Teams of a given Workspace ##

A GET will retrieve all of the Teams of which the Actor is a member in the given Workspace.

A Company Manager (of the Company in which the Workspace was created) or a Workspace Manager (of the Workspace) will GET all of the Teams in the Workspace.

### XML Example ###

In this example we are requesting the Teams of a Workspace with id 123, with xml in the accept header.

#### Request ####
```
GET /people/workspaces/123/teams HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<teams>
  <link rel="self" href="people/workspaces/123/teams" />

  <team>
    <link rel="self" href="..." />
    <name>Team Keen</name>
  </team>
  <team>
    <link rel="self" href="..." />
    <name>Dev Leppard</name>
  </team>
</teams>
```


### JSON Example ###

In this example we are requesting the Teams of a Workspace with id 123, with json in the accept header.

#### Request ####
```
GET /people/workspaces/123/teams HTTP/1.1
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
    "teams" : [{
      "links" : [ { "rel" : "self", "href" : "..." }],
      "name" : "Team Keen"
    },{
      "links" : [ { "rel" : "self", "href" : "..." }],
      "name" : "Dev Leppard"
    }]
  }

```

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this team list. | GET         |

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Actor is in the company, but not in the workspace (unless an appropriate manager)|403 Forbidden|
|Workspace does not exist (may have been deleted)|404 Not Found|
|Actor is not in the workspace and not in the company|404 Not Found|
