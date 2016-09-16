# Summary
Attachments are a way of associating a [Document](Document) with a [Task](FileRequest) and can be used to fufill a File Request.
## Alpha warning ##
The Task Attachments API is in Alpha

# Status #
| **Operation**                                                   | **Status** |
|:----------------------------------------------------------------|:-----------|
| [Retrieve an attachment](#retrieve-an-attachment) | Alpha |
| [Retrieve all attachments on a task](#retrieve-all-attachments-on-a-task) | Alpha |
| [Attach a document to a task](#attach-a-document-to-a-task) | Alpha |
| [Delete an attachment](#delete-an-attachment) | Alpha |


# Operations
## Retrieve an attachment
### Link relations
| **Name** | **Description**                                     | **Methods** |
|:---------|:----------------------------------------------------|:------------|
| `self`   | The URI of the Attachment.                |       `GET` |
| `parent` | The URI of the [Task](Task).                        |       `GET` |
| `delete` | The URI to delete the Attachment.               |      `DELETE` |
| `document` | The URI to the attached [Document](Document).               |      `GET` |
| `document-alternate` | The Web URL to the attached [Document](Document).               |      `GET` |
### Example
#### Request
```http
GET /tasks/12345/attachments/23 HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### JSON Response
```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```json
{
    "actors": [{
        "name": "Jimmy Snake",
        "email": "jimmy.snake@example.com",
        "rel": "owner",
        "links": [
            { "rel": "self", "href": ".." },
            { "rel": "avatar", "href": "..", "type": "img/jpeg" },
            { "rel": "alternate", "href": "..", "type": "text/html" }
        ]
    }],
    "createdDate": "Thu, 15 Sep 2016 12:38:28 GMT",
    "links": [
        { "rel": "self", "href": "..." },
        { "rel": "parent", "href": "..." },
        { "rel": "delete", "href": "..." },
        { "rel": "document", "href": "..." },
        { "rel": "document-alternate", "href": "...", "type": "text/html" }
    ]
}
```
#### XML Response
```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<attachment>
    <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
    </actor> 
    <createdDate>2007-10-10T09:02:17Z</createdDate>
    <link rel="self" href="..." />
    <link rel="parent" href="..." />
    <link rel="delete" href="..." />
    <link rel="document" href="..." />
    <link rel="document-alternate" href="..." type="text/html" />
</attachment>
```
### Error Responses ###

| **Case**                                              | **Response Code** |                **Error Code** |
|-------------------------------------------------------|-------------------|-------------------------------|
| Task does not exist                                   | `404 Not Found`   | `TaskNotFound`              |
| Task is deleted                                       | `404 Not Found`   | `TaskDeleted`              |
| User is not a member of the Workspace                 | `403 Forbidden`   | `WorkspaceMembershipRequired` |
| Workspace is locked                                   | `403 Forbidden`   | `WorkspaceLocked`             |
| Workspace is deleted                                  | `404 Not Found`   | `WorkspaceDeleted`              |

## Retrieve all attachments on a task
### Link relations
| **Name** | **Description**                                     | **Methods** |
|:---------|:----------------------------------------------------|:------------|
| `self`   | The URI of the Attachments on the Task.                |       `GET` |
| `parent` | The URI of the [Task](Task).                        |       `GET` |
| `create` | The URI to add an Attachment to the Task.               |      `POST` |
### Example
#### Request
```http
GET /tasks/12345/attachments HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
#### JSON Response
```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```json
{
    "links": [
        { "rel": "self", "href": "..." },
        { "rel": "parent", "href": "..." },
        { "rel": "create", "href": "..." } 
    ],
    "attachments": [{
            "actors": [{
                "name": "Jimmy Snake",
                "email": "jimmy.snake@example.com",
                "rel": "owner",
                "links": [
                    { "rel": "self", "href": ".." },
                    { "rel": "avatar", "href": "..", "type": "img/jpeg" },
                    { "rel": "alternate", "href": "..", "type": "text/html" }
                ]
            }],
            "createdDate": "Thu, 15 Sep 2016 12:38:28 GMT",
            "links": [
                { "rel": "self", "href": "..." },
                { "rel": "parent", "href": "..." },
                { "rel": "delete", "href": "..." },
                { "rel": "document", "href": "..." },
                { "rel": "document-alternate", "href": "...", "type": "text/html" }
            ]
    }]
}
```
#### XML Response
```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<attachments>
    <link rel="self" href=".." />
    <link rel="parent" href=".." />
    <link rel="create" href=".." />
    <items>
        <attachment>
            <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
                <link rel="self" href="..." />
                <link rel="avatar" href="..." type="image/jpg" />
                <link rel="alternate" href="..." type="text/html" />
            </actor> 
            <createdDate>2007-10-10T09:02:17Z</createdDate>
            <link rel="self" href="..." />
            <link rel="parent" href="..." />
            <link rel="delete" href="..." />
            <link rel="document" href="..." />
            <link rel="document-alternate" href="..." type="text/html" />
        </attachment>
    </items>
</attachments>
```
| **Case**                                              | **Response Code** |                **Error Code** |
|-------------------------------------------------------|-------------------|-------------------------------|
| Task does not exist                                   | `404 Not Found`   | `TaskNotFound`              |
| Task is deleted                                       | `404 Not Found`   | `TaskDeleted`              |
| User is not a member of the Workspace                 | `403 Forbidden`   | `WorkspaceMembershipRequired` |
| Workspace is locked                                   | `403 Forbidden`   | `WorkspaceLocked`             |
| Workspace is deleted                                  | `404 Not Found`   | `WorkspaceDeleted`              |

## Attach a document to a task
Currently only attaching a single Document at once is supported.
### Example
#### JSON Request
```http
POST /tasks/12345/attachments HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```json
[{
  "links":
    [{
      "href": "...",
      "rel": "document"
    }]
}]
```

#### XML Request
```http
POST /tasks/12345/attachments HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```xml
<attachments>
    <attachment>
        <link rel="document" href=".." />
    </attachment>
</attachments>

```

#### Response
If successful, this method will return an empty response with a Created status code and a `Location` header containing the URI of the created Attachment.
This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 201 Created
Location: /tasks/12345/attachments/1
```

### Error Responses ###

| **Case**                                              | **Response Code** |                **Error Code** |
|-------------------------------------------------------|-------------------|-------------------------------|
| Task does not exist                                   | `404 Not Found`   | `TaskNotFound`              |
| Task is deleted                                       | `404 Not Found`   | `TaskDeleted`              |
| Not a Document URI                                    | `400 Bad Request` | `BadRequest`                  |
| User is not a member of the Workspace                 | `403 Forbidden`   | `WorkspaceMembershipRequired` |
| Workspace is locked                                   | `403 Forbidden`   | `WorkspaceLocked`             |
| Workspace is archived                                 | `403 Forbidden`   | `WorkspaceArchived`           |
| Workspace is deleted                                  | `404 Not Found`   | `WorkspaceDeleted`              |

## Delete an attachment
### Example
#### Request
```http
DELETE /tasks/12345/attachments/23 HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response
If successful, this method will return an empty response with a No Content status code.
This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 204 No Content
```

### Error Responses ###

| **Case**                                              | **Response Code** |                **Error Code** |
|-------------------------------------------------------|-------------------|-------------------------------|
| Attachment does not exist                                   | `404 Not Found`   | `AttachmentNotFound`              |
| Attachment is deleted                                   | `404 Not Found`   | `AttachmentDeleted`              |
| Task does not exist                                   | `404 Not Found`   | `TaskNotFound`              |
| Task is deleted                                       | `404 Not Found`   | `TaskDeleted`              |
| User is not a member of the Workspace                 | `403 Forbidden`   | `WorkspaceMembershipRequired` |
| Workspace is locked                                   | `403 Forbidden`   | `WorkspaceLocked`             |
| Workspace is archived                                 | `403 Forbidden`   | `WorkspaceArchived`           |
| Workspace is deleted                                  | `404 Not Found`   | `WorkspaceDeleted`              |
