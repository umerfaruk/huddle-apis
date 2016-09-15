# THIS API IS DEPRECATED #

# Summary #

Changes represent the list of changes made to an object and its children.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving a changes list|Beta        |

# Operations #

## Retrieving a changes list ##

When [retrieving a workspace](Workspace#Retrieving_a_workspace), the response will advertise a _changes_ link, which you can use to GET the changes made to the workspace and its children.

Responses are paged, you can navigate between pages by following the _next_ and _prev_ links. The URI advertised by the workspace will always point to the most recent set of changes.
The earliest set of changes is advertised by the _last_ link. The most recent changes are available at the _first_ link.

### Example ###

In this example we are asking for the list of changes of a folder and its children.

#### Request ####
```
GET /workspaces/12345/changes
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
```

#### Response ####

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Last-Modified: Tue, 1 Feb 2011 13:18:42 GMT
```
```
<changes xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="..." />
  <link rel="next" href="..." />
  <link rel="last" href="..." />

  <change>
    <link rel="self" href="..." />
    <link rel="subject" href="..." />
    <link rel="workspace" href="..." />
    <link rel="parent" href="..." />
    <type>DocumentCheckedOut</type>
    <actor name="Mrs. Teasdale" email="teasdale@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
    </actor>
    <created>2011-02-01T13:18:42Z</created>
    <correlationId>2345-3567-1234-235566</correlationId>
  </change>

  <change>
    <link rel="self" href="..." />
    <link rel="subject" href="..." />
    <link rel="workspace" href="..." />
    <link rel="parent" href="..." />
    <type>ObjectCreated</type>
    <actor name="Rufus T. Firefly" email="rufus.firefly@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
    </actor>
    <created>2011-02-01T13:17:42Z</created>
  </change>
</changes>
```


---


# Syntax #

## Example ##

```
<changes xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="..." />
  <link rel="next" href="..." />
  <link rel="first" href="..." />
  <link rel="last" href="..." />

  <change>
    <link rel="self" href="..." />
    <link rel="subject" href="..." />
    <link rel="workspace" href="..." />
    <link rel="parent" href="..." />
    <type>DocumentCheckedOut</type>
    <actor name="Isidore McHohenheim" email="isidore.mchohenheim@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
    </actor>
    <created>2011-02-01T13:17:42Z</created>
    <correlationId>2345-3567-1234-235566</correlationId>
  </change>

  <change>
    <link rel="self" href="..." />
    <link rel="subject" href="..." />
    <link rel="workspace" href="..." />
    <link rel="parent" href="..." />
    <type>ObjectCreated</type>
    <actor name="Isidore McHohenheim" email="isidore.mchohenheim@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
    </actor>
    <created>2011-02-01T13:17:40Z</created>
  </change>
</changes>
```


## Parameters ##
| **Name** | **Description** | **Methods** | **Optional** |
|:---------|:----------------|:------------|:-------------|
| pagesize | Number of elements to return per page, between 50 - 2000. Default: 50 | GET         | Yes          |

Request with parameter
```
GET /workspaces/12345/changes?pagesize=200
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| change	  | A change element is a representation of the single change made to a folder or to one of its children, or a user. |
| type	    | The type of the change made to the resource. The possible types are: ObjectCreated, ObjectUpdated, ObjectDeleted, ObjectArchived, CommentCreated, CommentDeleted, DocumentMoved, DocumentUpdated, DocumentCheckedOut, DocumentUndoCheckOut, FolderMoved,  DocumentCopied, DocumentVersionDeleted, WorkflowUpdated, WorkflowCompleted, WorkflowAssignmentCompleted, WorkflowAssignmentClosed, PermissionChanged, |
| created	 | The date of the change. |
| correlationId | If a set of change elements are related, they will all include a correlationId . This Id is a string representation of a Guid and will have the same value for all related change events. |


## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this change list. | GET         |
| subject  | The URI of the resource for which the change applies (e.g. Document, Folder, User). | GET         |
| workspace | The URI of the workspace for which the change applies. | GET         |
| parent   | Changes that relate to a [Document](Document) or [Folder](Folder) resource will have a parent link. | GET         |
| next     | The URI of the next set of changes ordered from newest to oldest. |
| prev     | The URI of the previous set of changes ordered from newest to oldest. |
| first    | The URI of the most recent set of changes. |
| last     | The URI of the oldest set of changes. |

## Schema ##

```
start = changes

changes = element h:changes {
  link+,
  change+
}

change = element h:change {
  link+,
  element h:type {xsd:string},
  actor,
  element h:created {xsd:dateTime}
  element h:correlationId? {xsd:string}
}
```
