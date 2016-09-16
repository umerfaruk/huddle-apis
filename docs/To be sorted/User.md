# Summary #

User resources describe registered users of the Huddle applications. A user resource contains profile and contact information, and details of the user's relationship to [workspaces](Workspace), [accounts](Account), and [providers](Provider). From the User resource, you can navigate to active or archived [workspaces](Workspace) of which the user is a member, and perform operations against resources they contain.

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving a user|Beta        |

# Operations #

## Retrieving a user ##

Every [Actor](Actor) will advertise a _self_ link, which you can use to GET the user.

### Example ###

#### Request ####
```
GET /users/17654 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<user xmlns="http://schema.huddle.net/2011/02/">
  <link rel="alternate" type="text/html" href="..." />
  <link rel="avatar" type="image/jpg" href="..." />
  <link rel="self" href="..." />
  <link rel="meetings" href="..." />
  <link rel="approvals" href="..."/>
  <link rel="friends" href="..."/>
  <link rel="recommendations" href="..."/>
  <link rel="notifications-unseen-count" href="..."/>
  <link rel="notifications-received" href="..."/>
  <link rel="offline-items" href="..."/>
  <link rel="document-owned-approvals" href="..."/>
  <link rel="assignee-approvals" href="..."/>
  <link rel="locks-owned" href="..."/>
  <link rel="hub" href="..."/>
  <link rel="recent-items" href="..."/>
  <link rel="home" href="..."/>
  <link rel="sessions" href="..." />

  <membership>
    <workspaces>
      <workspace title="My workspace" type="shared">
        <link rel="self" href="..." />
        <link rel="documentLibrary" href="..." />
        <link rel="assignee-approvals" href="..." />

        <settings>
          <setting name="CanSync" value="true" />
          <setting name="IsManager" value="true" />
          <setting name="Status" value="Active" />
          ...
          <setting name="SomethingElse" value="foofoofoo" />
        </settings>
        
      </workspace>
    </workspaces> 
  </membership>
 
  <profile>   
    <personal>
      <firstname>Bob</firstname>
      <lastname>Gregory</lastname>
      <displayname>Bob The Mighty</displayname>
    </personal>  
    
    <company name="Huddle" role="Senior developer"> 
      <link rel="www" href="http://www.huddle.com" />
    </company>
    
    <contacts>
      <contact rel="mail" href="..." />
    </contacts>
  </profile>
  
  <internationalisation>
    <timezone>Europe/London</timezone>
    <utcOffset>60</utfOffset>
    <languageCode>en-gb</languageCode>
  </internationalisation>

  <client>
    <IsMobilePinEnabled>False</IsMobilePinEnabled>
  </client>
</user>
```


---


# Syntax #

## Example ##

```
<user xmlns="http://schema.huddle.net/2011/02/">
  <link rel="alternate" type="text/html" href="..." />
  <link rel="avatar" type="image/jpg" href="..." />
  <link rel="self" href="..." />
  <link rel="meetings" href="..." />
  <link rel="approvals" href="..."/>
  <link rel="friends" href="..."/>
  <link rel="recommendations" href="..."/>
  <link rel="notifications-unseen-count" href="..."/>
  <link rel="notifications-received" href="..."/>
  <link rel="offline-items" href="..."/>
  <link rel="document-owned-approvals" href="..."/>
  <link rel="assignee-approvals" href="..."/>
  <link rel="locks-owned" href="..."/>
  <link rel="hub" href="..."/>
  <link rel="recent-items" href="..."/>
  <link rel="home" href="..."/>

  <membership>
    <workspaces>
	  
      <workspace title="My wonderful workspace" type="shared">
        <link rel="self" href="..." />
        <link rel="documentLibrary" href="..." />
        <link rel="assignee-approvals" href="..." />

        <settings>
          <setting name="CanSync" value="true" />
          <setting name="IsManager" value="true" />
          <setting name="Status" value="Active" />
          <setting name="SomethingElse" value="foofoofoo" />
        </settings> 
      </workspace>
	   
      <workspace title="My workspace" type="shared">
        <link rel="self" href="..." />
        <link rel="calendar" href="..." />
        <link rel="documentLibrary" href="..." />
        <link rel="assignee-approvals" href="..." />

        <settings>
          <setting name="CanSync" value="true" />
          <setting name="IsManager" value="true" />
          <setting name="Status" value="Active" />
          <setting name="SomethingElse" value="foofoofoo" />
        </settings>

      </workspace>
    </workspaces> 
  </membership>
	
  <profile>
	  
    <personal>
      <firstname>Ian</firstname>
      <lastname>Cooper</lastname>
      <displayname>Ian Cooper</displayname>
    </personal>  
	  
    <company name="Huddle" role="Senior developer"> 
      <link rel="www" href="http://www.huddle.com" />
    </company>

    <contacts>
      <contact rel="mail" href="..." />
      <contact rel="telephone" href="..." />
      <contact rel="mobile" href="..." />
      <contact rel="skype" href="..." />  	  
      <contact rel="www" href="http://www.iancooper.name" />
    </contacts>
  </profile>
  
  <internationalisation>
    <timezone>Europe/London</timezone>
    <utcOffset>60</utcOffset>
    <languageCode>en-gb</languageCode>
  </internationalisation>	

  <client>
    <isMobilePinEnabled>False</isMobilePinEnabled>
  <client>
</user>
```

