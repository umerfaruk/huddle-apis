# Summary #

Discussions represent threaded conversations on topics of interest to members of a workspace.

Discussions are a Classic API See [Classic](Classic)

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|List discussions in a workspace|Released    |
|Create a discussion|Released    |
|List posts in a discussion|Released    |
|Post to a discussion|Released    |
|Delete a discussion|Released    |

# Operations #

## List discussions in a workspace ##

Gets the discussion topics for a specific workspace.
### Method ###
GET

### URL Format ###
https://api.huddle.net/v1/json/workspaces/{workspace-id}/discussions

|{workspaceid}| The Id of the workspace to get the discussion topics for|
|:------------|:--------------------------------------------------------|

### Example Response ###
```
{"Data":[{
"Id":375313, 
"Created": {"Day":11, "Hour":10, "Minute":35, "Month":7, "Year":2008}, 
"CreatedByUserDisplayName":"Antonello Caboni", 
"CreatedByUserId":112093, 
"LastPost": {"Id":375314, "LastUpdatedByUserDisplayName":"Antonello Caboni", "LastUpdatedByUserId":112093, "Updated": {"Day":11, "Hour":10, "Minute":35, "Month":7, "Year":2008}},
"PostCount":1, 
"Title":"test"
}], 
"Error":null, 
"Success":true}
```

## Create a discussion ##

Creates a new discussion in a specific workspace
### Method ###
POST

### URL Format ###
https://api.huddle.net/v1/json/workspaces/{workspace-id}/discussions/create

|{workspaceid} |The Id of the workspace to get the discussion topics for |
|:-------------|:--------------------------------------------------------|

### Example Request Body ###
```
{"Data":{"Title":"Discussion title","Text":"First post text",[optional]"DontNotifyUsers":true}}
```
### Example Response ###
```
{"Data":{"Id":234567,"Uri":"/workspaces/123456/discussions/234567"}, "Error":null,"Success":true}
```

## List posts in a discussion ##

Gets the posts for a given discussion.
### Method ###
GET

### URL Format ###
https://api.huddle.net/v1/json/discussions/{discussion-id}?count=2&page=1

|{discussionid}| The Id of the workspace to get the discussion topics for|
|:-------------|:--------------------------------------------------------|
|{count}       |The maximum number of posts to return. By default all posts are returned.|
|{page}        | The result page to return. If count is not specified, this parameter is ignored.|

### Example Response ###
```
{"Data":[ 
{"AuthorId":130886, 
"AuthorName":"Syd", 
"Content":"HTML content here", 
"Created":{"Day":15,"Hour":10,"Minute":10,"Month":8,"Year":2008}, "Id":123456 }, 
{"AuthorId":130886, 
"AuthorName":"Jimi", 
"Content":"We miss you Syd!", 
"Created":{"Day":15,"Hour":10,"Minute":11,"Month":8,"Year":2008}, "Id":7890} ], 
"Error":null,
"Success":true}
```

## Post to a discussion ##
Adds a new post to a discussion.

Although a successful request will return a 201 Created response, the ID contained in the response is for the newly created discussion post. The URI is null.

### Method ###
POST

### URL Format ###
https://api.huddle.net/v1/json/workspaces/{workspace-id}/discussions/create

|{discussionid} |The id of the discussion you are replying to.|
|:--------------|:--------------------------------------------|

### Example Request Body ###
```
{"Data":{"Text":"First post text"}}
```
### Example Response ###
```
{"Data":{"Id":234567,"Uri":null}, "Error":null,"Success":true}
```

## Delete a discussion ##
Deletes a discussion.

### Method ###
POST

### URL Format ###
https://api.huddle.net/v1/json/discussions/{discussionId}/delete

|{discussionid} |The id of the discussion you are deleting|
|:--------------|:----------------------------------------|


### Example Response ###
```
{"Data":{"DeletedId":234567}, "Error":null,"Success":true}
```

## Error Codes ##
400: The id provided could not be recognised

401: Authentication error

403: You do not have permission to access the item requested

500: There was an error processing your request
