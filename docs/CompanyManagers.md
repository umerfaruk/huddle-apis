# Summary #

The Company Managers API is a representation of [Members](Members) who are Managers of a [Company](Company) within Huddle.

## Beta warning ##

The Company Managers API is in Beta

|  |
|:-|

# Operations #

## Assign Company Manager permissions ##

To assign Company Manager permissions to a [Member](Member), POST the [Member](Member) link to the _managers_ link from the [Company](Company). The request will return an empty response with the location header indicating the location of the newly-created Manager.

This end point will only add one Manager - to create multiple Managers, use multiple POST requests.

### XML Example ###
#### Request ####
```
POST .../people/companies/123/managers HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<link rel="member" href=".../people/companies/123/members/654" />
```

#### Response ####
If successful the method returns a 201 Created response.
```
HTTP/1.1 201 Created
```

### JSON Example ###

#### Request ####

```
POST /people/companies/123/managers HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
{
  "rel": "member",
  "href": "/people/companies/123/members/654"
}
```

#### Response ####

```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.data+json
Location: /people/companies/123/managers/432
```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|201 Created|Member is already a company manager|
|400 Bad Request|Member does not exist|
|400 Bad Request|Member is not in that company|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Actor does not have company manager permissions|
|404 Not Found|Company does not exist|

## Revoke Company Manager permissions ##

To revoke Company Manager permissions from a [Member](Member), send a DELETE request to the Company Manager link contained in the [Member](Member) or [Members](Members) resources.

If the authenticated user has permissions to remove a member as a company manager, both the [Member](Member) and [Members](Members) resources will advertise a link with a @rel value of _companyManager_. To remove as a company manager, send a DELETE request to the _companyManager_ URI.


### Example ###
#### Request ####
```
DELETE /people/companies/123/managers/256 HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If the removal is successful, this method will return an empty response with an 204 No Content status code

```
HTTP/1.1 204 No Content
```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Actor does not have company manager permissions|
|404 Not Found|Company does not exist|
|404 Not Found|Manager does not exist|

