# Summary #

The Logout API signs the user out on the device they were using. 

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Logout |200         |

# Operations #

## Logout ##

When logging out, the endpoint extracts the AccessToken from either the header or cookie which provides the UserId and ClientId. The information is then used to revoke the AccessGrant and the cookie is then expired. 

The API will return a 200 if it successfully retrieves the AccessToken or if no AccessToken is sent. However, if a token is sent and it is malformed, the API will return a 401. 

### Example ###

#### Request ####

```
POST /logout
Accept: application/vnd.huddle.data+xml (or+json)
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```

## Parameters 
NA

#### Other Responses ####
|Case|Response|
|:---|:-------|
|Invalid Token sent|401|
|Non POST call|405|