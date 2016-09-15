# Summary #

Workspaces always contains a DocumentLibrary [folder](Folder) that is the root folder from which you can create new [documents](Document) or [folders](Folder).

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieving a workspace|Beta        |
|Retrieving a workspace calendar|Beta        |

# Operations #

## Retrieving a Workspace ##

When accessing the [User resource](User), the response will contain the list of workspaces of which the user is member of. Each workspace will advertise a _self_ link, which you can use to GET the workspace. The workspace resource also contains links to the document library, workspace changes and workspace members (see [membership](Membership)).

### Example ###

#### Request ####
```
GET /workspaces/27 HTTP/1.1
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<workspace xmlns="http://schema.huddle.net/2011/02/" 
           title="Infinite improbability drive" 
           description="Projects for stealing the infinite improbability drive" 
           type="shared">

  <link rel="self"
    href="..." />
  <link
    rel="documentLibrary"
    href="..." />
  <link
    rel="changes"
    href="..." />
  <link
    rel="workspaceMembers"
    href="..." />
  <link
    rel="permissions"
    href="..." />
  <link
    rel="assignee-approvals"
    href="..." />
  
  <settings>
    <setting name="CanSync" value="true" />
    <setting name="IsManager" value="true" />
    <setting name="Status" value="Active" />
  </settings>

</workspace>
```

## Retrieving a workspace calendar ##

IN PROGRESS


---


# Syntax #

## Example ##

```
<workspace xmlns="http://schema.huddle.net/2011/02/" 
           title="Title is mandatory" 
           description="Description is optional" 
           type="shared">

  <link rel="self" href="..." />
  <link rel="documentLibrary" hef="..." />
 
  <settings>
    <setting name="CanSync" value="true" />
    <setting name="IsManager" value="true" />
    <setting name="Status" value="Active" />
  </settings>

</workspace>
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| title    | The title of the workspace |
| description | A more complete description of what the workspace is for |
| type     | 'Shared' is the only valid value |
| settings | A list settings that indicate how the workspace has been configured. |

## Workspace settings ##

| **Name** | **Type** | **Description** |
|:---------|:---------|:----------------|
| CanSync  | boolean  | Indicates whether the contents of the workspace should be synced locally in its entirety |
| IsManager | boolean  | Indicates whether the user is a manager of the given workspace |
| Status   | string   | Indicates the current status of the workspace. Possible values are 'Active', 'Locked', 'Deleted' and 'Archived' |

## Link relations ##

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this workspace. | GET         |
| documentLibrary | The URI of the root document [folder](Folder) for this workspace. | GET         |
| permissions | Returns a list of workspace teams | GET         |
| assignee-approvals | Returns a list of an assignee's approvals in the workspace. See [Approvals by Workspace](Approvals#Retrieving_approvals_by_workspace) | GET         |
## Schema ##

```
start = workspace

workspace = element h:workspace {
  attribute title {xsd:string},
  attribute description {xsd:string}?,
  link+,
  settings
}

settings = element h:settings {
  setting+
}

setting = element h:setting {
  attribute name { xsd:string },
  attribute value { xsd:string }
}
```
