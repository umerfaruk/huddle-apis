# Summary #
The Company API is a representation of Companies within Huddle.
## Beta warning ##
The Company API is in Beta
# Operations #
## Retrieve a given company ##

You can GET the company for a given ID and it will display the representation for a single Company.

### XML Example ###

In this example we are asking for the company with id 123
#### Request ####
```
GET /people/companies/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/xml
```
```
<company>
  <link rel="self" href="..." />
  <link rel="members" href="..." />
  <link rel="members-search" ..." />
  <link rel="members-export" href="..." />
  <link rel="businessUnits" href="..." /> 
  <link rel="workspaces" href="..." />
  <link rel="workspaces-export" href="..." />
  <link rel="managers" href="..." />
  <link rel="bulk-delete" href="..." />
  <link rel="published-documents" href="..." />
  <link rel="whitelist" href="..." />
  <link rel="edit" href="..." />
  <link rel="internal-domains" href="..." />
  <link rel="settings" href="..." />
  <name>Huddle</name>
</company>
```

### JSON Example ###

In this example we are asking for a company specifying json in the accept header.

#### Request ####
```
GET /people/companies/123 HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
{
  "links" : [
    { "rel": "self", "href": "..."},
    { "rel": "members", "href": "..."},
    { "rel": "members-search", "href": "..."},
    { "rel": "members-export", "href": "..."},
    { "rel": "businessUnits", "href": "..."},
    { "rel": "workspaces", "href": "..."},
    { "rel": "managers", "href": "..."},
    { "rel": "bulk-delete", "href": "..."},
    { "rel": "published-documents", "href": "..."},
    { "rel": "whitelist", "href": "..."},
    { "rel": "edit", "href": "..."},
    { "rel": "internal-domains", "href": "..." },
    { "rel": "settings", "href": "..." },
  ],
  "name" : "Huddle"
}
```

### Other Responses ###
|Case|Response|
|:---|:-------|
|401 Unauthorized|Invalid authorization token|
|404 Not Found|Company does not exist|

---

## Retrieve all companies ##

You can GET all companies you manage by making a call to the company endpoint and not supplying an ID.

### XML Example ###

In this example we are asking for all companies.

#### Request ####
```
GET /people/companies HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<companies>
<link rel="self" href="people/companies" />
<company>
  <link rel="self" href="people/companies/123" />
  <link rel="members" href="people/companies/123/members" />
  <link rel="members-search" href="people/companies/123/members/search" />
  <link rel="businessunits" href="people/companies/123/businessunits" /> 
  <link rel="workspaces" href="people/companies/123/workspaces" />
  <link rel="managers" href="people/companies/123/managers" />
  <link rel="bulk-delete" href="people/companies/123/bulkdelete" />
  <link rel="published-documents" href="publishing/companies/123" />
  <link rel="whitelist" href="people/companies/123/whitelist" />
  <link rel="edit" href="..." />
  <link rel="internal-domains" href="..." />
  <link rel="settings" href="..." />
  <name>Huddle</name>
</company>
<company>
  <link rel="self" href="people/companies/987" />
  <link rel="members" href="people/companies/987/members" />
  <link rel="members-search" href="people/companies/987/members/search" />
  <link rel="businessunits" href="people/companies/987/businessunits" /> 
  <link rel="workspaces" href="people/companies/987/workspaces" />
  <link rel="managers" href="people/companies/987/managers" />
  <link rel="bulk-delete" href="people/companies/987/bulkdelete" />
  <link rel="published-documents" href="publishing/companies/987" />
  <link rel="whitelist" href="people/companies/987/whitelist" />
  <link rel="edit" href="..." />
  <link rel="internal-domains" href="..." />
  <link rel="settings" href="..." />
  <name>AnotherCompany</name>
</company>
</companies>
```


### JSON Example ###

In this example we are asking for all companies specifying json in the accept header.

#### Request ####
```
GET /people/companies HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####


```

  {
    "links" : [{
      "rel" : "self",
      "href" : "people/companies"
    }],
    "companies" :  [{
      "links" : [{
        "rel" : "self",
        "href" : "people/companies/123"
      },{
        "rel" : "members",
        "href" : "people/companies/123/members"
      },{
      },{
        "rel" : "members-search",
        "href" : "people/companies/123/members/search"
      },{
      "rel" : "businessunits",
      "href" : "people/companies/123/businessunits"
      },{
        "rel" : "workspaces",
        "href" : "people/companies/123/workspaces"
      },{
        "rel" : "managers",
        "href" : "people/companies/123/managers"
      },{
        "rel" : "bulk-delete",
        "href" : "people/companies/123/bulkdelete"
      },{
        "rel" : "published-documents",
        "href" : "publishing/companies/123"
      },{
        "rel" : "whitelist",
        "href" : "people/companies/123/whitelist"
      },{ 
        "rel": "edit", 
        "href": "..."
      },{ 
        "rel": "internal-domains", 
        "href": "..." 
      }],
      "name" : "Huddle"
    },{
      "links" : [{
        "rel" : "self",
        "href" : "people/companies/987"
      },{
        "rel" : "members",
        "href" : "people/companies/987/members"
      },{
        "rel" : "members-search",
        "href" : "people/companies/987/members/search"
      },{
        "rel" : "businessunits",
        "href" : "people/companies/987/businessunits"
      },{
        "rel" : "workspaces",
        "href" : "people/companies/987/workspaces"
      },{
        "rel" : "managers",
        "href" : "people/companies/987/managers"
      },{
        "rel" : "bulk-delete",
        "href" : "people/companies/987/bulkdelete"
      },{
        "rel" : "published-documents",
        "href" : "publishing/companies/987"
      },{
        "rel" : "whitelist",
        "href" : "people/companies/987/whitelist"
      },{ 
        "rel": "edit", 
        "href": "..."
      },{ 
        "rel": "internal-domains", 
        "href": "..." 
      }],{ 
        "rel": "settings", 
        "href": "..." 
      }]
      "name" : "AnotherCompany"
    }]
  }

```


---


## Add company manager ##

To make a member a manager of a company see https://github.com/Huddle/huddle-apis/wiki/CompanyManagers


## Remove company manager ##

For removing manager permissions for a member see https://github.com/Huddle/huddle-apis/wiki/CompanyManagers
