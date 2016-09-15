# Summary #

Huddle use a standard ErrorResult element to signal error information to a client.
An ErrorResult will contain some human-readable description of the error, plus an enumerated error uri to distinguish classes of error.
The uri should point to a human-readable description of the error with more information for developers, but can be treated as a error code programatically.


|  |
|:-|

# Syntax #

## Example ##

```
<?xml version="1.0" encoding="utf-8"?>
<ErrorResult xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <StatusCode>410</StatusCode>
  <ErrorMessages>
    <Message>The workspace is deleted.</Message>
  </ErrorMessages>
  <ErrorCode>WorkspaceDeleted</ErrorCode>
</ErrorResult>
```

## Properties ##

| Name | Description |
|:-----|:------------|
| StatusCode | DEPRECATED: The [HTTP Status code](HttpStatusCodes) that corresponds to this error. This element is deprecated and may be removed in a future version of the API |
| ErrorMessages | A list of messages giving more detail about the error |
| ErrorCode | A unique identifier for the specific error encountered |

## Schema ##

```
start = error

error = element ErrorResult {
  element StatusCode {xsd:int},
  element ErrorMessages { 
    element Message {text}+
  },
  element ErrorCode {xsd:string}?
}
```


## Example: Failed Validation ##

If validation of a request fails, we may return multiple error messages.
The example below is the response to a create task operation where the title is null, and the due date is out of bounds.

```
HTTP/1.1 400 Bad Request
Cache-Control: private
Content-Length: 393
Content-Type: application/vnd.huddle.error+xml
Date: Thu, 17 Feb 2011 12:10:41 GMT
```
```
<?xml version="1.0" encoding="utf-8"?>
<ErrorResult xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <StatusCode>400</StatusCode>
  <ErrorMessages>
    <Message>DueDate: Must be later than PlannedStartDate</Message>
    <Message>Title: You must provide a value</Message>
  </ErrorMessages>
  <ErrorCode>Validation</ErrorCode>
</ErrorResult>
```

## Example: Resource matching failed ##

If we are unable to match your request to a URI, or to match the body of your request to an acceptable resource, we will return a 404 with some debugging information
The response below is the result of a GET request for a malformed folder URI.
```
HTTP/1.1 404 Not Found
Cache-Control: private
Content-Length: 394
Content-Type: application/vnd.huddle.error+xml
Date: Thu, 17 Feb 2011 12:14:35 GMT
```
```
<?xml version="1.0" encoding="utf-8"?>
<ErrorResult>
  <StatusCode>404</StatusCode>
  <ErrorMessages>
    <Message>We were unable to handle your request, there is no method for GET at this URI.

Request mapped to the uri /folders/{folderId} 
the following parameters were bound:
folderId : foo is not a number
</Message>
  </ErrorMessages>
  <ErrorCode>ObjectNotFound</ErrorCode>
</ErrorResult>
```


# Error Responses #

## Create a document ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_an\_empty\_document\_with\_description\_more\_than\_2048\_characters\_is\_created|400  |BadRequest |Validation|Description: Must be between 0 and 2048 characters"|
|When\_an\_empty\_document\_without\_title\_is\_created|400  |BadRequest |Validation|Title: Must be between 1 and 128 characters|
|When\_an\_empty\_document\_with\_title\_more\_than\_128\_characters\_is\_created|400  |BadRequest |Validation|Title: Must be between 1 and 128 characters|
|When\_the\_folder\_id\_is\_a\_string|404  |NotFound   |    | |
|When\_the\_folder\_id\_is\_invalid|404  |NotFound   |    | |
|When\_the\_folder\_id\_is\_omitted|404  |NotFound   |    | |
|When\_creating\_a\_document\_in\_a\_folder\_that\_is\_in\_a\_workspace\_you\_are\_not\_member\_of|403  |Forbidden  |is not a member of the workspace| |
|When\_creating\_a\_document\_in\_the\_document\_library\_folder|403  |Forbidden  |Not authorised to write in the DocumentLibrary folder| |
|When\_creating\_a\_document\_in\_a\_folder\_that\_has\_been\_deleted|410  |Gone       |ObjectDeleted|The folder has been deleted|
|When\_creating\_a\_document\_and\_the\_user\_has\_no\_available\_space|403  |Forbidden  |NoSpaceAvailable|Storage capacity has been exceeded for workspace|
|When\_a\_member\_creates\_a\_document\_in\_a\_folder\_that\_has\_only\_read\_permission|403  |Forbidden  |    | |


