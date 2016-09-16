# Summary #

Search for document and folders

* [Status](#status)
* [Operations](#operations)
    * [Searching for a document](#searchdocument)
    * [Searching for a folder](#searchfolder)
    * [Parameters](#parameters)
    * [Properties](#properties)
    * [Link relations](#link relations)
    * [Schema](#schema)
* [Query Language](#query language)
    * [Words](#words)
    * [Boolean Operators](#boolean operators)
    * [OR](#or)
    * [AND](#and)
    * [Escaping Special Characters](#Escaping Special Characters)

# Status #
| **Operation** | **Status**    |
|:--------------|:--------------|
|Searching for a document| Beta |
|Searching for a folder  | Beta |
|Query Language          | Beta |

# Operations #

## Searching for a document ##

When searching for a document the response will be an array of results, each with a score, summary of what matched, and the document that was found.

### Example ###

#### Request ####

```http
GET /files/search/documents?query=something&workspaceids=1,2,3 HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```xml
<search>
    <link rel="self" href="..." />
    <results>
        <result>
            <score>100</score>
            <summary><![CDATA[<b>something</b>]]></summary>
            <document title="something" description="document description">
                <link rel="self" href="..." />
                 ... other document elements ...
            </document>
        </result>
    </results>
</search>
```

## Parameters ##
| **Name** | **Description** | **Methods** | **Optional** |
|:---------|:----------------|:------------|:-------------|
| query    | The term to search for (truncates to 150 characters) | GET         | No           |
| pagesize | The maximum number of elements to return, between 1 - 100. Default: 100 | GET         | Yes          |
| workspaceids | A comma separated lists of workspace ids | GET         | Yes          |

### Pagination ###

```http
GET /files/search/documents?query=something&workspaceids=1,2,3&page=2&pagesize=10 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

### Filter by workspaces ###

The `workspaceids` parameter is optional. If you do not provide it search will return results from all the workspaces that the user has access to.

Although optional, we strongly encourage you to provide a list of workspaces to search within.


```http
GET /files/search/documents?query=something&workspaceids=1,3 HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| result   | A result element is a representation of the single search result. |


## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this search. | GET         |

## Schema ##

```
start = results

results = element h:results {
	  link+,
          result+
}

result = element h:result {
  link+,
  element h:score{ xsd:float },
  element h:summary{ xsd:string },
  element h:document{}
}
```

## Searching for folders ##

When searching for folders the response will be an array of results, each with a score, and the folder that was found.

### Example ###

#### Request ####

```http
GET /files/search/folders?query=something&workspaceid=1&from=2015-05-31T19%3A00%3A17%2B00%3A00 HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```xml
<search>
    <link rel="self" href="..." />
    <results>
        <result> 
            <links rel="self" href="..." />
            <score>100</score>
            <folder title="something" description="folder description">
                <links rel="self" href="..." />
                <links rel="alternate" href="http://my.huddle.net/workspace/469/folders/123" type="text/html" />
                <actors name="document owner" email="document.owner@huddle.net" rel="owner">
               		<links rel="self" href="https://..." />
               		<links rel="avatar" href="https://..." />
               		<links rel="alternate" href="https://..." type="text/html" />
                </actors>
		<updated>Thu, 07 Feb 2013 12:12:58 GMT</updated>
		<created>Thu, 06 Dec 2012 17:43:40 GMT</created>
		<workspace title="workspace title">
		        <links rel="self" href="https://..." />
	        </workspace>
            </folder>
        </result>
    </results>
</search>
```

## Parameters ##
| **Name** | **Description** | **Methods** | **Optional** |
|:---------|:----------------|:------------|:-------------|
| query    | The term to search for (truncates to 150 characters) | GET         | No           |
| pagesize | The maximum number of elements to return, between 1 - 100. Default: 100 | GET         | Yes          |
| workspaceids | A comma separated lists of workspace ids | GET         | Yes          |
| from     | The date to start the search from in encoded ISO 8601 format | GET         | Yes          |

### Filter by workspaces ###

The `workspaceids` parameter is optional. If you do not provide it search will return results from all the workspaces that the user has access to.
# Query Language #

Wildcards `*` and `?` not supported.

## Words ##

Single Term (single word): `hello`

Phrase (group of words): `"hello huddle"`

## Boolean Operators ##
## OR ##
"hello huddle" Wednesday

"hello huddle" OR Wednesday

"hello huddle" OR "Wednesday morning"

## AND ##
"hello huddle" AND Wednesday

"hello huddle" AND "Wednesday morning"

## Escaping Special Characters ##
Lucene supports escaping special characters that are part of the query syntax. The current list special characters are:

+ - & | ! ( ) { } [ ] ^ " ~ **? : \**

To escape these character use the \ before the character. For example to search for (1+1):2 use the query:

\(1\+1\)\:2
