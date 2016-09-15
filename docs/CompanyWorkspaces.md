# Summary #

The Workspaces API is a representation of Workspaces of a Company within Huddle.

## Beta warning ##

The Workspaces API is in Beta

|  |
|:-|

# Operations #

## Retrieve Workspaces of a given Company ##

You can GET the Workspaces for a given Company and it will display the first page of Workspaces for that Company.

### XML Example ###

In this example we are asking for the Workspaces of a Company with id 123

#### Request ####
```
GET /people/companies/123/workspaces HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<workspaces>
  <link rel="self" href="people/companies/123/workspaces" />

  <workspace>
    <link rel="self" href="..." />
    <link rel="company" href="..." />
    <link rel="teams" href="..." />

    <name>My Important Workspace</name>
    <archived>false</archived>
    <locked>false</locked>
    <managers>
      <link
        rel="self"
        href="..." />
      <link
        rel="avatar"
        href="..." />
      <displayName>Andy McLoughlin</displayName>
    </managers>
    <createdDate>2014-07-04T14:13:57.447Z</createdDate>
    <memberCount>42</memberCount>
  </workspace>
  <workspace>
    <link rel="self" href="..." />
    <link rel="company" href="..." />
    <link rel="teams" href="..." />

    <name>My Secret Workspace</name>
    <archived>false</archived>
    <locked>true</locked>
    <managers>
      <link
        rel="self"
        href="..." />
      <link
        rel="avatar"
        href="..." />
      <displayName>Andy McLoughlin</displayName>
    </managers>
    <createdDate>2014-07-04T13:54:16.617Z</createdDate>
    <memberCount>17</memberCount>
  </workspace>
  <filteredWorkspaces>2</filteredWorkspaces>
  <totalWorkspaces>2</totalWorkspaces>
</workspaces>
```


### JSON Example ###

In this example we are asking for a company specifying json in the accept header.

#### Request ####
```
GET /people/companies/123/workspaces HTTP/1.1
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
    "workspaces" : [{
      "links" : [
        { "rel" : "self", "href" : "..." },
        { "rel" : "company", "href" : "..." },
        { "rel" : "teams", "href" : "..."}
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
    },{
      "links" : [
        { "rel" : "self", "href" : "..." },
        { "rel" : "company", "href" : "..." },
        { "rel" : "teams", "href" : "..."}
      ],
      "name" : "My Secret Workspace",
      "archived" : false,
      "locked" : true,
      "managers": [{
          "links": [
            { "rel": "self", "href": "..." },
            { "rel": "avatar", "href": "..."}
          ],
          "displayName": "Andy McLoughlin"
      }],
      "createdDate" : "2014-07-04T13:54:16.617Z",
      "memberCount" : 17
    }],
    "filteredWorkspaces": 2,
    "totalWorkspaces": 2
  }

```

## Parameters ##
| **Name** | **Description** | **Methods** | **Optional** | **Default** |
|:---------|:----------------|:------------|:-------------|:------------|
| page     | Page that you are requesting | GET         | Yes          | 1           |
| q        | Query with which to filter results. Space delimited query terms will return results that match all terms on name. Multiple terms constitute a phrase. Comma delimited phrases will match across any phrases. | GET         | Yes          |             |
| sort     | Sorting. Can either be in the form of fieldname (case-insensitive), or fieldname:asc/fieldname:desc. If direction is omitted, direction will be asc. Can sort on "name" and "membercount". | GET         | Yes          | name:asc    |

Request with parameter
```
GET /people/companies/123/workspaces?page=1&q=workspace 1, workspace 2&sort=name:desc HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this workspace list. | GET         |
| next     | The URI of the next page of workspaces, using sort order specified. | GET         |
| prev     | The URI of the previous page of workspaces, using sort order specified. | GET         |
| first    | The URI of the first page of workspaces, using sort order specified.  | GET         |
| last     | The URI of the last page of workspaces, using sort order specified. | GET         |

The 'prev' link is not present on the first page of workspaces.

The 'next' link is not present on the last page of workspaces.

The 'last' and 'first' links are not present if there is only a single page.

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Actor does not have company manager permissions|403 Forbidden|
|Requested page does not exist (greater than last page)|404 Not Found|
|Requested page is invalid, less than 1|400 Bad Request|
|Specified fieldName for sort does not exist or is not available for sorting|400 Bad Request|
|Sort order is not :asc or :desc|400 Bad Request|