## Retrieving a document ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_retrieving\_a\_document\_that\_does\_not\_exist\_xml|404  |NotFound   |ObjectNotFound|The object could not be found|
|When\_retrieving\_a\_document\_that\_is\_deleted\_xml|410  |Gone       |ObjectDeleted|The object has been deleted|
|When\_retrieving\_a\_document\_in\_a\_workspace\_of\_which\_you\_are\_not\_a\_member\_xml|403  |Forbidden  |Authorization|is not a member of the workspace|
|When\_a\_team\_member\_requests\_a\_document\_in\_a\_folder\_but\_the\_team\_has\_no\_permissions\_on\_that\_folder\_xml|403  |Forbidden  |Authorization|The user is not a member of a team which has permissions to view this document|
|When\_a\_team\_member\_requests\_a\_document\_created\_by\_himself\_in\_a\_folder\_but\_the\_team\_has\_no\_permissions\_on\_that\_folder\_xml|403  |Forbidden  |Authorization|The user does not have permissions to view this document|
|When\_the\_users\_account\_is\_suspended\_xml|403  |Forbidden  |AccountStatus|Account suspended|
|When\_a\_member\_retrieves\_a\_document\_that\_is\_locked\_by\_someone\_else|200  |OK         |created with no create version or delete link|
|When\_the\_doc\_id\_is\_a\_string\_xml|404  |NotFound   |    | |
|When\_the\_doc\_id\_is\_ommited|404  |NotFound   |    | |
|When\_a\_manager\_retrieves\_a\_document\_and\_the\_document\_has\_moving\_status|200  |OK         |should not have an unlock link|             |
|When\_we\_change\_the\_permissions\_of\_the\_current\_user\_to\_none\_between\_two\_calls|     | | | |


## Downloading document content ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_a\_manager\_downloads\_a\_deleted\_document|410  |Gone       |ObjectDeleted|The object has been deleted|
|When\_a\_members\_team\_has\_no\_permissions|403  |Forbidden  |Authorization|The user is not a member of a team which has permissions to view this document|
|When\_the\_user\_is\_manager\_of\_a\_payment\_failed\_account|403  |Forbidden  |AccountStatus|Account payment failed|
|When\_the\_user\_is\_not\_a\_member\_of\_the\_workspace|403  |Forbidden  |Authorization|is not a member of the workspace|
|When\_the\_version\_id\_is\_a\_string|404  |NotFound   |ResourceMatchingFailed|
|When\_the\_version\_id\_is\_invalid|404  |NotFound   |ObjectNotFound|The DocumentVersion is invalid|
|When\_the\_version\_id\_is\_omitted|404  |NotFound   |    | |
|When\_the\_document\_has\_no\_content|204  |NoContent  |    | |
|When\_the\_document\_doesnt\_have\_a\_media\_type|200  |OK         |    | |
|When\_downloading\_document\_content\_and\_the\_document\_has\_moving\_status|409  |Conflict   |DocumentProcessingStatus|The document is currently being moved|

## Uploading document content ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_a\_workspace\_manager\_tries\_to\_upload\_the\_contents\_of\_a\_document\_locked\_by\_someone\_else |409  |Conflict   |Conflict|The document is locked by another user|
|When\_a\_workspace\_manager\_tries\_to\_upload\_the\_contents\_of\_a\_document\_and\_he\_is\_not\_the\_owner\_of\_the\_upload\_process|403  |Forbidden  |Authorization|The user is not the owner of this version|
|When\_i\_upload\_the\_content\_of\_a\_file\_but\_I\_do\_not\_have\_permissions\_on\_the\_folder|403  |Forbidden  |    | |
|When\_a\_manager\_uploads\_the\_content\_of\_a\_file\_in\_a\_folder\_in\_which\_he\_does\_not\_have\_permission  |200  |OK??       |    | |
|When\_i\_upload\_the\_content\_of\_a\_file\_in\_a\_locked\_workspace|403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_the\_account\_is\_suspended |403  |Forbidden  |AccountStatus|Account suspended|
|When\_the\_document\_is\_deleted |410  |Gone       |ObjectDeleted|The object has been deleted|
|When\_there\_is\_no\_available\_space\_on\_the\_account|403  |Forbidden  |NoSpaceAvailable|Storage capacity has been exceeded for workspace|
|When\_a\_document\_owner\_tries\_to\_upload\_the\_contents\_of\_a\_document\_locked\_by\_the\_workspace\_manager|409  |Conflict   |The document is locked by another user| |
|When\_the\_upload\_guid\_has\_already\_been\_used|403  |Forbidden  |Authorization|This upload URI has already been used|
|When\_the\_upload\_id\_does\_not\_exist|404  |NotFound   |ObjectNotFound|The upload id does not exist|



