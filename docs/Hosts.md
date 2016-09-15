# Summary #

This endpoint is for managing email hosts of a registered SAML partner.

Currently this endpoint can only be consumed with admin permissions.

|  |
|:-|

# Operations #
## Adding a new email host for SAML partner ##

You can add a new email host to a SAML partner domain with a POST request to the emailhosts endpoint.

### Example ###
#### Request ####
```
POST /samlpartners/123/emailhosts HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
 {"hostName":"huddle.com"}
```

### Response ###
If POST is successful, this method will return a 201 Created with the Location header pointing to the created email host.

```
HTTP/1.1 201 Created
Location: /samlpartners/123/emailhosts/1234
```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Actor does not have admin permission|403 Forbidden|
|Requested SAML Partner Id does not exist|404 Not Found|
|Specified email host name already exists within the same or different SAML Partner|409 Conflict|


---


## Deleting an email host from SAML partner ##

This resource supports deleting individual email hosts. To delete an email host, send a DELETE request to the email host's self URI.


### Example ###
#### Request ####
```
DELETE /samlpartners/123/emailhosts/1234 HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If the delete is successful, this method will return an empty response with an 204 No Content status code

```
HTTP/1.1 204 No Content
```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Actor does not have admin permission|403 Forbidden|
|Requested SAML Partner Id does not exist|404 Not Found|
|Requested Email Host Id does not exist|404 Not Found|
|Specified email host id exists within different SAML Partner|404 Not Found|
