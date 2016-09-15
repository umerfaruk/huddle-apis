# Summary #

This endpoint tells you which users were involved in a share action.  See [document share](Document#Sharing_a_document) and [folder share](Folder#Sharing_a_folder).

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Get Share Information|Beta        |

# Operations #

## Get Share Information ##

Each share has a unique id of type Guid.  When a share has been performed you will be able to retrieve links to individual shares from the [Document Audit trail](AuditTrail) API.  Note that currently folders do not have an audit trail endpoint.

The response

### Example ###

#### Request ####
```
GET /notifications/shares/565eee8d-7d34-41e1-9c48-19bcc08ccb32 HTTP/1.1
```

#### Response ####
```
HTTP/1.1 200 OK
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<shareInformation>
  <people
    name="Mr Pink"
    email="mr.pink@reservoirdogs.com"
    rel="recipient">
    <link
      rel="self"
      href="/users/3" />
    <link
      rel="avatar"
      href="/users/3/avatar"
      type="image/jpeg" />
    <link
      rel="alternate"
      href="/user/mrpink"
      type="text/html" />
  </people>
  <people
    name="Mr White"
    email="mr.white@reservoirdogs.com"
    rel="recipient">
    <link
      rel="self"
      href="/users/4" />
    <link
      rel="avatar"
      href="/users/4/avatar"
      type="image/jpeg" />
    <link
      rel="alternate"
      href="/user/mrwhite"
      type="text/html" />
  </people>
</shareInformation>
```


---

### Schema ###

```
start = shareInformation

shareInformation = element people{
        actor+
}

actor = element actor { 

	attribute name { xsd:string },
	attribute email { xsd:string },
	attribute rel { xsd:string },

	link +
}
```
