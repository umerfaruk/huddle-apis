# Provisional #
## The following link is a proposed API specification and is not currently implemented. ##

# Update notifications #

As an alternative to polling, Huddle offers clients the ability to subscribe to receive 'push notifications'.

## Clients ##

Huddle's push infrastructure is hosted within the cloud using signalR - a real-time technology from Microsoft. As such, whilst the protocol is an abstraction comprising a number of REST-ful APIs,

**it is strongly recommended to use any of the available signalR clients.**

Supported client technologies currently include .Net, Mono, JavaScript, and objective-C.

**Note: Currently, javascript clients are only supported in huddle domain (i.e. my.huddle.net).**

## Huddle Push Protocol (HuPP) - signalR initiated calls ##

HuPP takes the form of a series of REST calls to a known signalR endpoint.

All calls are authenticated. As signalR can operate over WebSockets, the token is passed via QueryString with the key name 'oauth-token'.

Any available HTTP header can also be used, but will only be valid using SSE or Long-polling. This uses standard OAUTH2 authentication (specify using the Authentication HTTP header, either 'Oauth2 ' or 'Bearer ' prefixed before token string)

All connections with the Huddle Push Protocol follow a negotiate/subscribe/publish pattern:

### Negotiate ###
```
GET https://push.huddle.env/objectUpdated/negotiate?clientProtocol=1.3 HTTP/1.1
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

or

```
GET https://push.huddle.env/objectUpdated/negotiate?clientProtocol=1.3&oauth_token=frootymcnooty/vonbootycherooty HTTP/1.1
```

Initial call to HuPP endpoint. Used to negotiate best available transport (Web Sockets over SSL - WSS, HTTP Server Sent Events over SSL - HTTPS-SSE, or HTTP Long Polling over SSL - HTTPS-LP)

A valid Connection Token (string) will be returned within the message body. This must be used with all subsequent HuPP calls.

At this point the client is able to make subscribe calls. No updates will be received by the client but a persistent connection is established.

### Subscribe ###

After a client has successfully negotiated with a HuPP endpoint, calls can be made to subscribe for notifications.

The following calls can be sent along a signalR persistentConnection and will receive published messages via the OnReceived event. The manual HTTP call is listed for completeness only.

```
POST https://push.huddle.env/send?transport=serverSentEvents&connectionToken=<URLEncoded_ConnectionToken>&oauth_token=frootymcnooty/vonbootycherooty HTTP/1.1 data={messageBody}
```

#### Subscribe Messages ####
The POST will include a JSON representation of an array of 'subscriptionType-subscriptionId' values:

```
{
   "requestType": "subscribe",
   "objects": "
   [
    {
         "subscriptionType":"Document",
         "subscriptionId":"https://file/selfUri"
    }
    ,{
         "subscriptionType":"User",
         "subscriptionId":"https://user/selfUri"
    }...
   ]
}
```

|JSON name|Description|
|:--------|:----------|
|requestType|currently only 'subscribe' is valid|
|subscriptionType|topic the we wish to receive notifications on. Currently accepted initial values are 'document' and 'user'|
|subscriptionId|id (or selfUri) of the subscription to receive published notifications upon. Should include scheme (e.g. https://api.huddle.net/files/documents/29861855) and can include multiple subscribes in a single call.|

Connection Token must also be specified, as well as a valid Huddle OAUTH token using header or QueryString.

### Publish ###
#### Published Messages ####

Upon successfully subscribing to receive a topic of change, any resource that is amended through the Huddle API will result in messages being broadcasted to HuPP subscribed clients.

The currently supported Topics are 'document' and 'user' these map to a range of subscription messages that offer greater granularity than the topic.

|subscriptionType|Possible Messages|
|:---------------|:----------------|
|document        |documentMetaDataChanged, documentContentUploaded (will include documentLocked)|
|user            |offlineItemCreated, offlineItemDeleted|

An event on the HuPP signalR connection will fire OnReceived with a string representation of the message body.

##### message-body format for documentContentUploaded #####
```
{
    "SubscriptionType": "Document",
    "SubscriptionId": "https://files/selfUri"
    "MessageType":"documentContentUploaded",
    "ObjectId":"https://file/selfUri"
}
```

|subscriptionType|the original type of object subscribed to |
|:---------------|:-----------------------------------------|
|subscriptionId  |id of the subscribed topic                |
|messageType     |Type of object the that has changed. currently valid values are documentContentUploaded / offlineItemCreated / offlineItemRemoved|
|objectId        |id (or selfUri) of the object that has changed (e.g. my.huddle.net/apigateway/files/documents/29861855)|


##### message-body format for documentMetaDataChanged #####
```
{
    "MessageType" : "documentMetaDataChanged",
    "ObjectId" : "https://file/selfUri",
    "DocumentMetaDataChangeType":"<documentUpdated, objectUpdated, etc.>",
    "SubscriptionType": "Document",
    "SubscriptionId" : "https://file/selfUri"
}
```

And currently DocumentMetaDataChangeTypes are:
  * **documentUpdated**: Pushed when new version of a document is created.
  * **objectUpdated**: Pushed when title or description of a document is updated.
  * **documentCheckedOut**: Pushed when document is locked.
  * **documentUndoCheckOut**: Pushed when document is unlocked.
  * **commentCreated**: Pushed when there is a new comment for a document.
  * **commentDeleted**: Pushed when someone deletes a comment.
  * **approvalAssignmentAdded**: Pushed when someone is assigned to approve a document.
  * **objectDeleted**: Pushed when document is deleted.
  * **approvalAssignmentCompleted**: Pushed when someone approves a document.
  * **approvalCompleted**: Pushed when document is approved by all users.
  * **documentMoved**: Pushed when document is moved.
  * **approvalUpdated**: Pushed when more users are requested to approve a document or some users are removed from approval list.
  * **documentVersionDeleted**: Pushed when a specific version of a document is deleted.
  * **documentPublished**: Pushed when document is published to outside of Huddle.
  * **documentUnpublished**: Pushed when document is unpublished.


##### message-body format for offlineItemCreated #####
```
{
    "MessageType" : "offlineItemCreated",
    "ObjectId" : "https://file/selfUri",
    "SubscriptionType": "1",
    "SubscriptionId" : "https://user/selfUri"
}
```


##### message-body format for offlineItemDeleted #####
```
{
    "MessageType" : "offlineItemDeleted",
    "ObjectId" : "https://file/selfUri",
    "SubscriptionType": "1",
    "SubscriptionId" : "https://user/selfUri"
}
```


### Examples ###
#### user example ####
**Subscribe**
```
{
   "requestType": "subscribe",
   "objects":
   [
      {
         "subscriptionType": "user",
         "subscriptionId": "https://users/123"
      }
    ]
}
```
**Published messages**
```
{
    {
       "subscriptionType": "user",
       "subscriptionId": "https://users/123"
       "messageType": "offlineItemAdded"
       "objectId": "https://files/offlineItem/6543"
    }
    {
       "subscriptionType": "user",
       "subscriptionId": "https://users/123"
       "messageType": "offlineItemRemoved"
       "objectId": "https://files/offlineItem/6543"
    }
}
```
#### document example ####
**Subscribe**
```
{
   "requestType": "subscribe",
   "objects":
   [
      {
         "subscriptionType": "document",
         "subscriptionId": "https://files/document/87654"
      }
    ]

}
```
**Published messages**
```
{
   "subscriptionType": "document",
   "subscriptionId": "https://files/document/87654"
   "messageType": "documentLocked"
   "objectId": "https://files/document/87654"
}

{
   "subscriptionType": "document",
   "subscriptionId": "https://files/document/87654"
   "messageType": "documentUnLocked"
   "objectId": "https://files/document/87654"
}

{
   "subscriptionType": "document",
   "subscriptionId": "https://files/document/87654"
   "messageType": "documentCommentAdded"
   "objectId": "https://files/document/87654"
}
```

### Connection lifetime ###
signalR clients provide a number of abstractions - from multi-protocol support and fallback support, to a comprehensive set of events that describe connectivity state

#### signalR Events ####
|connected|event fired in signalR client on successful connection (and negotiate)|
|:--------|:---------------------------------------------------------------------|
|reconnected|fired after a short-term disconnection  (e.g. network blip). All existing subscriptions **should** be maintained.|
|disconnected|No more updates will be received, client should re-negotiate and re-subscribe for notifications.|
|connectionslow|The heartbeat from client to server was not received in the expected time|
|error    |an error occurred on the connection                                   |

### Notification History and Lifetime ###
No history or store of notifications - HuPP is **not** reliable messaging.
Therefore any client that experiences a break in connectivity should 'GET' the relevant resource first before subscribing for notifications - otherwise a historic change may not be accounted for.
