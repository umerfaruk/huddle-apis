# Summary #

SAML Patners are companies that give access to Huddle via their Single-Sign-On IDP. This endpoint allows managing SAML Partners.

Currently this endpoint can only be consumed with admin permissions.

|  |
|:-|

# Operations #
## Creating new SAML partner ##

You can add a new SAML partner with a POST request to the saml partners endpoint.

### Example ###
#### Request ####
```
POST /samlpartners HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
 {
   "name":"saml partner name",
   "ssoServiceUri":"http://sso.uri",
   "friendlyName":"saml partner friendly name",
   "encodedCertificate":"base 64 encoded certificate",
   "allowAutoProvisioning": true,
   "signingAlgorithm": "SHA256"
 }
```

### Response ###
If POST is successful, this method will return a 201 Created with the Location header pointing to the created SAML partner.

```
HTTP/1.1 201 Created
Location: /samlpartners/123
```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Actor does not have admin permission|403 Forbidden|
|Specified SAML partner name already exists|409 Conflict|


---


## Updating a SAML Partner ##

You can edit a SAML partner with a PUT request to the saml partners endpoint.


### Example ###
#### Request ####
```
PUT /samlpartners/123 HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
 {
   "name":"saml partner name",
   "ssoServiceUri":"http://sso.uri",
   "friendlyName":"saml partner friendly name",
   "encodedCertificate":"base 64 encoded certificate",
   "allowAutoProvisioning": true,
   "signingAlgorithm": "SHA1"
 }
```

#### Response ####

If the update is successful, this method will return 200 OK status code.

```
HTTP/1.1 200 OK
```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invalid authorization token|401 Unauthorized|
|Actor does not have admin permission|403 Forbidden|
|Requested SAML Partner Id does not exist|404 Not Found|
|Specified SAML partner name already exists|409 Conflict|
