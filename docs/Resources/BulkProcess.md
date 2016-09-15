# Summary #

The BulkProcess  resource enables operations to be performed against multiple resources in one request.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Bulk Delete    |Beta        |
|Bulk Move      |Beta        |
|Bulk Download  |Beta        |
|Bulk Publish   |Alpha       |

# Operations #

## Bulk Delete ##

This resource supports bulk deleting [documents](Document) and [folders](Folder). To bulk delete, issue a POST request to the URI of a delete link, with an entity body that contains a list of documents and folders. Each document and folder should be referenced with their URI.

Any folders that are passed in will be permanently deleted along with all their subfolders and documents. The caller must therefore have delete permissions for all the content of that folder.

Because bulk deletion may affect many subfolders, it can take some time, so it is treated as an asynchronous process: after submitting the initial request to initiate the bulk delete, clients are expected to poll a progress endpoint to retrieve the current status of the operation.

A successful POST to the delete endpoint will initiate the process and return a 202 Accepted response containing a link to GET the progress of the delete actions.

Subsequently querying that endpoint will return summary information about the documents and folders processed and if they were successfully deleted.

### Initiating a bulk delete operation ###

Bulk delete documents 12345, 54321 and folder 67890.

#### Request ####
```
POST .../files/bulkprocess/delete HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<bulkDelete>
  <filesCollection>
    <documents>
      <link rel="member" href=".../files/documents/12345" />    
      <link rel="member" href=".../files/documents/54321" />   
    </documents>
    <folders>
      <link rel="member" href=".../files/folders/67890" />   
    </folders>
  </filesCollection>
</bulkDelete>
```

#### Response ####
If successful, this method will return a 202 Accepted status code and a URI in the header that can be used in subsequent polling GET requests to check the status of the operation.

```
HTTP/1.1 202 Accepted
Content-Type: application/vnd.huddle.data+xml
Link: <.../files/bulkprocess/delete/7654abcd-ffgg-1234-xy89>;rel="progress"
```

### Getting the progress of a bulk delete operation ###

Authorized API clients can use the link returned by the POST to get the progress of the delete operation. The response contains the progress information for the bulk delete operation. A status flag will indicate if the processing of all items in the bulk delete is complete.

The GET call will return a 503 Service Unavailable response with Retry-After if there was a temporary problem retrieving the bulk delete progress specified by the URI.

The GET call will return a 404 response if there was a problem finding the progress for the bulk delete operation specified by the URI.

#### Request ####
```
GET .../files/bulkprocess/delete/7654abcd-ffgg-1234-xy89
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response (in progress) ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<bulkDeleteProgress status="InProgress">
  <link rel="self" href=".../files/bulkprocess/delete/7654abcd-ffgg-1234-xy89" />
  <documents>
    <documentProgress>
      <link rel="self" href=".../files/documents/12345" />
      <status>InProgress</status>   
    </documentProgress>
    <documentProgress>
      <link rel="self" href=".../files/documents/67890" /> 
      <status>Error</status> 
    </documentProgress>
  </documents>
  <folders>
    <folderProgress>
      <link rel="self" href=".../files/folders/54321" />     
      <status>Complete</status>
    </folderProgress>
  </folders>
  <invalidItems>
    <link rel="invalid" href="http://invalidurl" />
    <link rel="invalid" href="http://invalidurl2" />
  </invalidItems>
</bulkDeleteProgress >
```

#### Response (process complete) ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<bulkDeleteProgress status="Complete">
  <link rel="self" href=".../files/bulkprocess/delete/7654abcd-ffgg-1234-xy89" />
  <documents>
    <documentProgress>
      <link rel="self" href=".../files/documents/12345" />
      <status>Complete</status>   
    </documentProgress>
    <documentProgress>
      <link rel="self" href=".../files/documents/67890" /> 
      <status>Error</status> 
    </documentProgress>
  </documents>
  <folders>
    <folderProgress>
      <link rel="self" href=".../files/folders/54321" />     
      <status>Complete</status>
    </folderProgress>
  </folders>
  <invalidItems>
    <link rel="invalid" href="http://invalidurl" />
    <link rel="invalid" href="http://invalidurl2" />
  </invalidItems>
