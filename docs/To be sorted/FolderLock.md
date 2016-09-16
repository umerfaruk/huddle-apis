# Summary #

Operations on a [Folder](Folder) or [PagedFolder](PagedFolder) may result in the resource being locked. A client cannot initiate a lock, instead Huddle chooses when to lock the resource, to prevent conflicting updates.

When locking, Huddle will lock all folders that can be reached (recursively) from the document root of the workspace, easily identified by the parent link on the lock.

The lock is taken for the duration of the operation, and then automatically released afterwards. In many cases you will never need to be aware that a lock exists, as it will only be held for such a short period of time that there is no collision.

When a client issues a GET request to a [Folder](Folder) or [PagedFolder](PagedFolder) when a lock is present it will be shown in the response body.

It is worth noting that this resource extends the [Lock](Lock) resource used for [Document](Document) with additional properties reflecting the operation that the server locked the resource for. By contrast a [Document](Document) [Lock](Lock) is always user initiated.


# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving a Lock|Beta      |
|Unlocking a Folder|Beta     |


# Operations #

## Retrieving a Lock ##
When a Folder is locked, the response will contain a representation of the lock in the body of the response. 
 

```http
GET /files/folders/12345 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
```

In addition to the information above, the `<folder>` element will also contain a `<lock>` element like so:

```xml
<folder xmlns="https://schema.huddle.net/2011/02/" itemType="folder" title="My folder" displayName="My folder" description="Folder description">
 ... other folder elements ...
<lock>
  <link rel="self" href="..." />
  <link rel="delete" href="..." />
  <link rel="parent" href="..." />
  <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>
  <code>0</code>
  <intent>CreateFolder</intent>
  <created>2007-01-01T00:02:03Z</created>
</lock>
</folder>
```
```json
{
  "links": [
    {
      "rel": "self",
      "href": "..."
    },
    {
      "rel": "parent",
      "href": "..."
    },
    {
      "rel": "delete",
      "href": "..."
    }
  ],
  "actor": {
    "name": "...",
    "email": "...",
    "rel": "owner",
    "links": [
      {
        "rel": "self",
        "href": "..."
      },
      {
        "rel": "avatar",
        "href": "..."
      },
      {
        "rel": "alternate",
        "href": "...",
        "type": "text/html"
      }
    ]
  },
  "code": 3,
  "intent": "UpdatePermissions",
  "created": "Fri, 18 Dec 2015 09:22:25 GMT"
}
```

The lock element will contain a link with a @rel value of _self_, which can be used to retrieve the lock resource directly. The body of the response is the lock element.

We provide three key pieces of information for understanding the lock taken on a Folder: Who, When, and Why:

* Actor: Who is taking the lock
* Links: self=[uri of this lock]; parent=[uri of root folder]; delete[uri of this lock]
* Locked At: When did we take this lock

### Lock contention ###
If you try to perform an operation on a locked Folder then we will return a [423 Locked](https://tools.ietf.org/html/rfc4918) (WebDAV) and your request will fail.

The body of the response will be the Lock resource, which can be mined to understand who has taken the lock, why, and for how long.

You may choose to retry the operation in the belief that the locking operation will have completed and released the lock. This a valid approach, but be aware that operations that cascade over many folders such as a permissions change or deletion may hold the lock long enough that a retry will still fail, and your code should be prepared to eventually accept failure under those circumstances.

## Unlocking a Folder ##
You should not need to remove a lock, it will be automatically deleted when the operation completes. 

In case of error, we provide a mechanism to force an unlock. 

The lock element will contain a link with a @rel value of _delete_, which can be used to remove the lock.

By sending a DEL request to the Lock's self uri you can delete a lock, provided that the user making the request is the lock owner or a workspace manager.

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The URI of this lock. | GET         |
| delete   | The URI to delete the lock. | DELETE      |
| parent   | The URI of the root Folder for this workspace | GET         |

## Schema ##

```
namespace h = "http://schema.huddle.net/2011/02/"
include "base.rnc"

start = lock

lock = element h:lock {
  link+,
  actor,
  lockCode,
  lockIntent,
  lockedAt
}
```



