# Summary #

Members represent a member of a Company within Huddle.

## Beta warning ##

The Member API is in Beta

|  |
|:-|

# Operations #


---

## Retrieving a Member ##

When [retrieving Members of a Company](Members#Retrieve_members_of_a_given_company), the response will contain the list of Members within the Company. Each Member will advertise a self link, which you can use to GET the full Member.

#### Request ####
```
GET /people/companies/123/members/456
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

To retrieve a full list of Timezones, use see [Timezones](Timezones). For language codes, see [Language Codes](LanguageCodes)

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<member>
  <link rel="self" href="..." />
  <link rel="edit" href="..." />
  <link rel="delete" href="..." />
  <link rel="company" href="..." />
  <link rel="user" href="..." />
  <link rel="avatar" href="..." />
  <link rel="profile" href="..." />
  <link rel="companyManager" href="..." />

  <firstName>Andy</firstName>
  <lastName>McLoughlin</lastName>
  <displayName>AndyMcLoughlin</displayName>
  <company>Huddle</company>
  <companyUrl>http://www.huddle.com</companyUrl>
  <role>Founder</role>
  <businessAddress>Some Address, Some City, A12 3BC</businessAddress>
  <telephone>(01234)567890</telephone>
  <mobile>(07891)234567</mobile>
  <imAddress>andym</imAddress>
  <professionalWebsite>http://www.linkedin.com/in/andymcloughlin</professionalWebsite>
  <timezone>Europe/London</timezone>
  <languageCode>en-GB</languageCode>
  <internal>true</internal>
  <companyManager>true</companyManager>
  <lastLoginDate>2013-04-04T23:50:34</lastLoginDate>
</member>
```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Member does not exist|404 Not Found|
|Invalid fields|400 Bad Request|


---


## Editing a member ##

If the authenticated user is authorized to edit a Member, it will advertise a link with a @rel value of _edit_. To update the Member submit a PUT request to this URI with the editable fields of the Member. For an overview of editing resource in Huddle see [editing resources](Resources#Editing_Resources).

### Example ###

A GET request on the _edit_ link of a Member will return a resource with the editable fields of the Member.

#### Request ####

```
GET /people/companies/123/members/456/edit HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<member>
  <link rel="self" href="..." />
  <link rel="parent" href="..."/>
  
  <firstName>Andy</firstName>
  <lastName>McLoughlin</lastName>
  <company>Huddle</company>
  <companyUrl>http://www.huddle.com</companyUrl>
  <role>Founder</role>
  <businessAddress>Some Address, Some City, A12 3BC</businessAddress>
  <telephone>(01234)567890</telephone>
  <mobile>(07891)234567</mobile>
  <imAddress>andym</imAddress>
  <professionalWebsite>http://www.linkedin.com/in/andymcloughlin</professionalWebsite>
  <timezone>Europe/London</timezone>
  <language>en-GB</language>
</member>
```

The fields can then be updated in the editable Member, which can then be PUT back to the server:

#### Request ####

```
PUT /people/companies/123/members/456/edit HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<member>
  <firstName>Alastair</firstName>
  <lastName>Mitchell</lastName>
  <company>Huddle</company>
  <companyUrl>http://www.huddle.com</companyUrl>
  <role>Founder</role>
  <businessAddress>Some Address, Some City, A12 3BC</businessAddress>
  <telephone>(01234)567890</telephone>
  <mobile>(07891)234567</mobile>
  <imAddress>alim</imAddress>
  <professionalWebsite>http://www.linkedin.com/in/andymcloughlin</professionalWebsite>
  <timezone>Europe/London</timezone>
  <language>en-GB</language>
</member>
```

#### Response ####

If successful, this operation will return a 204 No content, with a [link header](Link#Header) giving the location of the updated document.

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```
HTTP/1.1 204 No Content
Link <...>;rel="parent"
```


---

## Deleting a member ##

If the authenticated user is authorized to delete, there will be a link with a @rel value of _delete_. To delete the Member, send a DELETE request to the _delete_ URI.

### Example ###
#### Request ####
```
DELETE /people/companies/123/members/456 HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

If the delete is successful, this method will return an empty response with an 204 No Content status code

```
HTTP/1.1 204 No Content
```


---

## Make member a company manager ##

To make a member a manager of a company see https://code.google.com/p/huddle-apis-dev/wiki/CompanyManagers#Assign_Company_Manager_permissions


## Remove company manager permission ##

For removing manager permissions for a member see https://code.google.com/p/huddle-apis-dev/wiki/CompanyManagers#Revoke_Company_Manager_permissions