</bulkDeleteProgress >
```

#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| status (on folderProgress or documentProgress)| Indicates the status of the delete operation on each element. Statuses: InProgress, Complete, Error. All elements start as InProgress. On successful delete status is set to Complete. If an error occurs when deleting the element status is set to Error. |
| status flag (on bulkDeleteProgress)| The status flag for all the items in the bulk delete operation. Will be Complete if all the items have been processed, InProgress otherwise. |
| invalidItems | Items that were passed in the initial POST request that are deemed invalid identifiers and cannot be matched to a folder or document resource. These items have not been processed |

## Bulk Move ##

This resource supports moving multiple [documents](Document) and [folders](Folder) to a single target folder.

To move files in bulk, issue a POST request to the URI of the bulk-move link returned in a [GET Folder](Folder#Retrieve_a_folder) request. The entity body should contain a list of documents and folders to move, as well as the target folder to move to. Each document and folder should be referenced by its URI.

Files and folders will be permanently moved to the target folder, along with all their subfolders and documents. When moving a folder, the caller must have permission to move all of the content of that folder to the target folder.

Moving a large quantity of files and folders can take some time, and because of this it is treated as an asynchronous process. After submitting the initial request to initiate the bulk move, clients can poll a progress endpoint to retrieve the current status of the operation.

A successful POST to the bulk-move endpoint will initiate the process and return a 202 Accepted response containing a link to GET the progress of the move operation.

Subsequently querying that endpoint will return summary information about the documents and folders processed and if they were successfully moved.

### Initiating a bulk move operation ###

Bulk move documents 12345, 54321 and folder 67890 to folder 82453.

#### Request ####
```
POST .../files/bulkprocess/move HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<bulkMove>
  <filesCollection>
    <documents>
      <link rel="member" href=".../files/documents/12345" />    
      <link rel="member" href=".../files/documents/54321" />   
    </documents>
    <folders>
      <link rel="member" href=".../files/folders/67890" />   
    </folders>
  </filesCollection>
  <targetFolder>
    <link rel="self" href=".../files/folders/82453" />  
  </targetFolder>
</bulkMove>
```

#### Response ####
If successful, this method will return a 202 Accepted status code and a URI in the header that can be used in subsequent polling GET requests to check the status of the operation.

```
HTTP/1.1 202 Accepted
Content-Type: application/vnd.huddle.data+xml
Link: <.../files/bulkprocess/move/7654abcd-ffgg-1234-xy89>;rel="progress"
```

### Getting the progress of a bulk move operation ###

Authorized API clients can use the link returned by the POST to get the progress of the move operation. The response contains the progress information for the bulk move operation. A status flag will indicate if the processing of all items in the bulk move is complete.

The GET call will return a 503 Service Unavailable response with Retry-After if there was a temporary problem retrieving the bulk move progress specified by the URI.

The GET call will return a 404 response if there was a problem finding the progress for the bulk move operation specified by the URI.

#### Request ####
```
GET .../files/bulkprocess/move/7654abcd-ffgg-1234-xy89
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response (in progress) ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<bulkMoveProgress status="InProgress">
  <link rel="self" href=".../files/bulkprocess/move/7654abcd-ffgg-1234-xy89" />
  <documents>
    <documentProgress>
      <link rel="self" href=".../files/documents/12345" />
      <status>InProgress</status>   
    </documentProgress>
    <documentProgress>
      <link rel="self" href=".../files/documents/67890" /> 
      <status>Error</status> 
    </documentProgress>
  </documents>
  <folders>
    <folderProgress>
      <link rel="self" href=".../files/folders/54321" />     
      <status>Complete</status>
    </folderProgress>
  </folders>
  <invalidItems>
    <link rel="invalid" href="http://invalidurl" />
    <link rel="invalid" href="http://invalidurl2" />
  </invalidItems>
