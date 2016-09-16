# Summary

## Alpha warning ##
The Task Audit Trail API is in Alpha

# Status #
| **Operation**                                                   | **Status** |
|:----------------------------------------------------------------|:-----------|
| [Retrieve the audit trail for a task](#retrieve-the-audit-trail-for-a-task) | Alpha |

# Operations

## Retrieve the audit trail for a task

### Link relations ###
| **Name** | **Description**                                     | **Methods** |
|:---------|:----------------------------------------------------|:------------|
| `self`   | The URI of the Audit Trail.                         |       `GET` |
| `parent` | The URI of the [Task](Task).                        |       `GET` |

### Example

#### Request
```http
GET /tasks/12345/audit-trail HTTP/1.1
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
<auditTrail>
  <link rel="self" href="..." />
  <link rel="parent" href="..." />
  <items>
    <auditEntry>
      [ fields tbd ]
      <actor name="Peter Gibson" email="peter.gibson@example.com">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>
    </auditEntry>
    ... more audit entries ...
  </items>
</auditTrail>
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
