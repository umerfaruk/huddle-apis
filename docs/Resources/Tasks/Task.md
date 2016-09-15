# Summary #
# These apis are now deprecated #
**We are creating a new REST api for [tasks](tasks)**

Tasks represent work items to be completed by an arbitary set of assignees.

# Operations #

## Create a task ##

See [the workspace calendar](WorkspaceCalendar#Create_a_task) for information on creating tasks.

## Retrieve a task ##

When [retrieving a calendar](Calendar) the response will contain a list of tasks. Each task advertises a URI attribute that can be used to GET the details for that task.

### Example ###

#### Request ####
```
GET /v2/calendar/events/123 HTTP/1.1
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/xml
```
```
<?xml version="1.0" encoding="utf-8"?>
<Task xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Id="..." Uri="...">
  <Title>A new test task</Title>
  <Description>Test description</Description>
  <Status>NotStarted</Status>
  <DueDate>2011-05-10T00:00:00Z</DueDate>
  <PlannedStartDate>2011-05-09T00:00:00Z</PlannedStartDate>
  <Owner Id="..." Uri="...">
    <DisplayName>Peter Gibson</DisplayName>
    <Username>...</Username>
    <LogoPath>...</LogoPath>
    <Email>...</Email>
  </Owner>
  <Assignments />
  <CompletedDate xsi:nil="true" />
  <CompletedBy xsi:nil="true" />
  <CreatedDate>2011-05-05T09:48:35.0151996Z</CreatedDate>
  <UpdatedDate>2011-05-05T09:48:35.1577686Z</UpdatedDate>
  <Attachments />
  <Comments />
  <WorkspaceSummary Id="..." Uri="...">
    <DisplayName>Default Workspace</DisplayName>
  </WorkspaceSummary>
  <CustomFields />
</Task>
```

## Edit a task ##

To edit a task, first obtain the [URI for the task](Task#Properties) from a representation of the task. A PUT request can then be made to the task's URI with the new representation of the task in the body.

Workspace managers or task creators have full access to edit a task. An assignee can only change the status of a task.

### Example ###

#### Request ####

```
PUT /v2/calendar/events/123 HTTP/1.1
Content-Type: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<?xml version="1.0" encoding="utf-8"?>
<Task xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Id="..." Uri="...">
  <Title>A new test task</Title>
  <Description>Test description</Description>
  <Status>NotStarted</Status>
  <DueDate>2011-05-10T00:00:00Z</DueDate>
  <PlannedStartDate>2011-05-09T00:00:00Z</PlannedStartDate>
  <Assignments />
  <Attachments />
  <Comments />
  <CustomFields />
</Task>
```

#### Response ####

If successful, this operation will return a [200 OK](HttpStatusCodes), and a representation of the edited task in the response body.

## Add a Comment to a Task ##

**Note:** In time this method of adding a comment might move from a templated to a hypermedia approach, though for the time being the below will apply.

To add a comment to a task, first obtain the [URI for the task](Task#Properties) from a representation of the task, and append /comments to it to determine the Task's Comments URI. A POST request can then be made to the Comments URI with a representation of the new comment.

Any member of the workspace is allowed to add comments to the task.

### Example ###

#### Request ####

```
POST /v2/calendar/events/11368352/comments HTTP/1.1
Content-Type: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Host: api.huddle.dev
Content-Length: 4902
Expect: 100-continue
```
```
<?xml version="1.0" encoding="utf-8"?>
<Comment xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Id="0">
  <CreatedDate xsi:nil="true" />
  <Text>New comment text</Text>
</Comment>
```

#### Response ####

If successful, this operation will return a [201 CREATED](HttpStatusCodes), and a representation of the new comment in the response body.

## Delete a task ##

You can delete a task by issuing a DELETE request to the task URI

### Example ###

#### Request ####
```
DELETE /v2/calendar/events/123 HTTP/1.1
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If successful, this operation will return a [200 OK](HttpStatusCodes).

```
HTTP/1.1 200 OK
Content-Type: application/xml
```


---


# Syntax #

## Examples ##

#### A task ####

```
<Task Id="..." Uri="...">
  <Title>My Task</Title>
  <Description>My Description</Description>
  <Status>NotStarted</Status>
  <DueDate>2011-05-10T00:00:00</DueDate>
  <PlannedStartDate>2011-05-05T00:00:00</DueDate>
  <Owner Id="..." Uri="...">
    <DisplayName>Joe Walsh</DisplayName>
    <Username>...</Username>
    <LogoPath>...</LogoPath>
    <Email>...</Email>
  </Owner>
  <Assignments>
    <Assignment>
      <Assignee Id="..." Uri="...">
        <DisplayName>Joe Walsh</DisplayName>
        <Username>...</Username>
        <LogoPath>...</LogoPath>
        <Email>...</Email>
      </Assignee>
      <Assigner Id="..." Uri="...">
        <DisplayName>Joe Walsh</DisplayName>
        <Username>...</Username>
        <LogoPath>...</LogoPath>
        <Email>...</Email>
      </Assigner>
    </Assignment>
  </Assignments>
  <CompletedDate xsi:nil="true" />
  <CompletedBy xsi:nil="true" />
  <CreatedDate>2011-05-05T16:12:47</CreatedDate>
  <UpdatedDate>2011-05-05T16:12:47</UpdatedDate>
  <Attachments />
  <Comments>
    <Comment Id="..." Uri="...">
      <CreatedDate>2011-05-05T16:12:47</CreatedDate>
      <User Id="..." Uri="...">
        <DisplayName>Joe Walsh</DisplayName>
        <Username>...</Username>
        <LogoPath>...</LogoPath>
        <Email>...</Email>
      </User>
      <Text>This is the comment added to this task</Text>
    </Comment>
  </Comments>
  <WorkspaceSummary Id="..." Uri="...">
    <DisplayName>Default Workspace</DisplayName>
  </WorkspaceSummary>
  <CustomFields />
</Task>
```

#### A completed task ####

```
TO BE ADDED
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| @Id      | A unique identifier of the task. |
| @Uri     | The current URI of the task. |
| Title    | The title of the task. |
| Description | Description of the task. |
| Status   | The state of the task, one of NotStarted, InProgress or Complete. |
| DueDate  | The date when the task should be completed by. |
| PlannedStartDate | The date by which the task should be started. |
| Owner    | The [UserSummary](UserSummary) of the task creator. |
| Assignments | A collection containing the details of the assignments for the task. |
| Assignments.Assignment.Assignee | The [UserSummary](UserSummary) of the user assigned to complete the task. |
| Assignments.Assignment.Assigner | The [UserSummary](UserSummary) of the user who assigned the assignee to complete the task. |
| CompletedDate | The date the task was marked as complete. |
| CompletedBy | The [UserSummary](UserSummary) of the user who marked the task as complete. |
| CreatedDate | The date the task was created on. |
| UpdatedDate | The date the task was last updated. |
| Attachments | A collection detailing the documents attached to the task. |
| Comments | A collection containing the comments on the task. |
| Comments.Comment.@Id | The unique identifier for an individual comment. |
| Comments.Comment.@Uri | The URI of the comment. |
| Comments.Comment.CreatedDate | The date when the comment was added to the task. |
| Comments.Comment.User | The [UserSummary](UserSummary) of user who added the comment to the task. |
| Comments.Comment.Text | The comment that was added to the task. |
| WorkspaceSummary | The [WorkspaceSummary](WorkspaceSummary) of the workspace the task belongs to. |
| CustomFields | A collection containing the custom field values assigned to the task. |