## Updating document title and description ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_a\_member\_with\_full\_permissions\_on\_the\_folder\_edits\_the\_details\_of\_a\_document\_created\_by\_another\_member|403  |Forbidden  |Authorization|The user is not the owner of the document or the workspace manager|
|When\_a\_member\_with\_see\_only\_permissions\_edits\_the\_details\_of\_a\_document\_created\_by\_the\_member|204  |NoContent  |    | |
|When\_a\_member\_with\_no\_permissions\_edits\_the\_details\_of\_a\_document\_created\_by\_the\_member|403  |Forbidden  |Authorization|The user is not a member of a team which has permissions to view folder " + _folderId_|
|When\_a\_member\_with\_full\_permissions\_edits\_the\_details\_of\_a\_document\_created\_by\_the\_member|204  |NoContent  |    | |
|When\_editing\_the\_details\_of\_a\_document\_of\_another\_workspace|403  |Forbidden  |Authorization|User " + TheActingUser.Username + " is not a member of the workspace|
|When\_the\_current\_user\_account\_is\_suspended|403  |Forbidden  |AccountStatus|
|When\_the\_current\_user\_account\_is\_payment\_failed|403  |Forbidden  |AccountStatus|Account payment failed|
|When\_the\_current\_user\_account\_is\_deleted|403  |Forbidden  |AccountStatus|Account deleted|
|When\_the\_current\_user\_account\_is\_subscribing|403  |Forbidden  |AccountStatus|Account not activated|
|When\_the\_workspace\_is\_locked|403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_the\_workspace\_is\_deleted|410  |Gone       |WorkspaceDeleted|The workspace has been deleted.|
|When\_the\_workspace\_is\_finalised|403  |Forbidden  |WorkspaceStatus|
|When\_the\_document\_is\_locked\_and\_the\_destination\_folder\_remains\_the\_same|204  |NoContent  |    | |
|When\_editing\_a\_deleted\_document|410  |Gone       |ObjectDeleted|The object has been deleted|
|When\_putting\_an\_empty\_title|400  |BadRequest |Validation|Title: Must be between 1 and 128 characters|
|When\_putting\_an\_title\_of\_more\_that\_128\_characters|400  |BadRequest |Validation|Title: Must be between 1 and 128 characters|
|When\_putting\_an\_description\_of\_more\_that\_2048\_characters|400  |BadRequest |Validation|Description: Must be between 0 and 2048 characters|
|When\_the\_title\_is\_not\_posted|400  |BadRequest |Validation|Title: You must provide a value|
|When\_the\_description\_is\_not\_posted|400  |BadRequest |Validation|Description: You must provide a value|
|When\_putting\_a\_string\_as\_a\_document\_id|404  |NotFound   |    | |
|When\_omitting\_the\_document\_id|404  |NotFound   |    | |
|When\_putting\_a\_non\_existing\_document\_id|404  |NotFound   |    | |
|When\_editing\_document\_details\_and\_the\_document\_has\_moving\_in\_process\_status|409  |Conflict   |DocumentProcessingStatus|The document is currently being moved|





## Move a document ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_the\_workspace\_is\_locked\_and\_a\_move\_is\_perfomed|403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_the\_account\_is\_payment\_failed\_and\_a\_move\_is\_performed|403  |Forbidden  |WorkspaceStatus|Account payment failed|
|When\_moving\_a\_document\_to\_a\_workspace\_that\_the\_user\_is\_not\_a\_member\_of|403  |Forbidden  |Authorization|is not a member of the workspace|
|When\_moving\_a\_locked\_document\_from\_a\_workspace\_that\_the\_user\_is\_not\_a\_member\_of|403  |Forbidden  |Authorization|User is not a member of the workspace|
|When\_a\_manager\_with\_see\_only\_permissions\_moves\_a\_document\_locked\_by\_himself\_to\_a\_folder\_with\_see\_only\_permissions|204  |NoContent  |    | |
|When\_a\_manager\_with\_see\_only\_permissions\_moves\_a\_document\_locked\_by\_someone\_else\_to\_a\_folder\_with\_see\_only\_permissions|409  |Conflict   |Conflict|The document is locked by another user|
|When\_a\_manager\_with\_see\_only\_permissions\_moves\_and\_edit\_a\_document\_created\_by\_a\_member\_to\_a\_folder\_with\_see\_only\_permissions|204  |NoContent  |    | |
|When\_a\_manager\_with\_see\_only\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_edit\_permissions|204  |NoContent  |    | |
|When\_a\_manager\_with\_see\_only\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_no\_permissions|204  |NoContent  |    | |
|When\_a\_manager\_with\_edit\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_see\_only\_permissions|204  |NoContent  |    | |
|When\_a\_manager\_with\_edit\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_edit\_permissions|204  |NoContent  |    | |
|When\_a\_manager\_with\_edit\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_no\_permissions|204  |NoContent  |    | |
|When\_a\_manager\_with\_no\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_see\_only\_permissions|204  |NoContent  |    | |
|When\_a\_manager\_with\_no\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_edit\_permissions|204  |NoContent  |    | |
|When\_a\_manager\_with\_no\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_no\_permissions|204  |NoContent  |    | |
|When\_a\_document\_owner\_with\_see\_only\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_edit\_permissions|403  |Forbidden  |Authorization|User does not have edit permission on folder " + _originalFolderId + " or its contents_|
|When\_a\_document\_owner\_with\_edit\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_see\_only\_permissions|403  |Forbidden  |Authorization|User does not have edit permission on target folder|
|When\_a\_document\_owner\_with\_edit\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_no\_permissions\_and\_changing\_title\_and\_descriptio|403  |Forbidden  |Authorization|User does not have edit permission on target folder {0}", _destinationFolderId_|
|When\_a\_document\_owner\_with\_no\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_edit\_permissions|403  |Forbidden  |Authorization|User does not have edit permission on folder " + _originalFolderId + " or its contents_|
|When\_a\_workspace\_member\_with\_edit\_permissions\_moves\_a\_document\_to\_a\_folder\_with\_see\_only\_permissions|403  |Forbidden  |Authorization|User does not have edit permission on target folder " + _destinationFolderId_|
|When\_a\_workspace\_member\_with\_edit\_permissions\_moves\_a\_document\_locked\_by\_himself\_to\_a\_folder\_with\_see\_permissions|403  |Forbidden  |Authorization|User does not have edit permission on target folder " + _destinationFolderId_|
|When\_a\_workspace\_member\_with\_edit\_permissions\_moves\_a\_document\_locked\_by\_himself\_to\_a\_folder\_with\_edit\_permissions|204  |NoContent  |    | |
|When\_a\_document\_owner\_with\_edit\_permissions\_moves\_a\_document\_locked\_by\_himself\_to\_a\_folder\_with\_edit\_permissions|204  |NoContent  |    | |
|When\_a\_document\_owner\_with\_edit\_permissions\_moves\_a\_document\_locked\_by\_someone\_else\_to\_a\_folder\_with\_edit\_permissions|409  |Conflict   |Conflict|The document is locked by another user|
|When\_the\_destination\_folder\_is\_deleted|403  |Forbidden  |Authorization|The target parent has been deleted|
|When\_the\_destination\_folder\_does\_not\_exist|400  |BadRequest |BadDataFormat|The target folder does not exist.|
|When\_the\_destination\_folder\_id\_is\_a\_string|400  |BadRequest |BadRequest|Parent link format is not valid.|
|When\_the\_destination\_folder\_id\_is\_ommited|400  |BadRequest |BadRequest|Parent link format is not valid.|
|When\_the\_destination\_folder\_is\_not\_posted|400  |BadRequest |BadRequest|Parent link was not provided.|
|When\_moving\_a\_document\_that\_has\_moving\_in\_process\_status|409  |Conflict   |Conflict|The document processing status is not complete|