</bulkMoveProgress >

```

#### Response (process complete) ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<bulkMoveProgress status="Complete">
  <link rel="self" href=".../files/bulkprocess/move/7654abcd-ffgg-1234-xy89" />
  <documents>
    <documentProgress>
      <link rel="self" href=".../files/documents/12345" />
      <status>Complete</status>   
    </documentProgress>
    <documentProgress>
      <link rel="self" href=".../files/documents/67890" /> 
      <status>Error</status> 
    </documentProgress>
  </documents>
  <folders>
    <folderProgress>
      <link rel="self" href=".../files/folders/54321" />     
      <status>Complete</status>
    </folderProgress>
  </folders>
  <invalidItems>
    <link rel="invalid" href="http://invalidurl" />
    <link rel="invalid" href="http://invalidurl2" />
  </invalidItems>
</bulkMoveProgress >

```

#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| status (on folderProgress or documentProgress)| Indicates the status of the move operation on each element. Statuses: InProgress, Complete, Error. All elements start as InProgress. On successful move status is set to Complete. If an error occurs when moving the element status is set to Error. |
| status flag (on bulkMoveProgress)| The status flag for all the items in the bulk move operation. Will be Complete if all the items have been processed, InProgress otherwise. |
| invalidItems | Items that were passed in the initial POST request that are deemed invalid identifiers and cannot be matched to a folder or document resource. These items have not been processed |


## Bulk Download ##

This resource supports downloading multiple [documents](Document) from a single source [folder](Folder) as a ZIP file. You can either download certain documents from the folder, or the whole folder.

Since downloaded content is already delivered in compressed form, zipping will not have a performance advantage over downloading the documents individually. This API is intended as a convenient way to download multiple files.

