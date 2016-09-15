# Summary #

Members of a workspace are able to invite other people into that workspace.

The API to invite people to a workspace is a Classic API See [Classic](Classic)

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Invite someone to a workspace|Released    |

# Operations #

## Invite someone to a workspace ##

After the request is processed, we will send an email invitation to each listed user, in the same manner as we do from the website. The email we send will be in the preferred language of the user sending the invitation, if available, otherwise in en-gb.

## Method ##

POST

## URL format ##

https://api.huddle.net/v1/xml/workspaces/{workspaceId}/invite

|{workspaceid}| The Id of the workspace to get the whiteboards for|
|:------------|:--------------------------------------------------|

### Example Request Body ###

```
POST /v1/xml/workspaces/89178/invite HTTP/1.1
Authorization: Basic Ym9idGhlbWlnaHR5OnBhc3N3b3Jk
Host: api.huddle.local
Content-Type: application/xml
Content-Length: 350
```

```
<InvitationRequest xmlns:i="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://schemas.datacontract.org/2004/07/Huddle.Client.Dtos"
       xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">

       <Addresses>
               <a:string>bob@huddle.net</a:string>
               <a:string>andy@huddle.net</a:string>
       </Addresses>

       <Message>O HAI! Join me in my workspace!</Message>
</InvitationRequest>

```

### Response ###

If the request works , you will receive a 202 Accepted.

## Error Codes ##

If an email address is unparseable (for example the string "I-am-not-an-email-address"), you'll receive a 400 with the following body:

```
HTTP/1.1 400 Bad Request
Content-Type: application/xml; charset=utf-8
Date: Wed, 22 Sep 2010 14:09:13 GMT
Content-Length: 453
```

```
<ResponseWrapper xmlns="http://schemas.datacontract.org/2004/07/Huddle.Client" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <Error xmlns:a="http://api.huddle.net/v1/xml">
   <a:Code>0</a:Code>
    <a:Msg>I-am-not-an-email-address could not be recognised as an email address</a:Msg>
    <a:PropertyErrors i:nil="true" xmlns:b="http://schemas.microsoft.com/2003/10/Serialization/Arrays"/>
    <a:Status>400</a:Status></Error>
    <Success>false</Success>
</ResponseWrapper>
```

If the user is not able to invite users to the chosen workspace you will receive the following 403 response

```
HTTP/1.1 403 Forbidden
Content-Type: application/xml; charset=utf-8
Date: Wed, 22 Sep 2010 14:31:30 GMT
Content-Length: 429
```

```
<ResponseWrapper xmlns="http://schemas.datacontract.org/2004/07/Huddle.Client" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
 <Error xmlns:a="http://api.huddle.net/v1/xml">
  <a:Code>1004</a:Code>
  <a:Msg>You are not authorised to call this method</a:Msg>
  <a:PropertyErrors i:nil="true" xmlns:b="http://schemas.microsoft.com/2003/10/Serialization/Arrays"/>
  <a:Status>403</a:Status></Error>
  <Success>false</Success>
</ResponseWrapper>
```
