# Summary #

Uploading documents via the Huddle API is a two-part process.

  1. Create a [new document](Folder#Create_a_file), or a new [document version](Document#create_a_new_version)
  1. Upload binary content to the [upload uri](Document#Link_relations) advertised by your newly created document resource.

# Upload binary content #

Once you have created either a [new document](Folder#Create_a_file), or a new [document version](Document#create_a_new_version), you can upload binary content.

If the document is locked by someone else, then this operation will fail with a 409 Conflict.

## Finding the upload URI ##

The response to a successful create version, or create document request will contain a [Link](Link) with a @rel value of _upload_.

Parse the body to find this link and extract the @href property. To upload data,
construct a `multipart/form-data` request and POST it to the upload uri.

## Resumable uploads ##
In order to make uploads more resilient, binary content can be uploaded in arbitrary,
sequential byte ranges in individual requests. That way if one request fails, only
that range needs to be uploaded again, rather than failing the upload and having
to start again from scratch.

The range of the current chunk is specified in the `Content-Range` header with the format
`bytes {start}-{end}/{total}`. For example:

```
Content-Range: 50-100/500
```

On successful processing of the chunk, the server will respond with 202 Accepted
containing the uploaded range:

```
HTTP/1.1 202 Accepted
Content-Length: 0
Range: 50-100
```

The chunks must be uploaded sequentially, i.e. the range of the fist chunk must
always start at 0 and subsequent chunks must start where the previous chunk ended.
An upload is considered complete when the whole content size has been received.

Since the chunk size is arbitrary, it is possible to upload the whole content
in one request simply by specifying the whole range, e.g. `Content-Range: 0-500/500`.
It is also possible to vary the chunk size during an upload, for example to account
for varying connection quality.

## Constructing the request ##

Huddle currently use `multipart/form-data` to encode uploaded documents. This allows us to include [mime-type](MediaType) and filename metadata inline with the binary content.

The `multipart/form-data` [media type](MediaType) is covered by [rfc2388](http://www.rfc-editor.org/rfc/rfc2388.txt).

If you are working in a browser-environment, you may wish to create an HTML form to perform the upload. Most languages will have some library support for Multipart form data but this article assumes you are writing your own.


## Constructing a request programmatically ##

A multipart request contains several "chunks" of data, separate by a boundary. To create a multipart request, first create a boundary-string that is unlikely to appear in the file content. This boundary string does not have to be unique, but it must not appear elsewhere in the request body.

#### Example ####

These examples are illustrative only, and do not represent best, or even sensible, practice.

```
string createBoundaryString() {
  return "my_upload_client_boundary" + Guid.NewGuid();
}
```

Next, use this boundary string to set your content-type header

#### Example ####
```
string MakeContentTypeHeader(string boundary) {
  return "Content-Type: multipart/form-data; boundary=" + boundary;
}
```

You can now use your boundary string to construct the request.
A successful request must have the following form. Note the leading dashes on the boundary, and the trailing dashes on the closing boundary. We've found that constructing an HTML form is an excellent way of exploring the multipart request format when [debugging uploads](BasicConcepts#Debugging).


#### Example ####
```
POST /uploads/abc HTTP/1.1
Host: api.huddle.net
Content-Type: multipart/form-data; boundary=[boundary-string]
Content-Length: 288
Content-Range: 0-123/1000

--[boundary-string]--
Content-Disposition: form-data; name="content"; filename="tps-report-may2010.doc"
Content-Type: application/msword

[BINARY CONTENT GOES HERE]
--[boundary-string]--
```

## Using HTML Forms ##
HTML forms natively use `multipart/form-data` when uploading files. This makes them ideal for integrating with the Huddle API.
to upload via an HTML form, create an `<input type="file" />` element with the name _content_. Your form must have a @method of `POST` and an @enctype of `multipart/form-data`


## File Names ##
When you create a document, see [Create a document](Folder#Create_a_document), create a new version, see [document Creating a new version of a document](Document#Creating_a_new_version_of_a), or update a document's title, see [Updating document title and description](Document#Updating_document_title_and_description) - you provide us with a title that we will use as the base filename, in the Content-Disposition header when you download the document, see [Downloading Document Content](Document#Downloading_document_content).

Note that this is assumed to be the base filename and does not include the extension part of the filename.

When you upload a document, see [Uploading Documents](UploadingDocuments) you provide us with a Content-Disposition from which we read the filename extension, which we will use as the extension in the document's filename in the Content-Disposition header when you download the document.

We read the mime type from the Content-Type that you provide in the Content-Type when uploading the document, and provide that to you in the Content-Type header when you download the document.

So for example if you create a document with

#### Example ####
```
POST /folders/12345/documents HTTP/1.1
Host: api.huddle.net
Authorization: OAuth2 fooglybooglynooglybeep
Content-Type: application/vnd.huddle.data+xml
X-Upload-Content-Type: application/msword
X-Upload-Content-Length: 54126
```
```
<document title="My File" description="Important Stuff" />
```

and then upload the content with

#### Example ####
```
POST /uploads/abc HTTP/1.1
Host: api.huddle.net
Content-Type: multipart/form-data; boundary=[boundary-string]
Content-Length: 288
Content-Range: 0-123/54126

--[boundary-string]--
Content-Disposition: form-data; name="content"; filename="Foo.doc"
Content-Type: application/msword

[BINARY CONTENT GOES HERE]
--[boundary-string]--
```

We will take "My File" from the title and ".doc" from the upload Content-Disposition filename and combine in the Content-Disposition when downloading

#### Example ####
```
HTTP/1.1 200 OK
Content-Type: application/msword
Content-Length: 198
Content-Disposition: attachment; filename="My File.doc"

Hello - this is the content of your file.
```

### A Common Error Understanding File Names ###

A common mistake is to supply us the extension as well and the base filename in the Title when creating the document. So it is an likely to have unintended consequences if you create a document as follows:

#### Example ####
```
POST /folders/12345/documents HTTP/1.1
Host: api.huddle.net
Authorization: OAuth2 fooglybooglynooglybeep
Content-Type: application/vnd.huddle.data+xml
X-Upload-Content-Type: application/msword
X-Upload-Content-Length: 54126
```
```
<document title="My File.doc" description="Important Stuff" />
```

and upload the content as follows

#### Example ####
```
POST /uploads/abc HTTP/1.1
Host: api.huddle.net
Content-Type: multipart/form-data; boundary=[boundary-string]
Content-Range: 0-54126/54126

--[boundary-string]--
Content-Disposition: form-data; name="content"; filename="My File.doc"
Content-Type: application/msword

[BINARY CONTENT GOES HERE]
--[boundary-string]--
```

then we will add the extension to the file, but as the base filename already has what appears to be an extension, it will appear 'doubled up' in the Content-Disposition filename

#### Example ####
```
HTTP/1.1 200 OK
Content-Type: application/msword
Content-Length: 198
Content-Disposition: attachment; filename="My File.doc.doc"

Hello - this is the content of your file.
```

## Monitoring Upload Progress ##

An upload can be a long-running operation To retrieve the document upload progress, GET the _upload_ URI.

In cases where the document upload is complete, the resource will include a link to get the uploaded document content.

When no information for the upload progress is available, the following message will be returned: 'Sorry, progress information is unavailable at the moment'. It is likely this is intermittent and subsequent calls will provide progress.

### Example ###

#### Request ####
```
GET /uploads/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<documentUploadProgress xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="/documentuploads/123" />
  <link rel="documentContent" href="/documents/123/content " />

  <totalBytes>987123</totalBytes>
  <bytesWritten>963699</bytesWritten>
  <bytesRemaining>23424</bytesRemaining>
  <estimatedSecondsRemaining>69</estimatedSecondsRemaining>
  <percentageComplete>95</percentageComplete>
  <documentStatus>Processing</documentStatus>
  <message />
</documentUploadProgress>
```

#### Failed Response ####

If a upload is already cancelled and you send a GET request, you will receive a 410 Gone response.

### Syntax ###

#### Examples ####

##### A document upload #####
```
<documentUploadProgress xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="/documentuploads/123" />
  <link rel="documentContent" href="/documents/123/content " />

  <totalBytes>987123</totalBytes>
  <bytesWritten>963699</bytesWritten>
  <bytesRemaining>23424</bytesRemaining>
  <estimatedSecondsRemaining>69</estimatedSecondsRemaining>
  <percentageComplete>95</percentageComplete>
  <documentStatus>Processing</documentStatus>
  <message />
</documentUploadProgress>
```

### Properties ###

| **Name** | **Description** |
|:---------|:----------------|
| totalBytes | Size of the upload in bytes. |
| bytesWritten | Approximate number of bytes already uploaded to Huddle. |
| bytesRemaining | Approximate number of bytes awaiting to be uploaded to Huddle |
| estimatedSecondsRemaining | Estimate of the remaining time to finish the upload. |
| percentageComplete | Estimate of what percentage of the document has been uploaded to Huddle. |
| documentStatus | Complete = the document is not processing | Processing = the document is empty or an upload is in progress | Moving = the document is being moved in another location | Error = a processing error occurred |

### Link relations ###

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this document upload. | GET         |
| documentContent | If the upload is complete then this link will give access to the [Document](Document) content. | GET         |

### Schema ###

```

start = documentUploadProgress

documentUploadProgress = element documentUploadProgress {
	  link+,
          element h:totalBytes  {xsd:unsignedLong},
          element h:bytesWritten  {xsd:unsignedLong},
          element h:bytesRemaining  {xsd:unsignedLong},
          element h:estimatedSecondsRemaining  {xsd:unsignedInt},
          element h:percentageComplete  {xsd:unsignedInt},
          element h:documentStatus {"Complete"|"Error"|"Moving"|"Processing"},

}

```

## Deleting an upload ##

To cancel an upload, perform a HTTP DELETE on the link with @rel value of _upload_ advertised on the [Document](Document) resource. This will delete the document version metadata that was created as part of the first stage of the [UploadingDocuments](UploadingDocuments) process.

### Example ###

#### Request ####
```
DELETE /uploads/123 HTTP/1.1
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

#### Failed Response ####

If a upload is already complete and you send a delete request, you will receive a 409 Conflict response.
