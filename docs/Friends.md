# Summary #

Friends represent a list of users that are in the same workspace(s) as the specified user.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving a friends list|Beta        |

# Operations #

## Retrieving a friends list ##

When retrieving a user, the response will advertise a friends link, which you can used by the specified user to GET a list of their own friends in all workspaces or in a specified workspace.

### Example ###

In this example we are asking for the list of friends for all workspaces.

#### Request ####
```
GET /users/12345/friends
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

### Example ###

You can then also filter your request be selecting a workspace.

#### Request ####
```
GET /users/12345/friends?workspace=123456
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders) and will return a subset of [User](User)

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Last-Modified: Tue, 1 Feb 2011 13:18:42 GMT
```
```
<friends xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="..." />

  <user xmlns="http://schema.huddle.net/2011/02/">
     <link rel="self" href="..." />
     <link rel="alternate" type="text/html" href="..." />
     <link rel="avatar" type="image/jpg" href="..." />
     <link rel="friends" href="..." />  
     <link rel="recommendations" href="..." />
	
     <profile>
        <personal>
        <firstname>Ian</firstname>
        <lastname>Cooper</lastname>
        <displayname>Ian Cooper</displayname>
        <about>This is my bio</about>
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
  </user>
</friends>
```


---


# Syntax #

## Example ##

```
<friends xmlns="http://schema.huddle.net/2011/02/">
  <link rel="self" href="..." />
  
  <user xmlns="http://schema.huddle.net/2011/02/">
    <link rel="self" href="..." />
    <link rel="alternate" type="text/html" href="..." />
    <link rel="avatar" type="image/jpg" href="..." />
    <link rel="friends" href="..." />  
    <link rel="recommendations" href="..." />
	
    <profile>
      <personal>
        <firstname>Ian</firstname>
        <lastname>Cooper</lastname>
        <displayname>Ian Cooper</displayname>
        <about>This is my bio</about>
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
  </user>
</friends>
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| Friends  | The friends element is the base element and will contain a collection of [Users](User) |

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this friends list. | GET         |


## Schema ##

```
start = friends

user = element h:user { 
  link+,
  membership,
  profile
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
  element h:about { xsd:string }
}

contacts = element h:contacts {
  contact+
}

```
