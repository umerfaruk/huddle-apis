# Summary #

This API allows workspace managers to view and edit settings which affect all the files and folders in a workspace. You can obtain the link for this resource from [a folder in the workspace](Folder).


|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving document library settings|Beta        |
|Editing document library settings|Beta        |


# Operations #

## Retrieving document library settings ##

Issue a GET request to the `document-library-settings` URI from a [Folder](Folder) resource.

### Example (JSON) ###

#### Request ####

```
GET ... HTTP/1.1
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
```
```
{
    "links": [
        { "rel": "self", "href": "..." },
        { "rel": "document-library", "href": "..." }
    ],
    "membersMayCreateFolders": true,
    "documentPublicationEnabled": true
}
```

### Example (XML) ###

#### Request ####

```
GET ... HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<documentLibrarySettings>
    <links>
        <link rel="self" href="..." />
        <link rel="documentLibrary" href="..." />
    </links>
    <membersMayCreateFolders>true</membersMayCreateFolders>
    <documentPublicationEnabled>true</documentPublicationEnabled>
</documentLibrarySettings>
```


## Editing document library settings ##

If the authenticated user is authorized to edit the document library settings, it will advertise a link with a @rel value of _edit_. To update it submit a PUT request to this URI with the settings you want to update. For an overview of editing resource in Huddle see [editing resources](Resources#Editing_Resources).

If you omit a setting in your request, the current value for that setting will not be changed.

If you send an invalid setting in your PUT request, the server will respond with a 400 Bad Request status code.


| Setting name | Valid values |
|:-------------|:-------------|
| `membersMayCreateFolders` | true or false |
| `documentPublicationEnabled` | true or false |

### Example (JSON) ###


#### Request ####
```
GET /files/workspaces/31395/folders/root/settings/edit HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty

```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+json
{
    "membersMayCreateFolders": false,
    "documentPublicationEnabled": false
}
```


#### Request ####
```
PUT /files/workspaces/31395/folders/root/settings/edit HTTP/1.1
Content-Type: application/vnd.huddle.data+json
Accept: application/vnd.huddle.data+json
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
{
    "membersMayCreateFolders": false,
    "documentPublicationEnabled": false
}
```

#### Response ####
```
HTTP/1.1 204 No Content
Content-Type: application/vnd.huddle.data+json
Link: </files/workspaces/31395/folders/root/settings/>;rel="parent"
```



### Example (XML) ###

#### Request ####
```
GET  /files/workspaces/31395/folders/root/settings/edit HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<documentLibrarySettings>
    <membersMayCreateFolders>false</membersMayCreateFolders>
    <documentPublicationEnabled>false</documentPublicationEnabled>
</documentLibrarySettings>
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<documentLibrarySettings>
    <links>
        <link rel="self" href="..." />
        <link rel="documentLibrary" href="..." />
    </links>
    <membersMayCreateFolders>false</membersMayCreateFolders>
    <documentPublicationEnabled>false</documentPublicationEnabled>
</documentLibrarySettings>
```

#### Request ####
```
PUT /files/workspaces/31395/folders/root/settings/edit HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```
```
<documentLibrarySettings>
    <membersMayCreateFolders>false</membersMayCreateFolders>
    <documentPublicationEnabled>false</documentPublicationEnabled>
</documentLibrarySettings>
```

#### Response ####
```
HTTP/1.1 204 No Content
Content-Type: application/vnd.huddle.data+xml
Link: </files/workspaces/31395/folders/root/settings/>;rel="parent"
```


## Link relations ##
| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| `self`   | The current URI of the document library settings | GET, POST   |
|`document-library` | The URI of the [document library](Folder) to which these settings pertain | GET         |

## Schema ##
```
start |= documentLibrarySettings

documentLibrarySettings = element h:documentLibrarySettings {
    element h:links {
        link *
    },
    element h:membersMayCreateFolders { text },
    element h:documentPublicationEnabled { text }
}
```
