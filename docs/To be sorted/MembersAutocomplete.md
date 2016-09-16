# Summary #

The Members Autocomplete API provides a list of Members (from a specific company) filtered by a query. It is intended for fast-feedback as-you-type scenarios.

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Autocomplete Members|200         |

# Operations #

## Autocomplete Members ##

When autocompleting Members, the response will contain an array of Members that match the query across the start of firstname, lastname or email. Members are filtered by their CompanyIds of which must contain the CompanyId of the request. Actor is an abbreviated Member resource.

Autocomplete results are also dependent on whether the requestor is an internal or external member of the company. If the requestor is an external member then they will receive no autocomplete results. If the requestor was internal, then the autocomplete results should contain all the members (internal or external) of the company. 

### Example ###

#### Request ####

```
GET /members/companies/{companyId}/autocomplete?q=isi&hits=10
Accept: application/vnd.huddle.data+xml (or+json)
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<memberAutocomplete>
    <link rel="self" href="..." />
    <members>
        <actor rel="member" name="Isidore McHohenheim" email="isidore.mchohenheim@example.com">
            <link rel="avatar" href="..." type="image/jpg" />
            <identity email="isidore.mchohenheim@example.com" />
        </actor>
    </members>
</memberAutocomplete>
```
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```
{
  "links": [
    { "rel": "self", "href": "..." }
  ],
  "members": [{
    "links": [
        { "rel": "avatar", "href" : "...", "type" : "..." }
    ],
    "name": "Isidore McHohenheim",
    "email": "isidore.mchohenheim@example.com",
    "rel": "member",
    "identity": {
        "email": "isidore.mchohenheim@example.com"
    }
  }]
}
```

Note: Unlike elsewhere in the API, the Actor links do not currently include self or alternate links.

## Parameters ##
| **Name** | **Description** | **Methods** | **Optional** |
|:---------|:----------------|:------------|:-------------|
| q        | The query to complete on | GET         | No           |
| hits     | The number of results to return. Defaults to 10. Max 20. | GET         | Yes          |

## Link relations ##
| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The URI of the resource | GET         |

#### Other Responses ####

|Case|Response|
|:---|:-------|
|q is less than 2 chars or whitespaces|400 Bad request|
|Hits less than 1 |400 Bad request|
|Hits more than 20|400 Bad request|
|Requestor is not a member of the company|404 not found|
