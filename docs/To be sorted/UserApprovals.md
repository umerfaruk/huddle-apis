# Summary #

It is possible for a user to request approval of a [Document](Document) resource from a set of specified assignees. The UserApprovals resource represents all the documents where the current authenticated user has an outstanding approval assignment.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving user approvals|Beta        |

# Operations #

## Retrieving user approvals ##

The [User](User) resource will advertise a link with @rel value of _approvals_. To retrieve the user approvals, GET the _approvals_ URI which will return a UserApprovals resource.

### Example ###

#### Request ####
```
GET /files/users/123/approvals HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<userApprovals xmlns="http://schema.huddle.net/2011/02/" count="2">
  <link rel="self" href="files/users/123/approvals" />
  <link rel="parent" href="users/123" />

  <documents>
    
    <document xmlns="http://schema.huddle.net/2011/02/" title="doc 1.">
      <link rel="self" href="..." />
   
      ... other document elements ...      

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
    </document>
    <document xmlns="http://schema.huddle.net/2011/02/" title="doc 2.">
      <link rel="self" href="..." />

      ... other document elements ... 
  
      <assignment>
        <link rel="self" href="..." />
        <link rel="edit" href="..." />
        <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="assignee">
          <link rel="self" href="..." />
          <link rel="avatar" href="..." type="image/jpg" />
          <link rel="alternate" href="..." type="text/html" />
        </actor>  
        <status>Open</status>  
        <created>2007-11-23T09:02:17Z</created>            
      </assignment>
    </document>

  </documents>

</userApprovals>
```


---


# Syntax #

## Example ##

### User with outstanding approvals ###

```
<userApprovals xmlns="http://schema.huddle.net/2011/02/" count="2">
  <link rel="self" href="files/users/123/approvals" />
  <link rel="parent" href="users/123" />

  <documents>
    
    <document xmlns="http://schema.huddle.net/2011/02/" title="doc 1.">
      <link rel="self" href="..." />

      ... other document elements ...  
 
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
    </document>
    <document xmlns="http://schema.huddle.net/2011/02/" title="doc 2.">
      <link rel="self" href="..." />

      ... other document elements ...   

      <assignment>
        <link rel="self" href="..." />
        <link rel="edit" href="..." />
        <actor name="Jimmy Snake" email="jimmy.snake@example.com" rel="assignee">
          <link rel="self" href="..." />
          <link rel="avatar" href="..." type="image/jpg" />
          <link rel="alternate" href="..." type="text/html" />
        </actor>  
        <status>Open</status>  
        <created>2007-11-23T09:02:17Z</created>            
      </assignment>
    </document>

  </documents>

</userApprovals>
```

### User with no outstanding approvals ###

```
<userApprovals xmlns="http://schema.huddle.net/2011/02/" count="0">
  <link rel="self" href="files/users/123/approvals" />
  <link rel="parent" href="users/123" />

  <documents/>

</userApprovals>
```


## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| documents | A list of [Document](Document) resources that have outstanding approval assignments for the current authenticated user. |
| count    | How many documents are in the documents element |

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this UserApprovals resource. | GET         |
| parent   | Retrieves the [User](User) resource that this resource belongs to. | GET         |

## Schema ##

```

start = userApprovals

userApprovals= element h:userApprovals{
  attribute h:count{xsd:int},
  link+,
  element h:documents{document*}
}
```
