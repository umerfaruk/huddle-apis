|  |
|:-|

# Summary #
It is possible to assign a document to one or more users ("assignees") for approval. The Approvals resource represents a read-only collection of approval information.

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving approvals by assignee|Beta        |
|Retrieving approvals by document owner|Beta        |
|Retrieving approvals by workspace|Beta        |

# Operations #

## Retrieving approvals by assignee ##

An "assignee" is a user to which an approval has been assigned. It is possible for a user to request a list approvals that have been assigned to them. This endpoint only returnes Open approvals, and is useful for answering the question: "What documents are awaiting my approval?".

### Example ###

In this example, we request approvals for assignee 999.

#### Request ####

```
GET /files/assignees/999/approvals HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<approvals>
   <link rel="self" href="/files/assignees/999/approvals" />
   <approval>
      <dueDate>2013-11-23T09:02:17Z</dueDate>   
      <assignments>
         <assignment>  
            <actor name="James Howard" rel="assignee">
               <link rel="self" href="/users/999" />
               <link rel="alternate" type="text/html" href="https://my.huddle.net/user/james"  />
            </actor>
            <status>Open</status>
         </assignment>
      </assignments>
      <document title="Some Ideas" content-type="application/vnd.huddle.note">
         <link rel="self" href="/files/documents/856" />
         <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/444/files/856" />
         <version>3</version>
         <updated>2011-10-10T09:02:17Z</updated>
         <actor name="Pete O'Grady" rel="updated-by">
            <link rel="self" href="/users/76771" />
            <link rel="alternate" type="text/html" href="https://my.huddle.net/user/peteg" />
         </actor>
         <workspace title="My Workspace">
	    <link rel="self" href="/workspace/444" />
            <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/444" />
         </workspace>	
      </document>     
   </approval>   
   <approval>
      <dueDate>2013-12-13T09:06:44Z</dueDate> 
      <assignments>
         <assignment>  
            <actor name="James Howard" rel="assignee">
               <link rel="self" href="/users/999" />
               <link rel="alternate" type="text/html" href="https://my.huddle.net/user/james"  />
            </actor>
            <status>Open</status>
         </assignment>
      </assignments>  
      <document title="My Cat" content-type="image/jpg">
         <link rel="self" href="/files/documents/456" />
         <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/555/files/456" />
         <version>7</version>
         <updated>2013-09-05T09:02:17Z</updated>
         <actor name="Rob Prince" rel="updated-by">
            <link rel="self" href="/users/89454" />
            <link rel="alternate" type="text/html" href="https://my.huddle.net/user/theprincemeister" />
         </actor>
         <workspace title="My Workspace">
	    <link rel="self" href="/workspace/555" />
            <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/555" />
         </workspace>	
      </document>       
   </approval>
</approvals>
```

## Retrieving approvals by document owner ##

It is possible for a user to request a list of approvals for documents they own. This endpoint returns approvals that contain at least one open assignment, and is useful for answering the question: "What documents that I own are awaiting approval by other users?".

### Example ###

In this example, we request approvals for documents owned by user 888.

#### Request ####

```
GET /files/documentowner/888/approvals HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<approvals>
   <link rel="self" href="/files/documentowner/888/approvals" />
   <approval>
      <dueDate>2013-11-23T09:02:17Z</dueDate>   
      <assignments>
         <assignment>  
            <actor name="Peter Gibson" rel="assignee">
               <link rel="self" href="/users/22244" />
               <link rel="alternate" type="text/html" href="https://my.huddle.net/user/pgibson"  />
            </actor>
            <status>Closed</status>
         </assignment>
         <assignment>
            <actor name="Barry Potter" rel="assignee">
               <link rel="self" href="/users/5222" />
               <link rel="alternate" type="text/html" href="https://my.huddle.net/user/barry.potter"  />
            </actor>	
            <status>Open</status>			
         </assignment>
      </assignments>
      <document title="Some Ideas" content-type="application/vnd.huddle.note">
         <link rel="self" href="/files/documents/856" />
         <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/444/files/856" />
         <version>3</version>
         <updated>2011-10-10T09:02:17Z</updated>
         <actor name="Pete O'Grady" rel="updated-by">
            <link rel="self" href="/users/76771" />
            <link rel="alternate" type="text/html" href="https://my.huddle.net/user/peteg" />
         </actor>
         <workspace title="My Workspace">
	    <link rel="self" href="/workspace/444" />
            <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/444" />
         </workspace>		
      </document>   
   </approval>   
   <approval>
      <dueDate>2013-12-13T09:06:44Z</dueDate>   
      <assignments>
         <assignment>  
            <actor name="Melissa Smith" rel="assignee">
               <link rel="self" href="/users/55520" />
               <link rel="alternate" type="text/html" href="https://my.huddle.net/user/msmith"  />
            </actor>
            <status>Open</status>
         </assignment>
      </assignments>   
      <document title="My Cat" content-type="image/jpg">
         <link rel="self" href="/files/documents/456" />
         <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/555/files/456" />
         <version>7</version>
         <updated>2013-09-05T09:02:17Z</updated>
         <actor name="Rob Prince" rel="updated-by">
            <link rel="self" href="/users/89454" />
            <link rel="alternate" type="text/html" href="https://my.huddle.net/user/theprincemeister" />
         </actor>
         <workspace title="My Workspace">
	    <link rel="self" href="/workspace/555" />
            <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/555" />
         </workspace>	
      </document>     
   </approval>
</approvals>
```

