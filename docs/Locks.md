# Summary #

Locks are resources attached to a [document](Document) that indicate that the document has been placed in a read-only state by a [user](Actor). The Locks resource represents a read-only collection of lock information.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving locks by owner|Beta        |

# Operations #

## Retrieving locks by owner ##

It is possible for a user to request a list of locks that they own. This is useful for answering the question: "What documents do I currently have locked?".

### Example ###

In this example, we request locks owned by user 789.

#### Request ####

```
GET /files/lockowners/789/locks HTTP/1.1
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
   <link rel="self" href="/files/lockowners/789/locks" />
   <lock>
     <link rel="self" href="/files/documents/123/lock" />
     <link rel="delete" href="/files/documents/123/lock" />
     <created>2013-11-23T09:02:17Z</created>
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
     <created>2013-12-01T09:02:17Z</created>
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

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The URI of this list of locks. | GET         |
| lock:delete | The URI to delete the lock. | DELETE      |
| document:self | The URI of the document resource | GET         |
| document:alternate | A website URL for the document | GET         |
| workspace:self | The URI of the workspace resource | GET         |
| workspace:alternate | A website URL for the workspace | GET         |

## Schema ##

```

start = locks

locks = element h:locks{
  link,
  lock+
}

lock = element h:lock {
  link+,
  element h:created {xsd:dateTime},
  document
}

document = element h:document {
  attribute title {xsd:string},
  attribute content-type {xsd:string},
  link+,
  workspace 
}

workspace = element h:workspace{
  attribute title {xsd:string},
  link+,
}

```
