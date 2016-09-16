# Summary #

The Invitations API is a representation of Invitations to a Team within Huddle.

## Beta warning ##

The Invitations API is in Beta

|  |
|:-|

# Operations #

## Create Invitation(s) ##

**Supports [What-If](WhatIfHeader)**

To create Invitations, POST a collection of email addresses, a collection of Team links and a message.

The response returns a collection of Invitations. If they are new members, then accepted will be false - indicating that an email has been sent to invite the person into Huddle. If they are an existing member of a company in Huddle, then accepted will be true - the person will be auto-added into the teams and a member summary for them is returned.

The Invitations link is obtained from a particular Workspace and all Team links must be Teams within that Workspace.

#### Request ####
The request will invite each of the supplied email addresses into Huddle and all of the supplied teams. Teams are supplied as links with a _rel_ of _parent_. All invitations will include the same supplied message. Existing Huddle users will only receive the message if the 'isDefaultMessage' flag is set to false.

In the examples newuser@huddle.com has never used Huddle before and existinguser@huddle.com is a member of a company in Huddle.

```
POST .../people/workspaces/123/invitations HTTP/1.1
Content-Type: application/vnd.huddle.data+xml (or +json)
Authorization: Bearer frootymcnooty/vonbootycherooty
```
```
<invitations>
  <link rel="parent" href="..."/>
  <emails>
    <email>newuser@huddle.com</email>
    <email>existinguser@huddle.com</email>
  </emails>
  <message>Hello!</message>
  <isDefaultMessage>true</isDefaultMessage>
</invitations>
```
```
{
  links: [{
    rel: "parent",
    href: "..."
  }],
  "emails": [ "newuser@huddle.com", "existinguser@huddle.com" ],
  "message": "Hello!",
  "isDefaultMessage": false
}
```

#### Response ####
If successful the method returns a 202 Accepted. One invitation will be returned per email address that was supplied in the request, even if the email addresses are being invited into multiple teams.

```
HTTP/1.1 202 Accepted
```
```
<invitations>
  <invitation>
    <email>newuser@huddle.com</email>
    <accepted>false</accepted>
  </invitation>
  <invitation>
    <email>existinguser@huddle.com</email>
    <accepted>true</accepted>
    <member>
      <links>
        <link rel="self" href="..." />
        <link rel="delete" href="..." />
        <link rel="avatar" href="..." />
        <link rel="profile" href="..." />
        <link rel="summary" href="..." />
        <link rel="companyManager" href="..." />
      </links>  
      <firstName>Existing</firstName>
      <lastName>User</lastName>
      <displayName>Existing User</displayName>
      <emailAddress>existinguser@huddle.com</emailAddress>
      <role>Founder</role>
      <internal>true</internal>
      <companyManager>false</companyManager>
      <lastLoginDate>2013-04-04T23:50:34</lastLoginDate>
  </member>
  </invitation>
</invitations>
```
```
{
  "invitations": [
    {
      "email": "newuser@huddle.com",
      "accepted": false,
      "member": null
    },
    {
      "email": "existinguser@huddle.com",
      "accepted": true,
      "member": {
        "links": [
          { "rel": "self", "href": "" },
          { "rel": "delete", "href": "" },
          { "rel": "company", "href": "" },
          { "rel": "user", "href": "" },
          { "rel": "avatar", "href": "" },
          { "rel": "profile", "href": "" },
          { "rel": "edit", "href": "" }
        ],
        "displayName": "Existing User",
        "firstName": "Existing",
        "lastName": "User",
        "emailAddress": "existinguser@huddle.com",
        "role": "Founder",
        "internal": true,
        "companyManager": false,
        "lastLoginDate": 2013-04-04T23:50:34
    }
  ]
}
```

### Invalid/Non-Whitelisted Emails ###

#### Request ####

```
POST .../people/workspaces/123/invitations HTTP/1.1
Content-Type: application/vnd.huddle.data+xml (or +json)
Authorization: Bearer frootymcnooty/vonbootycherooty
```
```
<invitations>
  <link rel="parent" href="..."/>
  <emails>
    <email>invalid.email</email>
    <email>nonwhitelisted@huddle.com</email>
    <email>inviter@huddle.com</email>
    <email>valid@huddle.com</email>
  </emails>
  <message>Hello!</message>
  <isDefaultMessage>true</isDefaultMessage>
</invitations>
```
```
{
  links: [{
    rel: "parent",
    href: "..."
  }],
  "emails": [ "invalid.email", "nonwhitelisted@huddle.com", "inviter@huddle.com", "valid@huddle.com" ],
  "message": "Hello!",
  "isDefaultMessage": true,
}
```

#### Response ####

```
HTTP/1.1 400 Bad Request
Content-Type: application/vnd.huddle.data+json
```
```
<error>
  <link
    rel="companyManager"
    href="..." />
  <emails>
    <email>
        <value>invalid.email</value>
        <reason>Invalid</reason>
    </email>
    <email>
        <value>nonwhitelisted@huddle.com</value>
        <reason>NotInWhitelist</reason>
    </email>
    <email>
        <value>inviter@huddle.com</value>
        <reason>SelfInvited</reason>
    </email>
  </emails>
</error>
```
```
{
  "links":
  [
   {
    "rel": "companyManager"
    "href": "..."
   }
  ],
  "emails":
  [
    {
      "value": "invalid.email"
      "reason": "Invalid"
    },
    {
      "value": "nonwhitelisted@huddle.com",
      "reason": "NotInWhitelist"
    },
    {
      "value": "inviter@huddle.com",
      "reason": "SelfInvited"
    }
  ]
}
```

#### Other Responses ####

|Case|Response|
|:---|:-------|
|Any of the Teams do not exist|400 Bad Request|
|All of the teams are not within the workspace, which the invitations link was obtained from|400 Bad Request|
|Any of the email addresses does not conform to RFC standards|400 Bad Request|
|Any of the email addresses is not whitelisted|400 Bad Request|
|Actor invited themselves|400 Bad Request|
|Message is over 2500 characters|400 Bad Request|
|Invalid authorization token|401 Unauthorized|
|Actor is not in team or not a workspace manager or a company manager|403 Forbidden|
|Workspace, which the invitations link was obtained from, does not exist|404 Not Found|
|Actor is not in the same company as the workspace|404 Not Found|