## Moving a document to a new workspace ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_moving\_a\_document\_that\_is\_processing|409  |Conflict   |Conflict|The document processing status is not complete|
|When\_editing\_a\_document\_and\_moving\_it\_to\_a\_deleted\_folder|403  |Forbidden  |Authorization|The target parent has been deleted|
|When\_user\_is\_manager\_in\_the\_source\_workspace\_but\_not\_in\_the\_destination\_workspace\_and\_has\_readonly\_permissions\_on\_both\_source\_and\_target\_folders|403  |Forbidden  |Authorization|Not authorised to write inside this folder|
|When\_moving\_a\_document\_to\_a\_workspace\_where\_the\_account\_is\_suspended|403  |Forbidden  |AccountStatus|Account suspended|
|When\_moving\_a\_document\_to\_a\_workspace\_where\_the\_account\_is\_payment\_required|403  |Forbidden  |AccountStatus|Account payment failed|
|When\_moving\_a\_document\_to\_a\_workspace\_where\_the\_account\_is\_subscribing|403  |Forbidden  |AccountStatus|Account not activated|
|When\_moving\_a\_document\_to\_a\_workspace\_where\_the\_account\_is\_deleted|403  |Forbidden  |AccountStatus|Account deleted|
|When\_moving\_a\_document\_to\_a\_workspace\_that\_is\_deleted|410  |Gone       |WorkspaceDeleted|The workspace has been deleted|
|When\_moving\_a\_document\_to\_a\_workspace\_that\_is\_locked|403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_moving\_a\_document\_to\_a\_workspace\_that\_is\_finalized|403  |Forbidden  |Authorization|The workspace has been finalised|


## Creating a new version of a document ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_the\_member\_creates\_new\_document\_version\_on\_a\_folder\_in\_which\_he\_has\_readonly\_permission|403  |Forbidden  |Authorization|Not authorised to write inside this folder|
|When\_the\_member\_creates\_new\_document\_version\_on\_a\_folder\_in\_which\_he\_has\_no\_permission|403  |Forbidden  |Authorization|Not authorised to write inside this folder|
|When\_the\_user\_is\_not\_member\_of\_the\_workspace|403  |Forbidden  |Authorization|is not a member of the workspace|
|When\_the\_manager\_creates\_new\_document\_version\_on\_a\_document\_locked\_by\_someone\_else|409  |Conflict   |Conflict|The document is locked by another user|
|When\_the\_title\_is\_empty|400  |BadRequest |Validation|Title: Must be between 1 and 128 characters|
|When\_an\_empty\_document\_with\_description\_more\_than\_2048\_characters\_is\_created|400  |BadRequest |Validation|Description: Must be between 0 and 2048 characters"|
|When\_an\_empty\_document\_with\_title\_more\_than\_128\_characters\_is\_created|400  |BadRequest |Validation|Title: Must be between 1 and 128 characters|
|When\_adding\_a\_new\_version\_and\_the\_document\_id\_is\_a\_string|404  |NotFound   |    | |
|When\_adding\_a\_new\_version\_and\_the\_document\_id\_is\_omitted|404  |NotFound   |    | |
|When\_adding\_a\_new\_version\_and\_the\_document\_id\_is\_invalid|404  |NotFound   |    | |
|When\_creating\_a\_new\_version\_and\_the\_account\_is\_suspended|403  |Forbidden  |AccountStatus|Account suspended|
|When\_creating\_a\_new\_version\_and\_the\_account\_is\_payment\_failed|403  |Forbidden  |AccountStatus|Account payment failed|
|When\_creating\_a\_new\_version\_and\_the\_account\_is\_deleted|403  |Forbidden  |AccountStatus|Account deleted|
|When\_creating\_a\_new\_version\_and\_the\_account\_is\_subscribing|403  |Forbidden  |AccountStatus|Account not activated|
|When\_creating\_a\_new\_version\_in\_a\_deleted\_workspace|410  |Gone       |WorkspaceDeleted|The workspace has been deleted.|
|When\_creating\_a\_new\_version\_in\_a\_locked\_workspace|403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_creating\_a\_new\_version\_and\_the\_document\_is\_deleted|410  |Gone       |ObjectDeleted|The object has been deleted|
|When\_posting\_without\_title\_but\_with\_description|201  |Created    |    | |
|When\_creating\_a\_new\_version\_and\_the\_document\_has\_moving\_status|409  |Conflict   |DocumentProcessingStatus|The document is currently being moved|



