# Summary #

Offline items reference documents that a user has selected to be easily accessible and selected to be available offline, and can be listed, added to and removed.

## Proposal warning ##

This is a proposed API specification and is not currently implemented.

## Beta warning ##

The offline items API is in Beta

|  |
|:-|

# Operations #

## Retrieve a user's offline items ##

You can GET the offline items for a user. Currently, the only type of offlineitem being returned is one referencing a document, but clients should aim to be compatible with an offlinteitems resource that references other types.

The maximum number of offline items returned is 50, and a user can only access their own offline items list.

An `If-Modified-Since` header can be specified in the request header to indicate that only if there are changes to the requested offline items resource since the specified time should they be sent.

Additionally, `Last-Modified` is included in the response to indicate the last time the offline items resource changed (i.e. a document was added or removed, or a document's properties were changed).

Please Note:

- offline items returned represent a subset of a standard document response, with some permission-based action links and metadata (move, create-version, delete) removed for implementation efficiency.


- when a document has been deleted, the corresponding offline item's `availabile` element will contain the value 'false', and the element's `reason` attribute will contain the value 'deleted'. In this case, the only information returned about the document is its self link.

### Example ###

In this example we are asking for the list of offline items for user with id 123.

#### Request ####
```
GET /files/users/123/offlineitems HTTP/1.1
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/xml
Last-Modified: Thu, 04 Dec 1986 12:45:26 GMT
```
```
<offlineitems>
  <link rel="self" href="files/users/123/offlineitems" />
  <maxItems>50</maxItems>
  <offlineitem type="document">
    <link rel="self" href="..." />
    <link rel="delete" href="..." />
    <available>true</available>
    <target>
      <document title="TPS Report May 2010" description="relentlessly mundane and enervating">
        <link rel="self" href="..." />    
        <link rel="content" href="..." title="..." type="..." />      
        <link rel="version-history" href="..." />      
        <link rel="comments" href="..." count="2" />
        <link rel="approvals" href="..." />
        <link rel="permissions" href="..." />      
        <link rel="previews" href="..." />
        <link rel="alternate" href="..." />
        <link rel="alternate" href="..." type="text/html" />      
        <link rel="audittrail" href="..." />
        <link rel="printed" href="..." />
        <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
          <link rel="self" href="..." />
          <link rel="avatar" href="..." type="image/jpg" />
          <link rel="alternate" href="..." type="text/html" />
        </actor>
        <actor name="Barry Potter" email="barry.potter@example.com" rel="updated-by">
          <link rel="self" href="..." />
          <link rel="avatar" href="..." type="image/jpg" />
          <link rel="alternate" href="..." type="text/html" />
        </actor>
        <size>19475</size>
        <version>98</version>
        <created>2007-10-10T09:02:17Z</created>
        <updated>2011-10-10T09:02:17Z</updated>
        <processingStatus>Complete</processingStatus>
        <views>9</views>
        <workspace>
          <link rel="self" href="..." />
        </workspace>
        <thumbnails>
          <thumbnail type=”medium”>
            <link rel="content" href="..." />
          </thumbnail>
        </thumbnails>
      </document>
    </target>
  </offlineitem>
  <offlineitem type="document">
    <link rel="self" href="..." />
    <link rel="delete" href="..." />
    <available reason="deleted">false</available>
    <target>
      <document>
        <link rel="self" href="..." />    
      </document>
    </target>
  </offlineitem>
  <offlineitem type="folder">
    <link rel="self" href="..." />
    <link rel="delete" href="..." />
    <available>true</available>
    <target>
      <folder xmlns="https://schema.huddle.net/2011/02/" title="My folder" displayName="My folder" description="Folder description">
        <link rel="self" href="files/folders/12345" />
        <link rel="parent" href="..." />
        <link rel="workspace-summary" href="..." />

        <actors>
          <actor name="Ian Cooper" email="ian.cooper@example.com" rel="owner">
            <link rel="self" href="..." />
            ... other actor elements...
          </actor> 
        </actors> 
        <folders>
          <folder title="sub folder" description="my subfolder">
            <link rel="self" href="..." />
            ... other folder elements...
          </folder>
        </folders>
        <documents>
          <document title="document title" description="document description">
          <link rel="self" href="..." />
          ... other document elements ...
          </document>
        </documents>
        <created>2007-10-10T09:02:17Z</created>
        <updated>2007-01-01T00:02:03Z</updated>
      </folder>   
    </target>
  </offlineitem>
</offlineitems>
```


---



### Example ###

In this example we are asking for the list of offline items for user with id 123, specifying json in the accept header.

#### Request ####
```
GET /files/users/123/offlineitems HTTP/1.1
Accept: application/json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```

{
  "links": [{
    "rel": "self",
    "href": "files/users/123/offlineitems"
  }],
  "maxItems" : 50,
  "items" : [{
    "type": "document",
    "links": [
      {
        "rel": "self",
        "href": "..."
      },
      {
        "rel": "delete",
        "href": "..."
      }
    ],
    "available": {
       "value": true
    },
    "target": {
      "document": {
          "title": "TPS report May 2010",
          "description": "relentlessly mundane and enervating.",
           ... 
      }
    }
  },{
    "type": "document",
    "links": [
      {
        "rel": "self",
        "href": "..."
      }
    ],
    "available": {
       "reason": "deleted",
       "value": false
    },
    "target": {
      "document": {
          "links": [
            {
              "rel": "self",
              "href": "..."
            }
          ]
      }
    }
  },{
    "type": "folder",
    "links": [
      {
        "rel": "self",
        "href": "..."
      },
      {
        "rel": "delete",
        "href": "..."
      }
    ],
    "available": {
       "value": true
    },
    "target": {
      "folder": {
          "title": "TPS report May 2010",
          "description": "relentlessly mundane and enervating.",
           ... 
      }
    }
  }]
}


```

## Create an offline item ##


You can POST a document link to this resource to add it to the user's list of offline items. The request will return an empty response with the location header indicating the location of the newly-created offline item.

This end point will only add one document - to add multiple documents use multiple POST requests.

If you have a full offline items list, then POSTing a new item will return a 409 Conflict and an error message indicating that the user's offline items list is full, and that an existing item will first need removing.

In order to add a document to offline items, a user must have read access permissions on that document. If the user lacks this permission, the request will return a 403 Forbidden response code.

### Example: XML ###

In this example we successfully add the document with id 456 to the offline items list of user with id 123, using XML.

#### Request ####
```
POST /files/users/123/offlineitems HTTP/1.1
Accept: application/xml
Content-Type: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<document>
  <link rel="self" href="/files/documents/456"/>
</document>
```

#### Response ####

```
HTTP/1.1 201 CREATED
Content-Type: application/xml
Location: /files/users/123/offlineitems/789
```



### Example: JSON ###

In this example we successfully add the document with id 456 to the offline items list of user with id 123, using JSON. Note that in JSON, we are posting a single item array.

#### Request ####
```
POST /files/users/123/offlineitems HTTP/1.1
Accept: application/json
Content-Type: application/json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
{"links": [{"rel": "document", "href": "/files/documents/2326246"}]}
```

#### Response ####

```
HTTP/1.1 201 CREATED
Content-Type: application/json
Location: /files/users/123/offlineitems/789
```


### Example ###

In this example we attempt to add the document with id 456 to the offline items list of user with id 123, but the user's list is already full.

#### Request ####
```
POST /files/users/123/offlineitems HTTP/1.1
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<document>
  <link rel="self" href="/files/documents/456"/>
</document>
```

#### Response ####

```
HTTP/1.1 409 CONFLICT
Content-Type: application/xml
```
```
<error>
Offline items list is full for this user. Item has not been added.
</error>
```


---



## Delete an offline item ##

You can remove a document from a users offline items list by sending a DELETE request to a specific offline item resource.

### Example ###

In this example we are deleting the offline item with id 456 from the list of the user with id 123.

#### Request ####
```
DELETE /files/users/123/offlineitems/456 HTTP/1.1
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 204 No Content
```
