# Summary #

Post a comment to a whiteboard.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Post a comment to a whiteboard|Beta        |

# Operations #

## Post a comment to a whiteboard ##

### Example ###

#### Request ####
```
POST /whiteboards/123/comments HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
x-notification-scope: Stakeholders
x-in-reply-to: 936ABD8E-2150-4D31-833A-4F2FCF612F70
```

```
<whiteboardComment>
   <text>This is a comment on a whiteboard</text>
</whiteboardComment>

```

#### Response ####
```
HTTP/1.1 200 OK
```



#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| content  | The content of the comment. Must be less than 2048 characters. |

#### Custom Headers ####

| **Name** | **Description** |
|:---------|:----------------|
| x-notification-scope| Optional, define the scope of the notification that will be sent for the comment. Possible values are : Stakeholders (owner, users who previously commented, users who received the notification that this comment is in response to), Contributors (owner, users who previously commented)  |
| x-in-reply-to| Mandatory if the x-notification-scope header has the value of stakeholders, id of the correlated message  |

#### Response ####

If successful, this method will return a 200 OK status code and an empty response.
