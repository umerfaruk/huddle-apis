# Summary #

Huddle use HTTP Status Codes to inform you of the success or failure of an action. Individual resources might overload the meaning of a status code, but unless explicitly stated, these definitions are valid for all operations.

| Status | Definition |
|:-------|:-----------|
| 200 OK | We have processed your request and returned the data your asked for. |
| 201 Created | We have processed your request and have created a new resource as a result. You will have a [Location header](HttpHeaders#Location) with the GETable URI of your new resource, and you should have a response containing your new resource |
| 202 Accepted | We have processed your request, but there is additional work still to be done. You should have received a [Location header](HttpHeaders#Location) with a GETable URI where you can check for the status of your operation. |
| 204 No Content | We have processed your request and there is no body content in the response. |
| 400 Bad Request | We are unable to service your request. It is likely to be incorrectly formatted or otherwise unreadable. You should have received an [error](ErrorResponse) structure containing more detailed information. |
| 401 Unauthorized | We are unable to authenticate your request. No processing has been performed. You should have received a [www-authenticate](HttpHeaders#Authenticate) header with more information. See: [Authentication](Authentication) for details about Huddle's authentication schemes. |
| 403 Forbidden | You are not allowed to access or modify the resource you have requested. You should have received an [error](ErrorResponse) structure containing more detailed information. See [access controls](AccessControlEntry) for details about Huddle's access control mechanism. |
| 404 Not Found | We are unable to process your request, as we can not find a resource matching the URI. You may have received an [error](ErrorResponse) structure containing more detailed information. |
| 409 Conflict | The resource you are attempting to create or modify conflicts with an existing resource. You should have received an [error](ErrorResponse) structure containing more detailed information. |
| 410 Gone | The resource you are attempting to modify or retrieve has been deleted. You should have received an [error](ErrorResponse) structure containing more detailed information. |
| 422 Unprocessable Entity | The request was well-formed but was unable to be followed due to semantic errors. You have to take action in order to resolve the issue. |
