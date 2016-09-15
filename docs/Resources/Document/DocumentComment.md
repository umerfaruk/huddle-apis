# Summary #

A DocumentComment recourse represents a single comment from a DocumentComments collection.

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Deleting a comment |Beta        |

# Operations #

## Deleting a comment ##

The [comments](DocumentComments) resource will advertise a link with @rel value for each comment. To delete a given comment, send a DELETE request to the URI.

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
