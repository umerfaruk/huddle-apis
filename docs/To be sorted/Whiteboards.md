# Summary #

Whiteboards are canvases that allow Huddle users to create wiki style pages inside workspaces.

Whiteboards are a Classic API See [Classic](Classic)

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|List whiteboards in a workspace|Released    |
|Get a whiteboard|Released    |

# Operations #

## List whiteboards in a workspace ##

Gets the whiteboards for a specific workspace. The response is available in either xml or json formats (determined by whether the Accept request header is application/xml or application/json).

### Method ###
GET

### URL Format ###
https://api.huddle.net/v2/workspaces/{workspaceid}/whiteboards

|{workspaceid}| The Id of the workspace to get the whiteboards for|
|:------------|:--------------------------------------------------|

### Example Response ###
```
HTTP/1.1 200 OK
Content-Type: application/xml
```
```
<WhiteboardsList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schema.huddle.net/xml/whiteboard-summary">
  <WhiteboardListItemDto
    Id="10006607"
    Uri="http://api.huddle.net/v2/workspaces/10006599/whiteboards/10006607">
    <Author
      Id="10006591"
      Uri="http://api.huddle.net/v2/users/10006591">
      <DisplayName>jim bob</DisplayName>
      <Username>JB</Username>
      <LogoPath>http://my.huddle.net/images/logos/avatar.jpg</LogoPath>
      <Email>jimbob@huddle.net</Email>
      <WebApplicationUrl>http://my.huddle.dev/user/jimbob</WebApplicationUrl>
    </Author>
    <Title>Examples of how to use Huddle</Title>
    <Description>This is an example whiteboard created for you automatically by Huddle</Description>
    <CreatedDate>2010-10-05T15:47:26Z</CreatedDate>
    <Lock>
      <LockDate
        xsi:nil="true" />
    </Lock>
  </WhiteboardListItemDto>
  <WhiteboardListItemDto
    Id="12324615"
    Uri="http://api.huddle.net/v2/workspaces/10006599/whiteboards/12324615">
    <Author
      Id="10006591"
      Uri="http://api.huddle.net/v2/users/10006591">
      <DisplayName>maurice</DisplayName>
      <Username>maurice</Username>
      <LogoPath>http://my.huddle.dev/images/logos/avatar.jpg</LogoPath>
      <Email>maurice@huddle.net</Email>
      <WebApplicationUrl>http://my.huddle.dev/user/maurice</WebApplicationUrl>
    </Author>
    <Title>How to bake cakes</Title>
    <Description />
    <CreatedDate>2010-12-16T10:08:04Z</CreatedDate>
    <Lock>
      <LockDate
        xsi:nil="true" />
    </Lock>
  </WhiteboardListItemDto>
</WhiteboardsList>
```

## Get a whiteboard ##

Gets a specific whiteboard in a specific workspace. The response is available in either xml or json formats (determined by whether the Accept request header is application/xml or application/json).

### Method ###
GET

### URL Format ###
https://api.huddle.net/v2/workspaces/{workspaceid}/whiteboards/{whiteboardid}

|{workspaceid}| The Id of the workspace to get the whiteboard for|
|:------------|:-------------------------------------------------|
|{whiteboardid}| The Id of the whiteboard to get                  |