## Deleting a document ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_a\_manager\_deletes\_a\_document\_owned\_by\_someone\_else\_in\_a\_folder\_on\_which\_he\_has\_readonly\_permission|200  |OK         |    | |
|When\_a\_member\_deletes\_the\_document\_created\_by\_someone\_else|403  |Forbidden  |Authorization|The user is not the owner of the document or the workspace manager|
|When\_a\_member\_deletes\_a\_document\_they\_own\_in\_a\_folder\_on\_which\_he\_has\_readonly\_permission|200  |OK         |    | |
|When\_a\_member\_deletes\_a\_document\_they\_own\_in\_a\_folder\_on\_which\_he\_has\_no\_permission|403  |Forbidden  |Authorization|The user is not a member of a team which has permissions to view folder|
|When\_a\_member\_deletes\_the\_document\_in\_a\_workspace\_he\_is\_not\_member\_of|403  |Forbidden  |Authorization|User " + _anotherUser.Username +" is not a member of the workspace_|
|When\_a\_document\_owner\_deletes\_the\_document\_in\_a\_workspace\_he\_is\_no\_longer\_a\_member\_of|403  |Forbidden  |Authorization|User " + _anotherUser.Username + " is not a member of the workspace_|
|When\_deleting\_the\_document\_and\_the\_account\_is\_suspended |403  |Forbidden  |AccountStatus|Account suspended|
|When\_deleting\_the\_document\_and\_the\_account\_is\_payment\_required|403  |Forbidden  |AccountStatus|Account payment failed|
|When\_deleting\_the\_document\_and\_the\_account\_is\_deleted |403  |Forbidden  |AccountStatus|Account deleted|
|When\_deleting\_the\_document\_and\_the\_account\_is\_subscribing|403  |Forbidden  |AccountStatus|Account not activated|
|When\_deleting\_the\_document\_and\_the\_workspace\_is\_deleted|410  |Gone       |WorkspaceDeleted|The workspace has been deleted|
|When\_deleting\_the\_document\_and\_the\_workspace\_is\_locked|403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_deleting\_the\_document\_and\_the\_workspace\_is\_finalised|403  |Forbidden  |WorkspaceStatus|The workspace has been finalised|
|When\_deleting\_an\_already\_deleted\_document|410  |Gone       |ObjectDeleted|The object has been deleted|
|When\_deleting\_a\_document\_and\_the\_id\_is\_a\_string|404  |NotFound   |    | |
|When\_deleting\_a\_document\_and\_the\_id\_is\_invalid|404  |NotFound   |    | |
|When\_deleting\_a\_document\_and\_the\_id\_is\_omitted|404  |NotFound   |    | |
|When\_deleting\_the\_document\_and\_the\_document\_has\_moving\_status|409  |Conflict   |DocumentProcessingStatus|The document is currently being moved|

## Restoring a document ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_a\_member\_restores\_a\_document\_that\_they\_did\_not\_delete|403  |Forbidden  |Authorization|The user does not have permission to delete document|
|When\_a\_user\_restores\_a\_document\_in\_a\_workspace\_he\_is\_not\_member\_of|403  |Forbidden  |Authorization|User " + _anotherUser.Username +" is not a member of the workspace_|
_anotherUser.Username + " is not a member of the workspace_
|When\_restoring\_the\_document\_and\_the\_account\_is\_suspended |403  |Forbidden  |AccountStatus|Account suspended|
|When\_restoring\_the\_document\_and\_the\_account\_is\_payment\_required|403  |Forbidden  |AccountStatus|Account payment failed|
|When\_restoring\_the\_document\_and\_the\_account\_is\_deleted |403  |Forbidden  |AccountStatus|Account deleted|
|When\_restoring\_the\_document\_and\_the\_account\_is\_subscribing|403  |Forbidden  |AccountStatus|Account not activated|
|When\_restoring\_the\_document\_and\_the\_workspace\_is\_deleted|410  |Gone       |WorkspaceDeleted|The workspace has been deleted|
|When\_restoring\_the\_document\_and\_the\_workspace\_is\_locked|403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_restoring\_the\_document\_and\_the\_workspace\_is\_finalised|403  |Forbidden  |WorkspaceStatus|The workspace has been finalised|
|When\_restoring\_a\_document\_and\_the\_id\_is\_a\_string|404  |NotFound   |    | |
|When\_restoring\_a\_document\_and\_the\_id\_is\_invalid|404  |NotFound   |    | |
|When\_restoring\_a\_document\_and\_the\_id\_is\_omitted|404  |NotFound   |    | |
|When\_restoring\_a\_permanently\_deleted\_document|410  |Gone       |ObjectDeleted|The object has been permanently deleted|

