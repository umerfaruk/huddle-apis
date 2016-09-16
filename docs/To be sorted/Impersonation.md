# Start an impersonation session

Request
```
POST https://login.huddle.net/impersonation    HTTP/1.1

... a cryptographically signed request ...
```

Repsonse
```
HTTP/1.1 200 OK

{
    "access_token":...,
    "expires_in": 1200,
    "links": [{"rel":"self", "href": "https://login.huddle.net/impersonation/e014c954-12ce-429c-a2e2-a3671e136ba9"}]
}
```

# End an impersonation session

Request
```
DELETE https://login.huddle.net/impersonation/e014c954-12ce-429c-a2e2-a3671e136ba9    HTTP/1.1
Host: login.huddle.net
Authorization: ... an impersonation access token ...
```

Response
```
HTTP/1.1 204 No Content
Set-Cookie: auth_token=;max-age=0
```