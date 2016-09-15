## Proposal warning ##

This is a proposed API specification and is not currently implemented.

## Beta warning ##

The provisioning flow is in Beta

|  |
|:-|

# Operations #

## Monitoring API ##

API to monitor the state of the auto-provisioning process

#### Request ####
```
GET /provision/monitor/e2eecad481ca47fe850ffd230283a069 HTTP/1.1
Accept: application/vnd.huddle.data+xml
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/xml
Link: /next-step
```

The link header will contain the URI the client will have to follow. It will be the same URI as the original request until the user has been provisioned completely, at which point it will be the Authentication endpoint URI.

## Saml Authentication API ##

API to authenticate or provision a user via SAML

#### Request ####
```
GET /saml/authentication/e2eecad481ca47fe850ffd230283a069 HTTP/1.1
Accept: application/vnd.huddle.data+xml
```

#### Response ####

```
HTTP/1.1 302 Found
Content-Type: application/xml
Link: </provision/monitor/e2eecad48>; rel=monitor
```

The link header will contain the URI the client will have to follow.
The rel property specifies the resource the link is related to. It can be either "monitor" or "auth-callback"
