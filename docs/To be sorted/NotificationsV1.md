# Summary #

Notifications are alerts of changes to content in Huddle that the author chose to make the recipient aware of.

Discussions are a Classic API See [Classic](Classic)

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Mark a list of notifications as read|Released    |


# Operations #

## Mark a list of notifications as read ##

Mark a list of notifications as read.

### Example Request ###
```
POST v1/json/notifications/delete HTTP/1.1
Host: api.huddle.net
```
```
{"Data":[277777,344444]
```
### Example Response ###
```
{"Error":null,"Success":true}
```

## Error Codes ##
400: The id provided could not be recognised

401: Authentication error

403: You do not have permission to access the item requested

500: There was an error processing your request
