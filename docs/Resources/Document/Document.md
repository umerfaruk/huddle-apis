# Summary #

Documents represent files stored within Huddle, such as spreadsheets or images.  Documents also have metadata for version history and commenting.

Documents have a locking mechanism which prevents others from modifying them. See [lock](Lock).


# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving a document|Beta        |
|Downloading document content|Beta        |
|Downloading a document thumbnail|Beta        |
|Updating document title and description|Beta        |
|Creating a new version of a document|Beta        |
|Uploading a document|Beta        |
|Deleting a document|Beta        |
|Sharing a document|Beta        |
|Copying a document|Beta        |
|Moving a document|Beta        |
|Difference between document versions |Beta        |
|Last viewed version of document |Beta  |

# Operations #


---

## Creating a document ##

You can can create a new document within a folder by using the advertised link with the @rel value create-document. For more information, see [Create a document](Folder#Create_a_document).


---

## Retrieving a document ##

When [retrieving a folder](Folder#Retrieve_a_folder), the response will contain the list of documents within the folder. Each document will advertise a _self_ link, which you can use to GET the full document.  This endpoint also supports HEAD requests.

An If-Modified-Since header can be specified in the Request header to indicate that only if there are changes to the Document resource since the specified time should they be sent.

Additionally, Last-Modified is included in the response to indicate the last time the Document resource was generated.

If a document has restricted permissions then the links with the following `rel` attributes will not be returned:
* `make-offline`
* `content`
* `copy`

Additionally the `previews` link will return a boolean value indicating if the document is restricted

### Example ###

#### Request ####
```
GET .../files/documents/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Last-Modified: Thu, 04 Dec 1986 12:45:26 GMT
```
```
<document itemType="Document" title="TPS report May 2010" description="relentlessly mundane and enervating.">
  <link rel="self" href="..." />
  <link rel="parent" href="..." title="..."/>
  <link rel="edit" href="..." />
  <link rel="move" href="..." />
  <link rel="delete" href="..." />
  <link rel="content" href="..." title="..." type="..." />
  <link rel="lock" href="..." />
  <link rel="version-history" href="..." />
  <link rel="create-version" href="..." />
  <link rel="comments" href="..." count="2" />
  <link rel="approvals" href="..." />
  <link rel="permissions" href="..." />
  <link rel="share" href="..." />
  <link rel="previews" href="..." restricted="false" />
  <link rel="alternate" href="..." />
  <link rel="alternate" href="..." type="text/html" />
  <link rel="shortlink" href="..." type="text/html" />
  <link rel="copy" href="..." />
  <link rel="audittrail" href="..." />
  <link rel="printed" href="..." />
  <link rel="viewed" href="..." />
  <link rel="make-offline" href="..." />
  <link rel="publish" href="..." />
  <link rel="publications" href="..." />
  <link rel="last-viewed" href="..." />
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
  <views>-1</views>
  <mimeType>application/...</mimeType>
  <extension>doc</extension>
  <workspace title="My workspace">
    <link rel="self" href="..." />
	<link rel="alternate" href="..." type="text/html" />
  </workspace>
  <thumbnails>
    <thumbnail type=”medium”>
      <link rel="content" href="..." />
    </thumbnail>
  </thumbnails>
  <publication>
    <link rel="bookmark" href="..." />
    <publishedDate>2014-06-15T09:43:17Z</publishedDate>
    <documentVersion title="My amazing document is amazing">
       <link rel="self" href="..." />
       <version>2</version>
       <contentType>application/pdf</contentType>
    </documentVersion>
    <scope>Public</scope>
  </publication>
  <approval>
     <status>Complete</status>
     <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="requester">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
     </actor>
  </approval>
</document>
```


---

## Downloading document content ##

See [Document Content](https://github.com/Huddle/huddle-apis/wiki/DocumentContent)

---

## Updating document title and description ##

If the authenticated user is authorized to edit a document, it will advertise a link with a @rel value of _edit_. To update document metadata submit a PUT request to this URI with the editable fields of the document. For an overview of editing resource in Huddle see [editing resources](Resources#Editing_Resources).

### Example ###

In this example we update the document's title and description using the URI advertised in the document resource.

First, they retrieve the _edit_ uri of the document they wish to edit.

#### Request ####

```
GET .../files/documents/1/edit HTTP/1.1
Accept: application/vnd.huddle.data+xml
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<document title="old title" description="old description">
  <link rel="parent" href="..." />
</document>
```

The user then updates the @title and @description attributes in the editable document resource and PUTs it back to the server.

If a document with the same title and the same extension already exists within the folder, you will receive a [409 Conflict](HttpStatusCodes).

#### Request ####

```
PUT .../files/documents/123/edit HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Content-Length: 102
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<document xmlns="http://schema.huddle.net/2011/02/" title="new title" description="new description" >
   <link rel="parent" href="..." />
</document>
```

#### Response ####

If successful, this operation will return a 204 No content, with a [link header](Link#Header) giving the location of the updated document.

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```
HTTP/1.1 204 No Content
Link <.../files/documents/123>;rel="parent"
```


---

## Moving a document ##

This resource supports moving a document to a new location. If the authenticated user can move a document, the document resource will advertise a link with a @rel of _move_. Note that the @href of this link is the same as the @href for editing a document, since a move is actually an edit operation. To move a document, PUT a new link to this _move_ uri, with a @rel value of _parent_, and an @href value pointing to the new parent folder.

### Example ###

In this example, the user wishes to move a document which has a _self_ uri of `.../files/documents/`**1**, and a _parent_ uri of `.../files/folders/`**2**.

The user wishes to move their document to a new parent with a _self_ uri of `.../files/folders/`**3**. First, they retrieve the _move_ uri of the document they wish to move.

#### Request ####

```
GET .../files/documents/1/edit HTTP/1.1
Accept: application/vnd.huddle.data+xml
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<document title="my document" description="">
  <link rel="parent" href=".../files/folders/2" />
</document>
```

The user then updates the _parent_ link in the editable document resource, setting the @href to the URI of the new parent folder, and PUTs it back to the server.

If a document with the same title and the same extension already exists within the folder, you will receive a [409 Conflict](HttpStatusCodes).

#### Request ####

```
PUT .../files/documents/1/edit HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
```
```
<document itemType="Document" title="my document" description="">
  <link rel="parent" href=".../files/folders/3" />
</document>
```

#### Response ####

The server responds with a 202 Accepted, and provides a link header with a @rel value of parent pointing to the document being moved, where the client can monitor the value of the 'processingStatus' element.

```
HTTP/1.1 202 Accepted
Content-Type: application/vnd.huddle.data+xml
Link: <.../files/documents/123>;rel="parent"
```

The client should follow the link header and perform a [GET](Document#get_document). When the 'processingStatus' element is equal to 'Moving' it means that the operation is still ongoing. When the operation is completed the 'processingStatus' element assumes the value 'Complete'.
While the document is moving it's not possible to download its content, and a [409 Conflict](HttpStatusCodes) will be returned if a client attempts to do so.

Example of a moving document:
```
<document itemType="Document" title="My Document">
  <link rel="self" href=".../files/documents/123" />

  ... other document elements ...

  <processingStatus>Moving</processingStatus>
</document>
```


---

## Creating a new version of a document ##

This resource supports creating a new version of a document.

If the authenticated user can create a new version, the document resource will advertise a link with a @rel value of _create-version_.

To create a new version POST to the _create-version_ URI with either an empty body or a document version representation (see example below). The document version consists of optional title and description attributes. Note that if these attributes are omitted, the previous version's title and description values are used.
The initial POST request must include the mime type and the size in bytes of the document content in the `X-Upload-Content-Type` and `X-Upload-Content-Length`, respectively.

If the document is locked by someone else, then this operation will fail with a [409 Conflict](HttpStatusCodes) response that contains information about the [lock](Lock).

If you want to be sure that no one else creates a new version or change the content of the document while you are creating a new version you should lock the document before performing the operation as described in the [lock](Lock) section.

### Example ###

#### Request ####

```
POST .../files/documents/12345/version HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Content-Length: 102
Authorization: OAuth2 frootymcnooty/vonbootycherooty
X-Upload-Content-Type: application/msword
X-Upload-Content-Length: 54126
```
```
<documentVersion xmlns="http://schema.huddle.net/2011/02/" title="document title" description="document description" versionNote="document note" extension=".doc" />
```

#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| @title   | The title of the document (optional). If this is not included in the request the new document version will have the same title as the previous version. |
| @description | The description of the document (optional). If this is not included in the request the new document version will have the same description as the previous version. |
| @versionNote | A note for the version (optional). |
| @extension | A extension for the document (optional).  This property is optional but recommended.  This helps us determine useful information about duplicate file names. See Response for more info. |

#### Response ####

If the response is successful, it will advertise a link for uploading the content of the document. For information on uploading a document see [Uploading Documents](UploadingDocuments).

Until new binary content is uploaded, the document resource will not advertise a _content_ link. Earlier versions of document content will still be accessible through the [VersionHistory](VersionHistory) resource.

If @extension is supplied and a document with the same title and the same extension already exists within the folder, you will receive a [409 Conflict](HttpStatusCodes).

If the document is locked by someone else, then this operation will fail with a [409 Conflict](HttpStatusCodes).

If a document with the same title already exists within the folder, you will receive a [409 Conflict](HttpStatusCodes) stating that you cannot create a document with a duplicate title. If "extension" was provided on request this will only look for duplicates in the folder for the same extension type.  Without "extension" is not provided, it will look for duplicates in the folder despite what extension type.

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
```
```
<document itemType="Document" title="document title" description="document description">
  <link rel="self" href="..." />
  ... other document elements ...
  <size>0</size>
  <version>2</version>
  <created>2011-10-10T09:02:17Z</created>
  <updated>2011-11-11T10:02:17Z</updated>
  <processingStatus>Complete</processingStatus>
</document>
```


---

## Deleting a document ##

This resource supports deleting a document. If the authenticated user can delete a document, the document resource will advertise a link with @rel value of _delete_. To delete the document, issue a DELETE request to the _delete_ URI.

#### Example ####

#### Request ####
```
DELETE .../files/documents/123 HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If successful, this method will return an empty response with an OK status code and a [link header](Link#Header) to the parent folder.

```
HTTP/1.1 200 OK
Link: <.../files/folders/123>;rel="parent"
```


---

## Restoring a document ##

This resource supports restoring a document. If the authenticated user can restore a document, /restore can be appended to the document resource URI to _restore_ URI. To restore the document, issue a PUT request to the _restore_ URI.

### Example ###

#### Request ####
```
PUT .../files/documents/123/restore HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If successful, this operation will return a 204 No content, with a [link header](Link#Header) giving the location of the restored document.

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```
HTTP/1.1 204 No Content
Location: .../files/documents/123
```
** Note that there is a restore window on comment, any comments deleted within the 30 seconds from when the document was deleted will be restored. Any comments deleted beforehand will remain as deleted.

---

## Copying a document ##

The copy document resource supports making a copy of a document.

To copy a document, first [retrieve the document](Document#Retrieving_a_document) to get its _copy_ link, then POST to the document's copy URI.

If the copy operation is successful, the copy document endpoint will return a 201 Created response, along with the URI of the new document copy in the Location header.

Documents can be copied to the same folder, to another folder in the same workspace, or to a folder in another workspace. When POSTing to the Copy Document endpoint, clients should include a link to the target folder in the POST body of the request (see Example below).

When a document is copied, the copy will have the same title as the original. However, if a document with the same title and description already exists in the target folder, then the copied document will be suffixed with a number. For example, a copy of the document "My document.doc" would be named "My document (1).doc".

It is important to note that the copy document operation copies the content of the document's most recent version. It does _not_ copy previous document versions, comments, activity history, and approvals.

### Example ###

In this example, we will copy document 123 to folder 456.

#### Step 1. Retrieve the document to get its copy link ####

##### Request #####
```
GET .../files/documents/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

##### Response #####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<document>
  <link rel="self" href=".../files/documents/123" />
  <link rel="copy" href=".../files/documents/123/copy" />
</document>
```
_Please note that only the relevant parts of the GET Document response are included in the above example. For the full GET Document response, please refer to [Retrieving a document](Document#Retrieving_a_document)._

#### Step 2. Post to the document's copy URI ####

##### Request #####
```
POST .../files/documents/123/copy HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<copyDocument>
  <targetFolder>
    <link rel="self" href=".../files/folders/456" />  
  </targetFolder>
</copyDocument>
```

##### Response #####
```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
Location: .../files/documents/789
```

### Error Responses ###

|Case|Response|
|:---|:-------|
|Document does not exist|404 Not Found|
|Document is deleted|410 Gone|
|Document is in a moving state|409 Conflict|
|Document is in a processing state|409 Conflict|
|Document is in an error state|409 Conflict|
|Target folder not provided|400 Bad Request|
|Target folder does not exist|400 Bad Request|
|Target folder is deleted|410 Gone|
|User does not have permission to copy documents from the source folder|403 Forbidden|
|User does not have permission to create documents in the target folder|403 Forbidden|
|User does not have permission to publish a document|403 Forbidden|
|User does not have permission to unpublish a document|403 Forbidden|


---

## Downloading a document thumbnail ##

For certain file types, a thumbnail is automatically and asynchronously created when the file is uploaded. This includes the most common image and video MIME types.

If the document version’s MIME type is supported, the Get Document response body will include a "thumbnails" element containing a "thumbnail" child element, which contains a link with a @rel value of _content_. A GET request to the thumbnail content URI will return the thumbnail content.

If a thumbnail is not available, a 404 will be returned.

### Example ###

In this example we have the thumbnail link from the document resource, and issue a GET request to it.

#### Request ####

```
GET ../files/documents/versions/123/thumbnails/medium/content; HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Cache-Control: private, max-age=600
Content-Type: image/jpeg
Content-Length: 930
```
```
A stream that makes up the image...
```


---

## Comments ##

For information on viewing, creating, or deleting document comments, see [Document Comments](DocumentComments).


---


---

## Publishing ##

For information on publishing, see [Publish Documents](PublishDocument).


---

## Approvals ##

For details on viewing or creating document approvals, see [Document Approvals](DocumentApprovals).


---

## Changes ##

For details on viewing changes made to a document, or to manually create document changes, see [Audit Trail](AuditTrail).


---

## Sharing a document ##

For details on sharing a document, see [Sharing](Share).


---

## Previews ##

For details on document previews, see [Previews](Previews).

## Difference between document versions ##

For details on differences between document versions, see [Document Version Difference](Difference).

## Last viewed version of document ##

The document resource will advertise a link "last-viewed". Follow this link to determine when you last viewed the document. The user is determined from the Authorization header given in the request.  See [Document Last Viewed](DocumentLastViewed).

# Syntax #

## Examples ##

#### A document ####
```
<document xmlns="http://schema.huddle.net/2011/02/" itemType="Document" title="TPS report May 2010" description="relentlessly mundane and enervating.">

  <link rel="self" href="..." />
  <link rel="parent" href="..." title="..."/>
  <link rel="edit" href="..." />
  <link rel="move" href="..." />
  <link rel="delete" href="..." />
  <link rel="content" href="..." title="..." type="..." />
  <link rel="lock" href="..." />
  <link rel="version-history" href="..." />
  <link rel="create-version" href="..." />
  <link rel="comments" href="..." count="2" />
  <link rel="approvals" href="..." />
  <link rel="permissions" href="..." />
  <link rel="share" href="..." />
  <link rel="previews" href="..." />
  <link rel="alternate" href="..." />
  <link rel="alternate" href="..." type="text/html" />
  <link rel="shortlink" href="..." type="text/html" />
  <link rel="copy" href="..." />
  <link rel="audittrail" href="..." />
  <link rel="printed" href="..." />
  <link rel="viewed" href="..." />
  <link rel="make-offline" href="..." />
  <link rel="publish" href="..." />
  <link rel="publications" href="..." />
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
  <thumbnails>
    <thumbnail type="medium">
      <link rel='content' href='...' />
    </thumbnail>
  </thumbnails>
  <size>1024</size>
  <version>98</version>
  <created>2007-10-10T09:02:17Z</created>
  <updated>2011-10-10T09:02:17Z</updated>
  <processingStatus>Complete</processingStatus>
  <views>-1</views>
</document>
```

#### A document retrieved by its assignee ####
```
<document itemType="Document" title="My Document">
  <link rel="self" href="..." />
  ... other document elements ...
  <assignment>
     <link rel="self" href="..." />
     <link rel="edit" href="..." />
     <actor name="Jimmy Snake" rel="assignee">
       <link rel="self" href="..." />
       <link rel="avatar" href="..." type="image/jpg" />
       <link rel="alternate" href="..." type="text/html" />
     </actor>  
     <status>Open</status>  
     <created>2007-10-10T09:02:17Z</created>            
   </assignment>
</document>
```

#### A locked document retrieved by the owner of the lock ####
```
<document itemType="Document" title="My Document">
  <link rel="self" href="..." />
  ... other document elements ...
  <lock>
    <link rel="self" href="..." />
    <link rel="delete" href="..." />
    <link rel="parent" href="..." />
    <actor name="Peter Gibson" rel="owner">
      <link rel="self" href="..." />
      <link rel="avatar" href="..." type="image/jpg" />
      <link rel="alternate" href="..." type="text/html" />
    </actor>
    <lockDate>2007-10-10T09:02:17Z</lockDate>
  </lock>
</document>
```

#### A locked document retrieved by a user that doesn't own the lock ####
```
<document itemType="Document" title="My Document">
  <link rel="self" href="..." />
  ... other document elements ...
  <lock>
    <link rel="self" href="..." />
    <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
      <link rel="self" href="..." />
      <link rel="avatar" href="..." type="image/jpg" />
      <link rel="alternate" href="..." type="text/html" />
    </actor>
    <lockDate>2007-10-10T09:02:17Z</lockDate>
  </lock>
</document>
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| @title   | The title of the document |
| @description | The description of the document |
| lock	    | Present if the document is [locked](Lock). |
| size	    | The size of the document expressed in bytes. |
| version	 | The version of the document. |
| created	 | Document created date. |
| updated	 | Document updated date. |
| processingStatus | <ul><li>Complete: the document is not processing</li><li>Processing: the document is empty or an upload is in progress</li><li>Moving: the document is being moved in another location</li><li>Error: a processing error occurred</li><li>Uploaded: The document has had all its content upload and has some final processing processing before it will be marked as complete</li></ul> |
| views    | Depreciated field |
| assignment | If the authenticated user has a document approval assignment for the latest version of the document, an assignment element will be present. See the [DocumentApprovals](DocumentApprovals) resource for more information |
| publications scope | none = not published | public = published on the internet  |
| approval | The approval for the document if applicable.  This currently only contains the approval status and requester. |

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this document. | GET         |
| parent   | The folder which contains this document. The @title parameter will give the folder name. | GET         |
| content  | If the document contains binary content, it can be downloaded at this URI. The @type attribute will give the mime-type of the file, and the @title attribute will give the filename. | GET         |
| delete   | If the user has permission to delete this document, they can do so at this URI. | DELETE      |
| lock     | If the user has permission to lock this document, they can do so at this URI, by posting an empty request. | POST        |
| edit     | If the user can update document details, they can do so at this URI. | PUT         |
| move     | If the user can move a document, they can do so at this URI. The URI is the same as the edit URI. | PUT         |
| version-history | Returns the [version history](VersionHistory) of the document. | GET         |
| upload   | If the user has permission to change the binary content of a document, they can do so at this URI. | POST        |
| create-version | If the user has permission to create a new document version, they can do so at this URI. | POST        |
| comments | If comments exist on the document, the user can view them at this URI. Returns the [DocumentComments](DocumentComments) resource. The @count attribute is the total number of comments for the document | GET         |
| approvals| If approvals exist on the document, the user can view them at this URI. Returns the [DocumentApprovals](DocumentApprovals) resource | GET         |
| share    | If the user has permission to share a document version, they can do so at this URI. | POST        |
| previews | If the document supports previewing, the user can retrieve preview details at this URI. | GET         |
| alternate | Huddle custom scheme URI. It follows the format: huddle://_relative\_self\_link_ so that external applications can register to handle a click on this link by the user and be able to retrieve the document. | N/A         |
| shortlink | A shortened link to the document on the Huddle website. | N/A         |
| copy     | To copy the document|             |
| audittrail | To get a list of changes made to the document. | GET         |
| printed  | To add a "Printed" change to a document's audit trail. | POST        |
| make-offline | To make this document available or unavailable in the current user's offline items collection, POST or DELETE respectively. Note that this link will only be shown if the account that the document belongs to has offline items functionality enabled | POST, DELETE |
| viewed  | To add a "Viewed" change to a document's audit trail. | POST        |
| last-viewed | Details on the version of document that the user last viewed. | GET |


## Schema ##

```

start = document

document = element h:document {
  attribute itemType {xsd:string},
  attribute title {xsd:string},
  attribute description {xsd:string}?,

  (empty | link + |
	(
	  link+,
          actor+,
	  lock?,
          element h:size {xsd:unsignedLong},
          element h:version {xsd:unsignedInt},
          element h:created {xsd:dateTime},
          element h:updated {xsd:dateTime},
          element h:processingStatus {"Complete"|"Error"|"Moving"|"Processing"|"Uploaded"},
          element h:views {xsd:unsignedInt},
          assignment?,
          thumbnails?
	))
}

lock = element h:lock {
  link+,
  actor
}

assignment= element h:assignment{
  link+,
  actor,
  element h:status {"Complete"|"Open"},
  element h:created {xsd:dateTime},
  element h:completed? {xsd:dateTime}
}

thumbnails = element h:thumbnails{
  thumbnail+
}

thumbnail = element h:thumbnail{
  link+
}

```
