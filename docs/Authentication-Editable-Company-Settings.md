# Summary #
The GET operation shows which fields are editable and provides a template for the PUT operation, which updates the company settings. The link for editing the settings can be found by retrieving the settings. Note these requests do not 
## Beta warning ##
The Company Settings API is in Beta
# Operations #
## Retrieve the editable company settings ##

If the company settings has not been created for a Company then a default template, which has  as disabled, will be returned.

### XML Example ###

In this example we are asking for the setting from the company with id 123
#### Request ####
```
GET /identity/companies/123/settings/edit HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/xml
```
```
<settings>
  <setting>
    <name>MobilePin</name>
    <enabled>true</enabled>
  </setting>
</settings>
```

### JSON Example ###

#### Request ####
```
GET /identity/companies/123/settings/edit HTTP/1.1
Accept: application/vnd.huddle.data+json
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
{
  settings: [
              {
                "name" : "TwoFactorAuth",
                "enabled" : true
              }
            ]
}
```

### Other Responses ###
|Case|Response|
|:---|:-------|
|401 Unauthorized|Invalid authorization token|
|404 Not Found|Not a company manager|
|404 Not Found|Company does not exist|

---

# Creating or updating the settings #
A PUT to the settings API will create or update it (overwriting the previous settings setup for the company) to the contents of the request.
The Actor must be a Company Manager to update the company settings.

## Beta warning ##
The Company Settings API is in Beta
# Operations #
## Retrieve the editable settings ##

If the company settings has not been created for a Company then a default template, which has the settings as disabled, will be returned.

### XML Example ###

#### Request ####
```
PUT /identity/companies/123/settings/edit HTTP/1.1
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
<settings>
  <setting>
    <name>TwoFactorAuth</name>
    <enabled>true</enabled>
  </setting>
</settings>
</companysettings>
```

### JSON Example ###

#### Request ####
```
PUT /identity/companies/123/settings/edit HTTP/1.1
Accept: application/vnd.huddle.data+json
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
{
  settings: [
              {
                "name" : "TwoFactorAuth",
                "enabled" : true
              }
            ]
}
```

### Other Responses ###
|Case|Response|
|:---|:-------|
|400 Unauthorized|Invalid company settings type|
|401 Unauthorized|Invalid authorization token|
|404 Not Found|Not a company manager|
|404 Not Found|Company does not exist|

---