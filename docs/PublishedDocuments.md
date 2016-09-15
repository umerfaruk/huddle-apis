# Summary #

Documents that have been made public can be aggregated under the company that published them, or the workspace they were published from.

To publish or unpublish a document, please refer to the [PublishDocument](PublishDocument) page.

|  |
|:-|


---


## Get Published Document ##

This resource supports retrieving previously published document.

User must have access permissions to the workspace to request the published document details.

### Example ###

#### Request ####

```
GET .../publishing/documents/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<publication>
	<link rel="self" href="..." />
	<link rel="bookmark" href="..." />
	<link rel="alternate" href="..." type="text/html"/>
	<link rel="unpublish" href="..." />
	<actor 	rel="publisher" name="Freddy the publisher" email="Freddy@publishing.example.com">
		<link rel="self" href="..." />
		<link rel="avatar" href="..." type="image/jpg" />
		<link rel="alternate" href="..." type="text/html" />
	</actor>			
	<documentVersion title="My amazing document is amazing">
		<link rel="self" href="..." />
		<link rel="parent" href="..." />
		<link rel="workspace" href="..." title="My workspace is the best!" />
		<publishedDate>2014-06-15T09:43:17Z</publishedDate>
		<version>2</version>
		<contentType>application/pdf</contentType>
	</documentVersion>
	<scope>Public</scope>
	<totalViewCount>12405</totalViewCount>
</publication>
```

#### Link relations ####

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this publication list. | GET         |
| parent   | The parent document related to this publication. | GET         |
| publisher| The user that published the document. | GET         |
| bookmark | The public URI to access the published document. | GET         |


---


## Retrieving a list of published documents by Company ##

The list of all documents that have been published by an organisation will be advertised by the [Company](Company) resource with a @rel value of _publications_ .

This endpoint gives information on who published which documents, and whether they are published internally or to the web.

### Example ###

#### Request ####

```
GET /publishing/companies/123/HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
<publications>
	<link rel="self" href="..." />
	<link rel="first" href="..." />
	<link rel="last" href="..." />
	<link rel="next" href="..." />
	<link rel="prev" href="..." />
    <totalPublications>50</totalPublications>
	<publication>
  	                <link rel="self" href="..." />
			<link rel="bookmark" href="..." />
	                <link rel="alternate" href="..." type="text/html"/>
			<link rel="unpublish" href="..." />

			<actor 	rel="publisher"
				name="Freddy the publisher"
				email="Freddy@publishing.example.com">
				<link rel="self" href="..." />
			        <link rel="avatar" href="..." type="image/jpg" />
			        <link rel="alternate" href="..." type="text/html" />
			</actor>			
			<documentVersion title="My amazing document is amazing">
				<link rel="self" href="..." />
				<link rel="parent" href="..." />
				<link rel="workspace" href="..." title="My workspace is the best!" />
				<publishedDate>2014-06-15T09:43:17Z</publishedDate>
				<version>2</version>
				<contentType>application/pdf</contentType>
			</documentVersion>
			<scope>Public</scope>
			<totalViewCount>12405</totalViewCount>
	</publication>

	<publication>
  	                <link rel="self" href="..." />
			<link rel="bookmark" href="..." />
			<link rel="unpublish" href="..." />

			<actor 	rel="publisher"
				name="Jimmy the publisher"
				email="Jimmy@publishing.example.com">
				<link rel="self" href="..." />
			        <link rel="avatar" href="..." type="image/jpg" />
			        <link rel="alternate" href="..." type="text/html" />
			</actor>
	
			<documentVersion title="My everyday document is dull">
				<link rel="self" href="..." />
				<link rel="parent" href="..." />
				<link rel="workspace" href="..." title="My workspace is bland" />
				<publishedDate>2014-06-15T09:43:17Z</publishedDate>
				<version>1</version>
				<contentType>image/jpg</contentType>
			</documentVersion>
			<publishedOn>2014-06-15T09:43:17Z</publishedOn>
			<scope>Company</scope>
			<totalViewCount>12405</totalViewCount>
	</publication>
</publications>
```

### Parameters ###
| **Name** | **Description** | **Methods** | **Optional** | **Default** |
|:---------|:----------------|:------------|:-------------|:------------|
| page     | Page that you are requesting | GET         | Yes          | 1           |
| q        | A list of space separated words by which you want to filter | GET         | Yes          | 1           |

Request with parameter
```
GET /publishing/companies/123?page=1&q=a_word HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```


### Link relations ###

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this publication list. | GET         |
| parent   | The parent document related to this publication. | GET         |
| publisher| The user that published the document. | GET         |
| bookmark | The public URI to access the published document. | GET         |
| alternate | Huddle custom scheme URI. It follows the format: huddle://_relative\_self\_link_ so that external applications can register to handle a click on this link by the user and be able to retrieve the document. | N/A         |
| next     | The URI of the next page of published documents, using sort order specified. | GET         |
| prev     | The URI of the previous page of published documents, using sort order specified. | GET         |
| first    | The URI of the first page of published documents, using sort order specified.  | GET         |
| last     | The URI of the last page of published documents, using sort order specified. | GET         |

The 'prev' link is not present on the first page of published documents.

The 'next' link is not present on the last page of published documents.

The 'last' and 'first' links are not present if there is only a single page.


---


## Retrieving a list of published documents by Workspace ##

