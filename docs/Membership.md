# Summary #

Members of a workspace are able to obtain the list of all the members of that workspace

The API to invite people to a workspace is a Classic API See [Classic](Classic)

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Get the list of workspace members|Released    |

# Operations #

## Get the list of workspace members ##

After the request is sent we will respond with the list of workspace members

## Method ##

GET

## URL format ##

http://api.huddle.net/v2/workspaces/{workspaceid}/members

|{workspaceid}| The Id of the workspace to get the whiteboards for|
|:------------|:--------------------------------------------------|

### Example Request Body ###

```
Authorization: Basic aW91bGlhOnBhc3N3b3Jk3edg
Accept: application/xml
Host: api.huddle.net
Connection: Keep-Alive
```

### Response ###

If the request works you will receive a 200 OK and a list of the members.

```
HTTP/1.1 200 OK
Cache-Control: private
Content-Length: 1417
Content-Type: application/xml
Server: Microsoft-IIS/7.0
X-AspNet-Version: 2.0.50727
X-Powered-By: ASP.NET
Date: Tue, 11 Oct 2011 11:38:34 GMT
```

```
<?xml version="1.0" encoding="utf-8"?>
<WorkspaceMembers xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <User
    Id="26816958"
    Uri="http://api.huddle.net/users/26816958">
    <DisplayName>Joe Walsh</DisplayName>
    <Username>Joe.Walsh</Username>
    <LogoPath>http://my.huddle.net/images/logos/avatar.jpg</LogoPath>
    <Email>Joe@Walsh.com</Email>
    <LastActivity>2011-10-11T11:38:16Z</LastActivity>
    <IsManager>true</IsManager>
    <Team
      Id="2229003">
      <DisplayName>Team A</DisplayName>
    </Team>
    <TimeZoneInfo>
      <Offset>1</Offset>
      <TzId>Europe/London</TzId>
    </TimeZoneInfo>
  </User>
  <User
    Id="26816974"
    Uri="http://api.huddle.net/users/26816974">
    <DisplayName>Joe Smith</DisplayName>
    <Username>Joe.Smith</Username>
    <LogoPath>http://my.huddle.net/images/logos/avatar.jpg</LogoPath>
    <Email>Joe@Smith.com</Email>
    <LastActivity
      xsi:nil="true" />
    <IsManager>false</IsManager>
    <Team
      Id="2229003">
      <DisplayName>Team B</DisplayName>
    </Team>
    <TimeZoneInfo>
      <Offset>1</Offset>
      <TzId>Europe/London</TzId>
    </TimeZoneInfo>
  </User>
</WorkspaceMembers>
```

## Error Codes ##

If the user is not able to get the list of members you will receive the following 403 response

```
HTTP/1.1 403 Forbidden
Cache-Control: private
Content-Length: 351
Content-Type: application/vnd.huddle.error+xml
Server: Microsoft-IIS/7.0
X-AspNet-Version: 2.0.50727
X-Powered-By: ASP.NET
Date: Tue, 11 Oct 2011 11:44:10 GMT
```

```
<?xml version="1.0" encoding="utf-8"?>
<ErrorResult xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <StatusCode>403</StatusCode>
  <ErrorMessages>
    <Message>User Joe is not a member of the workspace</Message>
  </ErrorMessages>
  <ErrorCode>Authorization</ErrorCode>
</ErrorResult>
```
