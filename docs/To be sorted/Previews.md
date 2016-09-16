|  |
|:-|

# Summary #

Document previews are supported for many file types, including Word, Excel, PDF, image, and video. If previews are supported for a document's file type, its GET Document response will advertise a link with a @rel of _previews_. The Previews URI can be used to get information about the previews that have been generated for the document. If no previews have been generated for a document, then the Previews URI can be used to generate one.

# Operations #

## Getting document preview details ##

The Previews URI can be used to get information about a document's previews. If previews are supported for the document's file type, the Previews URI is advertised in the GET Document response as a link with a @rel of _previews_.

Please note that video preview is an account capability, and as such, is not available to all accounts. If an account does not have the Video Preview feature enabled, the GET Document response will not advertise a _previews_ link.

A GET request to a document's Previews resource will return a 200 OK if a preview exists for the document. The body of the response will contain preview details, including available preview types and associated content links.

If a document does not have any previews, the Previews resource will return a 404 Not Found (but see [note on preview generation failure](#Preview_Generation_Failure) below).

### Example ###

The following example shows the steps required to get information about the preview(s) available for a document.

1. Obtain a document's Previews URI by [retrieving the document](Document#Retrieving_a_document).

#### Get Document Request ####
```
GET /files/documents/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Get Document Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<document>
  <link rel="previews" href="/files/documents/versions/123/previews" />
  [other document elements]
</document>
```

2. Get information about a document's previews by issuing a GET request to the Previews URI.

#### Get Previews Request ####

```
GET /files/documents/versions/123/previews HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Get Previews Response ####

The response contains details about the preview(s) available for the document. The types of previews available for a document vary according to the document type. Possible preview types include "flexpaper", "pdf", "image", "note", "video" and "audio".

#### Template response for "flexpaper", "pdf", "image" and "note" ####
```
<previews>
  <preview type="[preview-type]">
    <link rel="preview" href="/files/documents/versions/417/previews/[fileName]" />
  </preview>
</previews>
```

#### Template response for "video" and "audio" ####

```
<previews>
  <preview type="[preview-type]">
    <link rel="preview" href="/content/documents/versions/417/previews/[preview-type]/content" />
  </preview>
</previews>
```

Example responses for different preview types are provided below.

Response body for a flexpaper preview:

```
<previews>
  <preview type="flexpaper">
    <link rel="preview" href="/files/documents/versions/417/previews/data.swf" />
  </preview>
</previews>
```

Response body for a pdf preview:

```
<previews>
  <preview type="pdf">
    <link rel="preview" ref="/files/documents/versions/417/previews/preview.pdf" />
  </preview>
</previews>
```

Response body for an image preview:

```
<previews>
  <preview type="image">
    <link rel="preview" ref="/files/documents/versions/417/previews/scaled.jpg" />
  </preview>
</previews>
```

Response body for a video preview:

```
<previews>
  <preview type="video">
    <link rel="content" href="/content/documents/versions/123/previews/video/content" />
  </preview>
</previews>
```

#### Restricted preview ####

If the requesting user only has restricted permissions on a document, then the preview resource will indicate this to clients in the response body.

```
<previews isRestricted="true">
  <preview type="flexpaper">
    <link rel="preview" href="/files/documents/versions/417/previews/data.swf" />
  </preview>
</previews>
```

If the attribute is present and set to true, clients should want to limit functionality on say PDF controls for examples disallow selecting text.

## Downloading a document preview ##

Preview content can be downloaded by issuing a GET request to the Preview Content link (or template) advertised in the Get Previews response. See the [example responses](#Get_Previews_Response) above.

The Get Preview Content endpoint supports byte range requests. See [Byte Range Requests](ByteRangeRequests) for further information.

### Example ###

The following example shows how to get binary content for a document preview.

#### Request ####
```
GET /files/documents/versions/123/previews/video/content HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
​Content-Length: 123456
Content-Type: video/mp4
Accept-Ranges: bytes
```
```
binary content
```

## Creating a document preview ##

For most file types, previews are generated on demand, and a preview can be generated by issuing a POST request to the document's Previews URI. Because it could take some time to generate a preview for the document, this process occurs asynchronously.

Upon receiving a valid request to generate a preview, the endpoint will return a 202 Accepted response. The response will include a Location header containing a link that can be polled for preview generation status.

Please note that video previews are generated automatically when a video is uploaded, and therefore the on-demand preview generation process does not apply to videos. If a client attempts to generate a preview for a video by issuing a POST request to the Previews URI, the endpoint will respond with 405 Method Not Allowed response.

### Example ###

The following example shows a client issuing a POST request to create a preview, and subsequently polling the URI provided in the Location header to determine if preview generation has completed.

#### Create Preview Request ####

```
POST /files/documents/versions/123/previews  HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Content-Length: 0
```

#### Create Preview Response ####

```
HTTP/1.1 202 Accepted
Location: /files/documents/versions/123/previews
Content-Length: 0
```

#### Get Location ####

```
GET /files/documents/versions/123/previews HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Get Location Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<previews>
  <preview type="image">
    <template property="IMGfile" href="/files/documents/versions/2/previews/scaled.jpg" />
  </preview>
</previews>
```

#### Preview Generation Failure ####

Preview generation can sometimes fail, particularly in cases where the document is large or complicated. If preview generation has previously failed, subsequently issuing a GET request to the Previews endpoint will result in a 200 OK response, and the body of the response will contain the date that the failure occurred:

```
<previews>
  <lastfailedon>2013-04-24 11:20:00 AM</lastfailedon>
</previews>
```
