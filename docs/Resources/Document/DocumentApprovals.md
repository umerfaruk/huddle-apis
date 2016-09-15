# Summary #

It is possible for a user to request approval of a [Document](Document) resource from a set of specified assignees. The DocumentApprovals resource represents this collection of approval assignments for a given document.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving approvals for a document|Beta        |
|Creating or updating a document approval|Beta        |
|Updating document approval due date|Beta        |
|Updating document approval assignees|Beta        |
|Updating document approval assignment status|Beta        |

# Operations #

## Retrieving approvals for a document ##

The [document](Document) resource will advertise a link with @rel value of _approvals_. To retrieve the document approvals, GET the _approvals_ URI which will return a DocumentApprovals resource.

### Example ###

#### Request ####
```
GET files/documents/123/approvals HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<documentApprovals xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="files/documents/123/approvals" />
  <link rel="updateassigness" href="files/documents/123/approvals/updateassignees" />
  <link rel="updateduedate" href="files/documents/123/approvals/updateduedate" />
  <link rel="approvalrequest" href="files/documents/123/approvals/request" />
  <link rel="parent" href="files/documents/123" />
  
  <status>Open</status>
  <dueDate>2007-10-10T09:02:17Z</dueDate>  
  
  <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="requester">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>  

  <assignments>
    <assignment>
      <link rel="self" href="..." />     
      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Complete</status>
      <created>2007-10-10T09:02:17Z</created>
      <completed>2007-10-10T09:02:17Z</completed>
    </assignment>

    <assignment>
      <link rel="self" href="..." />
      <link rel="edit" href="..." />
      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Open</status>  
      <created>2007-10-10T09:02:17Z</created>            
    </assignment>
  </assignments>

</documentApprovals>
```

## Updating document approval due date ##

The DocumentApprovals resource will advertise a link with @rel value of _updateduedate_. This allows you update the due date of the approval.

### Example ###

#### Request ####
```
POST files/documents/123/approvals/updateduedate HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
<updateApprovalDueDate>
  <message>Can you approve this please?</message>
  <dueDate>2012-10-10T09:02:17Z</dueDate>
</updateApprovalDueDate>
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<documentApprovals xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="files/documents/123/approvals" />
  <link rel="updateassigness" href="files/documents/123/approvals/updateassignees" />
  <link rel="updateduedate" href="files/documents/123/approvals/updateduedate" />
  <link rel="approvalrequest" href="files/documents/123/approvals/request" />
  <link rel="parent" href="files/documents/123" />
  
  <status>Open</status>
  <dueDate>2012-10-10T09:02:17Z</dueDate>  
  
  <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="requester">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>  

  <assignments>
    <assignment>
      <link rel="self" href="..." />     
      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Complete</status>
      <created>2007-10-10T09:02:17Z</created>
      <completed>2007-10-10T09:02:17Z</completed>
    </assignment>

    <assignment>
      <link rel="self" href="..." />
      <link rel="edit" href="..." />
      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Open</status>  
      <created>2007-10-10T09:02:17Z</created>            
    </assignment>
  </assignments>

</documentApprovals>
```

## Updating document approval assignees ##

The DocumentApprovals resource will advertise a link with @rel value of _updateassignees_. This allows you to update the approval's assignee list by posting the full list to the link.

The request may optionally include an X-Allow-Invalid-Reviewers header, which should be true or false (these are case insensitive). It determines whether the update assignees request should fail (returning a 400 BAD REQUEST) if _any_ of the users cannot be added to the list of approvers for some reason, or just return a 200 OK to indicate that the update assignees request was sent to as many users as possible.

### Example ###

#### Request ####
```
POST files/documents/123/approvals/updateassignees HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
<updateApprovalAssignees>
  <message>Can you all approve this please?</message>
  <assignee href="..." />
  <assignee href="..." />
</updateApprovalAssignees>
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<documentApprovals xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="files/documents/123/approvals" />
  <link rel="updateassigness" href="files/documents/123/approvals/updateassignees" />
  <link rel="updateduedate" href="files/documents/123/approvals/updateduedate" />
  <link rel="approvalrequest" href="files/documents/123/approvals/request" />
  <link rel="parent" href="files/documents/123" />
  
  <status>Open</status>
  <dueDate>2007-10-10T09:02:17Z</dueDate>  

  <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="requester">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>  

  <assignments>
    <assignment>
      <link rel="self" href="..." />     
      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Complete</status>
      <created>2007-10-10T09:02:17Z</created>
      <completed>2007-10-10T09:02:17Z</completed>
    </assignment>

    <assignment>
      <link rel="self" href="..." />
      <link rel="edit" href="..." />
      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Open</status>  
      <created>2007-10-10T09:02:17Z</created>            
    </assignment>
  </assignments>

</documentApprovals>
```

