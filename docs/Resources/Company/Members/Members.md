# Summary #

The Members API is a representation of members of a company within Huddle.

## Beta warning ##

The company API is in Beta

|  |
|:-|

# Operations #

## Retrieve members of a given company ##

You can GET the members for a given company and it will display the first page of members for that company.

### XML Example ###

In this example we are asking for the members of a company with id 123

#### Request ####
```
GET /people/companies/123/members HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<members>
  <link rel="self" href="people/companies/123/members" />
  <link rel="search" href="people/companies/123/members/search"/>

  <member>
    <link rel="self" href="..." />
    <link rel="edit" href="..." />
    <link rel="delete" href="..." />
    <link rel="company" href="..." />
    <link rel="user" href="..." />
    <link rel="avatar" href="..." />
    <link rel="profile" href="..." />

    <firstName>Andy</firstName>
    <lastName>McLoughlin</lastName>
    <displayName>Andy</displayName>
    <emailAddress>andy@huddle.com</emailAddress>
    <role>Founder</role>
    <internal>true</internal>
    <companyManager>false</companyManager>
    <lastLoginDate>2013-04-04T23:50:34</lastLoginDate>
  </member>
  <member>
    <link rel="self" href="..." />
    <link rel="edit" href="..." />
    <link rel="delete" href="..." />
    <link rel="company" href="..." />
    <link rel="user" href="..." />
    <link rel="avatar" href="..." />
    <link rel="profile" href="..." />
    <link rel="companyManager" href="..." />
    
    <firstName>Ali</firstName>
    <lastName>Mitchell</lastName>
    <displayName>Ali</displayName>
    <emailAddress>ali@huddle.com</emailAddress>
    <role>Founder</role>
    <internal>true</internal>
    <companyManager>true</companyManager>
    <lastLoginDate>2013-04-04T23:50:34</lastLoginDate>
  </member>
  <filteredMembers>2</filteredMembers>
  <totalMembers>2</totalMembers>
</members>
```


### JSON Example ###

In this example we are asking for a company specifying json in the accept header.

#### Request ####
```
GET /people/companies/123/members HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```

  {
    "links" : [{
      "rel" : "self",
      "href" : "people/companies/123/members"
    },{
      "rel": "search",
      "href": "people/companies/123/members/search"
    }],
    "members" : [{
      "links" : [{
        "rel" : "self",
        "href" : "..."
      }, {
        "rel" : "edit",
        "href" : "..."
      }, {
        "rel" : "delete",
        "href" : "..."
      }, {
        "rel" : "company",
        "href" : "..."
      }, {
        "rel" : "user",
        "href" : "..."
      }, {
        "rel" : "avatar",
        "href" : ...
      }, {
        "rel" : "profile",
        "href" : ...
      }],
      "firstName" : "Andy",
      "lastName" : "McLoughlin",
      "displayName" : "Andy",
      "emailAddress" : "andy@huddle.com",
      "role" : "Founder",
      "internal" : true,
      "companyManager" : false,
      "lastLoginDate" : "Tue, 14 Aug 2012 10:59:39 GMT"
    },{
      "links" : [{
        "rel" : "self",
        "href" : "..."
      }, {
        "rel" : "edit",
        "href" : "..."
      }, {
        "rel" : "delete",
        "href" : "..."
      }, {
        "rel" : "company",
        "href" : "..."
      }, {
        "rel" : "user",
        "href" : "..."
      }, {
        "rel" : "avatar",
        "href" : ...
      }, {
        "rel" : "profile",
        "href" : ...
      }, {
        "rel" : "companyManager",
        "href" : ...
      }],
      "firstName" : "Ali",
      "lastName" : "Mitchell",
      "displayName" : "Ali",
      "emailAddress" : "ali@huddle.com",
      "role" : "Founder",
      "internal" : true,
      "companyManager" : true,
      "lastLoginDate" : "Tue, 14 Aug 2012 10:59:39 GMT",
    }],
    "filteredMembers": 2,
    "totalMembers": 2
  }

