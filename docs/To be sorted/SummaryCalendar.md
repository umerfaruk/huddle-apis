# Summary #

A user's summary calendar represents a collection of all the [tasks](Task) a user has access to across all their [workspaces](Workspace).

## Beta warning ##

The calendar APIs are in Beta and differ in format to the other APIs documented here, documentation about how to use the calendar APIs can be found at the CalendarApi page.

# Operations #

## Retrieving the summary calendar for a user ##
All query string params must be lowercase.

### Example ###

#### Request ####
```http
GET /v2/calendar/workspaces/all HTTP/1.1
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
<SummaryCalendar Uri="..." AltUri="..." xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Events>
    <Task ... />
  </Events>
</SummaryCalendar>
```


## Retrieving the summary calendar for a user (paged) ##
The paged calendar endpoint is the preferred way of retrieving tasks. It returns the same data as the non-paged endpoint.
In addition to URI parameters controlling paging and sort order, all filters from the non-paged endpoint are available.

| **Parameter** | **Default value** | **Additional notes** |
|:--------------|:------------------|:---------------------|
| `pagesize`    | `50`              | Minimum value: 0; Maximum value: 500 |
| `skipitems`   | 0                 |                      |
| `sort`        | `duedate,desc`    | Avalable fields: `title`, `startdate`, `duedate`, `status`; Available directions: `asc`, `desc` |

If there are more tasks than the current page size, the link collection will contain a `next` link.
If there is a previous page, the link collection will return a `prev` link.
Use these links to navigate between pages.

### Link relations ###
| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| next     | The URI of the next page, using sort order specified. | GET |
| prev     | The URI of the previous page, using sort order specified. | GET |

### Supported Filters ###
All filters are off by default.

| **Parameter**     | **Example**  |**Additional notes** |
| :---              | :---         | :---                |
| `assignedtousers` | `56,57`      | Should be a valid userIds. `none` can be used to find Tasks with no assignees. Should not be used in conjunction with `assignedtoteams` |
| `assignedtoteams` | `123,67`     | Finds Tasks assigned to Users in those teams. Should be a valid teamIds. Should not be used in conjunction with `assignedtousers` |
| `earliestduedate` | `2016-08-13` | Date format should be `YYYY-MM-DD` |
| `latestduedate`   | `2016-08-13` | Date format should be `YYYY-MM-DD` |
| `statuses`        | `InProgress` | Available statuses: `NotStarted`, `InProgress`, `Complete` |

### Example ###

#### Request ####
```http
GET /v2/calendar/workspaces/all/paged?pagesize=10&skipitems=100 HTTP/1.1
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
<SummaryCalendar Uri="..." AltUri="..." xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <links>
    <link rel="self" href=".../tasks/workspaces/all/paged?pagesize=10&skipitems=100" />
    <link rel="prev" href=".../tasks/workspaces/all/paged?pagesize=10&skipitems=90" />
    <link rel="next" href=".../tasks/workspaces/all/paged?pagesize=10&skipitems=110" />    
  <links>
  <Events>
    <Task ... />
  </Events>
</SummaryCalendar>
```


# Syntax #

## Example ##

```
<SummaryCalendar Uri="..." AltUri="...">
  <Events>
    <Task ... />
  </Events>
</SummaryCalendar>
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| @Uri     | The current URI of this summary calendar. |
| @AltUri  | The URI of an alternative representation of the user's calendar. This is currently an iCalendar feed that contains the user's meetings and file approvals, as well as the user's tasks. |
| Events   | A collection containing all the [tasks](Task) that the user has access to across all of their [workspaces](Workspace). |
