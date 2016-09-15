# Summary #

The invitations API describe the individual invitations created within Huddle.

### Check Invitation Status ###

An API endpoint which returns the invitation status of an invitation via the invitation code.
When the user clicks on the acceptance link from the invitation email, the user's UI will poll the endpoint until the invitation status has changed to accepted in case there is a delay in processing between Identity and People.
The endpoint has no permission restrictions but should only return the invitation status and workspace id.

#### Request ####

```
GET .../people/invitations/4a866af9-cfa9-4de2-9fd3-d4412f3bb021/status HTTP/1.1
Accept: application/vnd.huddle.data+xml (or +json)
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml (or+json)
```

```
<invitationstatus>
  <link
    rel="self"
    href="..." />
  <workspaceId>1041</workspaceId>
  <accepted>false</accepted>
</invitationstatus>
```

```
{
  "links": [
    {
      "rel": "self",
      "href": "..."
    }
  ],
  "workspaceId": 1041,
  "accepted": false
}
```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Invitation doesn't exist|404 Not found|
|Invitation code is malformed|405 Method not allowed|
|POST, DELETE, PUT used|405 Method not allowed|

## Properties ##

Invitation status is a complex resource

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this invitation status. | GET         |

## Schema ##

```
start = invitationstatus

invitationstatus = element h:invitationstatus {
  link+,
  workspaceId,
  accepted
}


```
