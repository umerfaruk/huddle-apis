# Summary #

Notifications are alerts of changes to content in Huddle. These include such events as new comments being added to a Document a user has created, requests for approval, and Documents that an author chose to share with other users.

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Get Unseen Notification Status|Beta        |
|Set Last Seen Notification|Beta        |
|Get Notifications|Beta        |

# Operations #


---

## Get Unseen Notification Status ##

This returns a count of unseen Notifications, the URI of the last Notification seen, and the date/time for that Notification.

An If-Modified-Since header can be specified in the Request.  If there are no changes since the client last requested the Document (determined by the If-Modified-Since header) then HTTP 304 Not Modified will be returned, with no resource (body). This is useful for [Caching](Caching).

### Example ###

#### Request ####
```
GET /notifications/user/123/received/last-seen HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Last-Modified: Thu, 24 Apr 2013 11:15:22 GMT
```

```
<notifications>
	<lastNotificationSeen>
		<id>...</id>
		<created>2013-04-24T11:15:22Z</created>
	</lastNotificationSeen>
	<unseenCount>1</unseenCount>
	<link rel="alternate" title="Received Notifications">
	<link rel="self" href="...">
</notifications>
```

### Schema ###

```

start = notifications

notifications = element notifications {  
	link+,
	element h:unseenCount {xsd:unsignedInt},
	lastNotificationSeen,
}

lastNotificationSeen = element h:lastNotificationSeen {
	id,
	element h:created {xsd:dateTime}
}

```


---

## Set Unseen Notification Status ##

This sets the last Notification seen. For this request, a PUT method is used because this is not the persisting of a completely new resource, the client is only updating which Notification was seen last. The date/time that this Notification was created and the `unseenCount` are both maintained by the API.

The schema for this is the same as that returned from the GET request above.

### Example ###

#### Request ####
```
PUT /notifications/user/123/received/last-seen HTTP/1.1
Accept: application/vnd.huddle.data+xml
Content-Type: application/vnd.huddle.data+xml
Content-Length: 106
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```
<notifications>
	<lastNotificationSeen>
		<id>...</id>
	</lastNotificationSeen>
</notifications>
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```

```
<notifications>
	<lastNotificationSeen>
		<id>...</id>
		<created>2013-04-24T11:15:22Z</created>
	</lastNotificationSeen>
	<unseenCount>1</unseenCount>
	<link rel="alternate" title="Received Notifications">
	<link rel="self" href="...">
</notifications>
```


---

## Get notifications by page ##

This returns the current page (subscription document) of an Atom Feed of Notifications. The current page is dynamic, containing the latest Notifications. Once the number of Notifications to be returned by the current page has exceeded the limit for a single response, all of the current Notifications will be archived on a static archive page.

Any feed page can contain 'prev-archive' and 'next-archive' links, which refer to the next oldest and next newest page respectively. Both the current page and the first archive page will contain only a 'prev-archive' link, and the oldest feed page will contain only a 'next-archive' link. Archive pages additionally include a 'current' link.

Any archive page after the first page is guaranteed to be static; they will not have any new content added, and the URI will also remain static. This allows clients of the API to maintain a local cache of archive feed pages, cached on the URI.

When loading unseen Notifications it should be noted that if the number of unseen Notifications exceeds the number on the current page, the client should use the 'prev-archive' links to download all the Notifications which have not yet been seen.

The Notifications contained within the pages are a filtered set of Notifications applicable only to the user specified in the Authorization header.

This endpoint will return an Atom feed, which is a standard XML-based Web content and metadata syndication format. The RFC for the Atom Syndication Format is available at http://www.ietf.org/rfc/rfc4287. The static pages will also follow this format, as described in the RFC Feed Paging and Archiving available at http://tools.ietf.org/html/rfc5005.

An If-Modified-Since header can be specified in the Request.  If there are no changes since the client last requested the Document (determined by the If-Modified-Since header) then HTTP 304 Not Modified will be returned, with no resource (body). This is useful for [Caching](Caching).

### Example ###

#### Request ####
```
GET /notifications/user/123/received HTTP/1.1
Accept: application/atom+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/atom+xml
Last-Modified: Thu, 24 Apr 2013 11:15:22 GMT
```

```
<?xml version="1.0" encoding="utf-8"?>
<feed xmlns:huddle="http://www.huddle.com/" xmlns="http://www.w3.org/2005/Atom">
    <title type="text">Notifications for Frooty McNooty - Page ...</title>
    <id>...</id>
    <updated>2013-06-19T13:11:21Z</updated>
    <link rel="self" href="..." />
    <link rel="next-archive" title="Page ..." href="..." />
    <link rel="prev-archive" title="Page ..." href="..." />
    <entry>
        <id>...</id>
        <title type="text">Social Media Policy</title>
        <published>2013-01-02T10:20:20Z</published>
        <updated>2013-01-02T10:20:20Z</updated>
        <author>
            <name>Ron Swanson</name>
            <email>ron.swanson@example.com</email>
            <link rel="self" href="..." />
            <link rel="avatar" type="image/jpeg" href="..." />
        </author>
        <link rel="alternate" href="..." />
        <content type="text">This is the comment. It's an optional field, based on whether it's appropriate or not.</content>
        <huddle:notificationAction>DocumentUpdated</huddle:notificationAction>
        <huddle:workspace title="My Workspace">
            <link rel="self" href="..." />
        </huddle:workspace>
        <huddle:recipients>
            <huddle:actor name="Peter Gibson" email="peter.gibson@example.com">
                <link rel="self" href="..." />
                <link rel="avatar" href="..." type="image/jpg" />
            </huddle:actor>
        </huddle:recipients>
        <huddle:recipientsCount>1</huddle:recipientsCount>
        <huddle:dueDate>2013-08-01T11:10:18Z</huddle:dueDate>
    </entry>
    <entry>
        ...
        <published>2013-04-24T11:15:22+00:00</published>
        ...
    </entry>
    <entry>
        ...
        <published>2013-04-24T10:12:39+00:00</published>
        ...
    </entry>
</feed>
```

### Schema ###

This feed fits the [Atom RELAX NG Compact Schema](http://tools.ietf.org/html/rfc4287#appendix-B). As well as that, there are custom elements as described below.

```
namespace atom = "http://www.w3.org/2005/Atom"
namespace xhtml = "http://www.w3.org/1999/xhtml"
namespace s = "http://www.ascc.net/xml/schematron"

start = feed

feed = element feed {  
	atomEntry,
}

atomEntry = element atom:entry {
	huddleWorkspace,
	huddleRecipients,
	element huddle:notificationAction {text}
}

huddleWorkspace = element huddle:workspace {
	atomLink+
}

huddleRecipients = element huddle:recipients {
	huddleActor*
}

huddleActor = element huddle:actor {
	attribute name {text},
	attribute email {text},
	atomLink+
}

```