To download files in bulk, issue a POST request to the URI of the bulk-download link returned in a [GET Folder](Folder#Retrieve_a_folder) request. The body of the request must contain a source folder for the download. It may additionally include a list of specific documents to zip and download. These elements must be specified by their URIs.

The list of documents must not contain documents from folders other than the specified source folder; nor may it contain sub-folders. Doing so will result in a 404 Not Found or a 400 Bad Request response, respectively.

If the list of documents is omitted, the request is assumed to refer to the whole folder; all documents from the top level of the folder (not subfolders) will be zipped.

Zipping a large quantity of data to be downloaded can take some time, and because of this it is treated as an asynchronous process. A successful POST to the bulk-download endpoint will initiate the asynchronous process and return a 202 Accepted response containing a URI for the progress of the zipping operation.

Clients can issue GET requests to the progress resource that was returned with the 202. The response will contain summary information about the progress of the zipping operation. Once the process is completed, the progress end-point will respond with a link to download the completed ZIP file.

### Initiating a bulk download operation ###

Issue a POST request to the bulk download endpoint that was returned from the [GET Folder](Folder#Retrieve_a_folder) request, with a body indicating the files you wish to zip and download.

#### Request (whole folder) ####

Generate a zip file containing every document at the top-level of folder 6738.

```
POST .../files/bulkprocess/download HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<bulkDownload>
  <sourceFolder>
    <link rel="self" href=".../files/folders/6738" />
  </sourceFolder>
</bulkDownload>
```

#### Request (specific files) ####

Generate a zip file containing only documents 3512 and 4955, from folder 6738.

```
POST .../files/bulkprocess/download HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<bulkDownload>
  <sourceFolder>
    <link rel="self" href=".../files/folders/6738" />
  </sourceFolder>
  <documents>
    <link rel="member" href=".../files/documents/3512"/>
    <link rel="member" href=".../files/documents/4955"/>
  </documents>
</bulkDownload>
```

#### Response ####
If successful, this method will return a 202 Accepted status code, and a URI in the header that can be used in subsequent polling GET requests to check the status of the operation.

```
HTTP/1.1 202 Accepted
Content-Type: application/vnd.huddle.data+xml
Link: <.../files/bulkprocess/download/7654abcd-ffgg-1234-xy89>;rel="progress"
```

### Getting the progress of a bulk download operation ###

Clients can issue GET requests to the URI in the 202 response to query the progress of the zipping operation. A status flag will indicate if the processing of the bulk download request is complete.

The GET call will return a 503 Service Unavailable response with Retry-After if there was a temporary problem retrieving the progress of the zipping operation specified by the URI.

The GET call will return a 404 response if there was a problem finding the progress for the zipping operation specified by the URI.

#### Request ####
```
GET .../files/bulkprocess/download/7654abcd-ffgg-1234-xy89
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response (in progress) ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<bulkDownloadProgress status="InProgress">
  <link rel="self" href=".../files/bulkprocess/download/7654abcd-ffgg-1234-xy89" />
  <documents>
    <documentProgress>
      <link rel="self" href=".../files/documents/3512" />
      <status>InProgress</status>   
    </documentProgress>
    <documentProgress>
      <link rel="self" href=".../files/documents/4955" /> 
      <status>Error</status> 
    </documentProgress>
  </documents>
  <invalidItems>
    <link rel="invalid" href="http://invalidurl" />
  </invalidItems>
</bulkDownloadProgress>
```

#### Response (process complete) ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<bulkDownloadProgress status="Complete">
  <link rel="self" href=".../files/bulkprocess/download/7654abcd-ffgg-1234-xy89" />
  <link rel="content" href=".../files/bulkprocess/download/7654abcd-ffgg-1234-xy89/content" />
  <documents>
    <documentProgress>
      <link rel="self" href=".../files/documents/3512" />
      <status>Complete</status>   
    </documentProgress>
    <documentProgress>
      <link rel="self" href=".../files/documents/4955" /> 
      <status>Error</status> 
    </documentProgress>
  </documents>
  <invalidItems>
    <link rel="invalid" href="http://invalidurl" />
    <link rel="invalid" href="http://invalidurl2" />
  </invalidItems>
</bulkDownloadProgress>

```

#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| status (on documentProgress)| Indicates the status of the zipping operation on each element. Statuses: InProgress, Complete, Error. All elements start as InProgress. On successful zipping the status is set to Complete. If an error occurs when zipping the element, the status is set to Error. |
| status flag (on bulkDownloadProgress)| The status flag for all the items in the zipping operation. Will be Complete if all the items have been processed, InProgress otherwise. |
| invalidItems | Items that were passed in the initial POST request that are deemed invalid identifiers and cannot be matched to a folder or document resource. These items will not be processed. |

### Getting the zipped content of a bulk download operation ###
Once the requested files have been zipped, the progress endpoint will return a 'content' link (see above). In order to retrieve your zip file, issue a GET request to this endpoint. Accept headers other than 'application/zip' are not supported at the content endpoint.

## Bulk Publish ##

Clients may [publish](PublishDocument) several documents at the same time. To bulk publish, issue a POST request to the bulk publish uri, with an entity body that contains a link to the folder containing your documents.

If the caller provides a folder link, all documents in that folder will be published. The caller must therefore have publish permissions for all the content of that folder. Folders may be published recursively, in which case all sub folders will also be published recursively.

Because bulk publishing may affect many documents, it can take some time, so it is treated as an asynchronous process. A successful POST to the publish endpoint will initiate the process and return a 202 Accepted response.

For the alpha release of this API, it is an error to provide more than one folder. Document URIs will be ignored. The beta release of this endpoint will support multiple folders and individual documents.

### Initiating a bulk publish operation ###

Bulk publish folder 67890.

#### Request ####

```
POST .../files/bulkprocess/publish HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<publish>
  <filesCollection>
    <folders recurse="true">
      <link rel="member" href=".../files/folders/67890" />   
    </folders>
  </filesCollection>
</publish>
```

#### Response ####

If successful, this method will return a 202 Accepted status code.

```
HTTP/1.1 202 Accepted
```
