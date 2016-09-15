# Summary #

The DocumentComments resource represents the collection of comments for a specific [document](Document) resource.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving document comments|Live        |
|Creating a new comment|Live        |

Also see "Mentioning Users and Teams within Comments" below.

# Operations #

## Retrieving document comments ##

The [document](Document) resource will advertise a link with @rel value of _comments_. To retrieve the document comments, GET the _comments_ URI which will return a DocumentComments resource.

### Example ###

#### Request ####
```
GET /documents/123/comments HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<documentComments xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="documents/123/comments" />
  <link rel="parent" href="documents/123" />
  <link rel="create" href="..." />
  
  <comments>
    <comment>
      <link rel="self" href="documents/123/comments/987" />
      <link rel="delete" href="documents/123/comments/987" />
      <content>This is a comment for the document</content>
      <created>2007-10-10T09:02:17Z</created>
      <version>12</version>

      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
    </comment>

    <comment>
      <link rel="self" href="documents/123/comments/986" />
      <content>This is another comment for the document</content>
      <created>2007-10-10T09:02:17Z</created>
      <version>12</version>

      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>     
    </comment>

  </comments>

</documentComments>
```

## Creating a new comment ##

It is possible to add comments to a [document](Document) resource. If the authenticated user is authorized to add comments to the document, the [DocumentComments](DocumentComments) resource will advertise a link with a @rel value of _create_. Posting to this URI will create the comment. See the example below.

### Example ###

#### Request ####
```
POST /documents/12345/comments HTTP/1.1
Host: api.huddle.net
Content-Type: application/vnd.huddle.data+xml
Content-Length: 102
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<comment>
   <content>This is a comment for the document</content>
   <replytoid>message_id</replytoid>
</comment>

```

#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| content  | The content of the comment. Must be less than 2049 characters. |
| replytoid| Optional : the id of the message you are replying to. |

#### Response ####

If successful, this method will return a 201 Created status code and a representation of the [comments](DocumentComments) resource for the relevant document.

```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
```
```
<documentComments xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="documents/123/comments" />
  <link rel="parent" href="documents/123" />
  <link rel="create" href="..." />

  <comments>

    <comment>
      <link rel="self" href="documents/123/comments/987" />
      <link rel="delete" href="documents/123/comments/987" />
      <content>This is a comment for the document</content>
      <created>2007-10-10T09:02:17Z</created>
      <version>12</version>

      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>
    </comment>

    <comment>
      <link rel="self" href="documents/123/comments/986" />
      <content>This is another comment for the document</content>
      <created>2007-10-10T09:02:17Z</created>
      <version>12</version>

      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>     
    </comment>

  </comments>

</documentComments>
```

### Mentioning Users and Teams within Comments [BETA]###

It is also possible to submit users, teams and everyone within a workspace to be notify embedded within the comment.  This is done by including valid markdown huddle user links of the following format:

```
* [User Name](https://<domain>.huddle.net/apigateway/users/<id>)
* [Team Name](https://<domain>.huddle.net/apigateway/teams/<id>)
* [Everyone](https://<domain>.huddle.net/workspace/<WorkspaceId>?teams=everyone)
```

Only users that have read access to the document will be notified of the comment.  The mentioned users will only receive be notified about this comment.  They will not be added to the subscribers list from this action.



### Example ###

#### Request ####
```
POST /documents/12345/comments HTTP/1.1
Host: api.huddle.net
Content-Type: application/vnd.huddle.data+xml
Content-Length: 102
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<comment>
   <content>
     Hey [John Smith](https://my.huddle.net/apigateway/users/111), This is a comment 
     for the document.  This will be owned by [Team A](https://my.huddle.net/apigateway/teams/222).
  </content>
</comment>

```

#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| content  | The content of the comment. Must be less than 2049 characters. |

#### Response ####

If successful, this method will return a 201 Created status code and a representation of the [comments](DocumentComments) resource for the relevant document.

Note that comments that are returned will not contain the links but will contain the link text.

```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+xml
```
```
<documentComments xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="documents/123/comments" />
  <link rel="parent" href="documents/123" />
  <link rel="create" href="..." />

  <comments>

    <comment>
      <link rel="self" href="documents/123/comments/987" />
      <link rel="delete" href="documents/123/comments/987" />
      <content>Hey John Smith, This is a comment for the document.  This will be owned by Team A.</content>
      <contentMarkdown>Hey [John Smith](https://my.huddle.net/apigateway/users/111), This is a comment for the document. This will be owned by [Team A]().
      </contentMarkdown>
      <created>2007-10-10T09:02:17Z</created>
      <version>12</version>

	  <actors>
		  <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
			<link rel="self" href="..." />
			<link rel="avatar" href="..." type="image/jpg" />
			<link rel="alternate" href="..." type="text/html" />
		  </actor>
	  </actors>
	  
	  <mentionedActors>
		<actor name="John Smith" email="john.smith@example.com">
			<link rel="self" href="..." />
			<link rel="avatar" href="..." type="image/jpg" />
			<link rel="alternate" href="..." type="text/html" />
		</actor>        
      </mentionedActors>
	  
    </comment>

    <comment>
      <link rel="self" href="documents/123/comments/986" />
      <content>This is another comment for the document</content>
      <created>2007-10-10T09:02:17Z</created>
      <version>12</version>

      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>     
    </comment>

  </comments>

</documentComments>
```

## Deleting a comment ##

It is possible to delete an individual comment via the [document comment](DocumentComment) resource. If the authenticated user is authorized to delete a comment, the [DocumentComments](DocumentComments) resource will advertise a link with a @rel value of _delete_. Deleting to this URI will delete the comment.

### Example ###

#### Request ####
```
DELETE /documents/123/comments/987 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 204 NO CONTENT
Content-Type: application/vnd.huddle.data+xml
```


---


# Syntax #

## Example ##

```
<documentComments xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="documents/123/comments" />
  <link rel="parent" href="documents/123" />
  <link rel="create" href="..." />

  <comments>

    <comment>
      <link rel="self" href="documents/123/comments/987" />
      <link rel="delete" href="documents/123/comments/987" />
      <content>This is a comment for the document</content>
      <created>2007-10-10T09:02:17Z</created>
      <version>12</version>

      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>
    </comment>

    <comment>
      <link rel="self" href="documents/123/comments/986" />
      <content>This is another comment for the document</content>
      <created>2007-10-10T09:02:17Z</created>
      <version>45</version>

      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>
	  
    </comment>

  </comments>

</documentComments>
```

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this DocumentComments resource. | GET         |
| parent   | Retrieves the [document](Document) that this resource belongs to. | GET         |
| create   | If the user has permission to create a new comment, they can do so at this URI. | POST        |

## Schema ##

```

start = documentComments

documentComments= element h:documentComments{
  link+,
  element h:comments{ comment* }
}

comment= element h:comment{
  actor,
  element h:created {xsd:dateTime},
  element h:content {xsd:string}
  element h:version {xsd:int}
}
```
