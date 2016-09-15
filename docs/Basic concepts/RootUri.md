# Summary #

The root URI is the published URI of the Huddle API. This URI currently redirects to the [user resource](User) of the authenticated user.

If the authorization header is broken or invalid, we will respond as documented on the [authentication](Authentication) page.

The entry point is

```
https://api.huddle.net/entry
```

## Example ##

### Request ###

```
GET /entry HTTP/1.1
Host: api.huddle.net
Authorization: OAuth2 dingdongmerrily
```

### Response ###

```
HTTP/1.1 303 See Other
Location: /users/17654
```
