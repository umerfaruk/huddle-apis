# Retrieving Document Content #

|  |
|:-|

## Status ##
| **Operation** | **Status** |
|:--------------|:-----------|
| Getting the raw content of a document | Live       |
| Getting the content of a document in a different format | Draft      |

## Getting the raw content of a document ##
Documents have binary content associated with them. If a document has binary content, it will advertise a link with a `rel` value of `content`. To download the content, GET the URI advertised in the link. The `title` attribute of the link will give the filename of the document, and the `type` attribute will give the mime-type.

To retrieve the original binary content of a document, issue a GET request to the `content` link with _no `Accept` header_.

If a document has multiple versions the content link will point to the content of the latest active version, which will be a version that is not deleted and has had content uploaded to it successfully.

A 204 HTTP No Content response will be returned from this endpoint in the case where the document has no binary content.



### Example ###

In this example we request the content of a document. The _content_ link that was advertised had an href of `.../files/documents/123/content`. The content of this document is a text file.

#### Request ####
```
GET ... HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK 
Content-Type: text/plain
Content-Length: 198
Content-Disposition: attachment; filename="filename.txt"
Supported-Accept: text/plain, application/pdf
Vary: Accept
x-download-reason: file-preview

Hello - this is the content of your text file. You uploaded it to Huddle to keep it safe and 
secure, but now it has been used in an API example by a bad developer and so everybody can see it. Oh no!
```

#### Headers ####

| **Name** | **Definition** |
|:---------|:---------------|
| Content-Type | The [media type](MediaType) of the file content you are downloading. |
| Content-Length | The total length of the file you are downloading. |
| Content-Disposition | The filename of the file you are downloading |
| Supported-Accept | A comma-separated list of MIME types to which your document may be converted. (This may differ between documents.) |
| Vary     | Used to indicate that different `Accept` headers may provoke different responses |
| x-download-reason     | Used to decribe the reason for download.  This value is then stored in the description field of the download activity entry |

Note, only the following download reasons are supported;

| Download reason code | Description |
|:---------------------|:------------|
| "unspecified" | Reason not known|
| "other/{freetext reason}"| Other download reason (note, reason has max length of 100 chars)|
| "file-preview"  | Download to preview file  |
| "open-file-other-app"  | Download to open in another app  |
| "offline-file"  | Automatic download for offline files  |
| "offline-file-preview"  | Automatic download for offline files (preview)  |
| "suggested-file-preview"  | Automatic download for suggested files |

If x-download-reason is specified, but not one of the support values, a 400 Bad Request will be returned.

## Getting the content of a document in a different format ##

Huddle supports automatic conversion of documents into different formats. For example, Word documents (`application/msword`) can be converted on-demand into a PDF (`application/pdf`) format and downloaded.

To retrieve the converted content, issue a GET request to the `content` link advertised on the [Document](Document) response, with the `Accept` header set to the MIME type of the converted format you wish to download.

Supported MIME types for a given document will be indicated by a `Supported-Accept` header.

If there already exists a converted copy of the document with the correct format, the binary content of that copy will be immediately returned in the response to the GET request, with a 200 OK status code.

If the document doesn't have a version with the MIME type specified by the `Accept` header, one will need to be generated. The response to your GET request will have a _202 Accepted_ status code and a `Location` header containing a link at which you can monitor the generation process. When the converted document has been generated, the progress endpoint will return a _200 OK_ status code and a a link containing the content download URI for your document. Issuing a GET to that endpoint with your original Accept header will return the content. If the conversion fails, the progress endpoint will return a _500 Internal Server Error_; the response body will contain an error message describing the failure.

If conversion to the requested MIME type is not supported for your document, the `content` response will contain a _406 Not Acceptable_ status code, along with a list of MIME types that are supported for that document.

Specifying a MIME type equal to that of the originally uploaded document is equivalent to specifying no MIME type at all (it will return the original content).

### Example: retrieving the supported MIME types ###
 
#### Request ####
```http
HEAD .../content HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Accept: application/pdf
```

#### Response ####

```http
HTTP/1.1 200 OK 
Content-Type: application/pdf
Content-Length: 198
Content-Disposition: attachment; filename="filename.pdf"
Supported-Accept: application/msword, text/plain, application/pdf
Vary: Accept
```

### Example: Content already generated ###
Here, the client requests an `application/pdf` version of a document, for which a PDF conversion has already been carried out (perhaps by a previous user). The content is returned directly.

