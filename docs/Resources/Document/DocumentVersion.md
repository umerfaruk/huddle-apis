# Summary #

The DocumentVersion resource represents one distinct version of a [document](Document).

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Delete a document version|Beta        |

# Operations #

## Delete a document version ##

If the authenticated user can delete a document version, the [DocumentVersion](DocumentVersion) contained in the [VersionHistory](VersionHistory) resource will advertise a link with @rel value of _delete_. To delete the document version, issue a DELETE request to the _delete_ URI.

### Example ###

#### Request ####
```
DELETE /documents/123/version/456 HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If successful, this method will return an empty response with an OK status code and a [link header](Link#Header) to the parent document and the [VersionHistory](VersionHistory) resource.

```
HTTP/1.1 200 OK
Link: </documents/123>;rel="parent",</documents/123/versions>;rel="version-history"
```


---


# Syntax #

## Example ##

```
<documentVersion title="..." description="...">
  <link rel="self" href="..." />
  <link rel="delete" href="..." />
  <link rel="content" href="..." />
    
  <actor name="Barry Potter" rel="last-edited-by">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
  </actor>
  
  <size>445645648</size>
  <version>2</version>
  <note>version note</note>
  <contentType>application/msword</contentType>
  <created>2011-02-15T09:43:17Z</created>
  <updated>2011-02-19T14:28:04Z</updated>
</documentVersion>
```

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this documentVersion. | GET         |
| delete   | Allows the user to delete the version. | DELETE      |
| content  | The content for this version of the document. | GET         |

## Schema ##

```
start = documentVersion

documentVersion = element h:documentVersion {
  attribute title {xsd:string},
  attribute description {xsd:string}?,
  link+,
  actor,
  element h:size {xsd:unsignedLong},
  element h:version {xsd:unsignedInt},
  element h:note {xsd:string}?,
  element h:contentType {xsd:string},
  element h:created {xsd:dateTime},
  element h:updated {xsd:dateTime}
}
```
