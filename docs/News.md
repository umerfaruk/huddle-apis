# Summary #

This classic API provides functionality for retrieving news feed (activity) for users. 
The Workspaces API also provides functionality for retrieving news feeds for an individual workspace.

News is a [Classic API](Classic)

## Get News for User ##

What's new feed items are generated when an item is created or modified in Huddle.

The values returned are as follows:
* `DateAdded` - The date
* `Description` - Varies by activity type. Can be e.g. Comment text
* `DisplayName` - Usually the display name of the person who did the activity
* `Operation` - Varies by activity type. Can be e.g. "Create"
* `TargetId` - The ID of the created or modified resource.
* `TargetText` - Varies by activity type. Can be e.g. "Document Title" or "Meeting title"
* `TargetType` - The type of resource that has been modified. e.g. Document, Task, Meeting
* `UpdatedItemDataType` - Usually same as Target Type, but can be different e.g. for comments, their updated item type is `document` or `whiteboard` depending on where the comment was made.
* `Uri` - Only provided for documents. The API Uri where the resource can be queried or retrieved
* `UserAvatar` - The link to the avatar of the user who created or modified the resource.
* `UserId` - The unique ID of the user who created or modified the resource.
* `VersionNumber` - Only provided for documents. e.g. 1
* `WorkspaceId` - The id of the workspace where the resource was modified. This will be `null` for the news feed items: User Profile Updated, Comments Updated.
* `WorkspaceTitle` - The title of the workspace where the resource was modified. This will be `null` for the news feed items: User Profile Updated, Comments Updated.

By default, the last 10 news items are returned. To control the number of items returned, use the optional "count" parameter in the querystring as follows: https://api.huddle.net/v1/json/workspaces/{workspace-id}/whats-new?count=5 The maximum number of items returned is 50.

No validation is performed on the count parameter, and we will default to 10 items if the parameter is missing or otherwise invalid. If the count parameter exceeds 50, we will return 50 items.

## Example 
In this example we request the whats new feed for the current authenticated user across all their workspaces

### Request
```http
GET https://{host}/v1/json/whats-new HTTP/1.1
Accept: application/json
Authorization: Bearer theresamooseloose/abootthishoose
```

### Response
```http
HTTP/1.1 200 OK
Content-Type: application/json
```
```json
{
    "Data": [
        {
            "DateAdded": {
                "Day": 7,
                "Hour": 10,
                "Minute": 52,
                "Month": 8,
                "Year": 2008
            },
            "Description": "test",
            "DisplayName": "Test User for Whats new Ip Restriction test",
            "TargetId": 382720,
            "TargetText": "I should appear in tests",
            "TargetType": "Whiteboard",
            "UpdatedItemDataType": "whiteboard",
            "Uri": null,
            "UserId": 382694,
            "UserAvatar": "https://api.huddle.net/files/users/89789/avatar?h=hdj8907sd89fe7f89",
            "VersionNumber": 1,
            "WorkspaceId": 382713,
            "WorkspaceTitle": "Open workspace"
        }
    ],
    "Error": null,
    "Success": true
}
```
### Error Codes

401: Authentication Error

### Operation Types
* `Create`
* `Update`
* `Null`

### Target types
* Comment (either on Document or Whiteboard)
* Document (create or update)
* Task (create or update)
* Whiteboard (create or update)
* User (when added to workspace)
* DiscussionPost (when someone comments on discussion)
* Discussion (create or update)
* Meeting (create or update)