# Summary #

The AuditTrail resource represents a collection of changes made to a specific [document](Document) resource.

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving a document's audit trail|Beta        |
|Adding a change to a document's audit trail|Beta        |

# Operations #

## Retrieving a document's audit trail ##

If the authenticated user is authorized to view a document, the document resource will advertise a link with @rel value of _audittrail_. To retrieve the document's audit trail, GET the _audittrail_ URI which will return an AuditTrail resource.

You can specify the maximum number of audit entries you want from the API by specifying a `limit` query-string parameter, up to a maximum of 5000. (If you exceed the maximum, the API will respond with 400 Bad Request.) The most recent audit entries will be returned. Omitting the query string will result in a default limit of 2000 being applied.


### Example ###

#### Request ####

```
GET .../files/documents/123/audittrail?limit=1000  HTTP/1.1
Accept: application/vnd.huddle.data+xml
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<changes xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <link rel="self" href=".../files/documents/123/audittrail" />
  <change>
    <link rel="self" href=".../files/change-sets/document/123/3" />
    <link rel="subject" href=".../files/documents/123" />
    <link rel="workspace" href=".../workspaces/6" />
    <link rel="parent" href=".../files/folders/99" />
    <link rel="share-information" href=".../notifications/shares/5df96158-5f6c-4d7f-a716-7877ad92c560" />
    <type>DocumentShared</type>
    <actor name="Bob Jones" email="bob.jones@123.com" rel="owner">
      <link rel="self" href=".../users/9" />
      <link rel="avatar" href=".../files/users/9/avatar" type="image/jpeg" />
      <link rel="alternate" href=".../user/bjones" type="text/html" />
    </actor>
    <createdDate>2013-05-08T13:52:29</createdDate>
    <versionNumber>1</versionNumber>
    <description>30</description>
    <clientId>my.huddle</clientId>
  </change>
  <change>
    <link rel="self" href=".../files/change-sets/document/123/2" />
    <link rel="subject" href=".../files/documents/123" />
    <link rel="workspace" href=".../workspaces/6" />
    <link rel="parent" href=".../files/folders/99" />
    <type>DocumentVersionViewed</type>
    <actor name="Bob Jones" email="bob.jones@123.com" rel="owner">
      <link rel="self" href=".../users/9" />
      <link rel="avatar" href=".../files/users/9/avatar" type="image/jpeg" />
      <link rel="alternate" href=".../user/bjones" type="text/html" />
    </actor>
    <createdDate>2013-05-07T10:21:03</createdDate>
    <versionNumber>1</versionNumber>
    <clientId>my.huddle</clientId>
  </change>
  <change>
    <link rel="self" href=".../files/change-sets/document/123/1" />
    <link rel="subject" href=".../files/documents/123" />
    <link rel="workspace" href=".../workspaces/6" />
    <link rel="parent" href=".../files/folders/99" />
    <type>ObjectCreated</type>
    <actor name="Bob Jones" email="bob.jones@123.com" rel="owner">
      <link rel="self" href=".../users/9" />
      <link rel="avatar" href=".../files/users/9/avatar" type="image/jpeg" />
      <link rel="alternate" href=".../user/bjones" type="text/html" />
    </actor>
    <createdDate>2013-05-07T10:20:03</createdDate>
    <clientId>my.huddle</clientId>
  </change>
</changes>
```

Note: Changes of type "Document Shared" include a  ["share-information"](ShareNotificationInformation) link that allows the caller to determine who the document was shared with. The "description" element of indicates the number of people the document was shared with.

## Adding a change to a document's audit trail ##

Most change types are automatically added to a document's audit trail, and cannot be manually added. For instance, when a document is created, an ObjectCreated audit entry is automatically added to the document's audit trail as part of the back-end document creation process.

However, there are certain change types that can be explicitly added by the caller. These change types are documented below. If a change type is not listed below, then it cannot be manually added.

### Printed ###

Document printing is a purely client-side activity, therefore document printed changes must be manually added to a document's [audit trail](AuditTrail) by the caller.

If the authenticated user is authorized to view a document, the [Document](Document) resource will advertise a link with a @rel value of _printed_. POSTing to this URI will add a change of type DocumentPrinted to the document's [AuditTrail](AuditTrail).

If the POST operation is successful, the endpoint will return a 201 Created response, along with the URI of the new change in the Location header.

### Example ###

#### Step 1. Retrieve the document to get its _printed_ link ####

##### Request #####
```
GET .../files/documents/123  HTTP/1.1
Accept: application/vnd.huddle.data+xml
```

##### Response #####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<document title="My Document">
  <link rel="self" href=".../files/documents/123" />
  <link rel="printed" href=".../files/documents/123/versions/456/audittrail/printed" />
  ... other document elements ...
