|  |
|:-|

# Range Requests #

Some of our endpoints support byte range requests. Endpoints that support byte range requests will advertise this feature by returning an _Accept-Ranges_ header.

Clients may wish to request byte ranges from endpoints that support this feature. To determine if an endpoint support byte range requests, the client should issue a HEAD (or GET) request to the endpoint. If the response advertises an _Accept-Ranges_ header, then it supports byte range requests.

### Example ###

The following example illustrates how to make byte range requests to an endpoint (assuming a content length of 10000 bytes).

#### Non-Range Request ####

1. The client issues a HEAD request to the endpoint to determine if it supports range requests.

```
HEAD /files/documents/versions/123/previews/video/content/ HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Accept-Ranges: bytes
​Content-Length: 10000
Content-Type: video/mp4
```

#### Range Request ####

2. The client issues a byte range request to the endpoint by making a GET request with a _Range_ header.

```
GET /files/documents/versions/123/previews/video/content/ HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Range: bytes=500-999
```

#### Response ####

When an endpoint that supports byte range requests receives a valid byte range request, it will return a 206 Partial Content response.

```
HTTP/1.1 206 Partial Content
Accept-Ranges: bytes
Content-Range: bytes 500-999/10000
Content-Length: 500
Content-Type: video/mp4
```
```
[[partial binary content]]
```

## Syntactically Invalid Range Requests ##

If the client sends a syntactically invalid _Range_ header, the endpoint will ignore the _Range_ header entirely and return a 200 OK full content response. This conforms to RFC 2616 Section 14.35.1.

#### Request ####
```
GET /files/documents/versions/123/previews/video/content/ HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Range: bytes=999-500
```

#### Response ####
```
HTTP/1.1 200 OK
Accept-Ranges: bytes
​Content-Length: 10000
Content-Type: video/mp4
```
```
[[full binary content]]
```

## Unsatisfiable Range Requests ##

If the client requests a range that is not satisfiable, the endpoint will return a 416 Requested Range Not Satisfiable response.

#### Request ####
```
GET /files/documents/versions/123/previews/video/content/ HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Range: bytes=10001-10500
```

#### Response ####
```
HTTP/1.1 416 Requested Range Not Satisfiable
Content-Range: bytes */10000
```

## Multiple Range Requests ##

The Huddle API does not currently support multiple range requests on any of its endpoints. If a multiple range request is received, the endpoint will return a 403 Forbidden response.
