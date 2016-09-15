# Summary #

When a user views a document, the view is recorded in the documents activity trail.  This API allows users to determine which version they last viewed of a document.  If they have the Difference feature enabled, they will receive a difference link that will allow them to view the changes.   

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Get last viewed |Beta        |

# Operations #

## Get last viewed ##

If the user has viewed the latest version, the difference link will not be returned (even if the user's account has the Difference feature turned on).

If the user has never viewed a version of this document they will receive a 204 NO CONTENT response.

If the user does not have access to the document they will receive the standard document 403 FORBIDDEN response.

If the document is deleted, they will receive the standard document 410 GONE response. 

#### Json Example ####

>#### Request ####
>```
>GET /files/documents/15538/versions/last-viewed HTTP/1.1
>Accept: application/vnd.huddle.data+json
>Authorization: OAuth2 frootymcnooty/vonbootycherooty
>```
>
>#### Response ####
>```
>HTTP/1.1 200 OK
>Content-Type: application/vnd.huddle.data+json
>```
>```
>{
>  "links": [
>    {
>      "rel": "self",
>      "href": "/files/documents/15538/versions/last-viewed"
>    },
>    {
>      "rel": "difference",
>      "href": "/files/documents/15538/versions/18064/difference/18069"
>    }
>  ],
>  "hasViewedLatestVersion": false,
>  "viewedOn": "Tue, 01 Mar 2016 21:07:47 GMT"
>  "version": {
>   "links": [
>    {
>      "rel": "self",
>      "href": "/files/documents/15538/version/18064"
>    },
>    "versionNumber": 3,
>    "createdDate": "Tue, 01 Mar 2016 16:41:53 GMT",
>    "createdBy": {
>      "name": "frootymcnooty",
>      "email": frootymcnooty@huddle.com",
>      "rel": "creator",
>      "links": [
>        {
>          "rel": "self",
>          "href": "/users/100"
>        },
>        {
>          "rel": "avatar",
>          "href": "/users/100/avatar?h=zzz"
>        },
>        {
>          "rel": "alternate",
>          "href": "/user/frootymcnooty",
>          "type": "text/html"
>        }
>      ]
>    }
>  }
>}
>```

#### Xml Example ####

>#### Request ####
>```
>GET /files/documents/15538/versions/last-viewed HTTP/1.1
>Accept: application/vnd.huddle.data+xml
>Authorization: OAuth2 frootymcnooty/vonbootycherooty
>```
>
>#### Response ####
>```
>HTTP/1.1 200 OK
>Content-Type: application/vnd.huddle.data+xml
>```
>```
><?xml version="1.0" encoding="utf-8"?>
><lastViewed xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
>  <link
>    rel="self"
>    href="/files/documents/15538/versions/last-viewed" />
>  <link
>    rel="difference"
>    href="/files/documents/15538/versions/18064/difference/18069" />
>  <HasViewedLatestVersion>false</HasViewedLatestVersion>
>  <ViewedOn>2016-03-01T21:07:47.70Z</ViewedOn>
>  <Version>
>    <link
>      rel="self"
>      href="/files/documents/15538/version/18064" />
>    <VersionNumber>3</VersionNumber>
>    <CreatedDate>2016-03-01T16:41:53.30Z</CreatedDate>
>    <CreatedBy
>      name="frootymcnooty"
>      email="frootymcnooty@huddle.com"
>      rel="creator">
>      <link
>        rel="self"
>        href="/users/100" />
>      <link
>        rel="avatar"
>        href="/users/100/avatar?h=zzz" />
>      <link
>        rel="alternate"
>        href="/user/frootymcnooty"
>        type="text/html" />
>    </CreatedBy>
>  </Version>
></lastViewed>
>```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| hasViewedLatestVersion   | Whether the user has viewed the latest version of the document or not |
| viewedOn | When the user last viewed the document |
| version	 | Information on the version they last viewed |

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this endpoint. | GET         |
| difference | If the difference feature is enabled, and the last viewed version is not the latest, a link to the document [difference](Difference) is available. | GET         |

#### Response - Other (no body) ###
| Status Code | Reason |
|:------------|:-------|
| 401         | Invalid authorization token |
| 403         | User does not have read access to document |
| 404         | Document does not exist |
| 410         | Document is gone (deleted) |