## Creating a document lock ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_an\_owner\_locks\_a\_document\_in\_a\_folder\_on\_which\_he\_has\_readonly\_permission|403  |Forbidden  |Authorization|User does not have edit permission on folder " + _expectedFolderId + " or its contents_|
|When\_an\_owner\_locks\_a\_document\_in\_a\_folder\_on\_which\_he\_has\_no\_permission|403  |Forbidden  |Authorization|User does not have edit permission on folder " + _expectedFolderId + " or its contents_|
|When\_a\_member\_locks\_a\_document\_in\_a\_workspace\_he\_is\_not\_member\_of|403  |Forbidden  |Authorization|User is not a member of the workspace|
|When\_a\_member\_locks\_a\_document\_that\_is\_already\_locked\_in\_a\_workspace\_he\_is\_not\_member\_of|403  |Forbidden  |Authorization|User is not a member of the workspace|
|When\_a\_manager\_locks\_an\_already\_locked\_document\_by\_himself|409  |Conflict   |Conflict|The document is already locked|
|When\_a\_manager\_locks\_an\_already\_locked\_document\_by\_someone\_else |409  |Conflict   |Conflict|The document is already locked|
|When\_a\_manager\_locks\_a\_deleted\_document|410  |Gone       |ObjectDeleted|The object has been deleted|
|When\_locking\_the\_document\_and\_the\_account\_is\_suspended|403  |Forbidden  |AccountStatus|Account suspended|
|When\_locking\_the\_document\_and\_the\_account\_is\_payment\_required|403  |Forbidden  |AccountStatus|Account payment failed|
|When\_locking\_the\_document\_and\_the\_account\_is\_deleted|403  |Forbidden  |AccountStatus|Account deleted|
|When\_locking\_the\_document\_and\_the\_account\_is\_subscribing |403  |Forbidden  |AccountStatus|Account not activated|
|When\_locking\_the\_document\_and\_the\_workspace\_is\_deleted|410  |Gone       |WorkspaceDeleted|The workspace has been deleted.|
|When\_locking\_the\_document\_and\_the\_workspace\_is\_locked |403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_locking\_the\_document\_and\_the\_workspace\_is\_finalised|403  |Forbidden  |WorkspaceStatus|
|When\_locking\_the\_document\_and\_the\_id\_is\_a\_string |404  |NotFound   |    | |
|When\_locking\_the\_document\_and\_the\_id\_is\_omitted|404  |NotFound   |    | |
|When\_locking\_the\_document\_and\_the\_id\_is\_invalid|404  |NotFound   |    | |
|When\_locking\_the\_document\_and\_the\_document\_has\_moving\_status  |409  |Conflict   |DocumentProcessingStatus|The document is currently being moved|




## Deleting a document lock ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_the\_member\_unlocks\_a\_document\_that\_someone\_else\_locked\_in\_a\_folder\_with\_edit\_permission|403  |Forbidden  |Authorization|The document is locked by another user|
|When\_the\_owner\_unlocks\_a\_document\_that\_someone\_else\_locked\_in\_a\_folder\_with\_read\_only\_permission|403  |Forbidden  |Authorization|User does not have edit permission on folder " + _expectedFolderId + " or its contents_|
|When\_the\_owner\_unlocks\_a\_document\_that\_they\_locked\_in\_a\_folder\_with\_read\_only\_permission|403  |Forbidden  |Authorization|User does not have edit permission on folder " + _expectedFolderId + " or its contents_|
|When\_the\_owner\_unlocks\_a\_document\_that\_they\_locked\_in\_a\_folder\_with\_no\_permission|403  |Forbidden  |Authorization|User does not have edit permission on folder " + _expectedFolderId + " or its contents_|
|When\_the\_owner\_unlocks\_a\_document\_that\_somebody\_else\_locked\_in\_a\_folder\_with\_edit\_permission|403  |Forbidden  |Authorization|The document is locked by another user|
|When\_a\_manager\_unlocks\_a\_document\_in\_a\_workspace\_they\_dont\_belong\_to|403  |Forbidden  |Authorization|not a member of the workspace|
|When\_a\_manager\_unlocks\_a\_document\_in\_a\_workspace\_they\_dont\_belong\_to\_and\_the\_document\_is\_not\_locked|403  |Forbidden  |Authorization|not a member of the workspace|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_account\_is\_suspended|403  |Forbidden  |AccountStatus|Account suspended|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_account\_is\_payment\_required|403  |Forbidden  |AccountStatus|Account payment failed|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_account\_is\_deleted|403  |Forbidden  |AccountStatus|Account deleted|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_account\_is\_subscribing|403  |Forbidden  |AccountStatus|Account not activated|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_workspace\_is\_deleted|410  |Gone       |ObjectDeleted|The workspace has been deleted|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_workspace\_is\_locked|403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_workspace\_is\_finalized|403  |Forbidden  |WorkspaceStatus|The workspace has been finalised|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_document\_is\_deleted|410  |Gone       |ObjectDeleted|The object has been deleted|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_document\_is\_not\_locked|404  |NotFound   |ObjectNotFound|There is no lock on the document|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_document\_id\_is\_a\_string|404  |NotFound   |ResourceMatchingFailed|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_document\_id\_is\_ommitted|404  |NotFound   |ResourceMatchingFailed|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_document\_id\_is\_invalid|404  |NotFound   |ResourceMatchingFailed|
|When\_a\_manager\_unlocks\_a\_document\_and\_the\_document\_has\_moving\_status|409  |Conflict   |DocumentProcessingStatus|The document is currently being moved|

