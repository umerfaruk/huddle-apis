# Summary #

Workspace calendars represent a collection of [tasks](Task) for a [workspace](Workspace).

## Beta warning ##

The calendar APIs are in Beta and differ in format to the other APIs documented here, documentation about how to use the calendar APIs can be found at the CalendarApi page.

# Operations #

## Retrieving a calendar ##

Each workspace includes a calendar that contains all the [Task](Task)s for that workspace. Query string params should be lowercase.

### Example ###

#### Request ####
```http
GET /v2/calendar/workspaces/123 HTTP/1.1
Content-Type: application/xml
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```http
HTTP/1.1 200 OK
Content-Type: application/xml
```

```xml
<WorkspaceCalendar xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Uri="...">
  <Workspace
    Id="..."
    Uri="...">
    <DisplayName>My workspace</DisplayName>
  </Workspace>
  <Events>
    <Task ... />
  </Events>
</WorkspaceCalendar>
```

## Retrieving a calendar (paged) ##
The paged calendar endpoint is the preferred way of retrieving tasks. It returns the same data as the non-paged endpoint.
In addition to URI parameters controlling paging and sort order, all filters from the non-paged endpoint are available.

| **Parameter** | **Default value** | **Additional notes** |
|:--------------|:------------------|:---------------------|
| `pagesize`    | `50`              | Minimum value: 0; Maximum value: 500 |
| `skipitems`   | `0`               |                      |
| `sort`        | `duedate,desc`    | Available fields: `title`, `startdate`, `duedate`, `status`; Available directions: `asc`, `desc` |

### Link relations ###
| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| next     | The URI of the next page, using sort order specified. | GET |
| prev     | The URI of the previous page, using sort order specified. | GET |

If there are more tasks than the current page size, the link collection will contain a `next` link.
If there is a previous page, the link collection will return a `prev` link.
Use these links to navigate between pages.

### Supported Filters ###
All filters are off by default.

| **Parameter**     | **Example**  |**Additional notes** |
| :---              | :---         | :---                |
| `assignedtousers` | `56,57`      | Should be a valid userIds. `none` can be used to find Tasks with no assignees. Should not be used in conjunction with `assignedtoteams` |
| `assignedtoteams` | `123,67`     | Finds Tasks assigned to Users in those teams. Should be a valid teamIds. Should not be used in conjunction with `assignedtousers` |
| `earliestduedate` | `2016-08-13` | Date format should be `YYYY-MM-DD` |
| `latestduedate`   | `2016-08-13` | Date format should be `YYYY-MM-DD` |
| `statuses`        | `InProgress` | Available statuses: `NotStarted`, `InProgress`, `Complete` |
| `customField`     | `123,A`      | A comma separated pair of `customFieldId,customFieldValue`. Can only filter based on one id,value pair. Custom field must belong to the workspace and be active. |

### Example ###

#### Request ####
```http
GET /tasks/workspaces/123/paged?pagesize=10&skipitems=100 HTTP/1.1
Content-Type: application/xml
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```xml
HTTP/1.1 200 OK
Content-Type: application/xml
```

```xml
<WorkspaceCalendar xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Uri="...">
  <links>
    <link rel="self" href=".../tasks/workspaces/123/paged?pagesize=10&skipitems=100" />
    <link rel="prev" href=".../tasks/workspaces/123/paged?pagesize=10&skipitems=90" />
    <link rel="next" href=".../tasks/workspaces/123/paged?pagesize=10&skipitems=110" />    
  <links>
  <Workspace
    Id="..."
    Uri="...">
    <DisplayName>My workspace</DisplayName>
  </Workspace>
  <Events>
    <Task ... />
  </Events>
</WorkspaceCalendar>
```

## Creating a task ##

This resource supports creating a new [Task](Task).

When [retrieving a workspace calendar](WorkspaceCalendar#Retrieving_a_calendar) the resource representation in the response will include a Uri attribute. To create a task a POST request should be made to the URI of the workspace calendar with a representation of the [task](Task) resource to be created.

### Example ###

#### Request ####
```http
POST /v2/calendar/workspaces/123 HTTP/1.1
Content-Type: application/xml
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<Task xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Id="0">
    <Status>NotStarted</Status>
    <DueDate>2011-05-11T00:00:00Z</DueDate>
    <PlannedStartDate>2011-05-10T00:00:00Z</PlannedStartDate>
    <CompletedDate xsi:nil="true" />
    <CompletedBy xsi:nil="true" />
    <CreatedDate xsi:nil="true" />
    <UpdatedDate xsi:nil="true" />
    <Title>A new test task</Title>
    <Description>Test description</Description>
</Task>
```

#### Response ####

If successful, this operation will return a [201 Created](HttpStatusCodes) with a [Location header](ResponseHeaders) pointing to your new [task](Task), and a representation of your new task in the response body.


---


# Syntax #

## Examples ##

#### A workspace calendar ####

```
IN PROGRESS
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
