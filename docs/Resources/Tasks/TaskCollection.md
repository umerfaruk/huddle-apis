# Summary

## Alpha warning ##
The Task Comments API is in Alpha

# Status #
| **Operation**                                                   | **Status** |
|:----------------------------------------------------------------|:-----------|
| [Retrieve a comment](#retrieve-a-comment)                       |      Alpha |
| [Retrieve all comments on a task](#retrieve-all-comments-on-a-task) |  Alpha |
| [Add a comment to a task](#add-a-comment-to-a-task)             |      Alpha |
| [Delete a comment](#delete-a-comment)                           |      Alpha |

# Operations

## Retrieve a comment

### Link relations ###
| **Name** | **Description**                                     | **Methods** |
|:---------|:----------------------------------------------------|:------------|
| `self`   | The URI of the Comment.                             |       `GET` |
| `parent` | The URI of the [Task](Task).                        |       `GET` |
| `delete` | The URI to delete the Comment.                      |    `DELETE` |

### Example

#### Request
```http
GET /tasks/12345/comments/456 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response
If successful, this method will return a response with an OK status code.
This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<comment>
  <link rel="self" href="..." />
  <link rel="parent" href="..." />
  <link rel="delete" href="..." />
  <content>This is a comment for the task.</content>
  <created>2007-10-10T09:02:17Z</created>
  <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>  
</comment>
```

### Error Responses ###

| **Case**                                              | **Response Code** |                  **Error Code** |
|-------------------------------------------------------|-------------------|---------------------------------|
| Comment does not exist                                |   `404 Not Found` |                `ObjectNotFound` |
| Comment is deleted                                    |   `404 Not Found` |                `ObjectNotFound` |
| Task does not exist                                   |   `404 Not Found` |                `ObjectNotFound` |
| Task is deleted                                       |   `404 Not Found` |                `ObjectNotFound` |
| User is not a member of the Workspace                 |   `403 Forbidden` |   `WorkspaceMembershipRequired` |
| Workspace is locked                                   |   `403 Forbidden` |               `WorkspaceLocked` |
| Workspace is archived                                 |   `403 Forbidden` |              `WorkspaceDeleted` |
| Workspace is deleted                                  | `400 Bad Request` |                    `BadRequest` |

---

## Retrieve all comments on a task

### Link relations ###
| **Name** | **Description**                                     | **Methods** |
|:---------|:----------------------------------------------------|:------------|
| `self`   | The URI of the Comments on the Task.                |       `GET` |
| `parent` | The URI of the [Task](Task).                        |       `GET` |
| `create` | The URI to add a Comment to the Task.               |      `POST` |

### Example

#### Request
```http
GET /tasks/12345/comments HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response
If successful, this method will return a response with an OK status code.
This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<comments>
  <link rel="self" href="..." />
  <link rel="parent" href="..." />
  <link rel="create" href="..." />
  <items>
    <comment>
      <link rel="self" href="..."/>
      ... other comment elemts ...
    </comment>
  </items>
</comments>
```

### Error Responses ###

| **Case**                                              | **Response Code** |                  **Error Code** |
|-------------------------------------------------------|-------------------|---------------------------------|
| Task does not exist                                   |   `404 Not Found` |                `ObjectNotFound` |
| Task is deleted                                       |   `404 Not Found` |                `ObjectNotFound` |
| User is not a member of the Workspace                 |   `403 Forbidden` |   `WorkspaceMembershipRequired` |
| Workspace is locked                                   |   `403 Forbidden` |               `WorkspaceLocked` |
| Workspace is archived                                 |   `403 Forbidden` |              `WorkspaceDeleted` |
| Workspace is deleted                                  | `400 Bad Request` |                    `BadRequest` |

---

## Add a comment to a task

### Example

#### Request
```http
POST /tasks/12345/comments HTTP/1.1
Content-Type: application/json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```json
{
  "content": "This is a comment for the task.",
}
```

#### Response
If successful, this method will return an empty response with a Created status code
and a `Location` header containing the URI of the created comment.
This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 201 Created
Location: /tasks/12345/comments/456
```

### Error Responses ###

| **Case**                                              | **Response Code** |                  **Error Code** |
|-------------------------------------------------------|-------------------|---------------------------------|
| Content is missing or empty                           | `400 Bad Request` |                    `BadRequest` |
| Task does not exist                                   |   `404 Not Found` |                `ObjectNotFound` |
| Task is deleted                                       |   `404 Not Found` |                `ObjectNotFound` |
| Workspace does not exist                              | `400 Bad Request` |                    `BadRequest` |
| User is not a member of the Workspace                 |   `403 Forbidden` |   `WorkspaceMembershipRequired` |
| Workspace is locked                                   |   `403 Forbidden` |               `WorkspaceLocked` |
| Workspace is archived                                 |   `403 Forbidden` |              `WorkspaceDeleted` |
| Workspace is deleted                                  | `400 Bad Request` |                    `BadRequest` |

---

## Delete a comment

### Example

#### Request
```http
DELETE /tasks/12345/comments/456 HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response
If successful, this method will return an empty response with a No Content status code.
This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 204 No Content
```

### Error Responses ###

| **Case**                                              | **Response Code** |                  **Error Code** |
|-------------------------------------------------------|-------------------|---------------------------------|
| Comment does not exist                                |   `404 Not Found` |                `ObjectNotFound` |
| Comment is deleted                                    |   `404 Not Found` |                `ObjectNotFound` |
| Task does not exist                                   |   `404 Not Found` |                `ObjectNotFound` |
| Task is deleted                                       |   `404 Not Found` |                `ObjectNotFound` |
| Workspace does not exist                              | `400 Bad Request` |                    `BadRequest` |
| User is not a member of the Workspace                 |   `403 Forbidden` |   `WorkspaceMembershipRequired` |
| Workspace is locked                                   |   `403 Forbidden` |               `WorkspaceLocked` |
| Workspace is archived                                 |   `403 Forbidden` |              `WorkspaceDeleted` |
| Workspace is deleted                                  | `400 Bad Request` |                    `BadRequest` |
