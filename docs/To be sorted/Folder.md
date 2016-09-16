# Summary #

Folders are used to organise, group, and protect [documents](Document) that are stored in Huddle.

Folders in Huddle are conceptually similar to folders on a hard drive in that folders can contain other folders as well as documents.

[Access controls](BasicConcepts#AccessControlEntries) can be set on a folder to allow different groups of people to read, write, and delete those documents.

# Status #
| **Operation**                         | **Status** |
|:--------------------------------------|:-----------|
| [Retrieving a folder](#retrieving-a-folder) | Live |
| [Retrieving a root folder](#retrieving-a-root-folder) | Live |
| [Retrieving a folder using paging](#retrieving-a-folder-using-paging) | Live |
| [Folder locking](#folder-locking) | Beta |
| [Creating a document](#creating-a-document) | Live |
| [Deleting a folder](#deleting-a-folder) | Live |
| [Restoring a folder](#restoring-a-folder) | Live |
| [Updating folder title and description](#updating-folder-title-and-description) | Live |
| [Creating a subfolder](#creating-a-subfolder) | Live |
| [Moving a folder](#moving-a-folder) | Live |
| [Sharing a folder](#sharing-a-folder) | Live |
| [Copying a folder](#copying-a-folder) | Live |
| [Uploading an archive](#uploading-an-archive) | Live |
| [Exporting a folder](#exporting-a-folder)| Beta |

# Operations #

## Retrieving a folder ##

The top level folder in any workspace is returned as part of a [Workspace](Workspace). You can navigate the folder hierarchy by following the [links](Link) returned in the folder structure.

An `If-Modified-Since` header can be specified in the Request header to indicate that only if there are changes to the Folder resource since the specified time should they be sent.

Additionally, `Last-Modified` is included in the response to indicate the last time the Folder resource was generated.

The root folder element contains a displayName attribute, as well as a title. In most cases, these will be the same; however, if the folder being retrieved is the document library (i.e. root) folder in the workspace, the displayName attribute's value will match the name of the workspace, which is generally more appropriate than "documentlibrary" for display purposes. Please note that this is a read-only property, and including a displayName attribute in any PUT or POST requests will have no effect.

### Example ###

#### Request ####
```http
GET /files/folders/12345 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
```

#### Response ####

This response uses the standard error codes and returns standard response headers.

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Last-Modified: Tue, 1 Feb 2011 13:18:42 GMT
```
```xml
<folder xmlns="https://schema.huddle.net/2011/02/" itemType="folder" title="My folder" displayName="My folder" description="Folder description">
  <link rel="self" href="files/folders/12345" />
  <link rel="collection" href="files/pagedfolders/12345" />
  <link rel="parent" href="..." />
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
  <link rel="alternate" href="..." type="text/html" />  
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
  <workspace title="My workspace">
    <link rel="self" href="..." />
	<link rel="alternate" href="..." type="text/html" />
  </workspace>
</folder>
```

### Optional projections ###

When retrieving a Folder, a `projection` query string can be added to the request URL in order to retrieve additional information about the Folder. Currently supported are `permissions`, `ancestry` and `subfolderChildCount`, which are described below.

#### Permissions summary ####

You can retrieve a summary of the folder's permissions by adding the `permissions` projection option as demonstrated below.

```http
GET /files/folders/12345?projection=permissions HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
```

In addition to the information above, the `<folder>` element will also contain a `<permissionsSummary>` element like so:

```xml
<folder xmlns="https://schema.huddle.net/2011/02/" itemType="folder" title="My folder" displayName="My folder" description="Folder description">
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
            <permission type="Restricted" />
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

The information returned includes a list of the teams which have permissions of some kind on the folder and what permissions these are. They do not include a list of members, with the exception of the **Workspace Managers** team. The permissionsSummary element also includes an attribute to indicate whether the folder is restricted to a subset of groups in the workspace or available to any group.

A `self` link is also included as a link to the full [Permissions](Permissions) entity for the folder, which _does_ include the member list for each team.

#### Ancestry ####

You can retrieve the folder's ancestry by adding the `ancestry` projection option as demonstrated below.

```http
GET /files/folders/12345?projection=ancestry HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
```

In addition to the information above, the `<folder>` element will also contain a `<ancestry>` element like so:

```xml
<folder xmlns="https://schema.huddle.net/2011/02/" itemType="folder" title="My folder" displayName="My folder" description="Folder description">
 ... other folder elements ...
 <ancestry>
    <folder title="Parent Folder">
      <link rel="self" href="..." />
      <link rel="parent" href="..." />
    </folder>
    <folder title="Grandparent Folder">
      <link rel="self" href="..." />
      <link rel="parent" href="..." />
    </folder>
  </ancestry>
</folder>
```

The information returned includes a list of the folders directly above this one in the folder structure, up until the document library. This is an ascending-ordered list, so the first parent will be first, then the grandparent, then the great-grandparent, and so on.

The limit of the amount of ancestors returned for a given folder is 52, so in order to get the full ancestry when there are more than 52 ancestors, one would need to make a request for the ancestry of the 52nd parent, and so on until the root is reached. The root will be identified by having no parent link.

Each folder includes the title (as an attribute) and a link to itself and its parent.

Finally, the root `<ancestry>` element includes a UNIX-style path from the root folder to the current folder.

#### Subfolder child count ####

You can retrieve the subfolders' child count (both folders and files) by adding the `subfolderChildCount` projection as demonstrated below.

```http
GET /files/folders/12345?projection=subfolderChildCount HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
```

In addition to the information above, the `<folder>` elements will also contain `folderCount` and `documentCount` attributes like so:

```xml
<folder itemType="folder" title="My folder" displayName="My folder" description="Folder description">
  ... other folder elements ...
  <folders>
    <folder title="sub folder" description="my subfolder" folderCount="5" documentCount="8">
        <link rel="self" href="..." />
        <link rel="collection" href="files/pagedfolders/12345" />
        ... other folder elements ...
    </folder>
  </folders>
```

The information returned only counts the **immediate** children of the folders and not their subsequent content. If a folder has subfolders which contain documents and folders these will not be included in the tally.


---

## Retrieving a root folder ##

This resource allows a user to retrieve the root folder of a workspace. The response is the same as when you issue a GET for a folder ([see above](#retrieving-a-folder)). The [optional projections](#optional-projections) described above for retrieving a folder are also valid for retrieving a root folder.

### Example ###

In the example below, we get the root folder for workspace 123.

#### Request ####
```http
GET /files/workspaces/123/folders/root HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Last-Modified: Tue, 1 Feb 2011 13:18:42 GMT
```
```xml
<folder>
  <link rel="self" href="files/folders/456" />
  <link rel="collection" href="files/pagedfolders/12345" />
  <link rel="workspace-summary" href="workspaces/123" />
  <link rel="publications" href="/publishing/workspaces/123/" />
  ...other folder elements...
</folder>
```

Please note that the above example, issuing a get request to "/files/workspaces/123/folders/root" results in the same response as issuing a get request to "files/folders/456".


---

## Retrieving a folder using paging ##
For folders with a significant number of children the unbounded nature of the Folder resource means that the server may take considerable time to return a response and the size of that response will be large.

If you are dealing with large folders we do not recommend retrieving the folder using the unbounded [Folder](Folder) resource, but instead use a version of that resource that allows you to page through the child items of that folder.

For more information on using a paged approach to retrieving a folder (which is encouraged for large folders) please see [PagedFolder](PagedFolder)

---

## Folder Locking ##

Operations on a  [Folder](Folder) or [PagedFolder](PagedFolder) may result in the resource being locked. Clients cannot initiate a lock, instead Huddle chooses when to lock the resource, to prevent conflicting updates.

When a client issues a GET request to a  [Folder](Folder) or [PagedFolder](PagedFolder) when a lock is present it will be shown in the response body.

The lock is taken for the duration of the operation, and then automatically released afterwards. In many cases you will never need to be aware that a lock exists, as it will only be held for such a short period of time that there is no collision.

If you try to perform an operation on a locked  [Folder](Folder) or [PagedFolder](PagedFolder) then we will return a [423 Locked](https://tools.ietf.org/html/rfc4918) (WebDAV) and your request will fail.

See [Folder Lock](FolderLock) for more details.

It is easier to explicitly list those operations that do **not** result in a Lock and note that all other operations do lock, than to list those that do.

The following operations do **not** lock a Folder:

* Retrieving a Folder (paged, or non-paged)
* Retrieving a root Folder
* Creating a document
* Uploading binary content
* Sharing a folder

---

## Creating a document ##

This resource supports creating a new [Document](Document). If the authenticated
user has permission to create a new document within a folder, the folder resource
will contain a [Link](Link) with a @rel value of _create-document_.

Uploading files via the Huddle API is a two-step process.
  1. To create a new document, issue a POST request to the [create-document URI](#link-relations) with an empty [Document](Document).
  2. Once a document has been created, you may upload content to its upload URI.

The initial POST request must include the the document content's mime type and the
size in bytes in the `X-Upload-Content-Type` and `X-Upload-Content-Length`, respectively.

For more information on uploading files, see [UploadingDocuments](UploadingDocuments).

### Request ###

#### Example ####
```http
POST /files/folders/12345/documents HTTP/1.1
Host: api.huddle.net
Authorization: OAuth2 fooglybooglynooglybeep
Content-Type: application/vnd.huddle.data+xml
X-Upload-Content-Type: application/msword
X-Upload-Content-Length: 54126
```
```xml
<document title="TPS report May 2010" description="relentlessly mundane and enervating." extension=".doc" />
```

| **Name**     | **Description**                 |
|:-------------|:--------------------------------|
| @title       | The title of the document       |
| @description | The description of the document |
| @extension   | The extension of the document   |

### Response - successful ###

If successful, this operation will return a [201 Created](HttpStatusCodes) with a [Location header](ResponseHeaders) pointing to your new [document](Document), and a representation of your new document in the body.

The new document will advertise an [upload uri](Document#Link_relations) to which you can upload content. For more information see [UploadingDocuments](UploadingDocuments).

### Response - Other ###

|Status Code|Reason|
|:----------|:-----|
|401|Invalid authorization token|
|403|No permissions to create documents in this folder|
|404|Folder does not exist|
|409|Conflict, if @extension is supplied and a document with the same title and the same extension already exists within the folder.  The conflict response contains a reason and a document with a _self_ and a _create-version_ [Link](Link). |
|422|Unprocessable Entity, if @extension is not supplied and more than one document with the same title already exist within the folder. |

#### Success Example ####
```http
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
Location: http://api.huddle.net/documents/83847
```
```xml
<document xmlns="https://schema.huddle.net/2011/02/"
          title="TPS report May 2010" description="relentlessly mundane and enervating.">   
  <link rel="self" href="..." />
      ... other document elements ...
</document>
```

#### Conflict Example ####
```http
HTTP/1.1 409 Conflict
Content-Type: application/vnd.huddle.data+xml
Location: http://api.huddle.net/documents/83847
```
```xml
<ErrorResult>
	<StatusCode>409</StatusCode>
	<ErrorMessages>
		<Message>The folder already contains a file with this title</Message>
	</ErrorMessages>
	<ErrorCode>Conflict</ErrorCode>
	<Links>   
		<link rel="self" href="..." />
		<link rel="create-version" href="..." />
	</Links>
</ErrorResult>
```

##### Creating a document from a template
It is also possible to create a document from a system template by adding the
name of the template as an URI parameter. This will create the document with the
content immediately and does not require uploading any binary content. Thus the
`X-Upload-Content-Type` and `X-Upload-Content-Length` headers are not required,
and no upload link will be returned.
The expected document POST request body is the same as for the regular create
document endpoint, but the extension will be ignored, if provided.

This API is mainly used by [Integrations](Integrations).

**Supported templates**

| **Name**       | **Description**                        |
|:---------------|:---------------------------------------|
| `empty-docx`   | An empty Microsoft Word document       |
| `empty-pptx`   | An empty Microsoft Powerpoint document |
| `empty-xlsx`   | An empty Microsoft Excel document      |


**Example**
```http
POST /files/folders/12345/documents?template=empty-docx HTTP/1.1
Host: api.huddle.net
Authorization: OAuth2 fooglybooglynooglybeep
Content-Type: application/vnd.huddle.data+xml
```
```xml
<document title="TPS report May 2010" description="relentlessly mundane and enervating." />
```

---

## Deleting a folder ##

This resource supports deleting a folder. If the authenticated user has permission to delete a folder, the folder resource will contain a [Link](Link) with a @rel value of _delete_. To delete a folder, send a DELETE request to the [delete URI](#link-relations).

### Request ###

#### Example ####
```http
DELETE /files/folders/123432 HTTP/1.1
Host: api.huddle.net
Authorization: OAuth2 howsaboutacookie
```

### Response ###

If successful, this method will return an empty response with an OK status code and a [link header](Link#Header) to the parent folder.

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 200 OK
Link: </folders/123>;rel="parent"
```

### Response - Other ###

|Status Code|Reason|
|:----------|:-----|
|401|Invalid authorization token|
|403|No permissions to create documents in this folder|
|404|Folder does not exist|

---

## Restoring a folder ##

This resource supports restoring a folder. If the authenticated user can restore a folder, /restore can be appended to the folder resource URI to _restore_ URI. To restore the folder, issue a PUT request to the _restore_ URI.

All documents that were deleted with the folder are also restored.

### Example ###

#### Request ####
```http
PUT /files/folders/123/restore HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If successful, this operation will return a 204 No content, with a [link header](Link#Header) giving the location of the restored folder.

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 204 No Content
Location: /files/folders/123
```


---

## Updating folder title and description ##

If the authenticated user is authorized to edit a folder, it will advertise a link with a @rel value of _edit_. To update folder metadata submit a PUT request to this URI with the editable fields of the folder. For an overview of editing resource in Huddle see [editing resources](Resources#editing-resources).

### Example ###

In this example we update the folder's title and description using the URI advertised in the folder resource.

#### Request ####

```http
PUT /files/folders/123/edit HTTP/1.1
Host: api.huddle.net
Content-Type: application/vnd.huddle.data+xml
Content-Length: 102
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```xml
<folder xmlns="http://schema.huddle.net/2011/02/" itemType="folder" title="new title" description="new description" >
	<link rel="parent" href="..." />
</folder>
```

If a folder with the same title already exists within the folder, you will receive a [409 Conflict](HttpStatusCodes).

#### Response ####

If successful, this operation will return a 204 No content, with a [link header](Link#Header) giving the location of the updated folder.

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 204 No Content
Link </folders/123>;rel="parent"
```


---

## Creating a subfolder ##

This resource supports creating a new sub folder. To create a new sub-folder, POST an [empty folder](#Syntax) structure to the containing folder's [create-folder URI](#link-relations). If the sub folder name supplied is already in the parent folder a number will be appended to the end of the folder name e.g. NewFolder(1)

### Request ###

#### Example ####

```http
POST /files/folders/12345 HTTP/1.1
Host: api.huddle.net
Authorization: OAuth2 fooglybooglynooglybeep
Content-Type: application/vnd.huddle.data+xml
```
```xml
<folder title="My new folder" description="This folder will one day contain awesome things" />
```

### Response ###

If successful, this response will return a [folder](Folder#Syntax).

If a folder with the same title already exists within the folder, you will receive a [409 Conflict](HttpStatusCodes).

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

#### Example ####
```http
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
Location: https://api.huddle.net/folders/988665
```
```xml
<folder xmlns="https://schema.huddle.net/2011/02/" title="My new folder" displayName="My new folder" description="This folder will one day contain awesome things">
  <link rel="self" href="/files/folders/988665" />
  <link rel="collection" href="files/pagedfolders/12345"
  ... other folder elements ...
</folder>
```


---

## Moving a folder ##

This resource supports moving a folder to a new location. If the authenticated user can move a folder, the folder resource will advertise a link with a @rel value of _move_. Note that the @href value of the move link is the same as the value for the edit link, since moving an item is in fact an edit operation. To move a folder, PUT a new link to this _move_ uri, with a @rel value of parent, and an @href value pointing to the new parent folder. For an overview of editing resource in Huddle see [editing resources](Resources#editing-resources).

### Example ###

In this example, the user wishes to move a folder which has a _self_ uri of `http://api.huddle.net/folders/`**1**, and a _parent_ uri of `http://api.huddle.net/folders/`**2**.

The user wishes to move their folder to a new parent with a _self_ uri of `http://api.huddle.net/folders/`**3**. First, they retrieve the _move_ uri of the folder they wish to move.

#### Request ####
```http
GET /files/folders/1/edit HTTP/1.1
Accept: application/vnd.huddle.data+xml
```

#### Response ####

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<folder title="My folder" displayName="My folder" description="">
  <link rel="parent" href="/files/folders/2" />
</folder>
```

The user then updates the parent link in the editable folder resource, setting the @href to the URI of the new parent folder, and PUTs it back to the server.

#### Request ####
```http
PUT /files/folders/1/edit HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
```
```xml
<folder title="my folder" description="">
  <link rel="parent" href="/files/folders/3" />
</folder>
```


#### Response ####

The server responds with a 204 No content, and provides a link header with a @rel value of _parent_ pointing back to the updated folder.

```http
HTTP/1.1 204 No Content
Content-Type: application/vnd.huddle.data+xml
Link: </folders/1>;rel="parent"
```


---

## Sharing a folder ##

It is possible to share the folder resource via the share link.

```xml
<link rel="share" href="..." />
```

See [Share](Share).


---

## Copying a folder ##

The copy folder resource supports making a copy of a folder. This operation also copies the folder's sub-folders and documents.

In order to copy a folder, the user must have write access to both the source folder and target folder. If any of the folder's sub-folders or documents cannot be copied, they will be silently skipped. The remaining sub-folders and documents will still be copied.

The folder copy will have the same title and description as the original. If a folder with the same title already exists in the target folder, then the copied folder will be re-titled. For example, a copy of the folder "My Folder" would be re-titled "My Folder (1)".

To copy a folder, first [retrieve the folder](Folder#retrieving-a-folder) to get its _copy_ link, then POST to the folder's copy URI, including a link to the target folder in the body of the request (see Example below). Folders can be copied to the same folder, to another folder in the same workspace, or to a folder in another workspace.

If the copy operation is successful, the copy folder endpoint will return a 201 Created response, along with the URI of the new folder copy in the Location header.

It should be noted that this endpoint copies the top-level folder synchronously, but the folder's sub-folders and documents are copied asynchronously.

### Example ###

In this example, we will copy folder 123 to folder 456.

#### Step 1. Retrieve the folder to get its copy link ####

##### Request #####
```http
GET .../files/folders/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

##### Response #####
```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<folder>
  <link rel="self" href=".../files/folders/123" />
  <link rel="copy" href=".../files/folders/123/copy" />
  ... other folder elements ...
</folder>
```

#### Step 2. Post to the folder's copy URI ####

##### Request #####
```http
POST .../folders/123/copy HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```xml
<copyFolder>
  <targetFolder>
    <link rel="self" href=".../files/folders/456" />
  </targetFolder>
</copyFolder>
```

##### Response #####
```http
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
Location: .../files/folders/789
```

### Error Responses ###

|Case|Response|
|:---|:-------|
|Folder does not exist|404 Not Found|
|Folder is deleted|410 Gone|
|Target folder not provided|400 Bad Request|
|Target folder does not exist|404 Not Found|
|Target folder is deleted|410 Gone|
|User does not have permission to copy folders from the source folder|403 Forbidden|
|User does not have permission to create folders in the target folder|403 Forbidden|



---

## Uploading an Archive ##

You can create entire directory structures containing folders and documents by
uploading a Zip archive containing the desired structure. If the authenticated
user has permission to both create documents and subfolders within a folder,
the folder resource will contain a [Link](Link) with a @rel value of _upload-archive_.

Uploading archives is analogous to [creating documents](#creating-a-document):
  1. Issue a POST request to the [upload-archive URI](#link-relations) with
     metadata about the archive.
  2. Once the upload has been created, you may upload content to its upload URI.

The initial POST request must include the the document content's mime type and the
size in bytes in the `X-Upload-Content-Type` and `X-Upload-Content-Length`, respectively.

For more information on uploading files, see [UploadingDocuments](UploadingDocuments).


Once the upload has been completed, the server will extract the archive and
create the folders and/or documents in the background. You can immediately start
browsing the newly created structure as the server is processing the archive,
and new content will appear until the extraction is completed.

If any conflicts occur during extraction, the conflicting files will be renamed
by appending ` (n)` to the title, where `n` is the number of existing duplicates.
There are no guarantees on the order of extraction, and as such no guarantees on
which of the conflicting files will be renamed.

Once the extraction has been completed, the authenticated user will receive
and email and a dashboard notification with the results of the extraction.


### Limitations: ###
* Only Zip archives are supported.
* Password protected archives are not supported.
* The maximum depth of the directory structure is 20. Any items deeper than 20 levels will be skipped.
* Names of files and folders in an archive are treated as case-insensitive and extracted case-preserving.


### Example ###

#### Request ####
```http
POST /files/folders/12345/archives HTTP/1.1
Host: api.huddle.net
Authorization: OAuth2 fooglybooglynooglybeep
Content-Type: application/vnd.huddle.data+xml
X-Upload-Content-Type: application/zip
X-Upload-Content-Length: 54126
```
```xml
<archive title="My massive structure" description="" extension=".zip" />
```
```json
{
  "title": "My massive structure",
  "description": "",
  "extension": ".zip"
}
```

#### Response ####
```http
HTTP/1.1 202 Accepted
Content-Type: application/vnd.huddle.data+xml
Location: http://api.huddle.net/folders/12345
```
```xml
<archive xmlns="https://schema.huddle.net/2011/02/"
          title="My massive structure" description="">
  <link rel="upload" href="..." />
</archive>
```
```json
{
  "links": [ {
    "rel": "upload",
    "href": "..."
  } ],
  "title": "My massive structure",
  "extension": ".zip"
}
```

---

## Exporting a folder ##
You can export the entire directory structure of a folder, its subfolders and documents
as a Zip archive that can be downloaded. If the authenticated
user has permission, the folder resource will contain a [Link](Link) with a @rel value of _export_.
The body of the POST allows specifying if subfolders, comments for each document, and if an index CSV should be created. An index is a CSV document that contains a list of all documents, their metadata, and the path to their content within the zip archive.

### Example ###

#### Request ####
```http
POST /files/folders/12345/export HTTP/1.1
Host: api.huddle.net
Authorization: OAuth2 fooglybooglynooglybeep
Content-Type: application/vnd.huddle.data+xml
```
```xml
<export subfolders="true" comments="true" index="true" />
```
```json
{
  "subfolders": true,
  "comments": true,
  "index": true
}
```

#### Response ####
```http
HTTP/1.1 202 Accepted
Content-Type: application/vnd.huddle.data+xml
Link: <.../files/exports/7654abcd-ffgg-1234-xy89>;rel="progress"
```

#### Response ####
|Status Code|Reason|Code|
|:----------|:-----|:---|
|202|The export process request has been accepted||
|401|Invalid authorization token||
|403|Not a Workspace Manager||
|404|Folder does not exist||
|400|Folder is empty|FolderIsEmpty|
|400|Folder content is too large (> 4GB)|ContentTooLarge|
|503|Service Unavailable, we can't process your request||

### Getting the progress of an export operation ###

Authorized API clients can use the link returned by the POST to get the progress of the export operation. The response contains the progress information for the export operation.

#### Request ####
```http
GET .../files/exports/7654abcd-ffgg-1234-xy89
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response (in progress) ####

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<exportFolderProgress status="InProgress">
  <link rel="self" href=".../files/exports/7654abcd-ffgg-1234-xy89" />
  <link rel="parent-folder" href="..." title="Folder Title" />
</exportFolderProgress>
```
```json
{
  "links": [ {
    "rel": "self",
    "href": "..."
  }, {
    "rel": "parent-folder",
    "href": "...",
    "title": "Folder Title"
  }],
  "status": "InProgress"
}
```

#### Response (process complete) ####
A completed export will return a [Link](Link) with a @rel value of _content_. Following this link will download the Zip.

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<exportFolderProgress status="Complete">
  <link rel="self" href="..." />
  <link rel="content" href="..." />
  <link rel="parent-folder" href="..." title="Folder Title" />
</exportFolderProgress >
```
```json
{
  "links": [{
    "rel": "self",
    "href": "..."
  }, {
    "rel": "content",
    "href": "..."
  }, {
    "rel": "parent-folder",
    "href": "...",
    "title": "Folder Title"
  }],
  "status": "Complete"
}
```

#### Response (process failed) ####
A failed export will return a list of failed documents and a reason code why they couldn't be exported.

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<exportFolderProgress status="Failed">
  <link rel="self" href=".../files/export/7654abcd-ffgg-1234-xy89" />
  <link rel="parent-folder" href="..." title="Folder Title" />
  <failedItems>
      <failedItem type="document" reason="NotFound" />
      <failedItem type="folder" reason="Deleted" />
  </failedItems>
</exportFolderProgress>
```
```json
{
  "links": [{
    "rel": "self",
    "href": "..."
  },{
    "rel": "parent-folder",
    "href": "...",
    "title": "Folder Title"
  }],
  "status": "Failed",
  "failedItems": [{
    "type": "document",
    "reason": "NotFound"
  }, {
    "type": "folder",
    "reason": "Deleted "
  }]
}
```

#### Response ####
|Status Code|Reason|
|:----------|:-----|
|200|OK, shows export status|
|401|Invalid authorization token|
|403|Not a Workspace Manager|
|404|Progress status was not found|
|503|Unavailable response, with Retry-After if there was a temporary problem retrieving the bulk export progress specified by the URI.|

### Cancelling an export operation ###
An export operation can be cancelled by issuing a DELETE request to the progress endpoint.

#### Request ####
```http
DELETE .../files/export/7654abcd-ffgg-1234-xy89
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<exportFolderProgress status="Cancelled">
  <link rel="self" href="..." />
  <link rel="parent-folder" href="..." title="Folder Title" />
</exportFolderProgress >
```
```json
{
  "links": [{
    "rel": "self",
    "href": "..."
  },{
    "rel": "parent-folder",
    "href": "...",
    "title": "Folder Title"
  }],
  "status": "Cancelled"
}
```

#### Other response codes ####
|Status Code|Reason|
|:----------|:-----|
|200|OK, Delete request was successful|
|401|Invalid authorization token|
|403|Not a Workspace Manager|
|404|Progress status was not found|


# Syntax #

## Example ##

```xml
<folder xmlns="http://schema.huddle.net/2011/02/" itemType="folder" title="An example folder" displayName="An example folder">
  <link rel="self" href="/files/folders/988665" />
  <link rel="collection" href="files/pagedfolders/12345" />
  <link rel="parent" href="..." />
  <link rel="delete" href="..." />
  <link rel="bulk-delete" href="..." />
  <link rel="bulk-move" href="..." />
  <link rel="edit" href="..." />  
  <link rel="create-folder" href="..." />
  <link rel="create-document" href="..." />
  <link rel="share" href="..." />
  <link rel="copy" href="..." />
  <link rel="alternate" href="..." type="text/html" />
  <link rel="copy" href="..." />

  <actor name="Ian Cooper" email="ian.cooper@example.com" rel="owner">
    <link rel="self" href="..." />
    ... other actor elements ...
  </actor>
  <folders>
    <folder title="Project ABC" description="Top secret">
      <link rel="self" href="..." />
      ... other folder elements ...
    </folder>
  </folders>
  <documents>
    <document title="Felix and Herbert" description="My pets">
      <link rel="self" href="..." />  
      ... other document elements ...
    </document>
  </documents>
  <created>2011-10-10T09:02:17Z</created>
  <updated>2011-10-10T09:03:17Z</updated>
</folder>
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| folders  | A list of folders representing the **immediate child** folders of this folder. There is no provision currently for recursively fetching a folder hierarchy |
| documents | A list of [documents](Document) representing the documents contained within the folder |
| ancestry | The set of folders containing this folder, ordered from nearest ancestor to furthest ancestor |

## Link relations ##

| **Name** | **Description** | **Methods** |
|----------|-----------------|-------------|
| self     | The current URI of this Folder | GET |
| collection            | The paged folder variant of this folder | GET |
| parent                | The folder which contains this folder | GET |
| create-document       | The URI for [creating documents](#creating-a-document) in this folder. | POST |
| create-folder         | The URI for [creating subfolders](#creating-a-subfolder) in this folder. | POST |
| upload-archive        | The URI for [uploading archives](#uploading-an-archive) in this folder. | POST |
| changes               | The URI for [the changes](Changes) made to the folder since its creation. | GET |
| delete                | If the user has permission to delete this folder, they can do so at this URI | DELETE |
| bulk-delete           | Allows bulk deletion of many folders and documents in one request. Note, this link is not owned by a particular folder.  See [Bulk Delete](BulkProcess#bulk-delete) for details. | POST |
| bulk-move             | Allows bulk moves of many folders and documents in one request. Note, this link is not owned by a particular folder.  See [Bulk Move](BulkProcess#bulk-move) for details. | POST |
| edit                  | If the user can update folder details, they can do so at this URI. | PUT |
| move                  | If the user can move a folder, they can do so at this URI. The URI is the same as the edit URI, but using a distinct link to advertise movability better indicates to the client that this operation is available. | PUT |
| permissions           | Returns the permissions currently in effect for this folder. | GET |
| cascading-permissions | Setting folder permissions including all subfolders. See [CascadingPermissions](CascadingPermissions). | POST |
| workspace             | Returns details of the workspace that this folder belongs to. | GET |
| share                 | To share the folder. | POST |
| copy                  | To copy the folder. | POST |
| alternate             | With a type of 'text/html', this is the link to view the folder in the Huddle website. | GET |
| export                | To export the folder. | POST |

## Schema ##

```
start |= folder

folder = element h:folder {
  attribute title {xsd:string},
  attribute description {xsd:string}?,

  (empty | link + | (
    link+,
    actor,
    element h:folders { folder* },
    element h:documents { document* },
    element h:created {xsd:dateTime},
    element h:updated {xsd:dateTime}
  ))
}
```