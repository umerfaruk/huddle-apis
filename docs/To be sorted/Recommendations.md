# Summary #

Recommendations represent the list of recommended documents for a given user.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving a list of recommended documents|Beta        |

# Operations #

## Retrieving a recommendations list ##

You can GET the recommended documents for a user and this will display a relevance ordered list of recommended documents based on the actions of the given user.

Recommendations will only return the top 50 recommended documents.

Please see [Document](Document) for more details of this resource.

### Example ###

In this example we are asking for the list of recommendations for user with id 123.

#### Request ####
```
GET /documents/recommendations/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<recommendations xmlns="http://schema.huddle.net/2011/02/">
   <link rel="self" href="..." />
  
   <recommendation>
     <document title="..." description="...">
       <link rel="self" href="..." />

       ... other document elements ...  
     </document>

     <workspace title="...">
       <link rel="self" href="..." />
     </workspace>  

   </recommendation>
   <recommendation>
     <document title="..." description="...">
       <link rel="self" href="..." />

       ... other document elements ...  
     </document>

     <workspace title="...">
       <link rel="self" href="..." />
     </workspace>  

   </recommendation>
</recommendations>
```


---


# Syntax #

## Examples ##
```
<recommendations xmlns="http://schema.huddle.net/2011/02/">
   <link rel="self" href="..." />
  
   <recommendation>
     <document title="..." description="...">
       <link rel="self" href="..." />

       ... other document elements ...  
     </document>

     <workspace title="...">
       <link rel="self" href="..." />
     </workspace>  

   </recommendation>
   <recommendation>
     <document title="..." description="...">
       <link rel="self" href="..." />

       ... other document elements ...  
     </document>

     <workspace title="...">
       <link rel="self" href="..." />
     </workspace>  

   </recommendation>
</recommendations>
```

Request with parameter
```
GET /documents/recommendations/123 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| recommendation | A recommendation element is a representation of the single recommended [Document](Document) for the user. |

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this recommendation list. | GET         |

## Schema ##

```

start = recommendations

recommendations = element h:recommendations {
	  link+,
          recommendation+
}

recommendation = element h:recommendation  {
  link+,
  element h:document{},
  element h:workspace{}
}

```