## Retrieving document version history lock ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_the\_filename\_is\_empty|400  |BadRequest |Validation|FileName: Must be between 1 and 2048 characters|
|When\_a\_member\_gets\_the\_history\_of\_a\_document\_created\_by\_himself\_in\_a\_folder\_with\_no\_permissions|403  |Forbidden  |Authorization|The user does not have permissions to view this document|
|When\_a\_member\_gets\_the\_history\_of\_a\_document\_in\_a\_workspace\_he\_is\_not\_member\_of  |403  |Forbidden  |Authorization|User " + _member.Username + " is not a member of the workspace_|
|When\_getting\_the\_history\_and\_the\_account\_is\_suspended|403  |Forbidden  |AccountStatus|Account suspended|
|When\_getting\_the\_history\_and\_the\_account\_is\_payment\_failed|403  |Forbidden  |AccountStatus|Account payment failed|
|When\_getting\_the\_history\_and\_the\_account\_is\_deleted|403  |Forbidden  |AccountStatus|Account deleted|
|When\_getting\_the\_history\_and\_the\_account\_is\_subscribing|403  |Forbidden  |AccountStatus|Account not activated|
|When\_getting\_the\_history\_and\_the\_workspace\_is\_deleted|410  |Gone       |WorkspaceDeleted|The workspace has been deleted|
|When\_getting\_the\_history\_and\_the\_workspace\_is\_locked|403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_getting\_the\_history\_and\_the\_document\_has\_been\_deleted |410  |Gone       |ObjectDeleted|The object has been deleted|
|When\_getting\_the\_history\_and\_the\_document\_id\_is\_a\_string |404  |NotFound   |    | |
|When\_getting\_the\_history\_and\_the\_document\_id\_is\_invalid|404  |NotFound   |    | |
|When\_getting\_the\_history\_and\_the\_document\_id\_is\_omitted|404  |NotFound   |    | |

## Restoring a folder ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_a\_member\_with\_permissions\_restores\_a\_folder\_deleted\_by\_the\_member|204  |NoContent  |    | |
|When\_a\_workspace\_manager\_with\_permissions\_restores\_a\_folder|204  |NoContent  |    | |
|When\_a\_member\_restores\_a\_folder\_that\_they\_did\_not\_delete|403  |Forbidden  |Authorization|The user does not have permission to delete folder|
|When\_a\_member\_restores\_a\_subfolder\_they\_own\_in\_a\_folder\_on\_which\_he\_has\_no\_permission|403  |Forbidden  |Authorization|The user is not a member of a team which has permissions to view folder|
|When\_a\_member\_deletes\_the\_folder\_in\_a\_workspace\_he\_is\_not\_member\_of|403  |Forbidden  |Authorization|User " + _anotherUser.Username +" is not a member of the workspace_|
|When\_a\_folder\_owner\_restores\_the\_folder\_in\_a\_workspace\_he\_is\_no\_longer\_a\_member\_of|403  |Forbidden  |Authorization|User " + _anotherUser.Username + " is not a member of the workspace_|
|When\_restoring\_the\_folder\_and\_the\_account\_is\_suspended |403  |Forbidden  |AccountStatus|Account suspended|
|When\_restoring\_the\_folder\_and\_the\_account\_is\_payment\_required|403  |Forbidden  |AccountStatus|Account payment failed|
|When\_restoring\_the\_folder\_and\_the\_account\_is\_deleted |403  |Forbidden  |AccountStatus|Account deleted|
|When\_restoring\_the\_folder\_and\_the\_account\_is\_subscribing|403  |Forbidden  |AccountStatus|Account not activated|
|When\_restoring\_the\_folder\_and\_the\_workspace\_is\_deleted|410  |Gone       |WorkspaceDeleted|The workspace has been deleted|
|When\_restoring\_the\_folder\_and\_the\_workspace\_is\_locked|403  |Forbidden  |WorkspaceStatus|The workspace has been locked|
|When\_restoring\_the\_folder\_and\_the\_workspace\_is\_finalised|403  |Forbidden  |WorkspaceStatus|The workspace has been finalised|
|When\_restoring\_a\_folder\_and\_the\_id\_is\_a\_string|404  |NotFound   |    | |
|When\_restoring\_a\_folder\_and\_the\_id\_is\_invalid|404  |NotFound   |    | |
|When\_restoring\_a\_folder\_and\_the\_id\_is\_omitted|404  |NotFound   |    | |
|When\_restoring\_a\_permanently\_deleted\_folder|410  |Gone       |ObjectDeleted|The object has been permanently deleted|

## Creating a discussion post ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_the\_discussion\_is\_deleted|410  |Gone       |DiscussionDeleted|The discussion has been deleted|
|When\_the\_workspace\_is\_locked|403  |Forbidden  |Authorisation|The workspace has been locked|
|When\_the\_workspace\_is\_deleted|410  |Gone       |WorkspaceDeleted|The workspace has been deleted|
|When\_the\_workspace\_is\_archived|403  |Forbidden  |Authorisation|The workspace has been finalised|
|When\_the\_account\_is\_suspended|403  |Forbidden  |Authorisation|Account suspended|
|When\_a\_new\_post\_is\_added\_to\_a\_payment\_failed\_account|403  |Forbidden  |Authorisation|Account payment failed|
|When\_the\_account\_is\_deleted|403  |Forbidden  |Authorisation|Account deleted|
|When\_a\_new\_post\_is\_added\_to\_a\_subscribing\_account|403  |Forbidden  |AccountStatus|Account not activated|
|When\_the\_user\_is\_not\_a\_workspace\_member|403  |Forbidden  |Authorisation|User {0} is not a member of the workspace|
|When\_the\_discussion\_post\_exceeds\_the\_limit|400  |BadRequest |BadDataFormat|The discussion post provided for discussion with ID 123 exceeded the character limit|


