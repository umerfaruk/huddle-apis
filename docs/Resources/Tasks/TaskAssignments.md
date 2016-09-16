# Summary
Manage Assignment of [Task](FileRequest) to [User](User) so that File Request users can .

## Alpha warning ##
The Task Assignment API is in Alpha


# Status #
| **Operation**                                                   | **Status** |
|:----------------------------------------------------------------|:-----------|
| [Retrieve assignments for a task](#retrieve-assignments-for-a-task) | Alpha |
| [Add assignee(s) to a task](#add-assignees-to-a-task) | Alpha |
| [Retrieve an assignment](#retrieve-an-assignment) | Alpha |
| [Un-assign task from user](#un-assign-task-from-user) | Alpha |


# Operations

## Retrieve assignments for a task

### Link relations
| **Name** | **Description**                                     | **Methods** |
|:---------|:----------------------------------------------------|:------------|
| `self`   | The URI of the Assignment on the Task.              |       `GET` |
| `parent` | The URI of the [Task](Task).                        |       `GET` |
| `create` | The URI to add an Assignment to the Task.           |      `POST` |

### Example
#### Request
```http
GET /tasks/12345/assignments HTTP/1.1
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
    "assignments": [{
        "links": [
            { "rel": "self", "href": "..." },
            { "rel": "parent", "href": "..." },
            { "rel": "delete", "href": "..." }
        ],
        "createdDate": "Thu, 15 Sep 2016 12:38:28 GMT",
        "actors": [{
            "name": "Jimmy Snake",
            "email": "jimmy.snake@example.com",
            "rel": "assignee",
            "links": [
                { "rel": "self", "href": ".." },
                { "rel": "avatar", "href": "..", "type": "img/jpeg" },
                { "rel": "alternate", "href": "..", "type": "text/html" }
            ]
        },{
            "name": "John Dow",
            "email": "john.doe@example.com",
            "rel": "assigner",
            "links": [
                { "rel": "self", "href": ".." },
                { "rel": "avatar", "href": "..", "type": "img/jpeg" },
                { "rel": "alternate", "href": "..", "type": "text/html" }
            ]
        }]
    }]
}
```
#### XML Response
```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<assignments>
    <link rel="self" href=".." />
    <link rel="parent" href=".." />
    <link rel="create" href=".." />
    <items>
        <assignment>
            <link rel="self" href="..." />
            <link rel="parent" href="..." />
            <link rel="delete" href="..." />
            <createdDate>2007-10-10T09:02:17Z</createdDate>
            <actor name="Peter Gibson" email="peter.gibson@example.com" rel="assignee">
                <link rel="self" href="..." />
                <link rel="avatar" href="..." type="image/jpg" />
                <link rel="alternate" href="..." type="text/html" />
            </actor>
            <actor name="JOhn Dow" email="john.doe@example.com" rel="assigner">
                <link rel="self" href="..." />
                <link rel="avatar" href="..." type="image/jpg" />
                <link rel="alternate" href="..." type="text/html" />
            </actor> 
        </assignment>
    </items>
</assignments>
```
| **Case**                                              | **Response Code** |                **Error Code** |
|-------------------------------------------------------|-------------------|-------------------------------|
| Task does not exist                                   | `404 Not Found`   | `TaskNotFound`              |
| Task is deleted                                       | `404 Not Found`   | `TaskDeleted`              |
| User is not a member of the Workspace                 | `403 Forbidden`   | `WorkspaceMembershipRequired` |
| Workspace is locked                                   | `403 Forbidden`   | `WorkspaceLocked`             |
| Workspace is deleted                                  | `404 Not Found`   | `WorkspaceDeleted`              |


## Add assignees to a task

Adds one or more assignees to the  task
### Example
#### JSON Request
```http
POST /tasks/12345/assignments HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```json
[{
  "links":
    [{
      "href": "...",
      "rel": "user"
    },{
      "href": "...",
      "rel": "user"
    }]
}]
```

#### XML Request
```http
POST /tasks/12345/assignments HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```xml
<assignments>
    <assignment>
        <link rel="user" href=".." />
    </assignment>
</assignments>

```

#### Response
If successful, this method will return an empty response with a Created status code and a `Location` header containing the URI of the created assignment if only one assignment is created. If more than one assignments are created `Location` header contains link to the task assignments.
This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

##### When single assignment is made
```http
HTTP/1.1 201 Created
Location: /tasks/12345/assignments/1
```

##### When multiple assignments is made
```http
HTTP/1.1 201 Created
Location: /tasks/12345/assignments
```

### Error Responses ###

| **Case**                                              | **Response Code** |                **Error Code** |
|-------------------------------------------------------|-------------------|-------------------------------|
| Task does not exist                                   | `404 Not Found`   | `TaskNotFound`              |
| Task is deleted                                       | `404 Not Found`   | `TaskDeleted`              |
| Not a user URI                                        | `400 Bad Request` | `BadRequest`                  |
| User is not a member of the Workspace                 | `403 Forbidden`   | `WorkspaceMembershipRequired` |
| Workspace is locked                                   | `403 Forbidden`   | `WorkspaceLocked`             |
| Workspace is archived                                 | `403 Forbidden`   | `WorkspaceArchived`           |
| Workspace is deleted                                  | `404 Not Found`   | `WorkspaceDeleted`              |

## Retrieve an assignment

### Link relations
| **Name** | **Description**                                     | **Methods** |
|:---------|:----------------------------------------------------|:------------|
| `self`   | The URI of the Asssignment.                         |       `GET` |
| `parent` | The URI of the [Task](Task).                        |       `GET` |
| `delete` | The URI to delete the Assignment.                   |    `DELETE` |

### Example
#### Request
```http
GET /tasks/12345/assignment/23 HTTP/1.1
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
        { "rel": "delete", "href": "..." }
    ],
    "createdDate": "Thu, 15 Sep 2016 12:38:28 GMT",
    "actors": [{
        "name": "Jimmy Snake",
        "email": "jimmy.snake@example.com",
        "rel": "assignee",
        "links": [
            { "rel": "self", "href": ".." },
            { "rel": "avatar", "href": "..", "type": "img/jpeg" },
            { "rel": "alternate", "href": "..", "type": "text/html" }
        ]
    },{
        "name": "John Dow",
        "email": "john.doe@example.com",
        "rel": "assigner",
        "links": [
            { "rel": "self", "href": ".." },
            { "rel": "avatar", "href": "..", "type": "img/jpeg" },
            { "rel": "alternate", "href": "..", "type": "text/html" }
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
<assignment>
    <link rel="self" href="..." />
    <link rel="parent" href="..." />
    <link rel="delete" href="..." />
    <createdDate>2007-10-10T09:02:17Z</createdDate>
    <actor name="Peter Gibson" email="peter.gibson@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
    </actor>
    <actor name="JOhn Dow" email="john.doe@example.com" rel="assigner">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
    </actor> 
</assignment>
```
### Error Responses ###

| **Case**                                              | **Response Code** |                **Error Code** |
|-------------------------------------------------------|-------------------|-------------------------------|
| Task does not exist                                   | `404 Not Found`   | `TaskNotFound`                |
| Assignment does not exist                             | `404 Not Found`   | `AssignmentNotFound`          |
| Task is deleted                                       | `404 Not Found`   | `TaskDeleted`                 |
| Assignment is deleted                                 | `404 Not Found`   | `AssignmentDeleted`           |
| User is not a member of the Workspace                 | `403 Forbidden`   | `WorkspaceMembershipRequired` |
| Workspace is locked                                   | `403 Forbidden`   | `WorkspaceLocked`             |
| Workspace is deleted                                  | `404 Not Found`   | `WorkspaceDeleted`            |


## Un-assign task from user

### Example
#### Request
```http
DELETE /tasks/12345/assignment/23 HTTP/1.1
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
| Assignment does not exist                                   | `404 Not Found`   | `AssignmentNotFound`              |
| assignment is deleted                                   | `404 Not Found`   | `AssignmentDeleted`              |
| Task does not exist                                   | `404 Not Found`   | `TaskNotFound`              |
| Task is deleted                                       | `404 Not Found`   | `TaskDeleted`              |
| User is not a member of the Workspace                 | `403 Forbidden`   | `WorkspaceMembershipRequired` |
| Workspace is locked                                   | `403 Forbidden`   | `WorkspaceLocked`             |
| Workspace is archived                                 | `403 Forbidden`   | `WorkspaceArchived`           |
| Workspace is deleted                                  | `404 Not Found`   | `WorkspaceDeleted`              |