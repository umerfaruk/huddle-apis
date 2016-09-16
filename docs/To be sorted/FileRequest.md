# Summary
Tasks represent work items to be completed by an arbitrary set of assignees. A File Request is a Task that is linked to a Folder location.

## Alpha warning ##
The File Request API is in Alpha

# Status #
| **Operation**                                                   | **Status** |
|:----------------------------------------------------------------|:-----------|
| [Retrieve a file request](#retrieve-a-file-request)             |      Alpha |
| [Retrieve all file requests in a workspace](#retrieve-all-file-requests-in-a-workspace) | Alpha |
| [Retrieve all file requests for the current user](#retrieve-all-file-requests-for-the-current-user) | Alpha |
| [Create a file request](#create-a-file-request)                 |      Alpha |
| [Edit a file request](#edit-a-file-request)                     |      Alpha |
| [Delete a file request](#delete-a-file-request)                 |      Alpha |
| [Restore a file request](#restore-a-file-request)               |      Alpha |

# Operations

## Retrieve a file request

### Link relations ###
| **Name** | **Description**                                     | **Methods** |
|:---------|:----------------------------------------------------|:------------|
| `self`   | The URI of the Task.                                |       `GET` |
| `alternate` | The Web URI of the Task.                         |       `GET` |
| `parent` | The URI of the [Workspace](Workspace).              |       `GET` |
| `destination-folder` | The URI of the [Destination Folder](Folder). |  `GET` |
| `edit`   | The URI to edit the Task.                           |       `PUT` |
| `delete` | The URI to delete the Task.                         |    `DELETE` |
| `audit-trail` | The URI of the Task's audit trail.             |       `GET` |
| `comments` | The URI of the Task's comments.                 | `GET`, `POST` |
| `attachments` | The URI of the Task's attachments.           | `GET`, `POST` |
| `assignments` | The URI of the Task's assignments.           | `GET`, `POST` |

### Example

#### Request
```http
GET /tasks/12345 HTTP/1.1
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
<task title="A new task" type="FileRequest">
    <link rel="self" href="..."/>
    <link rel="alternate" href="..." type="text/html"/>
    <link rel="parent" href="..." title="..."/>
    <link rel="destination-folder" href="..."/>
    <link rel="edit" href="..."/>
    <link rel="delete" href="..."/>
    <link rel="audit-trail" href="..."/>
    <link rel="comments" href="..." count="2"/>
    <link rel="attachments" href="..." count="2"/>
    <link rel="assignments" href="..." count="2"/>
    <createdDate>2011-05-05T09:48:35.0151996Z</createdDate>
    <updatedDate>2011-05-05T09:48:35.1577686Z</updatedDate>
    <dueDate>2011-05-10T00:00:00Z</dueDate>
    <completedDate>2011-05-10T00:00:00Z</completedDate>
    <status>NotStarted</status>
    <actor name="Peter Gibson" email="peter.gibson@example.com" rel="owner">
        <link rel="self" href="..."/>
        <link rel="avatar" href="..." type="image/jpg"/>
        <link rel="alternate" href="..." type="text/html"/>
    </actor>
    <actor name="Barry Potter" email="barry.potter@example.com" rel="completed-by">
        <link rel="self" href="..."/>
        <link rel="avatar" href="..." type="image/jpg"/>
        <link rel="alternate" href="..." type="text/html"/>
    </actor>
    <workspace title="My workspace">
        <link rel="self" href="..."/>
        <link rel="alternate" href="..." type="text/html"/>
    </workspace>
</task>
```

### Error Responses ###

| **Case**                                              | **Response Code** |                  **Error Code** |
|-------------------------------------------------------|-------------------|---------------------------------|
| Task does not exist                                   |   `404 Not Found` |                `ObjectNotFound` |
| Task is deleted                                       |   `404 Not Found` |                `ObjectNotFound` |
| User is not a member of the Workspace                |   `403 Forbidden` |   `WorkspaceMembershipRequired` |
| Workspace is locked                                   |   `403 Forbidden` |               `WorkspaceLocked` |
| Workspace is archived                                 |   `403 Forbidden` |              `WorkspaceDeleted` |
| Workspace is deleted                                  | `400 Bad Request` |                    `BadRequest` |

---

## Retrieve all file requests in a workspace
This endpoints returns all the tasks in a [Workspace](Workspace).

### Filters ###
Filters are query string parameters that are used to filter the tasks in your workspace, e.g.
```http
GET /tasks/workspace/123?pagesize=50&skip=50&sort=title,asc&earliestduedate=2016-08-13&latestduedate=2016-09-01&statuses=NotStarted,Complete
```

| **Parameter** | **Default value**                     | **Additional notes** |
|:--------------|:--------------------------------------|:---------------------|
| `pagesize`    | `50`                  | Minimum value: 1; Maximum value: 500 |
| `skipitems`   | `0`                                   |                      |
| `sort`        | `duedate,desc` | Avalable fields: `title`, `startdate`, `duedate`, `status`; Available directions: `asc`, `desc` |
| `earliestduedate` | | Expected date format is `YYYY-MM-DD`, e.g. `2016-08-13` |
| `latestduedate` |   | Expected date format is `YYYY-MM-DD`, e.g. `2016-08-13` |
| `statuses`    | | Comma separated list of statuses. Available statuses: `NotStarted`, `InProgress`, `Complete` |

All filters are disabled by default.
If there are more tasks than the current page size, the link collection will contain a `next` link.
If there are one or more previous pages, the link collection will return a `prev` and a `first` link.
Use these links to navigate between pages.

### Link relations ###
| **Name** | **Description**                                     | **Methods** |
|:---------|:----------------------------------------------------|:------------|
| `self`   | The URI of the Task collection.                     |       `GET` |
| `parent` | The URI of the [Workspace](Workspace).              |       `GET` |
| `current`| The URI of the current page, using the specified sort order and filters. | `GET` |
| `first`  | The URI of the first page, using the specified sort order and filters. | `GET` |
| `next`   | The URI of the next page, using the specified sort order and filters. | `GET` |
| `prev`   | The URI of the previous page, using the specified sort order and filters. | `GET` |

### Example

#### Request
```http
GET /tasks/workspace/123 HTTP/1.1
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
<tasks>
    <link rel="self" href="..." />
    <link rel="parent" href="..."/>
    <link rel="current" href="..." />
    <link rel="first" href="..." />
    <link rel="prev" href="..." />
    <link rel="next" href="..." />
    <items>
        <task title="A new task" type="FileRequest">
            <link rel="self" href="..."/>
            ... other task elements ...
        </task>
    </items>
</tasks>
```

### Error Responses ###

| **Case**                                              | **Response Code** |                  **Error Code** |
|-------------------------------------------------------|-------------------|---------------------------------|
| Workspace does not exist                              |   `404 Not Found` |                `ObjectNotFound` |
| User is not a member of the Workspace                 |   `403 Forbidden` |   `WorkspaceMembershipRequired` |
| Workspace is locked                                   |   `403 Forbidden` |               `WorkspaceLocked` |
| Workspace is deleted                                  |   `404 Not Found` |                `ObjectNotFound` |

---

## Retrieve all file requests for the current user
This endpoints returns all the tasks visible to the current [User](User),
regardless of [Workspace](Workspace).

### Filters ###
Filters are query string parameters that are used to filter the tasks in your workspace, e.g.
```http
GET /tasks?pagesize=50&skip=50&sort=title,asc&earliestduedate=2016-08-13&latestduedate=2016-09-01&statuses=NotStarted,Complete
```

| **Parameter** | **Default value**                     | **Additional notes** |
|:--------------|:--------------------------------------|:---------------------|
| `pagesize`    | `50`                  | Minimum value: 1; Maximum value: 500 |
| `skipitems`   | `0`                                   |                      |
| `sort`        | `duedate,desc` | Avalable fields: `title`, `startdate`, `duedate`, `status`; Available directions: `asc`, `desc` |
| `earliestduedate` | | Expected date format is `YYYY-MM-DD`, e.g. `2016-08-13` |
| `latestduedate` |   | Expected date format is `YYYY-MM-DD`, e.g. `2016-08-13` |
| `statuses`    | | Comma separated list of statuses. Available statuses: `NotStarted`, `InProgress`, `Complete` |

All filters are disabled by default.
If there are more tasks than the current page size, the link collection will contain a `next` link.
If there are one or more previous pages, the link collection will return a `prev` and a `first` link.
Use these links to navigate between pages.

### Link relations ###
| **Name** | **Description**                                     | **Methods** |
|:---------|:----------------------------------------------------|:------------|
| `self`   | The URI of the Task collection.                     |       `GET` |
| `parent` | The URI of the [User](User).                        |       `GET` |
| `current`| The URI of the current page, using the specified sort order and filters. | `GET` |
| `first`  | The URI of the first page, using the specified sort order and filters. | `GET` |
| `next`   | The URI of the next page, using the specified sort order and filters. | `GET` |
| `prev`   | The URI of the previous page, using the specified sort order and filters. | `GET` |

### Example

#### Request
```http
GET /tasks HTTP/1.1
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
<tasks>
    <link rel="self" href="..." />
    <link rel="parent" href="..."/>
    <link rel="current" href="..." />
    <link rel="first" href="..." />
    <link rel="prev" href="..." />
    <link rel="next" href="..." />
    <items>
        <task title="A new task" type="FileRequest">
            <link rel="self" href="..."/>
            ... other task elements ...
        </task>
    </items>
</tasks>
```

---

## Create a file request

### Example

#### Request
```http
POST /tasks HTTP/1.1
Content-Type: application/json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```json
{
  "title": "A new task",
  "status": "NotStarted",
  "dueDate": "2011-05-10T00:00:00Z",
  "links": [
    {
      "rel": "destination-folder",
      "href": "..."
    }
  ],
  "workspace": {
    "links": [
      {
        "rel": "self",
        "href": "..."
      }
    ]
  }
}
```

#### Response
If successful, this method will return an empty response with a Created status code
and a `Location` header containing the URI of the created task.
This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 201 Created
Location: /tasks/12345
```

### Error Responses ###

| **Case**                                              | **Response Code** |                  **Error Code** |
|-------------------------------------------------------|-------------------|---------------------------------|
| Title is missing or empty                             | `400 Bad Request` |                    `BadRequest` |
| Status is missing or empty                            | `400 Bad Request` |                    `BadRequest` |
| Destination Folder URI is malformed                   | `400 Bad Request` |                    `BadRequest` |
| Workspace URI is malformed                            | `400 Bad Request` |                    `BadRequest` |
| Any date is malformed                                 | `400 Bad Request` |                    `BadRequest` |
| Workspace does not exist                              | `400 Bad Request` |                    `BadRequest` |
| User is not a member of the Workspace                 |   `403 Forbidden` |   `WorkspaceMembershipRequired` |
| Workspace is locked                                   |   `403 Forbidden` |               `WorkspaceLocked` |
| Workspace is archived                                 |   `403 Forbidden` |              `WorkspaceDeleted` |
| Workspace is deleted                                  | `400 Bad Request` |                    `BadRequest` |

---

## Edit a file request

### Example

#### Request
```http
PUT /tasks/12345 HTTP/1.1
Content-Type: application/json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```json
{
  "title": "A new task",
  "status": "InProgress",
  "dueDate": "Mon, 15 Jun 2009 20:45:30 GMT"
}
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
| Task does not exist                                   |   `404 Not Found` |                `ObjectNotFound` |
| Title is missing or empty                             | `400 Bad Request` |                    `BadRequest` |
| Status is missing or empty                            | `400 Bad Request` |                    `BadRequest` |
| Any date is malformed                                 | `400 Bad Request` |                    `BadRequest` |
| User is not a member of the Workspace                 |   `403 Forbidden` |   `WorkspaceMembershipRequired` |
| Workspace is locked                                   |   `403 Forbidden` |               `WorkspaceLocked` |
| Workspace is archived                                 |   `403 Forbidden` |             `WorkspaceArchived` |
| Workspace is deleted                                  |   `404 Not Found` |                `ObjectNotFound` |

---

## Delete a file request

### Example

#### Request
```http
DELETE /tasks/12345 HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response
If successful, this method will return an empty response with a No Content status code and a [link header](Link#header) to restore the deleted Task.
This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 204 No Content
Link: <...>;rel="restore"
```

### Error Responses ###

| **Case**                                              | **Response Code** |                  **Error Code** |
|-------------------------------------------------------|-------------------|---------------------------------|
| Task does not exist                                   |   `404 Not Found` |                `ObjectNotFound` |
| Task is deleted                                       |   `404 Not Found` |                `ObjectNotFound` |
| User is not a member of the Workspace                 |   `403 Forbidden` |   `WorkspaceMembershipRequired` |
| Workspace is locked                                   |   `403 Forbidden` |               `WorkspaceLocked` |
| Workspace is archived                                 |   `403 Forbidden` |             `WorkspaceArchived` |
| Workspace is deleted                                  | `400 Bad Request` |                    `BadRequest` |

---

## Restore a file request

### Example

#### Request
```http
DELETE /tasks/12345/restore HTTP/1.1
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
| Task does not exist                                   |   `404 Not Found` |                `ObjectNotFound` |
| Task is not deleted                                   | `400 Bad Request` |                    `BadRequest` |
| User is not a member of the Workspace                 |   `403 Forbidden` |   `WorkspaceMembershipRequired` |
| Workspace is locked                                   |   `403 Forbidden` |               `WorkspaceLocked` |
| Workspace is archived                                 |   `403 Forbidden` |             `WorkspaceArchived` |
| Workspace is deleted                                  | `400 Bad Request` |                    `BadRequest` |
