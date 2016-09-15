# Summary #

Tasks represent work items to be completed by an arbitary set of assignees.

This documentation is around the Classic API See [Classic](Classic) which has been retired for Tasks in favour of the functionality documented in [Task](Task)

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Get a list of Tasks|Retired     |
|Update Task    |Retired     |

# Operations #

## Get a list of Tasks ##

Gets a list of tasks. If you do not specify a workspace Id it will return the authenticated users tasks. You can filter this list using the status query string parameter. The possible values are: all|completed|late|upcoming (e.g. status=late), the default is all. If you specify the workspace id, it will return all the tasks for that workspace. You can filter the users returned using the users query string (e.g. users=123,456,789)

### Example Request ###
```
POST v1/json/tasks?workspaceId={45678}&status={late}&users={123,456,789} HTTP/1.1
Host: api.huddle.net
```
### Example Response ###
```
{ "Data" : [ 
    { "AssignedTo" : { "Users" : [ { "Id" : 123, "Name" : "james", "Team" : "Test team", "TeamId" : 123 } ] },
      "Attachment" : { "ApplicationUrl" : "/workspace/document/123?workspaceid=123&directoryid=123", "ApprovalUrl" : "/workspace/document/123?workspaceid=123&directoryid=123&action=approve", 
      "Description" : "", 
      "FileName" : "Notes", "Id" : 123, "Title" : "Notes", "Url" : "files/123" }, 
      "CompletedBy" : null, "CompletedDate " : null, 
      "CreatedDate" : { "Day" : 3, "FormattedDate" : "03/06/2009 10:26",     "Hour" : 10, "Minute" : 26, "Month" : 6, "Year" : 2009 }, 
      "Description" : "", 
      "DueDate" : { "Day" : 3, "FormattedDate" : "03/06/2009 00:00", "Hour" : 0, "Minute" : 0, "Month" : 6, "Year" : 2009 }, 
      "Id" : 123, "IsApproval" : false, "Status" : "Upcoming", 
      "Title" : "File Attachment", "Uri" : "/huddleworkspace/task.aspx?workspaceid=123&taskid=123", "WorkspaceId" : 123, 
      "WorkspaceName" :    "Tasks" } ], 
"Error" : null, 
"Success" : true 
}
```

## Update Task ##

The method allows the authenticated user to update the properties of the specified task. The only action that can be performed for now is marking a task as complete or not complete. The action can be performed only if the authenticated user has been assigned the task or is a workspace manager.

### URI Format ###
https://api.huddle.net/v1/json/tasks/{taskid}


{taskId}: The id of the task you are updating.

### Example Request ###
```
POST v1/json/tasks/12345 HTTP/1.1
Host: api.huddle.net
```
```
{"Complete":true}
```
### Example Response ###
```
{"Error":null,"Success":true}
```

## Error Codes ##
400: The id provided could not be recognised

401: Authentication error

403: You do not have permission to access the item requested

500: There was an error processing your request
