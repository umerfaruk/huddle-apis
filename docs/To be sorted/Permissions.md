# Summary #

Permissions represents the list of actions you can perform over a workspace, folder or document.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieve permissions|Beta        |
|Update permissions|Beta        |

# Operations #

## Retrieving permissions ##

When [retrieving a workspace](Workspace#Retrieve_a_workspace), [retrieving a folder](Folder#Retrieve_a_folder) or [retrieving a document](Document#Retrieve_a_document), the response will contain a link to retrieve its permissions - a list of teams which have permissions for that resource, and what the permissions are.

Two faux-teams are included in the response: Workspace Managers, and the Default Permissions. The former is a collection of all the Workspace Managers in the Workspace, and the latter are the permissions that will be granted to new teams added to the workspace as starting defaults.

### Supported HTTP Headers ###

| **Header** | **More Information** |
|:-----------|:---------------------|
| If-Modified-Since | http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.25 |

### Example ###

Get the permissions for the folder 123.

#### Request ####
```http
GET /folders/123/permissions
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT
```

#### Response ####

This response uses the [standard error codes](ErrorHandling) and returns standard [response headers](ResponseHeaders).

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
Last-Modified: Tue, 1 Feb 2011 13:18:42 GMT
```
```xml
<permissionsSet>
  <link rel="self" href="/folders/123/permissions" />
  <teams>
    <team title="Default Permissions" type="default">
      <link rel="self" href="/teams/-1" />
      <permissions>
	 <permission type="Read" />
      </permissions>
      <parentPermissions>
	 <permission type="Read" />
      </parentPermissions>
      <members>
      </members>
    </team>
    <team title="team with no permissions" type="members">
      <link rel="self" href="/teams/111" />     
       <permissions/>
       <parentPermissions/>
       <members>
       <actor name="Barry Potter" email="barry.potter@example.com" rel="member">
         <link rel="self" href="..." />
         <link rel="avatar" href="image/jpg" />
         <link rel="alternate" href="text/html" />
        </actor>
       </members>
     </team>
     <team title="Workspace Managers" type="managers">
        <link rel="self" href="/workspace/54321/managers" />
        <permissions>
	  <permission type="Edit" />
        </permissions>
        <parentPermissions>
	  <permission type="Read" />
	  <permission type="Edit" />
        </parentPermissions>
        <members>
	  <actor name="Barry Potter" email="barry.potter@example.com" rel="manager">
	    <link rel="self" href="..." />
	    <link rel="avatar" href="..." type="image/jpg" />
	    <link rel="alternate" href="..." type="text/html" />
	  </actor>
        </members>
      </team>
   </teams>
</permissionsSet>
```

#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| teams    | A list of all teams in the workspace, plus the default team and the workspace managers team |
| members  | A list of all actors in that team |
| permissions | The type of permission (optional depending on resource which Permissions refer to - exists for [Folder](Folder), not for [Workspace](Workspace)). An empty element indicates no permissions for that team on the folder. |
| parentPermissions | This element indicates the team's permissions on the parent folder of the folder whose permissions are being requested. It is returned only when getting folder permissions for a non-top level folder, and when the team has some permissions on the parent folder: it is absent when getting workspace permissions, or permissions on the root folder in the workspace, or in cases where a team has no permissions on the parent folder. |

#### Link relations ####

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of this permissions. | GET, POST   |

#### Schema ####

```

start |= permissionsSet

permissionsSet= element permissionsSet{ 

  (link + | (
    link+,
    element teams{ team* },
  ))
}
```


---


## Updating permissions ##

To update permissions for a folder, POST details of changes to teams' permissions to the folder permissions resource. In addition to `read` and `edit` permissions, if an account has restricted view enabled then a permission of type `restricted` can be set.

When updating permissions, you must indicate whether you want changes to be made the current folder only, or if you want changes to be applied to the current folder and its subfolders. This is controlled by the "cascade" attribute on the permissions element. When "cascade" is "true", then permissions for that team will be cascaded to subfolders (otherwise permissions changes will only be applied to the current folder).

Because changing permissions may be a long-running operation, it is treated as an asynchronous process: after submitting the initial request to update permissions for a folder, clients are expected to poll a progress endpoint to retrieve the current status of the operation. Once the cascade operation has completed, the get progress response will include a link to a separate resource for getting the results of the operation.

For any permission types (e.g. **read** or **edit** or **restricted**) that you wish to grant to a team, include a permission element with the appropriate type. Any permission types that are not explicitly included in the post data will be denied. Therefore, to delete all existing permissions for a team on a folder, send a request with an empty permissions element (i.e. that contains no permission element children). Please note that when removing a team's permissions on a folder, you must cascade the changes to subfolders (otherwise a 409 Conflict response will be returned).

Do not attempt to send a workspace managers team in this request. Although it is returned in the GET request above, workspace manager teams cannot be granted or revoked access to a folder in their workspace. The Default Permissions team, however, can be modified here in the same way as other teams.

### Example ###

Update the permissions on folder 123

#### Request ####
```http
POST /folders/123/permissions
Accept: application/vnd.huddle.data+xml
Content-Type: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

```xml
<permissionChanges>
  <link rel="self" href="/folders/123/permissions" />
  <permissionChange cascade="false">
    <team title="teamA" type="members">
      <link rel="self" href="/teams/12345" />
      <permissions>
        <permission type="Read" />
        <permission type="Edit" />
      </permissions>
    </team>
  </permissionChange>
  <permissionChange cascade="true">
    <team title="teamB" type="members">
      <link rel="self" href="/teams/22345" />
      <permissions>
      </permissions>
    </team>  
  </permissionChange>
  <permissionChange cascade="true">
    <team title="teamC" type="members">
      <link rel="self" href="/teams/22345" />
      <permissions>
        <permission type="Restricted" />
      </permissions>
    </team>  
  </permissionChange>
</permissionChanges>
```

### Syntax ###

#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| permissionChange | Represents a change to a team's permissions on a folder. |

#### Link relations ####

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of the permissions resource being updated | GET         |

#### Response ####

If successful, this method will return a 202 Accepted status code and a link that can be used in subsequent polling GET requests to check the status of the operation.

```http
HTTP/1.1 202 Accepted
Content-Type: application/vnd.huddle.data+xml
```
```xml
<permissionseUpdate>
  <link rel="self" href="/folders/123/permissions" />
  <link rel="progress" href="/bulkprocess/folderpermissions/7b512a88-89a6-45f6-b81b-324e8bcfa4c3" />
</permissionseUpdate>
```

### Syntax ###

#### Link relations ####

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     | The current URI of the permissions resource being updated | GET         |
| progress | Can be used in subsequent polling GET requests to check the progress of the operation.| GET         |

### Getting the progress of a permissions update operation ###

Authorised API clients can use the link returned by the POST to GET the progress of the permissions update operation. If processing is complete, the response will additionally include a link to the permission update results resource.

#### Example ####

Get progress information for the previously initiated permissions update operation with id 7b512a88-89a6-45f6-b81b-324e8bcfa4c3.

#### Request ####
```http
GET /bulkprocess/folderpermissions/7b512a88-89a6-45f6-b81b-324e8bcfa4c3
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response (update in progress) ####

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<permissionseUpdateProgress>
  <link rel="self" href="/bulkprocess/folderpermissions/7b512a88-89a6-45f6-b81b-324e8bcfa4c3" />
  <isCompleted>60</isCompleted>
</permissionseUpdateProgress>
```

#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| percentageProcessed | The percentage of folders that have been processed |

#### Link relations ####

| **Name** | **Description** | **Methods** |
|:---------|:----------------|:------------|
| self     |  Link to check the progress of the operation.| GET         |

#### Schema ####
```
start |= permissionseUpdateProgress

permissionseUpdateProgress = element permissionseUpdateProgress {
  (
    link,
    isCompleted { xsd:boolean }
  )
}
```

#### Response (update is completed) ####

Once the update operation has completed, the response will additionally include a link to GET the results of the operation.
```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<permissionsUpdateProgress>
  <link rel="self" href="/bulkprocess/folderpermissions/7b512a88-89a6-45f6-b81b-324e8bcfa4c3" />
  <link rel="results" href="/bulkprocess/folderpermissions/7b512a88-89a6-45f6-b81b-324e8bcfa4c3/results" />
  <isCompleted>true</isCompleted>
</permissionsUpdateProgress>
```

#### Errors ####

Possible error responses returned by this endpoint can be found on the [Error Handling page](ErrorHandling#Retrieving_progress_for_a_cascade_folder_permissions_operation).

### Getting the results of an update permissions operation ###

#### Example ####

Get the results of a completed permissions update operation with id 7b512a88-89a6-45f6-b81b-324e8bcfa4c3.

#### Request ####
```http
GET /bulkprocess/folderpermissions/7b512a88-89a6-45f6-b81b-324e8bcfa4c3/results
Accept: application/vnd.huddle.data+xml
Authorization: OAuth2 frootymcnooty/vonbootycherooty
```

#### Response ####
```http
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```xml
<permissionsUpdateResults>
  <link rel="self" href="/bulkprocess/folderpermissions/7b512a88-89a6-45f6-b81b-324e8bcfa4c3/results" />
  <teamResults teamId="111">  
    <processedSuccessfully>
      <folder title="Folder A">
        <link rel="self" href="..." />
      </folder>
    </processedSuccessfully>
    <skipped />
  </teamResults>
  <teamResults teamId="222">  
    <processedSuccessfully>
      <folder title="Folder A">
        <link rel="self" href="..." />
      </folder>
      <folder title="Subfolder B">
        <link rel="self" href="..." />
      </folder>
    </processedSuccessfully>
    <skipped />
  </teamResults>
</permissionsUpdateResults>
```

#### Properties ####

| **Name** | **Description** |
|:---------|:----------------|
| processedSuccessfully | This element contains a number of folder elements representing folders which have had the new permissions applied successfully. |
| skipped  | This element contains a number of folder elements representing folders which have not had the new permissions applied. |
