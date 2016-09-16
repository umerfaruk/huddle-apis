# Summary #

In addition to wildcard emails for the SAML partner, individual emails can be added to the SAML partner's domain.

Currently this endpoint can only be consumed with admin permissions.

|  |
|:-|

# Operations #
## Creating new email address within partner ##

You can add a new email address to a SAML partner domain with a POST request to the emails endpoint.

### Example ###
#### Request ####
```
POST /samlpartners/123/emails HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
 {"email":"andy@huddle.com"}
```

### Response ###
If POST is successful, this method will return a 201 Created with the Location header pointing to the created email.

```
HTTP/1.1 201 Created
Location: /samlpartners/123/emails/1234
```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Actor does not have admin permission|403 Forbidden|
|Requested SAML Partner Id does not exist|404 Not Found|
|Specified email address already exists within the same or different SAML Partner|409 Conflict|


---


## Deleting an email address from partner domain ##

This resource supports deleting individual emails. To delete an email, send a DELETE request to the email's self URI.


### Example ###
#### Request ####
```
DELETE /samlpartners/123/emails/1234 HTTP/1.1
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
|Specified email id does not exist|404 Not Found|
|Specified email id exists within different SAML Partner|404 Not Found|