##### Failed Response #####

```
HTTP/1.1 400 BAD REQUEST
Content-Type: application/vnd.huddle.data+xml
```

```
<updateApprovalAssigneesFailure>
  <reason>The reason was...</reason>
  <link rel="assignee" href="..." />
  <link rel="assignee" href="..." />
</updateApprovalAssigneesFailure>
```

#### Request (with optional header) ####
```
POST files/documents/123/approvals/updateassignees HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
X-Allow-Invalid-Reviewers: true
```

```
<updateApprovalAssignees>
  <message>Can you all approve this please?</message>
  <assignee href="..." />
  <assignee href="..." />
</updateApprovalAssignees>
```

## Creating or updating a document approval ##

The DocumentApprovals resource will advertise a link with @rel value of _approvalrequest_. This allows you create a new document approval, or if one already exists, to update its due date and assignees.

The request may optionally include an X-Allow-Invalid-Reviewers header, which should be true or false (these are case insensitive). It determines whether the request should fail (returning a 400 BAD REQUEST) if _any_ of the users cannot be added to the list of approvers for some reason, or just return a 200 OK to indicate that the update assignees request was sent to as many users as possible.

### Example ###

#### Request ####
```
POST files/documents/123/approvals/request HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
<approvalRequest>
  <message>Can you all approve this please?</message>
  <assignee href="..." />
  <assignee href="..." />
  <dueDate>2012-10-10T09:02:17Z</dueDate>
</approvalRequest>
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<documentApprovals xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="files/documents/123/approvals" />
  <link rel="updateassigness" href="files/documents/123/approvals/updateassignees" />
  <link rel="updateduedate" href="files/documents/123/approvals/updateduedate" />
  <link rel="approvalrequest" href="files/documents/123/approvals/request" />
  <link rel="parent" href="files/documents/123" />
  
  <status>Open</status>
  <dueDate>2012-10-10T09:02:17Z</dueDate>  

  <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="requester">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>  

  <assignments>
    <assignment>
      <link rel="self" href="..." />     
      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Complete</status>
      <created>2007-10-10T09:02:17Z</created>
      <completed>2007-10-10T09:02:17Z</completed>
    </assignment>

    <assignment>
      <link rel="self" href="..." />
      <link rel="edit" href="..." />
      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Open</status>  
      <created>2007-10-10T09:02:17Z</created>            
    </assignment>
  </assignments>

</documentApprovals>
```

## Updating approval assignment status ##

If the authenticated user has a document approval assignment, a link will be displayed on the relevant assignment with rel _edit_. POST to this URI to change the status of the assignment (A GET request for this _edit_ resource is not currently implemented). A comment can also be included in the request body, which will be added to the [DocumentComments](DocumentComments) resource.

### Example ###

#### Request (with a comment) ####
```
POST /documents/123/approvals/assignment HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
<assignment>
  <status>Complete</status>
  <comment>I approve the document ... </comment>
</assignment>
```

#### Request (without a comment) ####
```
POST /documents/123/approvals/assignment HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
<assignment>
  <status>Complete</status>
</assignment>
```

| **Name** | **Description** |
|:---------|:----------------|
| status   | The desired state of the assignment. Can either be Complete or Rejected |
| comment  | If the status is set to Rejected, the comment is mandatory, otherwise it is optional. Maximum of 2048 characters. |

#### Response ####
If successful the request will return a 200 OK with the updated DocumentApprovals resource.

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Content-Location : http://api.huddle.net/....
```

```
<documentApprovals xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="documents/123/approvals" />
  <link rel="parent" href="documents/123" />
  <link rel="updateassigness" href="documents/123/approvals/updateassignees" />
  <link rel="updateduedate" href="documents/123/approvals/updateduedate" />
  <link rel="approvalrequest" href="documents/123/approvals/request" />

  <status>Open</status>
  <dueDate>2007-10-10T09:02:17Z</dueDate>  

  <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="requester">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>  

  <assignments>
    <assignment>
      <link rel="self" href="..." />     
      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Complete</status>
      <created>2007-10-10T09:02:17Z</created>
      <completed>2007-10-10T09:02:17Z</completed>
    </assignment>

    <assignment>
      <link rel="self" href="..." />
      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Open</status>  
      <created>2007-10-10T09:02:17Z</created>            
    </assignment>
  </assignments>