The list of all documents that have been published from a workspace will be advertised by the [Root folder](Folder#Retrieving_a_root_folder) resource with a @rel value of _publications_ .

This endpoint gives information on who published which documents, and whether they are published internally or to the web.

### Example ###

#### Request ####

```
GET /publishing/workspaces/123/ HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: Bearer frootymcnooty/vonbootycherooty
```

#### Response ####

```
<publications>
	<link rel="self" href="..." />
	<link rel="first" href="..." />
	<link rel="last" href="..." />
	<link rel="next" href="..." />
	<link rel="prev" href="..." />
    <totalPublications>50</totalPublications>
	<publication>
  	                <link rel="self" href="..." />
			<link rel="bookmark" href="..." />
	                <link rel="alternate" href="..." type="text/html"/>

			<actor 	rel="publisher"
				name="Freddy the publisher"
				email="Freddy@publishing.example.com">
				<link rel="self" href="..." />
			        <link rel="avatar" href="..." type="image/jpg" />
			        <link rel="alternate" href="..." type="text/html" />
			</actor>			
			<documentVersion title="My amazing document is amazing">
				<link rel="self" href="..." />
				<link rel="parent" href="..." />
				<link rel="workspace" href="..." title="My workspace is the best!" />
				<publishedDate>2014-06-15T09:43:17Z</publishedDate>
				<version>2</version>
				<contentType>application/pdf</contentType>
			</documentVersion>
			<scope>Public</scope>
			<totalViewCount>12405</totalViewCount>
	</publication>

	<publication>
  	                <link rel="self" href="..." />
			<link rel="bookmark" href="..." />

			<actor 	rel="publisher"
				name="Jimmy the publisher"
				email="Jimmy@publishing.example.com">
				<link rel="self" href="..." />
			        <link rel="avatar" href="..." type="image/jpg" />
			        <link rel="alternate" href="..." type="text/html" />
			</actor>
	
			<documentVersion title="My everyday document is dull">
				<link rel="self" href="..." />
				<link rel="parent" href="..." />
				<link rel="workspace" href="..." title="My workspace is bland" />
				<publishedDate>2014-06-15T09:43:17Z</publishedDate>
				<version>1</version>
				<contentType>image/jpg</contentType>
			</documentVersion>
			<publishedOn>2014-06-15T09:43:17Z</publishedOn>
			<scope>Company</scope>
			<totalViewCount>12405</totalViewCount>
	</publication>
</publications>
```

### Parameters ###
| **Name** | **Description** | **Methods** | **Optional** | **Default** |
|:---------|:----------------|:------------|:-------------|:------------|
| page     | Page that you are requesting | GET         | Yes          | 1           |
| q        | A list of space separated words for which you want to filter | GET         | Yes          | 1           |

Request with parameter
```
GET /publishing/workspaces/123?page=1&q=a_word HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: Bearer frootymcnooty/vonbootycherooty
```


### Link relations ###

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this publication list. | GET         |
| parent   | The parent document related to this publication. | GET         |
| publisher| The user that published the document. | GET         |
| bookmark | The public URI to access the published document. | GET         |
| alternate | Huddle custom scheme URI. It follows the format: huddle://_relative\_self\_link_ so that external applications can register to handle a click on this link by the user and be able to retrieve the document. | N/A         |
| next     | The URI of the next page of published documents, using sort order specified. | GET         |
| prev     | The URI of the previous page of published documents, using sort order specified. | GET         |
| first    | The URI of the first page of published documents, using sort order specified.  | GET         |
| last     | The URI of the last page of published documents, using sort order specified. | GET         |

The 'prev' link is not present on the first page of published documents.

The 'next' link is not present on the last page of published documents.

The 'last' and 'first' links are not present if there is only a single page.


---



## Register a hit on a Published Document ##

Register a hit on the published content so that the publisher can track / monitor the usage

### Parameters ###

| **Name** | **Description** |
|:---------|:----------------|
| documentId | the document's Id in Files-BC |
| versionId | the document's versionId |
| Uri Format | ../publishing/tracking/{documentId}/{versionId} |


### Example ###

#### Request ####
```
POST ../publishing/tracking/1/2 HTTP/1.1
```

#### Response ####
```
HTTP/1.1 200 Ok
```


---


## Use server Caching ##

Allows the use of server caching when requesting the same content multiple times. The first time a client requests a document, or a list of documents, the server will return the “Last-Modified” date value in the response header.

On any subsequent requests made by the client, where the content has not changed, if the client reuses the “Last-Modified” date header, then, the date will be used in the “If-Modified-Since” HTTP header and the server will respond with a 304, “NotModified” HTTP status code.

### Example: First time request ###

#### Request ####
```
GET /publishing/workspaces/123/ HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: Bearer frootymcnooty/vonbootycherooty
```

#### Response ####
#### Request ####

```
GET .../publishing/documents/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Last-Modified: Tue, 09 Dec 2014 15:43:12 GMT
```

```
<publication>
...
</publication>
```

### Example: Second time request same content ###

#### Request ####
```
GET /publishing/workspaces/123/ HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: Bearer frootymcnooty/vonbootycherooty
If-Modified-Since: Tue, 09 Dec 2014 15:44:00 GMT
```

#### Response ####

```
HTTP/1.1 304 Not Modified
Content-Type: application/vnd.huddle.data+xml
```
