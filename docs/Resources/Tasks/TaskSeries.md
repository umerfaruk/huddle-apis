# Summary #

Task series represent groups of related tasks. The create task series operation provides a convenient means by which a number of tasks each with start dates differing by a specified interval can be created at the same time. The delete task series operation provides a convenient way to delete all the tasks that were created as part of a series.

When creating tasks by using the task series API endpoint, all tasks will share the same title, description, status, owner, assignments and completed information: the only difference will be that the start dates of the individual tasks will be staggered according to a specified recurrence frequency (daily, every weekday, weekly or monthly). The number of tasks created is determined by a series end date value: for example, specifying weekly tasks until a date 3 months from now will create one task per week between the specified start date and the date three months from now.

Once a task series has been created, the tasks may be updated individually and behave as if they were created as individual tasks, with the exception that they may all be deleted by a single delete task series operation (they may also be deleted individually as normal tasks).

Please note that when creating task series, it's not possible to specify a due date: tasks created as part of a series necessarily have no due date at the time of creation (though due dates may be added to tasks individually once they've been created).

# Operations #

## Create a task series ##

This resource supports creating a new [TaskSeries](TaskSeries).

When [retrieving a workspace calendar](WorkspaceCalendar#Retrieving_a_calendar) the resource representation in the response will include a Uri attribute. To create a task series a POST request should be made to the URI of the workspace calendar with a representation of the [task series](TaskSeries) resource to be created.

### Example ###

#### Request ####
```
POST /v2/calendar/workspaces/123 HTTP/1.1
Content-Type: application/xml
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
<?xml version="1.0" encoding="utf-8"?>
<TaskSeries>
    <Status>NotStarted</Status>
    <PlannedStartDate>2013-08-10T00:00:00Z</PlannedStartDate>
    <EndDate>2013-08-24T00:00:00Z</EndDate>
    <CompletedDate xsi:nil="true" />
    <CompletedBy xsi:nil="true" />
    <CreatedDate xsi:nil="true" />
    <UpdatedDate xsi:nil="true" />
    <Title>Distribute weekly status report</Title>
    <Description>Test description</Description>
    <RecurrenceFrequency>Weekly</RecurrenceFrequency>
</TaskSeries>
```

#### Response ####

If successful, this operation will return a [201 Created](HttpStatusCodes) with a [Location header](ResponseHeaders) pointing to the first-occurring of the new tasks, and a representation of all newly-created tasks in the response body.

```
<TaskSeries id="987" uri="/v2/calendar/eventseries/987">
  <Task
    Id="20652"
    Uri="/v2/calendar/events/20652">
    <Title>Distribute weekly status report</Title>
    <Description>Test description</Description>
    <Status>NotStarted</Status>
    <DueDate
      xsi:nil="true" />
    <PlannedStartDate>2013-08-10T00:00:00Z</PlannedStartDate>
    <Owner
      Id="446048"
      Uri="/users/446048">
      <DisplayName>Joe Walsh</DisplayName>
      <Username>joe.walsh@myco.com</Username>
      <LogoPath>/images/logos/avatar.jpg</LogoPath>
      <Email>joe.walsh@myco.com</Email>
    </Owner>
    <Assignments />
    <CompletedDate
      xsi:nil="true" />
    <CompletedBy
      xsi:nil="true" />
    <CreatedDate>2013-09-03T13:43:42.0588214Z</CreatedDate>
    <UpdatedDate>2013-09-03T13:43:42.0588214Z</UpdatedDate>
    <Attachments />
    <Comments />
    <WorkspaceSummary
      Id="446057"
      Uri="/workspaces/446057">
      <DisplayName>Default Workspace</DisplayName>
    </WorkspaceSummary>
    <CustomFields />
    <SeriesId>33</SeriesId>
  </Task>
  <Task
    Id="20653"
    Uri="/v2/calendar/events/20653">
    <Title>Distribute weekly status report</Title>
    <Description>Test description</Description>
    <Status>NotStarted</Status>
    <DueDate
      xsi:nil="true" />
    <PlannedStartDate>2013-08-17T00:00:00Z</PlannedStartDate>
    <Owner
      Id="446048"
      Uri="/users/446048">
      <DisplayName>Joe Walsh</DisplayName>
      <Username>joe.walsh@myco.com</Username>
      <LogoPath>/images/logos/avatar.jpg</LogoPath>
      <Email>joe.walsh@myco.com</Email>
    </Owner>
    <Assignments />
    <CompletedDate
      xsi:nil="true" />
    <CompletedBy
      xsi:nil="true" />
    <CreatedDate>2013-09-03T13:43:42.0588214Z</CreatedDate>
    <UpdatedDate>2013-09-03T13:43:42.0588214Z</UpdatedDate>
    <Attachments />
    <Comments />
    <WorkspaceSummary
      Id="446057"
      Uri="/workspaces/446057">
      <DisplayName>Default Workspace</DisplayName>
    </WorkspaceSummary>
    <CustomFields />
    <SeriesId>33</SeriesId>
  </Task>
  <Task
    Id="20654"
    Uri="/v2/calendar/events/20654">
    <Title>Distribute weekly status report</Title>
    <Description>Test description</Description>
    <Status>NotStarted</Status>
    <DueDate
      xsi:nil="true" />
    <PlannedStartDate>2013-08-24T00:00:00Z</PlannedStartDate>
    <Owner
      Id="446048"
      Uri="/users/446048">
      <DisplayName>Joe Walsh</DisplayName>
      <Username>joe.walsh@myco.com</Username>
      <LogoPath>/images/logos/avatar.jpg</LogoPath>
      <Email>joe.walsh@myco.com</Email>
    </Owner>
    <Assignments />
    <CompletedDate
      xsi:nil="true" />
    <CompletedBy
      xsi:nil="true" />
    <CreatedDate>2013-09-03T13:43:42.0588214Z</CreatedDate>
    <UpdatedDate>2013-09-03T13:43:42.0588214Z</UpdatedDate>
    <Attachments />
    <Comments />
    <WorkspaceSummary
      Id="446057"
      Uri="/workspaces/446057">
      <DisplayName>Default Workspace</DisplayName>
    </WorkspaceSummary>
    <CustomFields />
    <SeriesId>33</SeriesId>
  </Task>    
</TaskSeries>
```

## Delete a task series ##

You can delete a task series by issuing a DELETE request to a task series URI. The task series URI is indicated by the _uri_ attribute of the _TaskSeries_ element that is returned upon successful creation of a task series (see above response for an example).

### Example ###

#### Request ####
```
DELETE /v2/calendar/eventseries/987 HTTP/1.1
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If successful, this operation will return a [200 OK](HttpStatusCodes).

```
HTTP/1.1 200 OK
Content-Type: application/xml
```


## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| @Id      | A unique identifier of the task or task series. |
| @Uri     | The current URI of the task or task series. |
| Title    | The title of all tasks in a series. |
| Description | Description of all tasks in a series. |
| Status   | The state of the tasks, one of NotStarted, InProgress or Complete. |
| PlannedStartDate | The date by which the earliest task should be started. |
| RecurrenceFrequency | The frequency of the recurring tasks |
| Owner    | The [UserSummary](UserSummary) of the creator of the tasks. |
| Assignments | A collection containing the details of the assignments for the tasks. |
| Assignments.Assignment.Assignee | The [UserSummary](UserSummary) of the user assigned to complete the tasks. |
| Assignments.Assignment.Assigner | The [UserSummary](UserSummary) of the user who assigned the assignee to complete the tasks. |
| CompletedDate | The date the tasks were marked as complete. |
| CompletedBy | The [UserSummary](UserSummary) of the user who marked the tasks as complete. |
