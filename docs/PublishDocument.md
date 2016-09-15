# Summary #

Documents can be published publicly or privately. A published document has a unique URL that can be shared with people who do not have access to the document in Huddle.

|  |
|:-|

# Operations #

## Publishing a document ##

If a document can be published, it will advertise a link with the @rel value _publish_. Submitting an empty POST to this endpoint will publish the latest version of the document.

If the Publish request is successful, the response will be a 202 accepted. A successful response will include a Link header with a rel value of _progress_. This _progress_ link will point to a monitoring resource. A deprecated _monitor_ link will also be advertised.

### Example ###

#### Request ####
```
POST .../files/documents/123/publications HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 202 Accepted
Link: <https://.../publishing/progress/660107d3-b06d-4972-9cfc-8c5ac1e381e5>;rel="progress"
Link: <https://.../files/documents/123/publications>;rel="monitor"
```


---


## Retrieving the progress of a publishing action ##

The `PublishingProgress` resource will contain information about the State of an ongoing Publishing request.

The endpoint will advertise a _bookmark_ link as soon as available.
It will also advertise a link to _publications_ when the _status_ is `Done`. The client should then follow this link to refresh its view of the document publications.


### Example ###

#### Request ####
```
GET /publishing/progress/660107d3-b06d-4972-9cfc-8c5ac1e381e5 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<publishingProgress xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="publishing/progress/660107d3-b06d-4972-9cfc-8c5ac1e381e5" />
  <link rel="bookmark" href="http://public.published.link/123" />
  <link rel="publications" href="documents/123/publications" />
  <status>Done</status>
</publishingProgress>
```

### Properties ###
| **Name** | **Description** |
|:---------|:----------------|
| Status   | The publication progress status: `NotStarted`, `InProgress`, `Done`, `Failed`|


---

## Unpublishing a document ##

Unpublish an already published document. The document will no longer be available publicly or privately.

Response contains a link to re-publish the document should that be desired immediately after unpublishing.

### Example ###

#### Request ####
```
DELETE .../files/documents/123/publications HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 202 Accepted
Link: <https://.../files/documents/123/publications>;rel="self"
Link: <https://.../files/documents/123/publications>;rel="publish"
```


---


## Retrieving a list of document version that we requested publication of ##

The [document](Document) resource will advertise a link with @rel value of _publications_ if the document has been published. To retrieve the history of the publications for the document as well as its public link, GET the _publication_ URI. "publish" and "unpublish" links will also be available if user has permission to publish/unpublish where appropriate.

### Example ###

#### Request ####
```
GET /documents/123/publications HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<documentPublication xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="documents/123/publications" />
  <link rel="parent" href="documents/123" />
  <link rel="bookmark" href="http://public.published.link/123" />
  <link rel="publish" href="documents/123/publications" />
  <link rel="unpublish" href="documents/123/publications" />
  
  <publications>
    <publication>
       <actor name="Peter Gibson" email="peter.gibson@example.com" rel="publisher">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>

      <publishedDate>2007-10-10T09:02:17Z</publishedDate>
      <publishingstate>PublishInProgress</publishingstate>
      <documentVersion>
         <link rel="self" href="documents/123/version/543" />
         <version>2</version>
         <note>new note</note>
      </documentVersion>

    </publication>

    <publication>
      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="publisher">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor> 

      <publishedDate>2007-10-10T09:02:17Z</publishedDate>
      <publishingstate>PublishInProgress</publishingstate>
      <documentVersion>
         <link rel="self" href="documents/123/version/542" />
         <version>1</version>
         <note>new note</note>
      </documentVersion>

          
    </publication>

  </publications>

</documentPublication>
```

### Properties ###
| **Name** | **Description** |
|:---------|:----------------|
| PublishingState| The publishing status of the published version: `NotPublished`, `PublishInProgress`, `Published`, `UnpublishInProgress`|


---



## Get Published Document ##

This information is retrieved from the [PublishedDocuments](PublishedDocuments) endpoint


## Retrieving a list of published documents by Company ##

This list is retrieved from the [PublishedDocuments](PublishedDocuments) endpoint

## Retrieving a list of published documents by Workspace ##

This list is retrieved from the [PublishedDocuments](PublishedDocuments) endpoint
