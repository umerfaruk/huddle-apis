# Summary #

Workspaces are collaboration spaces for people, files, and projects within Huddle. This classic API provides functionality for retrieving workspaces for users and their respective news feeds.

Workpspaces is a Classic API See [Classic](Classic) A newer Workspace API is available

|  |
|:-|

# Operations #

## Get News for Workspace ##
What's new feed items are generated when an item is created or modified in Huddle.net.
The **TargetId** is the unique Id of the modified resource.

The **TargetType** is the type of item that has been modified.

The **UserId** is the unique ID of the user who created or modified the resource.

The **Uri** is the Uri where the resource can be queried or retrieved. **If we do not yet support accessing a resource through the API, the value will be null.**

The **WorkspaceId** is the id of the workspace where the resource was modified. This property will be null when a user profile or comment section is updated.

By default, the last 10 news items are returned. To control the number of items returned, use the optional "count" parameter in the querystring as follows: https://api.huddle.net/v1/json/workspaces/{workspace-id}/whats-new?count=5 The maximum number of items returned is 50.

**No validation is performed on the count parameter, and we will default to 10 items if the parameter is missing or otherwise invalid. If the count parameter exceeds 50, we will return 50 items.
### URL Template ###
```
GET https://{host}/v1/json/workspaces/{workspace-id}/whats-new
```
### Example Response ###
```
{"Data": 
   [{"DateAdded":{"Day":7,"Hour":10,"Minute":52,"Month":8,"Year":2008},
   "Description":"test", 
   "DisplayName":"Test User for Whats new Ip Restriction test",
   "TargetId":382720, 
   "TargetText":"I should appear in tests", 
   "TargetType":"Whiteboard", 
   "Uri":null, 
   "UserId":382694, 
   "VersionNumber":1, 
   "WorkspaceId":382713, 
   "WorkspaceTitle":"Open workspace" }], 
"Error":null,"
Success":true}
```**

## Get Workspaces for User ##
Gets the workspaces for the authenticated user.
### URL Template ###
```
GET https://{host}/v1/json/workspaces
```
### Example Response ###
```
{"Data":
    [{ "Id":342758, 
     "Title":"My Workspace", 
     "StatusId":0, 
     "StatusValue":"Active", 
     "StatusDisplay":"Active", 
     "UserId":20563, 
     "Username":exampleuser, 
     "DisplayName":"Example User",    
     "LastAction":"\/Date(1225476059000+0000)\/"}],   
"Error":null,
"Success":true}
```
## Remove User From Workspace ##
Removes the authenticated user from a workspace
### URL Template ###
```
POST https://api.huddle.net/v1/json/workspaces/{workspaceId}/leave
```

{workspaceid} The workspace which the user wishes to leave

This operation completes in the context of the authenticated user
### Example Response ###
```
{"Data":{"RemovedId":234567}, "Error":null,"Success":true}
```
## Delete a workspace ##
Deletes a workspace
### URL Template ###
```
POST https://api.huddle.net/v1/json/workspaces/{workspaceId}/delete
```

{workspaceid} The workspace which the user wishes to leave

### Example Response ###
```
{"Data":{"DeletedId":234567}, "Error":null,"Success":true}
```
# Error Codes #
400: The id provided could not be recognised

401: Authentication error

403: You do not have permission to access the item requested

500: There was an error processing your request