### Example Response ###
```
HTTP/1.1 200 OK
Content-Type: application/xml
```
```
<WhiteboardDto xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  Id="10006607"
  Uri="http://api.huddle.net/v2/workspaces/10006599/whiteboards/10006607">
  <Author
    Id="10006591"
    Uri="http://api.huddle.net/v2/users/10006591" xmlns="http://schema.huddle.net/xml/whiteboard-summary">
    <DisplayName>dw</DisplayName>
    <Username>dw</Username>
    <LogoPath>http://my.huddle.net/images/logos/avatar.jpg</LogoPath>
    <Email>dw@huddle.net</Email>
    <WebApplicationUrl>http://my.huddle.net/user/dw</WebApplicationUrl>
  </Author>
  <Title xmlns="http://schema.huddle.net/xml/whiteboard-summary">Examples of how to use Huddle</Title>
  <Description xmlns="http://schema.huddle.net/xml/whiteboard-summary">This is an example whiteboard created for you automatically by Huddle</Description>
  <CreatedDate xmlns="http://schema.huddle.net/xml/whiteboard-summary">2010-10-05T15:47:26Z</CreatedDate>
  <Lock xmlns="http://schema.huddle.net/xml/whiteboard-summary">
    <LockDate
      xsi:nil="true" />
  </Lock>
  <LastModifiedBy
    Id="10006591"
    Uri="http://api.huddle.net/v2/users/10006591">
    <DisplayName>dw</DisplayName>
    <Username>dw</Username>
    <LogoPath>http://my.huddle.net/images/logos/avatar.jpg</LogoPath>
    <Email>dan@huddle.net</Email>
    <WebApplicationUrl>http://my.huddle.net/user/dw</WebApplicationUrl>
  </LastModifiedBy>
  <Comments />
  <Contents>
     ALOT OF CONTENT :)
  </Contents>
  <WebApplicationUrl>http://my.huddle.net/huddleworkspace/whiteboard.aspx?workspaceid=10006599&amp;whiteboardid=10006607</WebApplicationUrl>
  <LastUpdated>2010-10-05T15:47:26Z</LastUpdated>
</WhiteboardDto>
```

### Example Json Response (with comments) ###
```
HTTP/1.1 200 OK
Content-Type: application/json
```
```
{ 
    "lastModifiedBy" : { 
        "webApplicationUrl" : "http://my.huddle.net/user/dw",
        "primaryAccountId" : 10006598,
        "managedAccountIds" : [ 10006598 ],
        "useHuddlePhoneConferencing" : true,
        "id" : 10006591,
        "uri" : "http://api.huddle.net/v2/users/10006591",
        "displayName" : "dw",
        "username" : "dw",
        "logoPath" : "http://my.huddle.net/images/logos/avatar.jpg",
        "email" : "dw@huddle.net"
    },
    "comments" : [ { 
        "createdDate" : "Fri, 15 Jul 2011 14:44:24 GMT",
        "text" : "sdfdfsdf",
        "user" : { 
            "displayName" : "dw",
            "id" : 10006591,
            "webApplicationUrl" : "http://my.huddle.net/user/dw",
            "logoPath" : "http://my.huddle.net/images/logos/avatar.jpg"
        }
    } ],
    "contents" : "<p>ghsdgh<\/p><p>&nbsp;<\/p>",
    "webApplicationUrl" : "http://my.huddle.net/huddleworkspace/whiteboard.aspx?workspaceid=10006599&whiteboardid=12324615",
    "lastUpdated" : "Thu, 16 Dec 2010 10:08:04 GMT",
    "author" : { 
        "webApplicationUrl" : "http://my.huddle.net/user/dw",
        "primaryAccountId" : 10006598,
        "managedAccountIds" : [ 10006598 ],
        "useHuddlePhoneConferencing" : true,
        "id" : 10006591,
        "uri" : "http://api.huddle.net/v2/users/10006591",
        "displayName" : "dw",
        "username" : "dw",
        "logoPath" : "http://my.huddle.net/images/logos/avatar.jpg",
        "email" : "dw@huddle.net"
    },
    "title" : "dfgdfg",
    "description" : "",
    "createdDate" : "Thu, 16 Dec 2010 10:08:04 GMT",
    "id" : 12324615,
    "uri" : "http://api.huddle.net/v2/workspaces/10006599/whiteboards/12324615",
    "lock" : { }
}
```


## Error Codes ##
400: The id provided could not be recognised

401: Authentication error

403: You do not have permission to access the item requested

500: There was an error processing your request
