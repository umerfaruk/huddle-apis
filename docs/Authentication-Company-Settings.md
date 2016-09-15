# Summary #
The Company Settings API allows company managers to enable or disable authentication company settings for the company. Note this endpoint does not go via the apigateway
## Beta warning ##
The Company Settings Pin API is in Beta
# Operations #
## Retrieve the settings ##

As a company manager, I can get the settings of my company.

### XML Example ###

In this example we are asking for the settings from the company with id 123
#### Request ####
```
GET /identity/companies/123/settings HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/xml
```
```
<companysettings>
  <link rel="self" href="..." />
  <link rel="edit" href="..." />  
  <settings>
    <setting>
      <name>TwoFactorAuth</name>
      <enabled>true</enabled>
    </setting>
   </settings>
</companysettings>
```

### JSON Example ###

In this example we are asking for a company's settings whilst specifying json in the accept header.

#### Request ####
```
GET /identity/companies/123/settings HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
{
  "links": [
    {
      "rel": "self",
      "href": ".../companies/123/settings"
    },
    {
      "rel": "edit",
      "href": ".../companies/123/settings/edit"
    }
  ],
  "settings": [
    {
      "links": [
        {
          "rel": "self",
          "href": ".../companies/123/settings/twofactorauth"
        }
      ],
      "name": "TwoFactorAuth",
      "enabled": true
    }
  ]
}
```

### Link relations ###
|Name|Description|Methods|
|:---|:-------|:------|
|self|The current URI of this company's settings|GET|
|edit|The URI to update the settings for the company|PUT|

### Other Responses ###
|Case|Response|
|:---|:-------|
|401 Unauthorized|Invalid authorization token|
|403 Not Found|Not a company manager|
---
