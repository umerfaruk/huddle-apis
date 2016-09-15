# Summary #

It is possible for a user to share a resource with a set of specified recipients.

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Sharing a resource|Live        |

# Resources that support Sharing #
  * [Document](http://code.google.com/p/huddle-apis-dev/wiki/Document#Sharing_a_Document)
  * [Folder](http://code.google.com/p/huddle-apis-dev/wiki/Folder#Share_a_folder)

# Operations #

## Sharing a resource ##

If the resource supports sharing, it will advertise a link with @rel value of _share_. For example, to share a document, issue a POST request to the _share_ URI. The request body should contain a list of recipients and an optional message.

The request may optionally include an X-Allow-Invalid-Recipients header, which should be true or false (these are case insensitive). It determines whether the share request should fail (returning a 400 BAD REQUEST) if _any_ of the users cannot be shared with for some reason, or just return a 202 ACCEPTED to indicate that the share request will be sent to as many users as possible.

See the response details below for more information on what information is included in a 400 BAD REQUEST response.

For share document, once it has been processed, the share information is available from the [audit trail](AuditTrail).

### Example ###

#### Request (with a message) ####

```
POST /documents/123/share HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

##### XML

```xml
<share>
    <link rel="recipient" href="..." />
    <link rel="recipient" href="..." />
    <link rel="recipient" href="..." />
    <message>Hey, take a look at this exhilarating TPS report!</message>
</share>
```

##### JSON

```json
{
    "recipients" : [
        {
            "rel" : "recipient",
            "href" : "..."
        },{
            "rel" : "recipient",
            "href" : "..."
        },{
            "rel" : "recipient",
            "href" : "..."
        }
    ],
    "message" : "Hey, take a look at this exhilarating TPS report!"
}
```

#### Request (without a message) ####

```
POST /documents/123/share HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

##### XML
```xml
<share>
    <link rel="recipient" href="..." />
    <link rel="recipient" href="..." />
    <link rel="recipient" href="..." />
</share>
```

##### JSON
```json
{
    "recipients" : [
        {
            "rel" : "recipient",
            "href" : "..."
        },{
            "rel" : "recipient",
            "href" : "..."
        },{
            "rel" : "recipient",
            "href" : "..."
        }
    ]
}
```

#### Request (optional header) ####

```
POST /documents/123/share HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
X-Allow-Invalid-Recipients: true
```


### Properties ###

| **Name** | **Description** |
|:---------|:----------------|
| message  | An optional message to attach to the share which the recipients will see. Max length is 5000 characters. |

## Link relations ##

| **Name** | **Description** |
|:---------|:----------------|
| recipient | The URI of a user with whom the resource should be shared. |

#### Response ####
If successful the request will return a 202 ACCEPTED.

If the resource could not be shared with any of the recipients, the share will not take place and the response will return a 400 BAD REQUEST with a list of the recipients with whom the resource could not be shared.

If the message is longer than 5000 characters then the share will not take place and the response will return a 400 BAD REQUEST.

You will receive a link header to the share information resource.

##### Successful Response #####

```
HTTP/1.1 202 ACCEPTED
Content-Type: application/vnd.huddle.data+xml
Link: </notification/shares/5df96158-5f6c-4d7f-a716-7877ad92c560>;rel="share-information"
```

##### Failed Response #####

```
HTTP/1.1 400 BAD REQUEST
Content-Type: application/vnd.huddle.data+xml
```

```
<shareFailure>
    <reason>The reason was...</reason>
    <link rel="recipient" href="..." />
</shareFailure>
```

## Schema ##

```
start = share

share= element h:share{
  link+,
  element h:message{xsd:string}
}
```
