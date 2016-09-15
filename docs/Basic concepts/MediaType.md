# Summary #

MIME media types are a standard way of describing the format of content on the internet. The full MIME specification is available in [rfc 4288](http://www.faqs.org/rfcs/rfc4288.html).

The Huddle API makes extensive use of media types to advertise and control the format of requests and responses. Every Http response that includes a body will contain a [content type header](HttpHeaders) that specifies the media-type of the response. [Link](Link) elements may include a @type attribute to specify the media-type of the linked resource.

|  |
|:-|

# Media type structure #

Media types have a type, sub-type, and optional suffix.

Huddle-specific media types are all prefixed `application/vnd.huddle`. [Link](Link)s to images and downloadable files will have a media-type corresponding to the type of the file.

Responses from the Huddle API use the suffix to indicate the encoding of the resource. For example a media-type of `application/vnd.huddle.error+xml` indicates that the resource is an [error](ErrorHandling) and the resource is encoded in XML.

Media types may be parametised by adding additional data separated by a semi-colon.

In this example, we have a media type of "application/vnd.huddle" which additionally specifies a "version" parameter of "1" and a "describedby" attribute with a URI for schema information.

`application/vnd.huddle.data; version=1; describedby="https://schema.huddle.net/rng/folder.rnc"`

Sending generic media types, such as `application/xml`, may result in unexpected behaviour.  Avoid using these media wherever possible.

## Media Types in the Files Api ##

The APIs described by this documentation use the single media type `application/vnd.huddle.data` for successful requests, and the media type `application/vnd.huddle.error` for errors.

The media-type `application/vnd.huddle.data` is documented on the [resources overview](Resources).

## Example media types ##

| **Document type** | Media-Type |
|:------------------|:-----------|
| Jpeg image        | image/jpeg |
| Word document     | application/msword |
| Plain text        | text/plain |

## Example Huddle media types ##

| **Resource type** | **Data format** | **Media Type** |
|:------------------|:----------------|:---------------|
| Folder            | xml             | application/vnd.huddle.data+xml |
| Folder            | json            | application/vnd.huddle.data+json |
| Error             | xml             | application/vnd.huddle.error+xml |
| Error             | json            | application/vnd.huddle.error+json |

## JSONP support ##

In order to use [JSONP](http://en.wikipedia.org/wiki/JSONP) (where supported) you have to specify a "callback"  querystring parameter.

#### Example Request ####
```
GET /localisation/categories/filesappdetails?callback=myfunction HTTP/1.1 
```

myfunction is the name of the javascript function that will be invoked when the response is sent to the client

#### Example Response ####
```
HTTP/1.1 200 OK
Content-Type: application/javascript
```
```
myfunction({bar :{ baz: "qux"}})
```

# Choosing a media type #

When making an HTTP request, you should include an [accept header](HttpHeaders) to indicate the resource type and encoding you are expecting.

## Example ##

In this example we request xml data from the API.

#### Request ####
```
GET /someresource HTTP/1.1
Accept: application/vnd.huddle.data+xml  
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<foo> 
  <bar baz="qux" />
</foo>
```

In this example we request json data from the API.

#### Request ####
```
GET /someresource HTTP/1.1
Accept: application/vnd.huddle.data+json
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```
{
  bar :{ baz: "qux"}
}
```

In this example we request jsonp data from the API.

#### Request ####
```
GET /someresource?callback=myfunction HTTP/1.1
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/javascript
```
```
myfunction({bar :{ baz: "qux"}})
```

# Media type documentation #

Each API operation includes at least one request and response example, including the expected media types as the Content-Type header.  Please use these for reference.

# Data Encodings #

Huddle currently supports XML and JSON data encodings.

# Data Compression #

Huddle currently supports gzip data compression for a number of data encodings.

To enable data compression when calling the Huddle API's add the following header to your API request:

```
   Accept-Encoding : gzip, deflate
```

The data encodings that are supported for data compression are displayed in the following table.

| **Data Type** | **Media Type** |
|:--------------|:---------------|
| xml           | application/xml |
| xml           | application/vnd.huddle.data+xml |
| xml (error)   | application/vnd.huddle.error+xml |
| json          | application/json |
| json          | application/vnd.huddle.data+json |
| json (error)  | application/vnd.huddle.error+json |
