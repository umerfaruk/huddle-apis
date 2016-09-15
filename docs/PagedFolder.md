# Summary #

Folders are used to organise, group, and protect [documents](Document) that are stored in Huddle.

Folders in Huddle are conceptually similar to folders on a hard drive in that folders can contain other folders as well as documents.

[Access controls](BasicConcepts#AccessControlEntries) can be set on a folder to allow different groups of people to read, write, and delete those documents.

A paged folder allows consumers of the API to request a subset of the child items of the folder (other folders and documents) be delivered with each call. This is useful both to reduce the response time from the server, and decrease the size of the entity body in the response. Huddle recommends that clients do not make unbounded requests

A paged folder allows consumers of the API to request a subset of the child items of the folder (other folders and documents) be delivered with each call. This is useful both to reduce the response time from the server, and decrease the size of the entity body in the response. Huddle recommends that clients do not make unbounded requests.

The link for a paged folder is advertised in the [Folder](Folder) response, with a `rel` value of `collection`. The collection for a parent folder is also advertised in the [Folder](Folder) and [Document](Document) responses, with a `rel` of `parent-collection`.


|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|General Operations on a Folder|Live        |
|Retrieving a folder|Beta        |

# Operations #

## General Operations on a Folder ##
A Paged Folder resource supports most of the operations as a [Folder](Folder) resource, such as Creating a Document. Please see the documentation for that resource for those operations. This resource description only considers the retrieval of a Paged Folder resource.


## Retrieving a folder ##

The top level folder in any workspace is returned as part of a [Workspace](Workspace). You can navigate the folder hierarchy by following the [links](Link) returned in the folder structure.

The root folder element contains a displayName attribute, as well as a title. In most cases, these will be the same; however, if the folder being retrieved is the document library (i.e. root) folder in the workspace, the displayName attribute's value will match the name of the workspace, which is generally more appropriate than "documentlibrary" for display purposes. Please note that this is a read-only property, and including a displayName attribute in any PUT or POST requests will have no effect.

### Example (XML) ###

#### Request ####
```
GET /files/pagedfolders/12345 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
```

#### Response ####

This response uses the standard error codes and returns standard response headers.

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Last-Modified: Tue, 1 Feb 2011 13:18:42 GMT
```
```
<folder xmlns="https://schema.huddle.net/2011/02/" title="My folder" displayName="My folder" description="Folder description">
  <created>2007-10-10T09:02:17Z</created>
  <updated>2007-01-01T00:02:03Z</updated>
  <link rel="self" href="files/pagedfolders/12345" />
  <link rel="folder" href="files/folders/12345" />
  <link rel="paged-folder" href="files/pagedfolders/12345 />
  <link rel="parent" href="..." />
  <link rel="parent-folder" href="..." />
  <link rel="paged-parent-folder" href="..." />
  <link rel="delete" href="..." />
  <link rel="bulk-delete" href="..." />
  <link rel="bulk-move" href="..." />
  <link rel="bulk-download" href="..." />
  <link rel="edit" href="..." />
  <link rel="move" href="..." />
  <link rel="create-folder" href="..." />
  <link rel="create-document" href="..." />
  <link rel="permissions" href="..." />
  <link rel="editPermissions" href="..." />
  <link rel="workspace-summary" href="..." />
  <link rel="share" href="..." />
  <link rel="copy" href="..." />
  <link rel="document-library-settings" href="..." />
  <actors>
    <actor name="Ian Cooper" email="ian.cooper@example.com" rel="owner">
      <link rel="self" href="..." />
      ... other actor elements...
    </actor> 
  </actors>
  <permissionsSummary>... see below ...</permissionsSummary>
  <ancestry>... see below ...</ancestry>
  <collection>
    <links>
      <link rel="current" href="..." />
      <link rel="prev" href="..." />
      <link rel="next" href="..." />
      <link rel="first" href="" />
    <links/>
    <items>
      <folder type="folder" title="sub folder" description="my subfolder">
        <link rel="self" href="..." />
        <link rel="paged-parent-folder" href="..." />
          ... other folder elements...
      </folder>
      <document type="document" title="document title" description="document description">
        <link rel="self" href="..." />
        <link rel="parentCollection" href="..." />
          ... other document elements ...
      </document>
    </items>
  </collection>
  <queries>
    <query>
      <link href="/files/pagedfolders/12345" rel="paging"/>
      <prompt>Enter paging choices</prompt>
      <data>
        <parameter name="sortByFolderIndex" value="" />
        <parameter name="groupByFolderIndex" value="0" /> 
        <parameter name="sortByDocumentIndex" value="" />
        <parameter name="groupByDocumentIndex" value="0" />
        <parameter name="size" value="50" />
        <parameter name="direction" value="forwards" />
        <parameter name="orderBy" value="title" />
        <parameter name="q" value="" />
        <parameter name="paging" value="keyset" />
        <parameter name="grouping" value="folders;documents" />
      </data>
    </query>
  </queries>
</folder>
```

### Example (JSON) ###

#### Request ####
```
GET /files/pagedfolders/12345 HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
```
#### Response ####

This response uses the standard error codes and returns standard response headers.

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
Last-Modified: Tue, 1 Feb 2011 13:18:42 GMT
```
```
{
    "title": "My folder",
    "displayName": "My folder",
    "description": "Folder description",
    "links": [
        {"rel": "self", "href": "files/pagedfolders/12345"},
        {"rel": "folder", "href": "files/folders/12345"},
        {"rel": "paged-folder", "href": "files/pagedfolders/12345"},
        {"rel": "parent", "href": "..."},
        {"rel": "parent-folder", "href": "..."},
        {"rel": "paged-parent-folder", "href": "..."},
        {"rel": "delete", "href": "..."},
        {"rel": "bulk-delete", "href": "..."},
        {"rel": "bulk-move", "href": "..."},
        {"rel": "bulk-download", "href": "..."},
        {"rel": "edit", "href": "..."},
        {"rel": "move", "href": "..."},
        {"rel": "create-folder", "href": "..."},
        {"rel": "create-document", "href": "..."},
        {"rel": "permissions", "href": "..."},
        {"rel": "editPermissions", "href": "..."},
        {"rel": "workspace-summary", "href": "..."},
        {"rel": "share", "href": "..."},
        {"rel": "copy", "href": "..."},
        {"rel": "document-library-settings", "href": "..."}
    ],
    "actors": [
        {
            "name": "Ian Cooper",
            "email": "ian.cooper@example.com",
            "rel": "owner",
            ... other actor elements...
        }
    ],
    "permissionsSummary": ... see below ...,
    "ancestry": ... see below ...,
    "collection": {
        "links": [
            {"rel":"current", "href":"..."},
            {"rel":"prev", "href":"..."},
            {"rel":"next", "href":"..."},
            {"rel":"first", "href":"..."}
        ]
        "items": [
            {
                "type": "folder",
                "title": "sub folder",
                "description": "my subfolder"
                "links": [...]
                ... other folder elements ...
            },
            {
                "type": "document",
                "title": "document title",
                "description": "document description"
                "links": [...]
                ... other document elements ...
            }
        ]
    },
    "queries": [
        {
            "link": {"href":"/files/pagedfolders/12345", "rel": "paging"},
            "prompt": "Enter paging choices",
            "data": [
                {"name": "sortByFolderIndex", "value":""},
                {"name": "groupByFolderIndex", "value":"0"},
                {"name": "sortByDocumentIndex", "value":""},
                {"name": "groupByDocumentIndex", "value":"0"},
                {"name": "size", "value":"50"},
                {"name": "direction", "value":"forwards"},
                {"name": "orderBy", "value":"title"},
                {"name": "q", "value":""},
                {"name": "paging", "value":"keyset"},
                {"name": "grouping", "value":"folders;documents"},
            ]
        }
    ]
}
```

### Checking For Modifications ###
With a paged folder we recommend using a HEAD request with an If-Modified-Since header to the resource without a query string to determine if the underlying resource has changed.

An `If-Modified-Since` header can be specified in the Request header to indicate that only if there are changes to the Folder resource since the specified time should they be sent.

Additionally, `Last-Modified` is included in the response to indicate the last time the Folder resource was generated.

### Included projections ###

When retrieving a Folder, a `projection` query string can be added to the request URL in order to retrieve additional information about the Folder. Currently supported are `permissions` and `ancestry`.

With a Paged Folder resource, we fold the projections into the resource; it is not possible at the time to request a representation of the resource that omits them.

This means:

The `<folder>` element will contain a `<permissionsSummary>` element

```
<folder xmlns="https://schema.huddle.net/2011/02/" title="My folder" displayName="My folder" description="Folder description">
  ... other folder elements ...
  <permissionsSummary restricted=true>
    <link rel="self" href="files/folders/12345/permissions" />
    <actor name="Bob Loblaw" email="bob.loblaw@bobloblawlawblog.com" rel="owner">
      <link rel="self" href="..." />
      ... other actor elements ...
    </actor>
    <teams>
      <team title="Team A" type="members">
        <link rel="self" href="tag:huddle.net,Team:23456" />
        <permissions>
           <permission type="Read" />
           <permission type="Edit" />
         </permissions>
       </team>
       <team title="Workspace Managers" type="managers">
          <link rel="self" href="tag:huddle.net,Managers:54321" />
          <permissions>
            <permission type="Read" />
            <permission type="Edit" />
            <permission type="Delete" />
          </permissions>
          <members>
            <actor name="Barry Zuckercorn" email="barry@zuckercorn.com" rel="manager">
              <link rel="self" href="..." />
              ... other actor elements ...
            </actor>
          </members>
        </team>
     </teams>
  </permissionsSummary>
</folder>
```

The information returned includes a list of the teams which have permissions of some kind on the folder and what permissions these are. They do not include a list of members, with the exception of the **Workspace Managers** team. 
The permissionsSummary attribute "restricted" is deprecated and should be ignored.  

A `self` link is also included as a link to the full [Permissions](Permissions) entity for the folder, which _does_ include the member list for each team.

In addition to the information above, the `<folder>` element will also contain a `<ancestry>` element like so:

```
<folder xmlns="https://schema.huddle.net/2011/02/" title="My folder" displayName="My folder" description="Folder description">
 ... other folder elements ...
 <ancestry>
    <folder title="Parent Folder">
      <link rel="self" href="..." />
      <link rel="folder" href="..." />
      <link rel="paged-folder" href="..." />
      <link rel="paged-parent-folder" href="..." />
      <link rel="parent-folder" href="..." />
    </folder>
    <folder title="Grandparent Folder">
      <link rel="self" href="..." />
      <link rel="folder" href="..." />
      <link rel="paged-folder" href="..." />
      <link rel="paged-parent-folder" href="..." />
      <link rel="parent-folder" href="..." />
    </folder>
  </ancestry>
</folder>
```

The information returned includes a list of the folders directly above this one in the folder structure, up until the document library. This is an ascending-ordered list, so the first parent will be first, then the grandparent, then the great-grandparent, and so on.

The limit of the amount of ancestors returned for a given folder is 52, so in order to get the full ancestry when there are more than 52 ancestors, one would need to make a request for the ancestry of the 52nd parent, and so on until the root is reached. The root will be identified by having no parent link.

Each folder includes the title (as an attribute) and a link to itself and its parent.

Finally, the root `<ancestry>` element includes a UNIX-style path from the root folder to the current folder.

## Controlling Paging ##
The default paging for a Paged Folder is to return the first 50 items, sorted by title. You can adjust this by varying the query string used to provide the previous and next links. The queries element describes the syntax used when composing a query. Well-designed clients should use the queries element when building query strings as it is possible we will add new parameters, or retire them.

We pre-generate links for a default and expose them on the resource. If you create your own query string to replace the default, then links generated from that request will respect the choices made in that request.

Normally you would only want to change the orderBy and size parameters on a request, without index values, to refresh the view of a folder, paging items from the beginning. You should not need to set the index values manually from the client.

```
<queries>
   <query>
     <link href="/files/pagedfolders/12345" rel="paging"/>
     <prompt>Enter paging choices</prompt>
     <data>
       <parameter name="sortByFolderIndex" value="" />
       <parameter name="groupByFolderIndex" value="0" />
       <parameter name="sortByDocumentIndex" value="" />
       <parameter name="groupByDocumentIndex" value="0" />
       <parameter name="size" value="50" />
       <parameter name="direction" value="forwards" />
       <parameter name="orderBy" value="title" />
       <parameter name="q" value="" />
       <parameter name="paging" value="keyset" />
       <parameter name="grouping" value="folders;documents" />
     </data>
   </query>
</queries>
```

### Parameters ###
| **Name** | **Description** | **Methods** | **Optional** | **Default** | **Notes** |
|:---------|:----------------|:------------|:-------------|:------------|:----------|
| `sortByFolderIndex` | The index of the sort-by expression of the folder we want to page from in this set | GET         | Yes          | Return the first page | Must be used with `groupByFolderIndex` |
| `groupByFolderIndex` | The id of the folder we want to page from (to handle the case where sort-by values are the same) | GET         | Yes          | 0           | Must be used with `sortByFolderIndex` |
| `sortByDocumentIndex` | The index of the sort-by expression of the document we want to page from in this set | GET         | Yes          | Return the first page | Must be used with `groupByDocumentIndex` |
| `groupByDocumentIndex` | The id of the document we want to page from (to handle the case where sort-by values are the same) | GET         | Yes          | 0           | Must be used with `sortByDocumentIndex` |
| `direction` | The paging direction: `forwards` or `backwards`. If `forwards`, items with a sort-by expression _greater than_ the provided index will be returned. If `backwards`, items with a sort-by expression _less than_ the provided index will be returned. | GET         | Yes          | `forwards`  |           |
| `size`   | Number of items to retrieve on a page | GET         | Yes          | 50          | Maximum is 250 |
| `orderBy` | The order by which to sort items within a group | GET         | Yes          | title;extension;updateddate | Must be available to both folder and document |
| `q`      | A list of space separated words by which you want to filter on document or folder title | GET         | Yes          | ""          | Uses 'starts with' |
| `paging` | Style of paging documents to use | GET         | Yes          | keyset      | Only keyset is currently supported |
| `grouping` | Group by item type | GET         | Yes          | folders;documents | Only folders;documents is currently supported |

Request with parameter
```
GET //files/pagedfolders/12345?orderBy=title&size=10 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```


### Link relations ###

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| next     | The URI of the next page of folder items, using sort order specified. | GET         |
| prev     | The URI of the previous page of folder items, using sort order specified. | GET         |
| first    | The URI of the first page of folder items, using sort order specified.  | GET         |


The 'prev' link is not present on the first page of published documents.

The 'next' link is not present on the last page of published documents.

The 'last' and 'first' links are not present if there is only a single page.

### Error Responses ###

|Case|Response|
|:---|:-------|
|Folder does not exist|404 Not Found|
|Folder is deleted|410 Gone|
|User does not have permission to folder|403 Forbidden|
