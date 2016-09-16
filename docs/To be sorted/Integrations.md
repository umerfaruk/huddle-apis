# Summary
The integrations endpoint is used to retrieve information about available
integrations on a folder or document. Which integrations are available depends
on the account and, if applicable, the type of the document.

Extension | MIME Type | Integration
----------|-----------|------------
`.docx`   | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` | Office Online
`.pptx`   | `application/vnd.openxmlformats-officedocument.presentationml.presentation` | Office Online
`.xlsx`   | `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet` | Office Online


## Document Integrations
Document integrations can be used to view or edit a document using a third party
client. The link to this endpoint is advertised on the
[document response](Document#retrieving-a-document) with the @rel value `integrations`.

### Example
#### Request
```http
GET .../files/integrations/document/123 HTTP/1.1
Accept: application/vnd.huddle.data+json
```

#### Response
```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```json
{
    "officeOnline": {
        "canView": true,
        "canEdit": false
    }
}
```

#### Response - Other (no body) ###
| Status Code | Reason |
|:------------|:-------|
| 401         | Invalid authorization token |
| 404         | Document not found |


## Folder Integrations
Folder integrations can be used to determine which integrations are available. The link to this endpoint is advertised on the
[folder response](Folder#retrieving-a-folder) with the @rel value `integrations`.

It will return a list of integrations that are available.  This is currently only Microsoft Office Online.
If a user can edit the folder, the response contains document template integrations links with a @rel value `create` for every available mime type.

### Example
#### Request
```http
GET .../files/integrations/folder/123 HTTP/1.1
Accept: application/vnd.huddle.data+json
```

#### Response
```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```json
{
    "documentTemplates": {
        "links": [{
            "rel": "create",
            "type": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
            "href": "..."
        }, {
            "rel": "create",
            "type": "application/vnd.openxmlformats-officedocument.presentationml.presentation",
            "href": "..."
        }, {
            "rel": "create",
            "type": "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
            "href": "..."
        }]
    },
    "officeOnline": {
        "canView": true,
        "canEdit": true
    }
}
```

#### Response - Other (no body) ###
| Status Code | Reason |
|:------------|:-------|
| 401         | Invalid authorization token |
| 404         | Folder not found |
