# Summary #

Recent Items APIs provide the ability for a user to easily track Recent Items they have Viewed / worked on using any device. Any file, and can be listed, added to and removed and any device can add to the list.

## Beta warning ##

The Recent Items API is in Beta

|  |
|:-|

# Operations #

## Retrieve a user's Recent Items ##

The maximum number of recent items is 50 with subsequent entries resulting in the earliest entries moving off the list.

The POSTing / GETing of recent items is therefore on a LIFO context - with the last file added by the POST being the first retrieved in the GET.

A user may only access their own recent items list.

Huddle's current IMS-style caching will be used. An If-Modified-Since header can be specified in the request header to indicate that only if there are changes to the requested recent items resource since the specified time should they be sent.

Additionally, Last-Modified is included in the response to indicate the last time the recent items resource changed (i.e. a Document was added or removed from the user's recent file list).

**Please Note**:

- Data returned by the recent items API does not represent the Huddle catalog of data. This data has been provided by a client of Huddle and is a representation of the document at the moment in time a recent file was added.

- Permissions may have changed, and the document may not be available to the user

- To obtain the true latest state of a document a GET should be performed on the Self URI of that document.

### Example ###

In this example we are asking for the recent items list for user with id 313.

#### Request ####
```
GET ... HTTP/1.1
Accept: application/xml
Authorization: Bearer theresamooseloose/abootthishoose
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/xml
Last-Modified: Thu, 14 Oct 2014 11:11:11 GMT
```
```
<recentitems>
  <link rel="self" href="..." />
  <maxItems>50</maxItems>
  <recentitem type="Document">
    <link rel="delete" href="..." />
    <link rel="self" href="..." />
    <link rel="alternate" href="..." />
    <title="A recent file document 1 title">
    <contenttype>text/plain</contenttype>
    <workspacetitle>"The parent workspace for document 1" />
    <dateaddedutc>2014-12-23T01:33:44Z />
    <extension>txt</extension>
  </recentitem>
  <recentitem type="document">
    <link rel="delete" href="..." />
    <link rel="self" href="..." />
    <link rel="alternate" href="..." />
    <title="A recent file document 2 title">
    <contenttype>text/plain</contenttype>
    <workspacetitle>"The parent workspace for document 2" />
    <dateaddedutc>2014-12-24T11:21:27Z />
    <extension>txt</extension>
  </recentitem>
</recentitems>
```

### Example ###

In this example we are asking for the recent items list for User with id 313, with json in the accept header.

#### Request ####
```
GET ... HTTP/1.1
Accept: application/json
Authorization: Bearer theresamooseloose/abootthishoose
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/json
Last-Modified: Thu, 14 Oct 2014 11:11:11 GMT
```
```
{
  "links": [
    {
      "rel": "self",
      "href": "..."
    }
  ],
  "maxItems": 50,
  "items": [
    {
      "type": "Document",
      "links": [
        {
          "rel": "delete",
          "href": "..."
        },
        {
          "rel": "self",
          "href": "..."
        },
        {
          "rel": "alternate",
          "href": "..."
        }
      ],
      "title": "A recent file document 1 title",
      "workspacetitletitle": "The parent workspace for document 1",
      "contenttype": "text/plain",
      "dateaddedutc": "2015-02-28T10:01:06Z",
      "extension": "txt"
    },
    {
      "type": "Document",
      "links": [
        {
          "rel": "delete",
          "href": "..."
        },
        {
          "rel": "self",
          "href": "..."
        },
        {
          "rel": "alternate",
          "href": "..."
        }
      ],
      "title": "A recent file document 2 title",
      "workspacetitletitle": "The parent workspace for document 2",
      "contenttype": "text/plain",
      "dateaddedutc": "2015-02-28T10:01:06Z",
      "extension": "txt"
    }
  ]
}

```

## Add a File to a user's Recent Items List ##

A file is added to a user's recent list by POSTing to the recent files endpoint. A limited amount of state must be POSTed by clients to populate the list with pertinent information.

As such any retrieved data may be **out of date**. Resources may exist on the list that the user no longer has access to. A GET against the document resource will both return the right state and whether the user still has access.

The list will is sorted on date added UTC (added server-side on POST). An item that already exists on the list will result in a 201 result, with the file sorted in the list according to the date posted and the existing entries on the list.

### Example: XML ###

In this example we successfully add a Document (id=131) to the recent items list of a User (id=313), using XML.

#### Request ####
```
POST .../files/users/313/recentitems HTTP/1.1
Accept: application/xml
Content-Type: application/xml
Authorization: Bearer theresamooseloose/abootthishoose
```
```
<recentitem>
  <type>Document</type>
  <link rel="self" href="..."/>
  <title>a very important text file</title>
  <contenttype>text/plain</contenttype>
  <workspacetitle>a very important workspace</workspacetitle>
  <extension>txt</extension>
</recentitem>
```

#### Response ####

```
HTTP/1.1 201 CREATED
Content-Type: application/xml
Location: ...
```

### Example: JSON ###

In this example we successfully add a Document (id=131) to the recent items list of a User (id=313), using JSON.

#### Request ####
```
POST /files/users/313/recentitems HTTP/1.1
Accept: application/json
Content-Type: application/json
Authorization: Bearer theresamooseloose/abootthishoose
```
```

{
  "links": [{
      "rel": "self",
      "href": "..."
  }],
  "type":"document",
  "title": "a very important text file",
  "contenttype": "text/plain",
  "workspacetitle": "a very important workspace",
  "extension": "txt"
}

```

#### Response ####

```
HTTP/1.1 201 CREATED
Content-Type: application/json
Location: ...
```

### Example ###

## Remove a File from a user's Recent Item List ##

A Document can be removed from a User's recent file list by issuing a DELETE against the User's Recent Items API

### Example: XML ###

#### Request ####
```
DELETE ... HTTP/1.1
Accept: application/xml
Authorization: Bearer theresamooseloose/abootthishoose
```

#### Response ####

```
HTTP/1.1 204 No Content
```