## Schema ##

```
start = user

user = element h:user { 
  link+,
  membership,
  profile
}

membership = element h:membership {
  element h:workspaces { workspace+ }
}

profile = element h:profile {
  link+,
  personal,
  registration,
  company,
  contacts    
}

company = element h:company {
  attribute name { xsd:string },
  attribute role { xsd:string },
  link* 
}

personal = element h:personal {
  element h:firstname { xsd:string },
  element h:lastname{ xsd:string },
  element h:displayname { xsd:string }
}

contacts = element h:contacts {
  contact+
}

internationalisation = element h:internationalisation {
  element h:timezone { xsd:string }
  element h:utcOffset { xsd:string }
  element h:languageCode { xsd:string }
}

client = element h:client {
  element h:isMobilePinEnabled { xsd:string }
}

```

### Properties ###

User is a complex resource.

#### Membership ####

| **Name** | **Description** |
|:---------|:----------------|
| workspaces | A set of [workspaces](Workspace) of which the user is a member and that workspace is active or archived. Only displayed to requests authenticated as the user |

#### Personal ####

| **Name** | **Description** |
|:---------|:----------------|
| firstname | The first or given name of the user. |
| lastname | The last name of the user. |
| displayname | The name of the user as it appears in the application. This is the @name value used by the [actor](Actor) element. |

#### Company ####

| **Name** | **Description** |
|:---------|:----------------|
| @name    | The name of the company |
| @role    | The user's position within the company |

#### Internationalisation ####

| **Name** | **Description** |
|:---------|:----------------|
| @timezone | The user's timezone preference, in IANA time zone format |
| @utcOffset| The offset in minutes |
| @languageCode | The user's language preference, in ISO 639-1 standard format |

#### Client ####

| **Name** | **Description** |
|:---------|:----------------|
| @isMobilePinEnabled | True if the user is in a company that uses Mobile Pin, this will be consumed by mobile clients. |


#### Contacts ####

The contacts section comprises many contact whose @rel values are described below

| mail | The primary email of the user. |
|:-----|:-------------------------------|
| telephone | The telephone of the user.     |
| mobile | The mobile phone of the user.  |
| skype | The Skype identifier of the user. |
| www  | An external website related to the containing resource. |


## Link relations ##

| **Name** | **Description** |
|:---------|:----------------|
| self     | The current URI of this user. |
| alternate | When used in a profile structure this link points to an alternative representation of the user profile. A type attribute "text/html" signifies the user's profile page in the web application. Other media types may follow. |
| avatar   | The avatar chosen by the user. |
| friends  | The URI to retrieve a list of friends for the user. |
| meetings | This user has one or more meetings that they can attend. |
| approvals | The URI to retrieve the [UserApprovals](UserApprovals) resource, which is a list of documents that require approval by the user. Note this link will only be present if the authenticated user matches the user represented by this resource. |
| recommendations | The URI to retrieve a list of recommended [documents](Document) for the user |
| notifications-unseen-count | The URI to retrieve the details about the current [Notifications](Notifications) unseen count for the user |
| notifications-received | The URI to retrieve the details about the current [Notifications](Notifications) for the user |
| offline-items | The URI to retrieve the list of [OfflineItem](OfflineItem) for the user. This functionality is enabled at the account level, so if not enabled, no link will be present. |
| document-owned-approvals | The URI to retrieve a list of approvals for documents owned by the User. See [Approvals by Document Owner](Approvals#Retrieving_approvals_by_document_owner) |
| assignee-approvals | The URI to retrieve a list of approvals assigned to the User. See [Approvals by Assignee](Approvals#Retrieving_approvals_by_assignee) |
| locks-owned | The URI to retrieve a list of document locks owned by the User. See [Locks by Owner](Locks#Retrieving_locks_by_owner) |
| hub      | The URI to used to receive update notifications for objects the client is interested in. See [update notification](UpdateNotifications#Real-time)|
| recent-items | The URI to used to retrieve the recent items list. See [Recents](Recents)|
| home     | The URI to used to retrieve the web-home document that lists URI templates for various web pages, e.g. Dashboard / Workspace Overview. See [HomeWeb](HomeWeb)|