</documentApprovals>
```


---


# Syntax #

## Example ##

### Approvals with due date ###

```
<documentApprovals xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="documents/123/approvals" />
  <link rel="parent" href="documents/123" />
  <link rel="updateassigness" href="documents/123/approvals/updateassignees" />
  <link rel="updateduedate" href="documents/123/approvals/updateduedate" />
  <link rel="approvalrequest" href="documents/123/approvals/request" />

  <status>Open</status>
  <dueDate>2007-10-10T09:02:17Z</dueDate>  

  <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="requester">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>  

  <assignments>
    <assignment>
      <link rel="self" href="..." />
      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Complete</status>
      <created>2007-10-10T09:02:17Z</created>
      <completed>2007-10-10T09:02:17Z</completed>      
    </assignment>

    <assignment>
      <link rel="self" href="..." />
      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Open</status>       
      <created>2007-10-10T09:02:17Z</created>       
    </assignment>
  </assignments>

</documentApprovals>
```

### Approvals with no due date ###

```
<documentApprovals xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="documents/123/approvals" />
  <link rel="parent" href="documents/123" />
  <link rel="updateassigness" href="documents/123/approvals/updateassignees" />
  <link rel="updateduedate" href="documents/123/approvals/updateduedate" />
  <link rel="approvalrequest" href="documents/123/approvals/request" />

  <status>Open</status>

  <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="requester">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>  

  <assignments>
    <assignment>
      <link rel="self" href="..." />
      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Complete</status>
      <created>2007-10-10T09:02:17Z</created>
      <completed>2007-10-10T09:02:17Z</completed>      
    </assignment>

    <assignment>
      <link rel="self" href="..." />
      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="assignee">
        <link rel="self" href="..." type="text/html" />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Open</status>   
      <created>2007-10-10T09:02:17Z</created>           
    </assignment>
  </assignments>

</documentApprovals>
```

### Approvals with open assignment for the authenticated user ###

```
<documentApprovals xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="documents/123/approvals" />
  <link rel="parent" href="documents/123" />
  <link rel="updateassigness" href="documents/123/approvals/updateassignees" />
  <link rel="updateduedate" href="documents/123/approvals/updateduedate" />
  <link rel="approvalrequest" href="documents/123/approvals/request" />

  <status>Open</status>
  <dueDate>2007-10-10T09:02:17Z</dueDate>  

  <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="requester">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
  </actor>  

  <assignments>
    <assignment>
      <link rel="self" href="..." />
      <actor name="Peter Gibson" email="peter.gibson@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Complete</status>
      <created>2007-10-10T09:02:17Z</created>
      <completed>2007-10-10T09:02:17Z</completed>      
    </assignment>

    <assignment>
      <link rel="self" href="..." />
      <link rel="edit" href="..." />
      <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="assignee">
        <link rel="self" href="..." />
        <link rel="avatar" href="..." type="image/jpg" />
        <link rel="alternate" href="..." type="text/html" />
      </actor>  
      <status>Open</status>       
      <created>2007-10-10T09:02:17Z</created>       
    </assignment>
  </assignments>

</documentApprovals>
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| status (for the documentApproval element) | Present if the document has approval assignments. Value of 'Open' if there are still outstanding assignments, or 'Complete' if all approvals are complete |
| dueDate  | Present if the document has approval assignments that require a completion date.  |
| actor  | The actor will be of rel "requester".  This field will not be present if the approval was created before 2015-Sept.  |
| assignment| An assignment represents a document approval request for one assignee. |
| status (for the assignment element) | Value of 'Open' if assignee has yet to approve the [Document](Document) resource, or 'Complete' if assignee has approved the document |
| completed | Present if the assigment status is in 'Complete' state. The date at which the assignee approved the document |

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this DocumentApprovals resource. | GET         |
| parent   | Retrieves the [document](Document) that this resource belongs to. | GET         |
| updateassignees | Used to replace the assignee list for the approval of the document. | POST        |
| updateduedate | Used to update the due date for approval of the document. | POST        |
| edit (for the assignment element) | If the authenticated user has an approval assignment, this link can be used to edit the status of that assignment. This link will be present on the relevant assignment. | POST        |

## Schema ##

```

start = documentApprovals

documentApprovals= element h:documentApprovals{
  link+,
  element h:status? {"Complete"|"Open"},
  actor, 
  element h:dueDate? {xsd:dateTime},
  element h:assignments{assignment*}
}

assignment= element h:assignment{
  link+,
  actor,
  element h:status {"Complete"|"Open"},
  element h:created {xsd:dateTime},
  element h:completed? {xsd:dateTime}
}
```
