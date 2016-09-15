# Summary #

The Localisation API allows you to receive translated text resources that Huddle uses within its own applications.

Please note that JSON and JSONP ([MediaType#JSONP\_support](MediaType#JSONP_support)) are currently the only return types for all of the endpoints of this API. Also you do not require a OAuth2 token.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Get Cultures   |Beta        |
|Get Categories |Beta        |
|Get Resources  |Beta        |

# Operations #

## Get Cultures ##

You can retrieve the cultures that currently have translations.

### Example ###

#### Request ####
```
GET /localisation/cultures HTTP/1.1
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/json
```
```
{
   "fr":"/localisation/cultures/fr/categories",
   "en-gb":"/localisation/cultures/en-gb/categories
}
```


---

## Get Categories ##

You can retrieve all of the categories.

### Example ###

#### Request ####
```
GET /localisation/cultures/categories HTTP/1.1
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/json
```
```
{
   "categories":
   [
      {
         "links":
         [
            {"rel":"self","href":"/localisation/categories/filesappdetails"}
         ],
         "id":"filesappdetails"
      }
   ]
}
```

---


## Get Categories for a specific language ##

You can retrieve all of the categories for a culture.

### Example ###

#### Request ####
```
GET /localisation/cultures/en-gb/categories HTTP/1.1
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/json
```
```
{
   "categories":
   [
      {
         "links":
         [
            {"rel":"self","href":"/localisation/cultures/en-gb/categories/filesappdetails"}
         ],
         "id":"filesappdetails"
      }
   ]
}
```

---

## Retrieving resources ##

You can retrieve a category's resources in multiple languages.  The language code must be a valid ISO language code (please see [Language codes](http://msdn.microsoft.com/en-gb/goglobal/bb896001.aspx)).

The Localisation API supports requests for cultures it does not yet have translations for.  If this is the case, you will be returned "en-gb" resources.

Note that Localisation API currently supports the current formats only:

  * 2 letter code e.g. fr (French)
  * 3 letter code e.g. syr (Syriac‎)
  * 2 letter code with 2 letter sub culture e.g. en-gb (English United Kingdom)
  * 3 letter code with 2 letter sub culture e.g. gsw-fr (Alsatian France)


There are two different end points for retrieving resources. One where you can specify the language code in the URL and another one where you can specify it in the Accept-Language header

### Example for obtaining a specific language by URI ###

#### Request ####
```
GET /localisation/cultures/en-gb/categories/filesappdetails HTTP/1.1
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/json
```
```
{ 
   "category": "filesappdetails",
   "resources": {
      "a_resource":"Hello",
      "go_button":"Go"
   }
}
```

### Example for French (language code in the URI) ###

#### Request ####
```
GET /localisation/cultures/fr/categories/filesappdetails HTTP/1.1
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/json
```
```
{ 
   "category": "filesappdetails",
   "resources": {
      "a_resource":"Bonjour",
      "go_button":"Aller"
   }
}
```

### Example for obtaining a resource by Accept-Language header ###

#### Request ####
```
GET /localisation/cultures/categories/filesappdetails HTTP/1.1
Accept-Language: en-gb
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/json
```
```
{ 
   "category": "filesappdetails",
   "resources": {
      "a_resource":"Hello",
      "go_button":"Go"
   }
}
```



---