#### Sequence diagram ####
```
Client                            Server
  |           GET content           |
  |     Accept: application/pdf     |
  | ------------------------------> |
  |                                 |
  | <------------------------------ |
  |             200 OK              |
  |  Content-Type: application/pdf  |
  |      ...binary content...       |
```

#### Request ####
```http
GET .../content HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Accept: application/pdf
```

#### Response ####

```http
HTTP/1.1 200 OK 
Content-Type: application/pdf
Content-Length: 198
Content-Disposition: attachment; filename="filename.pdf"
Supported-Accept: application/msword, text/plain, application/pdf
Vary: Accept

... generated binary PDF content ...
```


### Example: Content not yet generated ###

Here, the client requests an `application/pdf` version of a document, for which a PDF version has never been generated. The client is invited to poll an endpoint to monitor the conversion; on the second poll the conversion is completed and the client is redirected back to the content endpoint.

#### Sequence diagram ####
```
Client                            Server
  |           GET content           |
  |     Accept: application/pdf     |
  | ------------------------------> |
  |                                 |
  | <------------------------------ |
  |          202 Accepted           |
  |   Location: .../conversion/...  |
  |                                 |
  |                                 |
  |      GET .../conversion/...     |
  | ------------------------------> |
  |                                 |
  | <------------------------------ |
  |             200 OK              |
  |     (conversion in progress)    |
  |                                 |
  |                                 |
  |      GET .../conversion/...     |
  | ------------------------------> |
  |                                 |
  | <------------------------------ |
  |          200 Accepted           |
  |    Link: .../content/...        |
  |      (conversion complete)      |
  |                                 |
  |                                 |
  |           GET content           |
  |     Accept: application/pdf     |
  | ------------------------------> |
  |                                 |
  | <------------------------------ |
  |             200 OK              |
  |  Content-Type: application/pdf  |
  |      ...binary content...       |
```

#### Request ####
```http
GET .../content HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Accept: application/pdf
```

#### Response ####

```http
HTTP/1.1 202 Accepted 
Location: .../versions/235253235/conversion/application%2Fpdf
Supported-Accept: application/msword, text/plain, application/pdf
Vary: Accept
```

Now the client follows the link in the `Location` header and learns that the conversion is still in progress:

#### Request ####
```http
GET .../versions/235253235/conversion/application%2Fpdf HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Accept: application/json
```

#### Response ####

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "status": "InProgress",
    "links": [
        {"rel": "self", "href": ".../versions/235253235/conversion/application%2Fpdf"},
        {"rel": "content":, "href": ".../content"}
    ]
}
```

On the second poll, the conversion has completed:

#### Request ####
```http
GET .../versions/235253235/conversion/application%2Fpdf HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Accept: application/json
```

#### Response ####

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "status": "Complete",
    "links": [
        {"rel": "self", "href": ".../versions/235253235/conversion/application%2Fpdf"},
        {"rel": "content":, "href": ".../content"}
    ]
}
```

Now the client can follow the redirect with the desired `Accept` header to re-issue the original request and get the content.

#### Request ####
```http
GET .../content HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Accept: application/pdf
```

#### Response ####

```http
HTTP/1.1 200 OK 
Content-Type: application/pdf
Content-Length: 198
Content-Disposition: attachment; filename="filename.pdf"
Supported-Accept: application/msword, text/plain, application/pdf
Vary: Accept

... generated binary PDF content ...
```


### Example: conversion to specified MIME type not supported ###

Here the client sets the `Accept` header to a MIME type which is not one of the MIME types listed in the `Supported-Accept` header (in other words, conversion to that format is not supported for this document). The server responds with a 406 Not Acceptable, and the `Supported-Accept` header details the MIME types which are supported for that document.

#### Request ####
```http
GET .../content HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Accept: video/mp4
```

#### Response ####

```http
HTTP/1.1 406 Not Acceptable
Supported-Accept: application/msword, text/plain, application/pdf
Vary: Accept
```


### Example: document conversion process failed ###

While polling the progress endpoint, the client learns that the conversion process has failed due to an error. Retrying the conversion for this document _may_ result in success.

#### Request ####
```http
GET .../versions/235253235/conversion/application%2Fpdf HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Accept: application/json
```

#### Response ####

```http
HTTP/1.1 500 Internal Server Error
Content-Type: application/json
```
