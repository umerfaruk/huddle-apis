# Summary #
This resource enables operations to be performed against multiple resources in a single request.

|  |
|:-|

# Status #
|Operation|Status|
|:--------|:-----|
|Bulk Delete|Beta  |

# Operations #
## Bulk Delete ##
This resource supports bulk deleting members from one company. To bulk delete, issue a POST request to the bulk-delete endpoint, with an entity body that contains a list of members. Each member should be referenced with their self-link and a rel of 'member'.

The caller must have company manager permissions.

Any already deleted members in the request will remain unchanged by this operation, and will be treated as a successful deletion.

The link for this API is advertised in the company response.

### Example ###
Bulk delete members 123 and 456 from company 42

#### XML ####
#### Request ####
```
POST .../people/companies/42/bulkdelete HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<bulkDelete>
   <link rel="member" href=".../people/companies/42/members/123" />    
   <link rel="member" href=".../people/companies/42/members/456" />  
</bulkDelete>
```
#### Response ####
If successful the method returns a 204 No Content code.
```
HTTP/1.1 204 No Content
```
#### JSON ####
#### Request ####
```
POST .../people/companies/42/bulkdelete HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
{
"links": [{
    "rel": "member",
    "href": ".../people/companies/42/members/123"
  },
  {
    "rel": "member",
    "href": ".../people/companies/42/members/456"
  }]
}
```
#### Response ####
If successful the method returns a 204 No Content code.
```
HTTP/1.1 204 No Content
```

### Error Responses ###

|Case|Response|
|:---|:-------|
|User does not have company manager permissions|403 Forbidden|
|Company does not exist|404 Not Found|
|Member does not exist|400 Bad Request|
|Member is not in that company|400 Bad Request|
