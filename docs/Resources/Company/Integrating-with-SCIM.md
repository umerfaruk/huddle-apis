To provision users into Huddle, there's a number of APIs that support the [SCIM](http://www.simplecloud.info/) specification.

# Authentication #

To authenticate to the SCIM APIs, use the [client credentials flow](https://github.com/Huddle/huddle-apis/wiki/OAuth%20Integration#client-credentials-flow) to obtain an access token

# Operations #
## Users ##
### Create a new user ###
To create a new user, you can issue a POST request
#### JSON example ####
##### Request #####
```
POST /people/companies/123/users HTTP/1.1
Content-Type: application/scim+json
Accept: application/scim+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Host: api.huddle.net
{
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
  "userName":"bjensen@huddle.net",
  "name":{
    "familyName": "Jensen",
    "givenName": "Barbara"
  },
  "phoneNumbers":[
    {
      "value":"555-555-8377"
    }
  ],
  "title":"Tour Guide",
  "preferredLanguage":"en-us",
  "timezone":"America/Los_Angeles"
}
```
##### Response #####
If successful the method returns a 201 Created response with the created resource. The response includes _id_ and _meta_ data as well as a location header
```
HTTP/1.1 201 Created
Content-Type: application/scim+json
Location: https://api.huddle.net/people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645
{
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
  "id": "78579b5f-39c2-4932-b72a-2485ae233645",
  "userName":"bjensen@huddle.net",
  "name":{
    "familyName": "Jensen",
    "givenName": "Barbara"
  },
  "phoneNumbers":[
    {
      "value":"555-555-8377"
    }
  ],
  "title":"Tour Guide",
  "preferredLanguage":"en-us",
  "timezone":"America/Los_Angeles",
  "meta":{
    "location":"https://api.huddle.net/people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645"
  },
}
```
#### XML example ####
##### Request #####
```
POST /people/companies/123/users HTTP/1.1
Content-Type: application/xml
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Host: api.huddle.net
<?xml version="1.0" encoding="utf-8"?>
<User xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <schemas>
    <schema>urn:ietf:params:scim:schemas:core:2.0:User</schema>
  </schemas>
  <userName>bjensen@huddle.net</userName>
  <name>
    <givenName>Barbara</givenName>
    <familyName>Jensen</familyName>
  </name>
  <phoneNumbers>
    <phoneNumber>
      <value>555-555-5555</value>
    </phoneNumber>
  </phoneNumbers>
  <title>Tour Guide</title>
  <preferredLanguage>en-us</preferredLanguage>
  <timezone>America/Los_Angeles</timezone>
</User>
```
##### Response #####
```
HTTP/1.1 201 Created
Content-Type: application/xml
Location: https://api.huddle.net/people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645
<?xml version="1.0" encoding="utf-8"?>
<User xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <schemas>
    <schema>urn:ietf:params:scim:schemas:core:2.0:User</schema>
  </schemas>
  <id>78579b5f-39c2-4932-b72a-2485ae233645</id>
  <userName>bjensen@huddle.net</userName>
  <name>
    <givenName>Barbara</givenName>
    <familyName>Jensen</familyName>
  </name>
  <phoneNumbers>
    <phoneNumber>
      <value>555-555-5555</value>
    </phoneNumber>
  </phoneNumbers>
  <title>Tour Guide</title>
  <preferredLanguage>en-us</preferredLanguage>
  <timezone>America/Los_Angeles</timezone>
  <meta>
    <location>https://api.huddle.net/people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645</Location>
  </meta>
</User>
```

##### Other Responses #####

|Case|Response|
|:---|:-------|
|400 Bad Request|Request is invalid|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Actor does not have provisioning permissions|
|403 Forbidden|Actor is not in company|
|403 Forbidden|Email domain of provisioned user is not authorized for company|
|409 Conflict|Provisioned user already exists|

---

### Retrieve a user ###
To retrieve a user by their unique id, you can issue a GET request
#### JSON example ####
##### Request #####
```
GET /people/companies/123/users/0c094b2f-953a-457c-96d1-603a7d3e02ce
Accept: application/scim+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
##### Response #####
```
HTTP/1.1 200 OK
Content-Type: application/scim+json
Location: https://api.huddle.net/people/companies/123/users/0c094b2f-953a-457c-96d1-603a7d3e02ce
{
    "schemas":["urn:ietf:params:scim:schemas:core:2.0:User"],
    "id":"0c094b2f-953a-457c-96d1-603a7d3e02ce",
    "userName": "bjensen@example.com",
    "name": {
        "familyName": "Jensen",
        "givenName": "Barbara"
    },
    "phoneNumbers": [{"value" : "555-555-8377"}],
    "title": "Tour Guide",
    "preferredLanguage": "en-us",
    "timezone": "America/Los_Angeles",
    "meta": {
        "location":"https://api.huddle.net/people/companies/123/users/0c094b2f-953a-457c-96d1-603a7d3e02ce"
    }
}
```

#### XML example ####
##### Request #####
```
GET /people/companies/123/users/0c094b2f-953a-457c-96d1-603a7d3e02ce
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
##### Response #####
```
HTTP/1.1 200 OK
Content-Type: application/xml
Location: https://api.huddle.net/people/companies/123/users/0c094b2f-953a-457c-96d1-603a7d3e02ce
<?xml version="1.0" encoding="utf-8"?>
<User xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <schemas>
    <schema>urn:ietf:params:scim:schemas:core:2.0:User</schema>
  </schemas>
  <id>0c094b2f-953a-457c-96d1-603a7d3e02ce</id>
  <userName>bjensen@example.com</userName>
  <name>
    <givenName>Barbara</givenName>
    <familyName>Jensen</familyName>
  </name>
  <phoneNumbers>
    <phoneNumber>
      <value>555-555-8377</value>
    </phoneNumber>
  </phoneNumbers>
  <title>Tour Guide</title>
  <preferredLanguage>en-us</preferredLanguage>
  <timezone>America/Los_Angeles</timezone>
  <meta>
    <location>https://api.huddle.net/people/companies/123/users/0c094b2f-953a-457c-96d1-603a7d3e02ce</Location>
  </meta>
</User>
```
##### Other Responses #####

|Case|Response|
|:---|:-------|
|400 Bad Request|Request is invalid|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Actor does not have provisioning permissions|
|403 Forbidden|Actor is not in company|
|404 Not found|SCIM user not found|
---
### Update a user ###
To update a user, you can issue a PUT request
#### JSON example ####
##### Request #####
```
PUT /people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645 HTTP/1.1
Content-Type: application/scim+json
Accept: application/scim+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Host: api.huddle.net
{
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
  "id": "78579b5f-39c2-4932-b72a-2485ae233645",
  "userName":"bjensen@huddle.net",
  "name":{
    "familyName": "Jensen",
    "givenName": "Barbara"
  },
  "phoneNumbers":[
    {
      "value":"555-555-8377"
    }
  ],
  "title":"Tour Guide",
  "preferredLanguage":"en-us",
  "timezone":"America/Los_Angeles"
}
```
##### Response #####
If successful the method returns a 200 OK response with the updated resource. The response includes _id_ and _meta_ data as well as a location header
```
HTTP/1.1 200 OK
Content-Type: application/scim+json
Location: https://api.huddle.net/people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645
{
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
  "id": "78579b5f-39c2-4932-b72a-2485ae233645",
  "userName":"bjensen@huddle.net",
  "name":{
    "familyName": "Jensen",
    "givenName": "Barbara"
  },
  "phoneNumbers":[
    {
      "value":"555-555-8377"
    }
  ],
  "title":"Tour Guide",
  "preferredLanguage":"en-us",
  "timezone":"America/Los_Angeles",
  "meta":{
    "location":"https://api.huddle.net/people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645"
  },
}
```
#### XML example ####
##### Request #####
```
PUT /people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645 HTTP/1.1
Content-Type: application/xml
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Host: api.huddle.net
<?xml version="1.0" encoding="utf-8"?>
<User xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <schemas>
    <schema>urn:ietf:params:scim:schemas:core:2.0:User</schema>
  </schemas>
  <id>78579b5f-39c2-4932-b72a-2485ae233645</id>
  <userName>bjensen@huddle.net</userName>
  <name>
    <givenName>Barbara</givenName>
    <familyName>Jensen</familyName>
  </name>
  <phoneNumbers>
    <phoneNumber>
      <value>555-555-5555</value>
    </phoneNumber>
  </phoneNumbers>
  <title>Tour Guide</title>
  <preferredLanguage>en-us</preferredLanguage>
  <timezone>America/Los_Angeles</timezone>
</User>
```
##### Response #####
```
HTTP/1.1 200 OK
Content-Type: application/xml
Location: https://api.huddle.net/people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645
<?xml version="1.0" encoding="utf-8"?>
<User xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <schemas>
    <schema>urn:ietf:params:scim:schemas:core:2.0:User</schema>
  </schemas>
  <id>78579b5f-39c2-4932-b72a-2485ae233645</id>
  <userName>bjensen@huddle.net</userName>
  <name>
    <givenName>Barbara</givenName>
    <familyName>Jensen</familyName>
  </name>
  <phoneNumbers>
    <phoneNumber>
      <value>555-555-5555</value>
    </phoneNumber>
  </phoneNumbers>
  <title>Tour Guide</title>
  <preferredLanguage>en-us</preferredLanguage>
  <timezone>America/Los_Angeles</timezone>
  <meta>
    <location>https://api.huddle.net/people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645</Location>
  </meta>
</User>
```

##### Other Responses #####

|Case|Response|
|:---|:-------|
|400 Bad Request|Request is invalid|
|400 Bad request|SCIM id in url does not match id in request|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Actor does not have provisioning permissions|
|403 Forbidden|Actor is not in company|
|403 Forbidden|Email domain of provisioned user is not authorized for company|
|404 Not found|SCIM user not found|
|404 Not found|User does not have a member for the company|
|409 Conflict|The new requested username (email) is already in use|

---

### Query for SCIM User ###
To query for a SCIM user you can issue a GET request. Only the username attribute and the eq operator are supported.
#### JSON example ####
##### Request #####
```
GET /people/companies/123/users?filter=username eq "bjensen@huddle.net" HTTP/1.1
Accept: application/scim+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Host: api.huddle.net
```
##### Response #####
If successful the method returns a 200 with the matching SCIM user.
```
HTTP/1.1 200 OK
Content-Type: application/scim+json
{
  "schemas":["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
  "totalResults":1,
  "Resources":[{
    "id": "78579b5f-39c2-4932-b72a-2485ae233645",
    "userName":"bjensen@huddle.net",
    "name":{
      "familyName": "Jensen",
      "givenName": "Barbara"
    },
    "phoneNumbers":[
      {
        "value":"555-555-8377"
      }
    ],
    "title":"Tour Guide",
    "preferredLanguage":"en-us",
    "timezone":"America/Los_Angeles",
    "meta":{
      "location":"https://api.huddle.net/people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645"
    }
  }]
}
```
#### XML example ####
##### Request #####
```
GET /people/companies/123/users?filter=username eq "bjensen@huddle.net" HTTP/1.1
Accept: application/xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Host: api.huddle.net
```
##### Response #####
```
HTTP/1.1 200 OK
Content-Type: application/xml
<?xml version="1.0" encoding="utf-8"?>
<ListResponse xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Schemas>
    <string>urn:ietf:params:scim:api:messages:2.0:ListResponse</string>
  </Schemas>
  <TotalResults>1</TotalResults>
  <Resources>
    <User>
      <schemas>
        <schema>urn:ietf:params:scim:schemas:core:2.0:User</schema>
      </schemas>
      <id>78579b5f-39c2-4932-b72a-2485ae233645</id>
      <userName>bjensen@huddle.net</userName>
      <name>
        <givenName>Barbara</givenName>
        <familyName>Jensen</familyName>
      </name>
      <phoneNumbers>
        <phoneNumber>
          <value>555-555-8377</value>
        </phoneNumber>
      </phoneNumbers>
      <title>Tour Guide</title>
      <preferredLanguage>en-us</preferredLanguage>
      <timezone>America/Los_Angeles</timezone>
      <meta>
        <Location>https://api.huddle.net/people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645</Location>
      </meta>
    </User>
  </Resources>
</ListResponse>
```

##### Other Responses #####

|Case|Response|
|:---|:-------|
|400 Bad Request|Request is invalid|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Actor does not have provisioning permissions|
|403 Forbidden|Actor is not in company|
|403 Forbidden|Email domain of provisioned user is not authorized for company|

---

### Delete SCIM User ###
To delete a SCIM user you can issue a DELETE request. 
#### Example ####
##### Request #####
```
DELETE /people/companies/123/users/78579b5f-39c2-4932-b72a-2485ae233645 HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
Host: api.huddle.net
```
##### Response #####
If successful the method returns a 204.
```
HTTP/1.1 204 No Content
```
##### Other Responses #####

|Case|Response|
|:---|:-------|
|401 Unauthorized|Invalid authorization token|
|403 Forbidden|Actor does not have provisioning permissions|
|403 Forbidden|Actor is not in company|
|403 Forbidden|Email domain of provisioned user is not authorized for company|
|404 Not found|SCIM user not found|