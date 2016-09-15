# Summary #

Post to a discussion.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Post to a discussion|Beta        |

# Operations #

## Post to a discussion ##

### Example ###

#### Request ####
```
POST /dicussions/123/posts HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
x-notification-scope: Stakeholders
x-in-reply-to: 936ABD8E-2150-4D31-833A-4F2FCF612F70
```

```
<discussionPost>
   <text>This is a post in a discussion</text>
</discussionPost>

```

#### Response ####
```
HTTP/1.1 200 OK
```



#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| content  | The content of the post. Must be less than 262.145 characters. |

#### Custom Headers ####

| **Name** | **Description** |
|:---------|:----------------|
| x-notification-scope| Optional, define the scope of the notification that will be sent for the comment. Possible values are : Stakeholders (owner, users who previously commented, users who were involved in the discussion), Contributors (owner, users who previously commented)  |
| x-in-reply-to| Mandatory if the x-notification-scope header has the value of stakeholders, id of the correlated message  |

#### Response ####

If successful, this method will return a 200 OK status code and an empty response.