</document>
```

#### Step 2. Post to the document's printed URI ####

##### Request #####
```
POST .../files/documents/123/versions/456/audittrail/printed  HTTP/1.1
Content-Length: 0
```

##### Response #####
```
HTTP/1.1 201 Created
Content-Length: 0
Location: .../files/change-sets/document/123/3
```

### Viewed ###

Similarly to Printing, a Document may be viewed on a disconnected client at a time other than download. For example, a disconnected client could download a lot of content ahead of time (implicit / automatic download) and thereafter a user could View the Content.

Therefore we should provide clients with the ability to specify document viewed changes outside of the regular API entries.

If the authenticated user is authorized to view a document, the [Document](Document) resource will advertise a link with a @rel value of _opened_. POSTing to this URI will add a change of type DocumentViewed to the document's [AuditTrail](AuditTrail).

If the POST operation is successful, the endpoint will return a 201 Created response, along with the URI of the new change in the Location header.

### Example ###

#### Step 1. Retrieve the document to get its _"viewed"_ link ####

##### Request #####
```
GET .../files/documents/123  HTTP/1.1
Accept: application/vnd.huddle.data+xml
```

##### Response #####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<document title="My Document">
  <link rel="self" href=".../files/documents/123" />
  <link rel="viewed" href=".../files/documents/123/versions/456/audittrail/viewed" />
  ... other document elements ...
</document>
```

#### Step 2. Post to the document's viewed URI ####

##### Request #####
```
POST .../files/documents/123/versions/456/audittrail/viewed HTTP/1.1
Content-Length: 0
```

##### Response #####
```
HTTP/1.1 201 Created
Content-Length: 0
Location: .../files/change-sets/document/123/3
```



# Syntax #

## Example ##
```xml
<changes xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <link rel="self" href=".../files/documents/123/audittrail" />
  <change>
    <link rel="self" href=".../files/change-sets/document/123/3" />
    <link rel="subject" href=".../files/documents/123" />
    <link rel="workspace" href=".../workspaces/6" />
    <link rel="parent" href=".../files/folders/99" />
    <link rel="share-information" href=".../notifications/shares/5df96158-5f6c-4d7f-a716-7877ad92c560" />
    <type>DocumentShared</type>
    <actor name="Bob Jones" email="bob.jones@123.com" rel="owner">
      <link rel="self" href=".../users/9" />
      <link rel="avatar" href=".../files/users/9/avatar" type="image/jpeg" />
      <link rel="alternate" href=".../user/bjones" type="text/html" />
    </actor>
    <createdDate>2013-05-08T13:52:29</createdDate>
    <versionNumber>1</versionNumber>
    <description>30</description>
    <clientId>my.huddle</clientId>
  </change>
  <change>
    <link rel="self" href=".../files/change-sets/document/123/2" />
    <link rel="subject" href=".../files/documents/123" />
    <link rel="workspace" href=".../workspaces/6" />
    <link rel="parent" href=".../files/folders/99" />
    <type>DocumentVersionViewed</type>
    <actor name="Bob Jones" email="bob.jones@123.com" rel="owner">
      <link rel="self" href=".../users/9" />
      <link rel="avatar" href=".../files/users/9/avatar" type="image/jpeg" />
      <link rel="alternate" href=".../user/bjones" type="text/html" />
    </actor>
    <createdDate>2013-05-07T10:21:03</createdDate>
    <versionNumber>1</versionNumber>
    <clientId>my.huddle</clientId>
  </change>
  <change>
    <link rel="self" href=".../files/change-sets/document/123/1" />
    <link rel="subject" href=".../files/documents/123" />
    <link rel="workspace" href=".../workspaces/6" />
    <link rel="parent" href=".../files/folders/99" />
    <type>ObjectCreated</type>
    <actor name="Bob Jones" email="bob.jones@123.com" rel="owner">
      <link rel="self" href=".../users/9" />
      <link rel="avatar" href=".../files/users/9/avatar" type="image/jpeg" />
      <link rel="alternate" href=".../user/bjones" type="text/html" />
    </actor>
    <createdDate>2013-05-07T10:20:03</createdDate>
    <clientId>my.huddle</clientId>
  </change>
</changes>
```

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The URI of this AuditTrail resource. | GET         |

## Query string parameters ##

| **Name** | **Description** |
|:---------|:----------------|
| limit    | The maximum number of audit entries to return. |

## Schema ##
```
start = changes

changes = element h:changes {
  link+,
  change+
}

change = element h:change {
  link+,
  element h:type {xsd:string},
  actor,
  element h:created {xsd:dateTime}
  element h:correlationId? {xsd:string}
  element h:versionNumber? {xsc:number}
  element h:description? {xsc:string}
  element h:clientId? {xsc:string}
}
```
