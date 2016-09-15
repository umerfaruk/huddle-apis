# Summary #

Locks are resources attached to a [document](document) that indicate that the document has been placed in a read-only state by a [user](Actor). A locked [document](document) can't be edited by someone else than the current lock _owner_.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Locking a document|Beta        |
|Unlocking a document|Beta        |
|Retrieving a lock|Beta        |
|Retrieving locks by owner|Beta        |

# Operations #


## Locking a document ##

When [retrieving a document](Document#Retrieve_a_document), the response will advertise a link with a @rel value of _lock_, which you can use lock the document.

When locking a document, the caller may optionally include a  'lockIntent' in the request body. This field enables state to convey the intent of the lock - currently only one value is valid `lockedForEdit`

### Example ###

In this example document 123 is locked.

#### Request ####
```
POST /files/documents/123/lock HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Content-Length:38
```
```
<lockIntent>lockedForEdit</lockIntent>
```

#### Response ####

If successful, this operation returns a 201 Created with a representation of the lock. The response will contain a _delete_ link which can be used to unlock the document.

If the document is already locked, a 409 Conflict will be returned with a Location header giving the URI of the locked document.

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
```
```
<lock xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="/documents/123/lock" />
  <link rel="delete" href="..." />
  <link rel="parent" href="..." />

  <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>
  <lockIntent>lockedForEdit</lockIntent>
  <lockDate>2007-10-10T09:02:17Z</lockDate>
</lock>
```

## Unlocking a document ##

When a document is locked, the response will contain a representation of the lock in the body of the response. The lock element will contain a link with a @rel value of _delete_, which can be used to remove the lock. The link for unlocking a document is also advertised within the lock element of a [locked document response](Document#A_locked_document_retrieved_by_the_owner_of_the_lock).

### Example ###

In the following example we will unlock document 123.

#### Request ####

```
DELETE /files/documents/123/lock HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If successful, this method will return an empty response with an OK status code and a [link header](Link#Header) to the document. If the document is not locked we will return a 404. If the current user does not have permission to delete the lock, the endpoint will return a 403.

```
HTTP/1.1  200 OK
Link: </documents/123>;rel="parent"
```

## Retrieving a lock ##

When a document is locked, the response will contain a representation of the lock in the body of the response. The lock element will contain a link with a @rel value of _self_, which can be used to retrieve the lock. The link for retrieving a lock is also advertised within the lock element of a [locked document response](Document#A_locked_document_retrieved_by_the_owner_of_the_lock).

### Example ###

In this example, we will get the lock for document 123.

#### Request ####
```
GET /files/documents/123/lock HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If the document is not locked, the endpoint will return a 404.

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<lock xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="/documents/123/lock" />
  <link rel="delete" href="..." />
  <link rel="parent" href="..." />

  <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>
  <lockIntent>lockedForEdit</lockIntent>
  <lockDate>2007-10-10T09:02:17Z</lockDate>
</lock>
```

## Retrieving locks by owner ##

It is possible for a user to request a list of locks that they own. This is useful for answering the question: "What documents do I currently have locked?".

### Example ###

In this example, we request locks owned by user 789.

#### Request ####

```
GET /files/lockowner/789/locks HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<locks>
   <link rel="self" href="/files/lockowner/789/locks" />
   <lock>
     <link rel="self" href="/files/documents/123/lock" />
     <link rel="delete" href="/files/documents/123/lock" />
     <createdDate>2013-11-23T09:02:17Z</createdDate>
      <document title="My Cat" content-type="image/jpg">
         <link rel="self" href="/files/documents/123" />
         <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/555/files/123" />
         <workspace title="Don't Edit My Cats">
	    <link rel="self" href="/workspaces/555" />
            <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/555" />
         </workspace>	
      </document>  
   </lock>
   <lock>
     <link rel="self" href="/files/documents/322/lock" />
     <link rel="delete" href="/files/documents/322/lock" />
     <createdDate>2013-12-01T09:02:17Z</createdDate>
     <document title="My Other Cat" content-type="image/jpg">
         <link rel="self" href="/files/documents/322" />
         <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/444/files/322" />
         <workspace title="More Cat Pictures">
	    <link rel="self" href="/workspaces/444" />
            <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/444" />
         </workspace>	
     </document>  
   </lock>
</locks>
```

# Syntax #

## Example ##

```
<lock>
  <link rel="self" href="..." />
  <link rel="delete" href="..." />
  <link rel="parent" href="..." />

  <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>
</lock>
```

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The URI of this lock. | GET         |
| delete   | The URI to delete the lock. | DELETE      |
| parent   | The URI of the locked document. | GET         |

## Schema ##

```
namespace h = "http://schema.huddle.net/2011/02/"
include "base.rnc"

start = lock

lock = element h:lock {
  link+,
  actor
}
```