## Retrieving approvals by workspace ##

It is possible for a user to request a list of approvals for a given workspace. This endpoint returns approvals that contain at least one open assignment, and is useful for getting an overview of approvals in a workspace. Please note that the authenticated user must be a workspace manager of the workspace to use this endpoint.

### Example ###

In this example, we request approvals for documents in workspace 123.

#### Request ####

```
GET /files/workspaces/123/approvals HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<approvals>
   <link rel="self" href="/files/workspaces/123/approvals" />
   <approval>
      <dueDate>2013-11-23T09:02:17Z</dueDate>   
      <assignments>
         <assignment>  
            <actor name="Peter Gibson" rel="assignee">
               <link rel="self" href="/users/22244" />
               <link rel="alternate" type="text/html" href="https://my.huddle.net/user/pgibson"  />
            </actor>
            <status>Closed</status>
         </assignment>
         <assignment>
            <actor name="Barry Potter" rel="assignee">
               <link rel="self" href="/users/5222" />
               <link rel="alternate" type="text/html" href="https://my.huddle.net/user/barry.potter"  />
            </actor>	
            <status>Open</status>			
         </assignment>
      </assignments>
      <document title="Some Ideas" content-type="application/vnd.huddle.note">
         <link rel="self" href="/files/documents/856" />
         <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/123/files/856" />
         <version>3</version>
         <updated>2011-10-10T09:02:17Z</updated>
         <actor name="Pete O'Grady" rel="updated-by">
            <link rel="self" href="/users/76771" />
            <link rel="alternate" type="text/html" href="https://my.huddle.net/user/peteg" />
         </actor>
         <workspace title="My Workspace">
	    <link rel="self" href="/workspace/123" />
            <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/123" />
         </workspace>		
      </document>   
   </approval>   
   <approval>
      <dueDate>2013-12-13T09:06:44Z</dueDate>   
      <assignments>
         <assignment>  
            <actor name="Melissa Smith" rel="assignee">
               <link rel="self" href="/users/55520" />
               <link rel="alternate" type="text/html" href="https://my.huddle.net/user/msmith"  />
            </actor>
            <status>Open</status>
         </assignment>
      </assignments>   
      <document title="My Cat" content-type="image/jpg">
         <link rel="self" href="/files/documents/456" />
         <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/123/files/456" />
         <version>7</version>
         <updated>2013-09-05T09:02:17Z</updated>
         <actor name="Rob Prince" rel="updated-by">
            <link rel="self" href="/users/89454" />
            <link rel="alternate" type="text/html" href="https://my.huddle.net/user/theprincemeister" />
         </actor>
         <workspace title="My Workspace">
	    <link rel="self" href="/workspace/123" />
            <link rel="alternate" type="text/html" href="http://my.huddle.net/workspaces/123" />
         </workspace>	
      </document>     
   </approval>
</approvals>
```

# Syntax #

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The URI of this approvals resource | GET         |
| document:self | The URI of the document resource | GET         |
| document:alternate | A website URL for the document | GET         |
| actor:self | The URI of the user resource | GET         |
| actor:alternate | A website URL for the user | GET         |
| workspace:self | The URI of the workspace resource | GET         |
| workspace:alternate | A website URL for the workspace | GET         |

## Schema ##

```
start = approvals

approvals = element h:approvals{
  link,
  approval+
}

approval = element h:approval{
  element h:dueDate? {xsd:dateTime},
  element h:assignments{assignment*}
  document
}

assignment = element h:assignment{
  actor,
  element h:status {"Complete"|"Open"}
}

document = element h:document {
  attribute title {xsd:string},
  attribute content-type {xsd:string},
  link+,
  element h:version {xsd:string},
  element h:updated {xsd:dateTime},
  actor,
  workspace 
}

workspace = element h:workspace{
  attribute title {xsd:string},
  link+,
}

actor = element h:actor { 
  attribute name { xsd:string },
  attribute rel { xsd:string },
  link +
}

```
