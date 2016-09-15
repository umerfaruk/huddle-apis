# Summary (BETA)#

The Session Timeout API which logs the user out of its current session. 

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|SessionTimeout|200   (or 204?)      |

# Operations #

## Logout ##

This API is called by the my.huddle web client when user's session has timed out. The API endpoint extracts the AccessToken from either the header or cookie which provides the UserId, ClientId and RefreshToken. The RefreshToken is then used to revoke the single AccessGrant of the session and the cookie is then expired. 

The API will return a 200 if it successfully retrieves the AccessToken or if no AccessToken is sent. However, if a token is sent and it is malformed, the API will return a 401. In the case where no AccessToken is sent we just expire the cookie.

### Example ###

#### Request ####

```
POST /sessiontimeout
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