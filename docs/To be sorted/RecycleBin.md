# Summary #

The Recycle Bin resource allows workspace managers to view the recently-deleted items in a workspace

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
| GET deleted items | Beta       |

# Operations #

## GET workspace recycle bin ##

This resource lists the deleted items in a workspace. A maximum of 50 items are returned.

If successful, this method will return a 200 OK status code, and a list of the deleted documents and folders in this workspace. The list is arranged in reverse-chronological order by the deleted date of the item.

The location of this resource is advertised as a link on the [Folder](Folder) resource, with a rel value of _recycle-bin_.

### Example ###

#### Request (XML) ####
```
GET ... HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response (XML) ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<recycleBin>
  <link rel="self" href="..." />
  <link rel="documentLibrary" href="..." />

  <deletedItems>
    <deletedItem type="folder">
      <link rel="self" href="..."/>
      <link rel="restore" href="..." />

      <title>...</title>
      <description>...</description>
      <deletedDate>2014-12-17T11:32:06Z</deletedDate>

      <actor rel="deleter" name="..." email="...">
      ... other actor elements ...
      </actor>
    </deletedItem>

    <deletedItem type="document">
      <link rel="self" href="..."/>
      <link rel="restore" href="..." />

      <title>...</title>
      <description>...</description>
      <mimeType>...</mimeType>
      <extension>...</extension>
      <deletedDate>2014-12-17T10:17:34Z</deletedDate>

      <actor rel="deleter" name="..." email="...">
      ... other actor elements ...
      </actor>
    </deletedItem>
  </deletedItems>
</recycleBin>
```

#### Request (JSON) ####
```
GET ... HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response (JSON) ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```
{
  "links": [
    {"rel": "self", "href": "..."},
    {"rel": "documentLibrary", "href": "..."}
  ],

  "deletedItems": [
    {
      "type": "folder",
      "links": [
        {"rel": "self", "href": "..."},
        {"rel": "restore", "href": "..."}
      ],
      "title": "...",
      "description": "...",
      "deletedDate": "..."
      "actors": [
        {
          "rel": "deleter",
          "name": "...",
          "email": "...",
          ... other actor elements ...
        }
      ]
    },
    {
      "type": "document",
      "links": [
        {"rel": "self", "href": "..."},
        {"rel": "restore", "href": "..."}
      ],
      "title": "...",
      "description": "...",
      "mimeType": "...",
      "extension": "...",
      "deletedDate": "...",
      "actors": [
        {
          "rel": "deleter",
          "name": "...",
          "email": "...",
          ... other actor elements ...
        }
      ]
    }
  ]
}
```

### Status codes ###

| **Status code** | **Meaning** |
|:----------------|:------------|
| 200 OK          | The operation was successful; the list of deleted items will be included in the response body |
| 401 Not Authorised | You failed to include a valid OAuth token in your request, or the token has expired |
| 403 Forbidden   | You are not a workspace manager of the workspace |
| 404 Not Found   | The workspace does not exist |