## Notification Headers ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_the\_x-notification-scope\_has\_no\_value|400  |BadRequest |BadRequest|Value required for notification scope header|
|When\_the\_x-notification-scope\_has\_a\_non\_valid\_value|400  |BadRequest |BadRequest|The notification scope of dfgdfg does not exist and is not valid|
|When\_the\_x-notification\_scope\_header\_has\_the\_value\_of\_stakeholders\_and\_the\_x-in\_reply-to is missing|400  |BadRequest |BadRequest|No x-in-reply-to header specified|
|When\_the\_x-in\_reply\_to\_has\_no\_value|400  |BadRequest |BadRequest|The notification provided is not a valid Guid for discussion 27229183|
|When\_the\_x-in\_reply\_to\_value\_is\_not\_a\_guid|400  |BadRequest |BadRequest|The notification provided is not a valid Guid for discussion 123|
|When\_the\_x-in\_reply\_to\_is\_a\_guid\_that\_does\_not\_correspond\_to\_the\_discussion|404  |NotFound   |ObjectNotFound|Cannot retrieve notifications group for notification id 123|

## Editing an assignment ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_an\_open\_assignment\_is\_rejected\_without\_a\_comment|400  |BadRequest |Validation|A comment must be specified when rejecting an approval|
|When\_an\_open\_assignment\_is\_rejected\_with\_an\_empty\_comment|400  |BadRequest |Validation|A comment must be specified when rejecting an approval|
|When\_a\_complete\_assignment\_is\_rejected|400  |BadRequest |Validation|Assignment has already been completed|
|When\_a\_complete\_assignment\_is\_marked\_open|400  |BadRequest |Validation|Assignment has already been completed|
|When\_a\_complete\_assignment\_is\_marked\_complete|400  |BadRequest |Validation|Assignment has already been completed|
|When\_an\_open\_assignment\_is\_rejected\_with\_a\_comment\_2049\_chars\_in\_length|400  |BadRequest |Validation|Comment: Must be between 0 and 2048 characters|

## Creating a new comment ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_a\_comment\_is\_posted\_with\_2049\_chars|400  |BadRequest |Validation|Content: Must be between 1 and 2048 characters|
|When\_a\_comment\_is\_posted\_with\_0\_chars|400  |BadRequest |Validation|Content: Must be between 1 and 2048 characters|
|When\_a\_comment\_is\_posted\_with\_a\_stakeholder\_notification\_scope\_but\_no\_id|400  |BadRequest |BadRequest|No x-in-reply-to header specified|

## Creating a new comment on a whiteboard ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_a\_comment\_is\_posted\_with\_2049\_chars|400  |BadRequest |Validation|The whiteboard comment provided for whiteboard with ID 123 exceeded the character limit|
|When\_a\_comment\_is\_posted\_with\_0\_chars|400  |BadRequest |Validation|Content: Must be between 1 and 2048 characters|
|When\_a\_comment\_is\_posted\_with\_a\_stakeholder\_notification\_scope\_but\_no\_id|400  |BadRequest |BadRequest|No x-in-reply-to header specified|
|When\_the\_whiteboard\_is\_deleted|410  |Gone       |WhiteboardDeleted|The whiteboard has been deleted|
|When\_the\_workspace\_is\_locked|403  |Forbidden  |Authorisation|The workspace has been locked|
|When\_the\_workspace\_is\_deleted|410  |Gone       |WorkspaceDeleted|The workspace has been deleted|
|When\_the\_workspace\_is\_archived|403  |Forbidden  |Authorisation|The workspace has been finalised|
|When\_the\_account\_is\_suspended|403  |Forbidden  |Authorisation|Account suspended|
|When\_a\_new\_post\_is\_added\_to\_a\_payment\_failed\_account|403  |Forbidden  |Authorisation|Account payment failed|
|When\_the\_account\_is\_deleted|403  |Forbidden  |Authorisation|Account deleted|
|When\_a\_new\_post\_is\_added\_to\_a\_subscribing\_account|403  |Forbidden  |AccountStatus|Account not activated|
|When\_the\_user\_is\_not\_a\_workspace\_member|403  |Forbidden  |Authorisation|User {0} is not a member of the workspace|
|When\_the\_whiteboard\_id\_is\_invalid|404  |NotFound   |ObjectNotFound|Cannot add a comment for whiteboard id 123|

## Retrieving progress for a cascade folder permissions operation ##
|Case|Error|Description|Code|Error Message|
|:---|:----|:----------|:---|:------------|
|When\_the\_folder\_does\_not\_exist|404  |NotFound   |ObjectNotFound|The folder could not be found.|
|When\_there\_is\_no\_permission\_cascade\_progress\_data\_for\_the\_folder|404  |NotFound   |ObjectNotFound|No permission cascade progress data found for folder 123|
|When\_the\_user\_is\_authorised\_to\_edit\_permissions\_on\_the\_folder|403  |Forbidden  |Authorisation|The user does not have rights to edit permissions on this folder.|
