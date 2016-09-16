# Summary #

The VersionHistory resource represents the collection of the [versions](DocumentVersion) of a [document](Document).

Individual versions adhere to the [documentVersion](DocumentVersion) schema.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving a document version history|Beta        |

# Operations #

## Retrieve document version history ##

The VersionHistory for a document is returned.
If the authenticated user can view the version history, the document resource will advertise a link with @rel value of _version-history_. To retrieve the document version history, GET the _version-history_ URI.

### Example ###

#### Request ####
```
GET /documents/123/versions HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<versionHistory xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="documents/123/versions" />
  <link rel="parent" href="documents/123" />

  <documentVersion title="a new title" description="a new description!!!!">
    <link rel="self" href="documents/123/version/543" />
    <link rel="content" href="..." title="..." />
    
    <actor name="Barry Potter" email="barry.potter@example.com" rel="updated-by">
      <link rel="self" href="..." />
      <link rel="avatar" href="..." type="image/jpg" />
      <link rel="alternate" href="..." type="text/html" />
    </actor>
    
    <size>445645648</size>
    <version>2</version>
    <note>new note</note>
    <contentType>application/msword</contentType>
    <created>2011-02-15T09:43:17Z</created>
    <updated>2011-02-19T14:28:04Z</updated>
  </documentVersion>
  
  <documentVersion title="an old title">
    <link rel="self" href="documents/123/version/432" />
    <link rel="delete" href="documents/123/version/432" />
    <link rel="content" href="..." title="..."/>
    
    <actor name="Peter Gibson" email="peter.gibson@example.com" rel="updated-by">
      <link rel="self" href="..." />
      <link rel="avatar" href="..." type="image/jpg" />
      <link rel="alternate" href="..." type="text/html" />
    </actor>
    
    <size>445645648</size>
    <version>1</version>
    <contentType>application/msword</contentType>
    <created>2011-02-15T09:43:17Z</created>
    <updated>2011-02-15T09:43:17Z</updated>
  </documentVersion>
       
</versionHistory>
```


---


# Syntax #

## Example ##

```
<versionHistory xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="..." />
  <link rel="parent" href="..." />
  <link rel="latest-version" href="..." />

  <documentVersion title="..." description="...">
    <link rel="self" href="..." />
    <link rel="content" href="..." title="..."/>
    
    <actor name="Barry Potter" email="barry.potter@example.com" rel="updated-by">
      <link rel="self" href="..." />
      <link rel="avatar" href="..." type="image/jpg" />
      <link rel="alternate" href="..." type="text/html" />
    </actor>
    
    <size>445645648</size>
    <version>2</version>
    <note>new note</note>
    <contentType>application/msword</contentType>
    <created>2011-02-15T09:43:17Z</created>
    <updated>2011-02-19T14:28:04Z</updated>
  </documentVersion>
  
  <documentVersion title="an old title">
    <link rel="self" href="..." />
    <link rel="delete" href="..." />
    <link rel="content" href="..." title="..."/>
    
    <actor name="Peter Gibson" email="peter.gibson@example.com" rel="updated-by">
      <link rel="self" href="..." />
      <link rel="avatar" href="..." type="image/jpg" />
      <link rel="alternate" href="..." type="text/html" />
    </actor>
    
    <size>445645648</size>
    <version>1</version>
    <contentType>application/msword</contentType>
    <created>2011-02-15T09:43:17Z</created>
    <updated>2011-02-15T09:43:17Z</updated>
  </documentVersion>
       
</versionHistory>
```

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this versionHistory. | GET         |
| parent   | The URI of the document. | GET         |
| latest-version | The URI of the latest version of the document. | GET         |

## Schema ##

```

start = versionHistory

versionHistory = element h:versionHistory {
  link+,
  documentVersion+
}
```