```

## Parameters ##
| **Name** | **Description** | **Methods** | **Optional** | **Default** |
|:---------|:----------------|:------------|:-------------|:------------|
| page     | Page that you are requesting | GET         | Yes          | 1           |
| q        | Query with which to filter results. Space delimited query terms will return results that match all terms across first name, last name, display name, email address, email domain and role. Multiple terms constitute a phrase. Comma delimited phrases will match across any phrases. | GET         | Yes          |             |
| sort     | Sorting. Can either be in the form of fieldName, or fieldName:asc/fieldName:desc. If direction is omitted, direction will be asc. Can sort on displayname or lastlogindate fields. | GET         | Yes          | displayname:asc |

Request with parameter
```
GET /people/companies/123/members?page=1&q=ali mitchell,andy&sort=displayname:desc HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this member list. | GET         |
| search   | Enpoint used to search members of the company | POST        |
| next     | The URI of the next page of members, using sort order specified. | GET         |
| prev     | The URI of the previous page of members, using sort order specified. | GET         |
| first    | The URI of the first page of members, using sort order specified.  | GET         |
| last     | The URI of the last page of members, using sort order specified. | GET         |

The 'prev' link is not present on the first page of members.

The 'next' link is not present on the last page of members.

The 'last' and 'first' links are not present if there is only a single page.

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Actor does not have company manager permissions|403 Forbidden|
|Requested page does not exist (greater than last page)|404 Not Found|
|Requested page is invalid, less than 1|400 Bad Request|
|Specified fieldName for sort does not exist or is not available for sorting|400 Bad Request|
|Sort order is not :asc or :desc|400 Bad Request|


---

## Searching members ##
The members resource can be search by sending a POST to the search endpoint. This will create a cached search resource on the server which can subsequently be used to GET the search results.

### Example ###
#### Request ####
```
POST /people/companies/123/members/search HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
q=Andy
```

#### Response ####

If POST is successful, this method will return a 201 Created with the Location header pointing to the created search.

```
HTTP/1.1 201 Created
Location: /people/companies/123/members/search/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Content-Type: application/vnd.huddle.data+xml
```
```
<memberSearch>
  <link rel="self" href="/people/companies/123/members/search/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" />
  <query>Andy</query>
</memberSearch>
```
#### Request ####

Following the response from the POST we have the location of the search that we can issue a GET against to retrieve the results.
```
GET /people/companies/123/members/search/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Expires: Thu, 04 Dec 2014 10:59:39 GMT
```
```
<members>
  <link rel="self" href="/people/companies/123/members/search/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" />
  <link rel="search" href="people/companies/123/members/search"/>

  <member>
    <link rel="self" href="..." />
    <link rel="edit" href="..." />
    <link rel="delete" href="..." />
    <link rel="company" href="..." />
    <link rel="avatar" href="..." />
    <link rel="profile" href="..." />

    <firstName>Andy</firstName>
    <lastName>McLoughlin</lastName>
    <displayName>Andy</displayName>
    <emailAddress>andy@huddle.com</emailAddress>
    <role>Founder</role>
    <internal>true</internal>
    <companyManager>false</companyManager>
    <lastLoginDate>2013-04-04T23:50:34</lastLoginDate>
  </member>
  <filteredMembers>1</filteredMembers>
  <totalMembers>2</totalMembers>
</members>
```

### Paging and Sorting a Search Query ###

As with the standard Members response we can sort and page the responses if this is available on the result set it will be exposed on the response in the links section.

#### Request ####
```
GET /people/companies/123/members/search/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx?page=2 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Request ####
```
GET /people/companies/123/members/search/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx?sort=lastlogindate:desc HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```


---


## Deleting a member ##

This resource supports deleting individual members. If the authenticated user can delete a member, each member resource will advertise a link with a @rel value of _delete_. To delete a member, send a DELETE request to the _delete_ URI.


### Example ###
#### Request ####
```
DELETE /people/companies/123/members/256 HTTP/1.1
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


---

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this members list. | GET         |
| next     | The URI of the next set of members ordered from ??? | GET         |
| prev     | The URI of the previous set of members ordered from ??? | GET         |
| first    | The URI of the first page of members. | GET         |
| last     | The URI of the last page of members. | GET         |
